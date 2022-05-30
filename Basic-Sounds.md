***
This Guide has been updated to 1.4. If you need to view the old 1.3 version of this wiki page, click [here](https://github.com/tModLoader/tModLoader/wiki/Basic-Sounds/0dd271b8a344c26fb1a1b375b1250d4f7069c626)
***

# Introduction
We can use sounds in many places in tModLoader and this guide will educate you on how tModLoader organize and use sounds.

# Sound Basics
There are 2 concepts to be aware of. The first is the actual sound asset. This correlates with a specific sound file, such as a .wav file. The second concept is the `SoundStyle`, which is an object representing a sound asset and associated playback settings.

## Sound Assets

Sound assets can be found in your ModSources folder and in the Terraria install directory. Sound assets can be many different sound file formats including .wav, .ogg, .mp3, and .xnb. For example, [ExampleMod](https://github.com/tModLoader/tModLoader/tree/1.4/ExampleMod/Assets/Sounds/Items/Guns) has an `ExampleGun.ogg` file found in `ExampleMod\Assets\Sounds\Items\Guns\ExampleGun.ogg`. In your Terraria install folder, you'll find hundreds of sound files in the `C:\Program Files (x86)\Steam\steamapps\common\Terraria\Content\Sounds` folder. These files are .xnb files, which is a special format that you can't listen to directly by double clicking. To listen to these sounds on your computer, see the [Extract](#Extract) section below.

## SoundStyle

Existing `SoundStyle`s can be found in the `Terraria.ID.SoundID` class and new `SoundStyle`s can be created in your mod for variations on existing sound assets or playing sound assets contained within your mod.

To create a new `SoundStyle`, you'll need the path to the sound file without the file extension. Using that path, you can create the SoundStyle object and store it for later use. 

```cs
// Creating a SoundStyle from a sound file in this Mod, then playing it
SoundStyle ExampleGunSoundStyle = new SoundStyle("ExampleMod/Assets/Sounds/Items/Guns/ExampleGun");
SoundEngine.PlaySound(ExampleGunSoundStyle);

// Creating a SoundStyle from a sound file from Terraria, then playing it
SoundStyle CoinsSoundStyle = new SoundStyle("Terraria/Sounds/Coins");
SoundEngine.PlaySound(CoinsSoundStyle);
```
Instead of assigning to a variable, you can directly play the sound:
```cs
// Creating a SoundStyle from a sound file in this Mod, then playing it
SoundEngine.PlaySound(new SoundStyle("ExampleMod/Assets/Sounds/Items/Guns/ExampleGun"));

// Creating a SoundStyle from a sound file from Terraria, then playing it
SoundEngine.PlaySound(new SoundStyle("Terraria/Sounds/Coins"));
```
`SoundStyle`s can be further customized to adjust volume, pitch, overlap behavior, and more. See the [Customizing Sound Playback](#Customizing-Sound-Playback) section below to learn more.

# Play Sounds
Some sounds play automatically, while others can be manually played.

## Pre-Determined Event Sounds

The most common usage of sounds is to play them when certain events occur. For example, when an `Item` is used, the `Item.UseSound` is played automatically. By assigning a `SoundStyle` to these existing fields, the sound will play automatically when expected. 

We can assign a `SoundStyle` to `Item.UseSound`, `NPC.HitSound`, `NPC.DeathSound`, `ModWall.HitSound`, and `ModTile.HitSound`. We will do these in the corresponding `SetDefaults` override. Below are some examples using existing `SoundStyle`s:

```cs
// using Terraria.ID; needed at top of .cs file

public override void SetDefaults()
{
	// other code
	Item.UseSound = SoundID.Item1;  // sword swing sound
}

public override void SetDefaults()
{
	// other code
	NPC.HitSound = SoundID.NPCHit24; // Giant Tortoise hit sound
	NPC.DeathSound = SoundID.NPCDeath4; // Bat death sound
}
```
Other available existing sounds to use can be found through [Intellisense](https://github.com/tModLoader/tModLoader/wiki/Why-Use-an-IDE#autocomplete--intellisense), but be aware that most sounds have fairly generic names. You'll want to consult the [Finding Sounds](#Finding-Sounds) section below if you are trying to find a specific sound.    
![](https://i.imgur.com/LIX0kff.png)     

When using an existing `SoundStyle`, you inherit the playback settings assigned to that `SoundStyle`. See the [Customizing Sound Playback](#Customizing-Sound-Playback) section below to learn more.

## Play Sounds Manually

To play a sound manually, use the `SoundEngine.PlaySound` method. This method is in the `Terraria.Audio;` namespace, so make sure you have `using Terraria.Audio;` at the top of your .cs file.

The `SoundEngine.PlaySound` method has 2 parameters. The first is the `SoundStyle`, this is required. The 2nd parameter is an optional `Vector2` representing the position of the sound in world coordinates. If the position is ommitted, the sound is played without any panning effect as if it were happening in the center of the screen.

To play an existing sound, simply call the method:
```cs
SoundEngine.PlaySound(SoundID.Item59); // piggy bank oink
```

### Where do I put this code?
Wherever makes sense, but commonly `ModNPC.AI` or `ModProjectile.AI` are the most frequent places to manually play sounds. Using randomness or timers can help make your content seem natural.

# Customizing Sound Playback

Many exising `SoundStyle`s found in the `SoundID` class are pre-configured with various playback customization. For example, if you play `SoundID.NPCHit24`, you'll notice that it is half as loud as `new SoundStyle("Terraria/Sounds/NPC_Hit_24")`. Each `SoundStyle` has data attached to it that configures these custom behaviors.

To customize a `SoundStyle`, we will use the [`with` syntax](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/with-expression). The `with` syntax basically allows us to create a copy of an existing object except with some specified changes. For example, if we use `NPC.HitSound = SoundID.NPCHit4;` in our `ModNPC` but find that the volume is too high to fit our enemy, we could create a `SoundStyle` copy with custom volume like this: `NPC.HitSound = SoundID.NPCHit4 with { Volume = 0.7f };` 

The `with` syntax works with modded and vanilla sounds, and also allows multiple tweaks. The most common tweaks are explored below. Here is how multiple tweaks can be used in a single `with` statement:

```cs
NPC.HitSound = SoundID.NPCHit4 with { 
    Volume = 0.5f, 
    Pitch = 0.5f, 
    PitchVariance = 0.1f, 
    MaxInstances = 3, 
    SoundLimitBehavior = SoundLimitBehavior.IgnoreNew 
};
```

## Volume
Volume defaults to `1f` (100%) and can range from `0f` to `1f`.

## Pitch
Pitch can be adjusted up or down and defaults to `0f`. The lower limit, `-1f`, is down one octive and the upper limit, `1f`, is up one octive.

## Pitch Variance
Pitch variance is a randomness added to the pitch each time the sound is played. This can add some variety to sounds to make them less repetitive. 

## MaxInstances
This dicatates how many instances of a sound can be playing at the same time. The default is `1`. Adjust this to allow overlapping sounds.

## SoundLimitBehavior
When the `MaxInstances` limit is reached, this tweak adjusts what will happen. The default is to `ReplaceOldest`, which will restart the sound. The other option is `IgnoreNew`, which will ignore the latest attempt to play the sound.

# Finding Sounds
For many mods, reusing sounds that come with Terraria is a great idea. Terraria comes with over 700 sounds, but it can be difficult to remember or find a specific sound. There are several approaches to finding a Terraria sound to use. 
1. Extract all the sounds and play them in your media player
2. Find a sound from an item or npc you remember
3. Consult the source code to find the code that plays a specific sound

## Extract
The Terraria sound files are .xnb files, you cannot play these files on your computer directly. [TConvert](https://forums.terraria.org/index.php?threads/tconvert-extract-content-files-and-convert-them-back.61706/) can extract the Terraria sound files and save them as .wav files that you can easily load into VLC or whatever media player you have on your computer. Playing them all one after the other can be a quick way to find a unique sound to use.

## Find a Sound on the Terraria Wiki
If you consult the [Sound IDs page on the Official Terraria wiki](https://terraria.wiki.gg/wiki/Sound_IDs), you can find and play any sound. The internal name column corresponds to the `SoundID` field you would use. Use this approach if you know a specific enemy or item makes a sound and you want to find that sound. 

## Mod Testing Mods
Some mods can help find and play existing sounds:

[Modders Toolkit](https://forums.terraria.org/index.php?threads/modders-toolkit-a-mod-for-modders-doing-modding.55738/) has an option to log sounds, allowing you to see what code is causing sounds to play and also see modded sound paths as well as sound types and sound styles. It also has tools to test volume and pitch customization and generate sound playback code that you can paste into your mod.

## Source Code (Expert Prerequisites needed)
By searching `NPC.SetDefaults` or `Item.SetDefaults` using the `ItemID` or `NPCID` number, you can easily find sounds corresponding to what you want.

Not all sounds played in Terraria are just `UseSound`,`HitSound`, or `DeathSound` data, so you must use this approach if you wish to find sounds that are played under special circumstances. For example, you may wonder about the quack sounds that ducks play on occasion. Searching the source for the NPCID of Duck, 362, you will find one of the results shows `SoundEngine.PlaySound(30, (int)position.X, (int)position.Y);` nested within the condition that it is daytime and a 1 in 200 chance. We can now use this information to implement a new duck correctly.

We'll have to change this code to fit the tModLoader approach, so change it to `SoundEngine.PlaySound(SoundID.Duck, position);`

## Spread Sheets
Look up UseSound, HitSound, and DeathSound of vanilla Items and NPC here:    
[Vanilla Item Field Values](https://github.com/tModLoader/tModLoader/wiki/Vanilla-Item-Field-Values)    
[Vanilla NPC Field Values](https://github.com/tModLoader/tModLoader/wiki/Vanilla-NPC-Field-Values)    
Note: The spread sheets are currently out of date, so you'll have to convert the number it says to a `SoundID` field. For example, if the `UseSound` of an item says "20", you'll have to change that to `SoundID.Item20`.

## Find Sounds from other Sources
If you want to find a sound on the internet, try to look for sounds that are legally free to use.

# Common Errors
### The name SoundID doesn't exist in the current context
Add `using Terraria.ID;` to the top of your source file.

### The name SoundEngine doesn't exist in the current context
Add `using Terraria.Audio;` to the top of your source file.

### The type or namespace name 'SoundStyle' could not be found (are you missing a using directive or an assembly reference?)
Add `using Terraria.Audio;` to the top of your source file.

### "Ensure that the specified stream contains valid PCM mono or stereo wave data."
Run your .wav through [Audacity](http://www.audacityteam.org/download/) using `File->Export Audio->Wav (Microsoft) signed 16-bit PCM`. For reference, the following are the allowed parameters for Wav files:
1. Must be a PCM wave file
2. Can only be mono or stereo
3. Must be 8 or 16 bit
4. Sample rate must be between 8,000 Hz and 48,000 Hz

## Relevant References
* [Vanilla SoundIDs](https://github.com/tModLoader/tModLoader/wiki/Vanilla-Sound-IDs)

## Not covered in Basic level
Music -- Music is handled in a separate manner.