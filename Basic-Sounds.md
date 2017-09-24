# Basic Sounds
We can use sounds in many places in tModLoader and this guide will educate you on how Terraria/tModLoader organize and use sounds.

## How are sounds organized?
Terraria has 2 ways of organizing sounds. 

The first is a pair of numbers (ints): a sound Type/ID and a sound style. An example would be Item sounds. Item use sounds have always shared the same sound type, but had different sound styles to specify which sound within the sound type to play. For example: The sound that all summon items use is Sound type 2 and sound style 44, while the Space gun plays Sound type 2 and sound style 12. 

The second way is a LegacySoundStyle object. A LegacySoundStyle object is actually just that same pair of numbers as above but contained in an object with a few additional features such as pitch variance and volume. Using the above examples, summon weapons would use SoundID.Item44 and SoundID.Item12 respectively. 

Knowing that there are 2 separate ways to specify a particular sound is good to know.

## How to set sounds?

We can specify sounds for when certain things happen in the game. We can specify Item.UseSound, NPC.HitSound, and NPC.DeathSound. We will do these in ModItem.SetDefaults and ModNPC.SetDefaults. Below are some examples:

    public override void SetDefaults()
    {
        // other code
        item.UseSound = SoundID.Item1;  // sword swing sound
    }

    public override void SetDefaults()
    {
        // other code
        npc.HitSound = SoundID.NPCHit24; // Giant Tortoise hit sound
        npc.DeathSound = SoundID.NPCDeath4; // Bat death sound
    }

ModTiles have a ModTile.soundType and ModTile.soundStyle that specify the sound that is played when the tile is hit. Note that these aren't LegacySoundStyle objects, but rather ints. 

## How to listen to vanilla sounds and find the one I want?
To use a vanilla sound, first you must figure out which one you want. Below are several ways to find the sound you want.

### Mod
[Dust and Sound Catalogue 2](https://forums.terraria.org/index.php?threads/dust-and-sound-catalogue-2.42370/) lets you experiment in game with sounds.

### Extract
[TConvert](https://forums.terraria.org/index.php?threads/tconvert-extract-content-files-and-convert-them-back.61706/) can extract the sound files to .wav files that you can easily load into VLC or whatever media player you have on your computer.

### Spread Sheets
Look up UseSound, HitSound, and DeathSound of vanilla Items and NPC here:
[Vanilla Item Field Values](https://github.com/blushiemagic/tModLoader/wiki/Vanilla-Item-Field-Values)
[Vanilla NPC Field Values](https://github.com/blushiemagic/tModLoader/wiki/Vanilla-NPC-Field-Values)
Note: The spread sheets are currently out of date, so you'll have to convert the number it says to a LegacySoundStyle instance. For example, if the useSound of an item says "20", you'll have to change that to "SoundID.Item20".

### Source Code (Expert Prerequisites needed)
By searching NPC.SetDefaults or Item.SetDefaults using the ItemID or NPCID number, you can easily find sounds corresponding to what you want.

Not all sounds played in Terraria are just UseSound, HitSound, or DeathSound data, so you must use this approach if you wish to find sounds that are played under special circumstances. For example, you may wonder about the quack sounds that ducks play on occasion. Searching the source for the NPCID of Duck, 362, you will find one of the results shows `Main.PlaySound(30, (int)this.position.X, (int)this.position.Y, 1, 1f, 0f);` nested within the condition that it is daytime and a 1 in 200 chance. We can now use this information to implement a new duck correctly.

## How to play sounds manually?
There are several methods we can use with various parameters. Read above for the 2 ways sounds are represented. Here are the method signatures, all in the Terraria.Main class:

    public static SoundEffectInstance PlaySound(LegacySoundStyle type, Vector2 position)
    public static SoundEffectInstance PlaySound(LegacySoundStyle type, int x = -1, int y = -1)
    public static SoundEffectInstance PlaySound(int type, int x = -1, int y = -1, int Style = 1, float volumeScale = 1f, float pitchOffset = 0f)

The simplest way to play vanilla sounds is as follows:

   Main.PlaySound(SoundID.Item59); // piggy bank oink

You will notice that we omit the optional parameters of the 2nd method signature. If that confuses you, Google it. What even are x and y? x and y represent the position in world space that the sound should play. Most of the time, however, we want to specify that. Here are a couple examples of ways to do that:

    Main.PlaySound(SoundID.Item56, player.Center.X); // Boing
    Main.PlaySound(SoundID.Item56, (int)player.Center.X, (int)player.Center.Y); // Boing

Here we are using the 1st and 2nd method signatures, this time providing either a valid Vector2 (player.Center) or 2 ints (X and Y) as parameters. By passing in a position, we can give our sounds a position which will affect the pan and volume appropriately.

Note that for playing a sound manually in a ModTile hook, you'll want to multiply tile coordinates by 16 to properly position the tile in world coordinates.

### Where do I put this code?
Wherever makes sense, but usually things like ModNPC.AI, ModProjectile.AI, ModTile.KillSound.

### What about custom mod sounds?
This whole guide focused on vanilla sounds, but everything we learned above is the same for custom sounds if you understand a few concepts.

Replace LegacySoundStyles in method arguments from SoundID with mod.GetLegacySoundSlot calls.
Replace style with the results from mod.GetSoundSlot in cases where you put both type and style.

Examples:

   Main.PlaySound(SoundLoader.customSoundType, -1, -1, mod.GetSoundSlot(SoundType.Custom, "Sounds/Custom/WatchOut"));
   Main.PlaySound(mod.GetLegacySoundSlot(SoundType.Custom, "Sounds/Custom/BananaImpact").WithVolume(.7f).WithPitchVariance(.5f));
   item.UseSound = mod.GetLegacySoundSlot(SoundType.Item, "Sounds/Item/FireballSound");
   npc.HitSound = mod.GetLegacySoundSlot(SoundType.NPCHit, "Sounds/NPCHit/EnemyHurtSqueak");
   Main.PlaySound(2, -1, -1, mod.GetSoundSlot(SoundType.Item, "Sounds/Item/Wooo"));

## Additional Tricks

### Adjust Volume
We can use either LegacySoundStyle methods or PlaySound parameters to affect volume:
    
    Main.PlaySound(SoundID.Item59.WithVolume(.5f));
    Main.PlaySound(2, -1, -1, 59, .5f);
### Adjust Pitch
Same for random pitch:

    Main.PlaySound(SoundID.Item59.WithPitchVariance(.2f)); // randomly up or down at most .2f pitch
For specific pitch:
    
    Main.PlaySound(2, -1, -1, 59, 1f, -.2f); // down a few notes
For something like Harp, consult source code.

## Common Errors
### The name SoundID doesn't exist in the current context
Add `using Terraria.ID;` to the top of your source file.
### No sound plays with custom sounds
Check the spelling and try again.

## Relevant References
* [Vanilla SoundIDs](https://github.com/blushiemagic/tModLoader/wiki/Vanilla-Sound-IDs)

## Not covered in Basic level
ModSound -- A way to allow overlapping sounds and other advanced things.
Music -- Music is handled in a separate manner.