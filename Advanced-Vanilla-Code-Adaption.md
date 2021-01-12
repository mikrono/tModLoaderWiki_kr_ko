# Vanilla Code Adaption
This guide teaches how to navigate the jungle that is Terraria source code to find or adapt code. Why? Maybe you wish to customize an aiStyle, maybe you want to know how Magma Stone works, or maybe you want to mimic a vanilla effect.

## TL;DR
* Change `this.X` code to the corresponding object, such as `npc.X` or `player.X`
* If you are manually copying aiStyles, you can delete all code related to other types to make the code more readable
  * For example, Projectile AiStyle 1 is 3500 lines of code, but can be pruned down to about 5 lines of code, see [Basic Projectile - AiStyle 1](https://github.com/tModLoader/tModLoader/wiki/Basic-Projectile#aistyle-1) for that code.
* Use F2 to rename auto-generated variable names (num233, flag83, etc) as you figure out what each variable represents

## Obtaining Vanilla Source Code
It is against the law to publish the decompiled source code, but anyone with a competent computer can decompile their personal copy of Terraria.exe no problem. Note that you may wish to end up with a decompiled tModLoader rather than a decompiled vanilla Terraria so that you know where tModLoader hooks are. See [Advanced Prerequisites](https://github.com/tModLoader/tModLoader/wiki/Advanced-Prerequisites) for more info on how to decompile tModLoader. For just adaption purposes, it might be easier to do via the "Simple Way" because it creates `this.thrownDamage += 0.15f;` instead of `thrownDamage += 0.15f;`, helping mass-replace `this` and `base` with `player` (tML setup doesn't do that).

After you have a copy of the source, open the `Terraria.csproj` file so you can easily search the whole project.

## Example: Armor Effects: Ninja Shirt
A simple example for investigating vanilla code is figuring out how vanilla armor or accessories achieve their special effects. For this example, we want to know how Ninja Shirt increases throwing damage by 15%. (Follow along for whatever effect you are interested in.) The first step is to find the ItemID. Searching ItemID.cs, you will find where NinjaShirt is defined:

![](http://i.imgur.com/fxnOPXA.png)

We now must search the entire project for "257", using the Quick Find tool (Ctrl-F, don't mistake the "Search Solution Explorer as the search tool, that is different). Be sure to set "Match whole word" here to limit results. In the results, you will see many references to "257", you must now trim down the results to find the relevant results. You may find a result in Item.SetDefaults1, setting the default values for the result, but this code isn't responsible for the throwing damage bonus, so we must continue searching. Continuing down the list of results, you can skip the results in NPC.cs, as those probably relate to whatever NPCID is equal to 257, not the ItemID of 257. Finally, you will come to the results in Player.cs and your screen will show this:

![](http://i.imgur.com/n0YKKbB.png)

Bingo! This looks right. Now we want to bring this code, `this.thrownDamage += 0.15f;`, into our ModItem's code. First, override the UpdateAccessory hook in your ModItem class. Next, paste the code in and you'll notice that your IDE is complaining about the code you just pasted. The code has "this", which refers to the Player instance. Our code is in ModItem, not Player, so trying to use "this" and modifying the value of the "thrownDamage" field won't work, since ModItem doesn't have a "thrownDamage" field. Luckily, the UpdateAccessory hook has a Player object passed in, so we change our code to `player.thrownDamage += 0.15f;` and it all works out.

## Example: Accessory Effect: Magma Stone

Another example. Search ItemID.cs to find `MagmaStone = 1322;`. Now search for "1322", you'll find a result in Player.UpdateAccessory that does `this.magmaStone = true;`. (It is worth noting that this code is setting a bool field in the Player object and is exactly the same thing us modders would do in our ModPlayer.) Now we must investigate this "magmaStone" variable. Rather than searching the code using the find menu, this time we will use the Find All References feature of Visual Studio:

![](http://i.imgur.com/RtAjhYF.png)

Now we go through the 9 results in search of what we want to know. 2 of the results are setting the bool to true and correspond to ModItem.UpdateAccessory. 1 of the results sets the magmaStone bool to false, this is akin to ModPlayer.ResetEffects. The Results in Player.StatusNPC and Player.StatusPvP seem to be applying a buff to the target. Looking up 24 in BuffID shows us the buff is OnFire. These results correspond to ModPlayer.ModifyHitPvp and ModPlayer.ModifyHitNPC. The 6th result is in Player.ItemCheck and appears to be spawning some Dust. We know that Fire particles spawn when melee weapons are swung, so this code must correspond to that. How would you mimic this behavior yourself? We need to search nearby code and look for tModLoader hooks. tModLoader hooks have code like `PlayerHooks.Something` or `ItemLoader.Something`, so lets search for that. Luckily, this result is right next to the hook!

![](http://i.imgur.com/UyxK0l6.png)

Now let's look in the [documentation](http://tmodloader.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod_player.html#a48027ba9e2e4188d3b73b49fa60071f2)! As you can see, the documentation also agrees, this is the hook to use to spawn dust and other effects.

The last 3 results are in Projectile.cs. They do the same thing that earlier results did, but for melee projectiles instead of melee items. (Dust, Hit NPC, Hit PvP) The corresponding hooks we use to replicate this behavior is in: ModPlayer.ModifyHitNPCWithProj, ModPlayer.ModifyHitPvpWithProj, and GlobalProjectile.PostAI.

Remember to adapt the code you find to the context the hooks give you. Replace "this" with the correct object variable, "Main.npc[i]" with "target", and so on.

## Example: Projectile or NPC AI code
This guide teaches how to adapt vanilla AI code for new uses. In Beginner level coding, we sometimes use vanilla aiStyles for our ModProjectiles or ModNPC. In Intermediate level coding, we take that a step further by setting aiType to mimic the specifics of an aiStyle or even use PostAI or PreAI to add additional code to vanilla aiStyles for our particular thing. This is not always enough, so in this tutorial, we will learn how to investigate vanilla source code to find the information or code snippets we need to completely customize the things in our Mod. 

## Example: Item and Projectile: Shadowbeam Staff clone
Shadowbeam is a pretty neat weapon that shoots a projectile that bounces several times instantaneously. Lets make a weaker version of the Shadowbeam Staff and then attempt to customize the look of the projectile.

### Weaker Shadowbeam Staff clone
The first step in our journey is cloning the vanilla item. This is seen a few times in ExampleMod, but below is the code.

```cs
using Microsoft.Xna.Framework;
using Terraria.ID;
using Terraria.ModLoader;

namespace ExampleMod.Items
{
	class ShadowbeamStaffClone : ModItem
	{
		public override string Texture => "Terraria/Item_" + ItemID.ShadowbeamStaff;

		public override void SetDefaults()
		{
			item.CloneDefaults(ItemID.ShadowbeamStaff);
			item.color = Color.Purple; // This just tweaks the weapon sprite in inventory, just so we know which ShadowBeam Staff is ours and not vanillas
		}
	}
}
```

![](https://i.imgur.com/ADBy27n.png)    
Huh, something is off, the staff isn't held right. Lets find the value of `ItemID.ShadowbeamStaff` in ItemID.cs. We find that it is 1444. Lets now search the source for 1444.
![](https://i.imgur.com/RvhdTQC.png)     
Out of these matching results, we see an NPC loot drop, prefix assignment, SetDefaults, ItemID, and 2 pieces of code that we haven't seen before. The one in AchievementInitializer sounds like it is related to achievements, so lets look at this last result. This last result is a crazy mess due to the decompilation process, but we can see a switch statement here that has a goto. Goto might be new to you, so read [this](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/goto) first. Now that we know what goto is, lets click on the label that the goto is assigned and press F12 on the keyboard to follow it.
![](https://i.imgur.com/864MpVV.png)    
Ahah! We forgot to set Item.staff to true. If you studied ExampleMod's ExampleStaff, you would have already known about this. Lets add this to our code in SetStaticDefaults and also take this opportunity to reduce the damage of our weapon:

```cs
using Microsoft.Xna.Framework;
using Terraria;
using Terraria.ID;
using Terraria.ModLoader;

namespace ExampleMod.Items
{
	class ShadowbeamStaffClone : ModItem
	{
		public override string Texture => "Terraria/Item_" + ItemID.ShadowbeamStaff;

		public override void SetStaticDefaults()
		{
			Item.staff[item.type] = true;
		}

		public override void SetDefaults()
		{
			item.CloneDefaults(ItemID.ShadowbeamStaff);
			item.color = Color.Purple; // This just tweaks the weapon sprite in inventory, just so we know which ShadowBeam Staff is ours and not vanillas
			item.damage = 8; // Down from 53
		}
	}
}
```

Now we have a working clone of Shadowbeam Staff that is weaker. Feel free to adjust item.mana and other fields to your liking.

### Shadowbeam Projectile AI
Now that we have the Item working, you may think that modifying the color or number of bounces would be simple, but it is not. This is all hard-coded into the AI of the Shadowbeam Projectile, so unfortunately we have to make a new ModProjectile and edit a copy of the vanilla AI code. Lets first investigate to find where the purple color of the Shadowbeam staff projectile comes from. First, lets look at the Shadowbeam Staff item's `item.shoot` value. In Item.SetDefaults, we see `this.shoot = 294;`. Great, lets find that in ProjectileID.cs:     
![](https://i.imgur.com/wZJOeIg.png)     
Ah, notice that there is also a `ShadowBeamHostile`. ProjectileIDs with Hostile in their name usually refer to projectiles shot by enemy NPCs, be aware of this in your future source code exploration. Lets search the source for "294". Most of the 45 results are referring to the Item and Tile that share the ID of 45, but the 4 results in Projectile.cs seem promising. The first result shows us the Projectile.SetDefaults. Of note is `this.extraUpdates = 100;`, what could that possibly be? As a spoiler, that is how the bouncing effect works. It lets the AI code execute 100 times each frame rather than once as normal. Without it, the instant bouncing effect wouldn't be easily possible. Also noteworthy is `this.aiStyle = 48`, we will have to find the code for this later.   
![](https://i.imgur.com/kYPxxnI.png)    
The second result is in a method called Projectile.Damage. If you remember, the Shadowbeam Staff loses damage as it penetrates enemies, this must be related to that effect. Hopefully you decompiled tModLoader instead of Terraria because you'll be able to find that the ModifyHitNPC hook is called close to this code. Later we'll need to adapt this code into out replacement ModProjectile we make.   
![](https://i.imgur.com/J8cVwsp.png)    
The 3rd result is in HandleMovement. It is a bit hard to find, but the related tModLoader hook is OnTileCollide.  
The 4th result is in Projectile.AI, this is the code we'll need to adapt. Below you can see a neat feature of Visual Studio in action. Hover over the guidelines and you'll see a listing of the nested conditions that lead to this code. Notably, we see that this code is within an if statement checking `this.aiStyle == 48`.   
![](https://i.imgur.com/ml8sPMu.png)    
Now that we've found all the relevant code, now we need to investigate how the color is decided. This might be a good time to check what other projectiles use projectile aiStyle 48. Searching for `this.aiStyle = 48` reveals that HeatRay, UFOLaser, MagnetSphereBolt, and ShadowBeamHostile all also share aiStyle 48. We know that HeatRay is yellow, but Shadowbeam Staff is purple. Knowing this, we know that the code for the color must be within an if statement within the AI code. Going back to the 4th result, the result in the AI method, we look for anything that looks like code related to color. Ahah! We find the code `int num448 = Dust.NewDust(vector33, 1, 1, 173, 0f, 0f, 0, default(Color), 1f);` and look up the 173rd dust in the [dust spritesheet](https://github.com/tModLoader/tModLoader/wiki/Basic-Dust):    
![](https://i.imgur.com/FLhIi1G.png)    
Yep, we've figured out why the Shadowbeam Staff projectile is purple, it is because dust 173 is spawned in AI. Let's remember to change this to spawn dust 178, a lime green projectile, for our modified clone. If you look at the rest of the code for aiStyle 48, you'll probably notice that there is nothing limiting the number of bounces. Through some trial and error, you'll discover that projectile.extraUpates and projectile.timeLeft are responsible. A timeLeft of 300 would normally stick around for 5 seconds, but because of extraUpdates being 100, it actually only lives for 3 ticks/frames. Lets change timeLeft to 200 for our weaker clone, this will shorten the reach of our weapon.   

Now that we've found all relevant pieces of code in the source code, lets make a ModProjectile clone of the vanilla ShadowBeamFriendly projectile. Lets first also remember to add in `item.shoot = mod.ProjectileType<ShadowbeamStaffCloneProjectile>();` to our ModItem so our weapon won't shoot the vanilla projectile. Next, we need to copy over the AI code we found to our own AI method and remove anything related to other projectile types that we are not interested in. We also need to remember to remove the if statements originally checking for the projectile type we were interested in but leave the code from inside that if statement block. Remember, when adopting vanilla code, be sure to change `this.` and `base.` things to the correct thing, in this case `projectile.`. Also remember not to copy over `this.aiStlye = 48` since we will be copying over and editing the vanilla AI code instead. Also, rename things like num447 with the F2 button in Visual Studio. Below is the complete code after taking into account all the vanilla code:

Vanilla:    
![](https://i.imgur.com/2ZMWHOg.png)    
Our New Projectile:    
![](https://i.imgur.com/dyawndQ.png)    

```cs
using Microsoft.Xna.Framework;
using Terraria;
using Terraria.ID;
using Terraria.ModLoader;

namespace ExampleMod.Items
{
	class ShadowbeamStaffClone : ModItem
	{
		// Remove this and the item.color line in SetDefaults once you make your own sprite/texture.
		public override string Texture => "Terraria/Item_" + ItemID.ShadowbeamStaff;

		public override void SetStaticDefaults()
		{
			Item.staff[item.type] = true;
		}

		public override void SetDefaults()
		{
			item.CloneDefaults(ItemID.ShadowbeamStaff);
			item.color = Color.Purple; // This just tweaks the weapon sprite in inventory, just so we know which ShadowBeam Staff is ours and not vanillas. Remove this line once you have your own sprite/texture
			item.damage = 8; // Down from 53
			// Very important!
			item.shoot = mod.ProjectileType<ShadowbeamStaffCloneProjectile>();
		}
	}

	class ShadowbeamStaffCloneProjectile : ModProjectile
	{
		public override void SetDefaults()
		{
			projectile.width = 4;
			projectile.height = 4;
			// NO! projectile.aiStyle = 48;
			projectile.friendly = true;
			projectile.magic = true;
			projectile.extraUpdates = 100;
			projectile.timeLeft = 200; // lowered from 300
			projectile.penetrate = -1;
		}

		// Note, this Texture is actually just a blank texture, FYI.
		public override string Texture => "Terraria/Projectile_" + ProjectileID.ShadowBeamFriendly;

		public override bool OnTileCollide(Vector2 oldVelocity)
		{
			if (projectile.velocity.X != oldVelocity.X)
			{
				projectile.position.X = projectile.position.X + projectile.velocity.X;
				projectile.velocity.X = -oldVelocity.X;
			}
			if (projectile.velocity.Y != oldVelocity.Y)
			{
				projectile.position.Y = projectile.position.Y + projectile.velocity.Y;
				projectile.velocity.Y = -oldVelocity.Y;
			}
			return false; // return false because we are handling collision
		}

		public override void ModifyHitNPC(NPC target, ref int damage, ref float knockback, ref bool crit, ref int hitDirection)
		{
			projectile.damage = (int)(projectile.damage * 0.8);
		}

		public override void AI()
		{
			projectile.localAI[0] += 1f;
			if (projectile.localAI[0] > 9f)
			{
				for (int i = 0; i < 4; i++)
				{
					Vector2 projectilePosition = projectile.position;
					projectilePosition -= projectile.velocity * ((float)i * 0.25f);
					projectile.alpha = 255;
					// Important, changed 173 to 178!
					int dust = Dust.NewDust(projectilePosition, 1, 1, 178, 0f, 0f, 0, default(Color), 1f);
					Main.dust[dust].noGravity = true;
					Main.dust[dust].position = projectilePosition;
					Main.dust[dust].scale = (float)Main.rand.Next(70, 110) * 0.013f;
					Main.dust[dust].velocity *= 0.2f;
				}
			}
		}
	}
}
```


If you've been following along without just copying the code above, you would have noticed that dust 178 defaults to being affected by gravity. This is easily remedied by adding `Main.dust[dust].noGravity = true;` to the code.

Well, we've done it, we've investigated and cloned a vanilla projectile AI. We've also done some very basic editing to the AI to change the color of the trail. We can do the same for other projectiles as well, search the source code, identify code we wish to customize, then copy and then edit the vanilla code into a new ModProjectile.

## Example: NPC: NPC clone with modified projectile (Hoplite)

### Exact Clone

To start, lets make a basic clone of an NPC as seen in Party Zombie. Lets clone the Hoplite. After some searching, we discover `GreekSkeleton = 481;` in NPCID and use that for our basic clone. For simplicity, I will use the `=>` functionality ([Read about =>](https://msdn.microsoft.com/en-us/magazine/dn802602.aspx))

The following is the code:

```cs
using Terraria;
using Terraria.ID;
using Terraria.ModLoader;

namespace ExampleMod.NPCs
{
	class SuperHoplite : ModNPC
	{
		public override string Texture => "Terraria/NPC_" + NPCID.GreekSkeleton;

		public override void SetStaticDefaults()
		{
			DisplayName.SetDefault("Super Hoplite");
			Main.npcFrameCount[npc.type] = Main.npcFrameCount[NPCID.GreekSkeleton];
		}

		public override void SetDefaults()
		{
			npc.CloneDefaults(NPCID.GreekSkeleton);
			aiType = NPCID.GreekSkeleton;
			animationType = NPCID.GreekSkeleton;
		}
	}
}
```  

Go in game and then use a mod with an NPC spawner like Cheat Sheet or Heros Mod and spawn your NPC. You'll notice that the sprite and behavior is the same as the original Hoplite. (Feel free to change the sprite later the normal way.) While underground, you'll notice that our Hoplite clone attacks with that javelin as normal. The goal of the rest of this tutorial is to change that projectile and other behaviors.

### Preparatory work for modifications

Unfortunately, there is no easy way to change the projectile that is shot by our clone. Our clone, as seen in our SetDefaults method, simply clones the vanilla AI code. The projectile the vanilla AI will spawn to attack the player is hard-coded into the AI method. The first thing we need is the [decompiled Terraria source code](https://github.com/tModLoader/tModLoader/wiki/Advanced-Prerequisites#tmodloader-source-code). Once you have that, now we must find the AI code to copy. Find NPCID.GreekSkeleton and see that its value is 481, now search for 481. The first result we need to find is result in the SetDefaults method so we can find out which aiStyle GreekSkeleton/Hoplite is using:    
![](https://i.imgur.com/mcDqzru.png)    
Great, we now know to search the source code for `aiStyle == 3`:     
![](https://i.imgur.com/kGTeWrw.png)      
Now follow the code into the AI_003_Fighters method and copy all the code and paste it into an AI method in our own ModNPC class. (Be careful not to mess up the `{ }` pairs) We will also set `npc.aiStyle = -1;` in SetDefaults (since the CloneDefaults method would set that to 3) and delete `aiType = NPCID.GreekSkeleton;` since we don't want both our copy of the AI code and the vanilla code to run. Once we have copied the code into our AI method, we need to do the usual cleanup: Change `base.` and `this.` to `npc.` everywhere they appear in the method:

Before:    
![](https://i.imgur.com/FGLIGWO.png)  

After:   
![](https://i.imgur.com/hd3iXFx.png)   

Find and Replace:    
![](https://i.imgur.com/ekINkM2.png)    
![](https://i.imgur.com/0f9wFM3.png)    

You'll probably notice that there are a lot of NPC type specific sections of code. If you want, you can clean up the code further by deleting sections of the code that would only run for other NPC, but if not, just leave it. Many errors will show up. Most of them can be solved simply by letting Visual Studio add the suggested using statements to our code. If you aren't using Visual Studio or you aren't seeing suggestions or errors, you need to fix that. Read the [FAQ](https://github.com/tModLoader/tModLoader/wiki/Developing-with-Visual-Studio#faq) if you don't see suggestions. One error in this particular piece of code is `NPC.gravity`, just change those to 0.3f:    
Before:    
![](https://i.imgur.com/qdegobF.png)    
After:    
![](https://i.imgur.com/3jGCdYb.png)    

If you test in game now, you'll notice that they behave very odd. This is because all the code that use to dictate Hoplite specific behavior is now inaccessible because npc.type does not equal 481 any more. Lets fix this. Use find to find all and replace checks for `npc.type == 481` with `(npc.type == 481 || true)`. Doing this will keep 481 around which will make it easier to find in the next sections. 
![](https://i.imgur.com/XATbFHQ.png)    

Save and rebuild, then go in game. It works!...almost. Comparing Hoplite and our Hoplite clone, our clone has a little bit different behavior. Search again in the AI code we copied over for 481 and you'll find `npc.type != 481`. This is falsely evaluating to true, so lets replace this with `false`. Now test, and our clone is perfect. 

Now that we have laid the groundwork, we can now start modifying the AI code.

### Change Projectile

Now lets change the projectile. First, make a hostile projectile or find some other projectile that you wish to use. Now we need to search the AI code to find how the code decides to spawn that javelin for Hoplite. This code being almost 4000 lines of code, we don't want to search blindly. Lets again search for `481` and look for code that seems to either be spawning projectiles or deciding on a projectile to spawn. Most likely to have the results we need are lines with `if (npc.type == 481)` on them. Lets look at the first result:    
![](https://i.imgur.com/mikBCBp.png)    
Ok, this is setting some local int to 100, but if you look later where that variable is used, it doesn't seem to be related to projectile spawning. Also, the ProjectileID of 100 is called `DeathLaser`, so this is likely wrong.     
![](https://i.imgur.com/FvS9gol.png)    
The next few results are assigning float values, so they are likely not related.    
![](https://i.imgur.com/ueQGlEw.png)    
Finally, we come to a result that is setting a couple int variables. Looking up 508 in ProjectileID.cs, we find `JavelinHostile = 508;`, which definitely sounds correct! Great! Lets change that to some hostile projectile and then use Visual Studio to "Find all References" so we can figure out what `num151` is:    
![](https://i.imgur.com/qbD8jcy.png)    
![](https://i.imgur.com/sCz0ujk.png)    
Ahah, using the VS hotkey of `ctrl+shift+space` or `ctrl+K, P`, we see that `num151` is passed in as the damage parameter of NewProjectile. Feel free to change that line as well.

Lets test in game:    
![](https://i.imgur.com/kPfkhCs.png)    

Well, it all works. Results [video.](https://gfycat.com/UnsteadyAcrobaticCurassow)

### Change Throw Distance, Accuracy, and Aim
Looking at the Projectile.NewProjectile code, we can find that num147 and num149 refer to SpeedX and SpeedY:    
![](https://i.imgur.com/vvZkdK2.png)    
Lets use `F2` to quickly make the source code more readable by renaming those variables, then search source for 481 again to find code specific to the original Hoplite. Going through the Find results again, you'll notice that one of the results seems to signify the NPC aiming above the head of the player, another seems to determine throw strength, and another seems to tweak the speedX and speedY to make the projectile a little inaccurate. I'll leave this as an exercise to the reader. Use `F2` to rename variables as you figure out their purpose to begin to understand little pieces of the AI code.

### Change Animation
This guide will be done later if anyone expresses interest.

## Custom Flail
Making a flail, you might have noticed that the range of the flail is hard to customize. (TODO)

## ExampleLamp Tile
These are the exact steps to creating ExampleLamp. Lamps are special because they can be turned on and off by wires. Up until now, no example existed in ExampleMod of this.

First, we need to find the Lamp TileID in [TileID.cs](https://github.com/tModLoader/tModLoader/wiki/Vanilla-Tile-IDs). We find that `Lamps = 93;`. Next, lets find a specific lamp style we wish to copy. Use [TConvert](https://forums.terraria.org/index.php?threads/tconvert-extract-content-files-and-convert-them-back.61706/) to extract Tiles_93.xnb and then open Tiles_93.png in an image editor of your choice. We'll be modifying one of these lamps for ExampleLamp, but you'll want to just use Tiles_93.png as a guide in your mod.

![](https://i.imgur.com/SyMfAb7.png)    

I like this one, so I'll crop the image and edit it to fit ExampleMod art style. I measured the top lamp and noticed that it was 54 pixels high (if we include the 2 padding pixels) and made sure to crop the selected lamp to that same height. The padding pixels can sometimes be confusing, just remember that most tiles are made from 16x16 pixel image patches.

![](https://i.imgur.com/LFGJANI.png)

Make sure to save the file, then do the same for the Item image. I just looked up the ItemID on the [wiki](https://terraria.gamepedia.com/Lamp). Turns out we used the Rich Mahogany Lamp as our guide, lets first do the ModItem by searching the source for 2087, the ItemID of Rich Mahogany Lamp. In our initial search, we found no results! Looking at ItemID.cs, we can see that RichMahoganyLamp is next to many other lamp items. Sometimes the source code uses ranges to apply code to many different item types. The lamp item furthest above `RichMahoganyLamp` is `CactusLamp = 2082;`, lets search for 2082. This time we find the code we need for our `ModItem.SetDefaults()`:

```cs
if (type >= 2082 && type <= 2091)
{
	this.useStyle = 1;
	this.useTurn = true;
	this.useAnimation = 15;
	this.useTime = 10;
	this.autoReuse = true;
	this.maxStack = 99;
	this.consumable = true;
	this.createTile = 93;
	this.placeStyle = type + 1 - 2082;
	this.width = 10;
	this.height = 24;
	this.value = 500;
	return;
}
```

Here is what we end up with converting this code into a ModItem. We've added a recipe and replaced item.createTile with the correct TileType. We've also removed item.placeStyle since our tile only has 1 style:

```cs
using Terraria.ID;
using Terraria.ModLoader;

namespace ExampleMod.Items.Placeable
{
	class ExampleLamp : ModItem
	{
		public override void SetDefaults()
		{
			item.useStyle = 1;
			item.useTurn = true;
			item.useAnimation = 15;
			item.useTime = 10;
			item.autoReuse = true;
			item.maxStack = 99;
			item.consumable = true;
			item.createTile = mod.TileType<Tiles.ExampleLamp>();
			item.width = 10;
			item.height = 24;
			item.value = 500;
		}

		public override void AddRecipes()
		{
			ModRecipe recipe = new ModRecipe(mod);
			recipe.AddIngredient(ItemID.WoodenChair);
			recipe.AddIngredient(mod.ItemType<ExampleBlock>(), 10);
			recipe.AddTile(mod.TileType<Tiles.ExampleWorkbench>());
			recipe.SetResult(this);
			recipe.AddRecipe();
		}
	}
}
```
Now that the ModItem is done, lets do the ModTile. First we have to find all the lines of code we need in `ModTile.SetDefaults`. First, read through [Basic Tile](https://github.com/tModLoader/tModLoader/wiki/Basic-Tile) so you are aware of all the various fields relating to tiles, such as `Main.tileSolid`, `Main.tileFrameImportant`, etc. Now lets search for the TileID  (93) in the source code. As always, there will be many results that are unrelated to what we want. Luckily, we find various Main.tileX results, lets add those to our ModTile. We can ignore Item.cs results, but Lang.cs shows adding a map entry. In our ModTile, we can utilize vanilla localized text by doing this: `AddMapEntry(new Color(253, 221, 3), Language.GetText("MapObject.FloorLamp"));` We got that color from another result: `array[93][0] = color;` and looking at `color`.

![](https://i.imgur.com/jWYvbLl.png)

Finally, there are other results, but lets go straight to the TileObjectData results:

```cs
TileObjectData.newTile.CopyFrom(TileObjectData.Style1xX);
TileObjectData.newTile.WaterDeath = true;
TileObjectData.newTile.WaterPlacement = LiquidPlacement.NotAllowed;
TileObjectData.newTile.LavaPlacement = LiquidPlacement.NotAllowed;
TileObjectData.newSubTile.CopyFrom(TileObjectData.newTile);
TileObjectData.newSubTile.LavaDeath = false;
TileObjectData.newSubTile.LavaPlacement = LiquidPlacement.Allowed;
TileObjectData.addSubTile(23);
TileObjectData.addTile(93);
```

We can copy and paste this straight into our ModTile after we replace 93 with Type. We also don't need SubTiles, so lets delete those lines as well. Here is our ModTile so far:

```cs
using Microsoft.Xna.Framework;
using Terraria;
using Terraria.Enums;
using Terraria.Localization;
using Terraria.ModLoader;
using Terraria.ObjectData;

namespace ExampleMod.Tiles
{
	class ExampleLamp : ModTile
	{
		public override void SetDefaults()
		{
			Main.tileFlame[Type] = true;
			Main.tileLighted[Type] = true;
			Main.tileFrameImportant[Type] = true;
			Main.tileNoAttach[Type] = true;
			Main.tileWaterDeath[Type] = true;
			Main.tileLavaDeath[Type] = true;

			TileObjectData.newTile.CopyFrom(TileObjectData.Style1xX);
			TileObjectData.newTile.WaterDeath = true;
			TileObjectData.newTile.WaterPlacement = LiquidPlacement.NotAllowed;
			TileObjectData.newTile.LavaPlacement = LiquidPlacement.NotAllowed;
			TileObjectData.addTile(Type);

			AddMapEntry(new Color(253, 221, 3), Language.GetText("MapObject.FloorLamp"));
		}
	}
}
```

Lets take a look in game! 

![](https://thumbs.gfycat.com/FluffyGrizzledGordonsetter-small.gif)    

Looks like we have several problems still: We can't toggle it, we can't mine it to recover it, the flame texture is totally messed up, it isn't actually lighting up anything, the tile doesn't alternate orientation, and we are missing the tiny sparks that randomly spawn sometimes. Looking at these 6, we'll find results for 5 of these in the remaining search results. For the mining problem, just follow other tiles in ExampleMod.

### Toggle
One of the results is in Wiring.HitWireSingle, this sounds like what we want. The corresponding tModLoader hook is `ModTile.HitWire`. We can copy in the code and have Visual Studio suggest the correct using statements. Feel free to clean up the code by renaming variables.

### Lighting
Another result seems to be modifying 3 variables based on tile style. This is probably related to the lighting because lighting uses red, green, and blue. Once again, we search nearby for a TileLoader method and learn that `ModDust.ModifyLight` is where this code belongs. Remember to clean up the code, removing all the code related to other tile styles.

### Sparks
One of the find results is near a call to `Dust.NewDust`. This must be the sparks code. Searching for `TileLoader` after this line shows us that `ModTile.DrawEffects` is the hook we want to use. Copy this code in as well. Clean up the code and adapt it to fit our tile.

### Orientation
One of the results is near code relating to SpriteEffects. If you have looked at ExampleAnimatedTile, you would recognize this code. Migrate this code into `ModTile.SetSpriteEffects`.

### Flame
Finally, the last relevant result relates to drawing the Flame texture. Looking at the code around this result, it becomes clear that `Main.tileFlame` uses hard-coded TileIDs to draw different flame textures. We'll need to re-implement this ourselves and delete `Main.tileFlame[Type] = true;` from our code to get this working. The closest TileLoader result tells us that `ModTile.PostDraw` is the hook we will need to use. In addition to adapting this code, we will make a new Flame texture in our mod.

### Final Code and Result
Here is the [final code](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/Tiles/ExampleLamp.cs) and result, see ExampleMod for the sprites.

![](https://thumbs.gfycat.com/ImportantMixedAmericanwarmblood-small.gif)

