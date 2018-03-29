# Vanilla Code Adaption
This guide teaches how to navigate the jungle that is Terraria source code to find or adapt code. Why? Maybe you wish to customize an aiStyle, maybe you want to know how Magma Stone works, or maybe you want to mimic a vanilla effect.

## TL;DR
* Change `this.X` code to the corresponding object, such as `npc.X` or `player.X`
* If you are manually copying aiStyles, you can delete all code related to other types to make the code more readable
* Use F2 to rename auto-generated variable names (num233, flag83, etc) as you figure out what each variable represents

## Obtaining Vanilla Source Code
It is against the law to publish the decompiled source code, but anyone with a competent computer can decompile their personal copy of Terraria.exe no problem. Note that you may wish to end up with a decompiled tModLoader rather than a decompiled vanilla Terraria so that you know where tModLoader hooks are. See [Advanced Prerequisites](https://github.com/blushiemagic/tModLoader/wiki/Advanced-Prerequisites) for more info.

## Example: Armor Effects: Ninja Shirt
A simple example for investigating vanilla code is figuring out how vanilla armor or accessories achieve their special effects. For this example, we want to know how Ninja Shirt increases throwing damage by 15%. (Follow along for whatever effect you are interested in.) The first step is to find the ItemID. Searching ItemID.cs, you will find where NinjaShirt is defined:

![](http://i.imgur.com/fxnOPXA.png)

We now must search the entire project for "257". Be sure to set "Match whole word" here to limit results. In the results, you will see many references to "257", you must now trim down the results to find the relevant results. You may find a result in Item.SetDefaults1, setting the default values for the result, but this code isn't responsible for the throwing damage bonus, so we must continue searching. Continuing down the list of results, you can skip the results in NPC.cs, as those probably relate to whatever NPCID is equal to 257, not the ItemID of 257. Finally, you will come to the results in Player.cs and your screen will show this:

![](http://i.imgur.com/n0YKKbB.png)

Bingo! This looks right. Now we want to bring this code, `this.thrownDamage += 0.15f;`, into our ModItem's code. First, override the UpdateAccessory hook in your ModItem class. Next, paste the code in and you'll notice that your IDE is complaining about the code you just pasted. The code has "this", which refers to the Player instance. Our code is in ModItem, not Player, so trying to use "this" and modifying the value of the "thrownDamage" field won't work, since ModItem doesn't have a "thrownDamage" field. Luckily, the UpdateAccessory hook has a Player object passed in, so we change our code to `player.thrownDamage += 0.15f;` and it all works out.

## Example: Accessory Effect: Magma Stone

Another example. Search ItemID.cs to find `MagmaStone = 1322;`. Now search for "1322", you'll find a result in Player.UpdateAccessory that does `this.magmaStone = true;`. (It is worth noting that this code is setting a bool field in the Player object and is exactly the same thing us modders would do in our ModPlayer.) Now we must investigate this "magmaStone" variable. Rather than searching the code using the find menu, this time we will use the Find All References feature of Visual Studio:

![](http://i.imgur.com/RtAjhYF.png)

Now we go through the 9 results in search of what we want to know. 2 of the results are setting the bool to true and correspond to ModItem.UpdateAccessory. 1 of the results sets the magmaStone bool to false, this is akin to ModPlayer.ResetEffects. The Results in Player.StatusNPC and Player.StatusPvP seem to be applying a buff to the target. Looking up 24 in BuffID shows us the buff is OnFire. These results correspond to ModPlayer.ModifyHitPvp and ModPlayer.ModifyHitNPC. The 6th result is in Player.ItemCheck and appears to be spawning some Dust. We know that Fire particles spawn when melee weapons are swung, so this code must correspond to that. How would you mimic this behavior yourself? We need to search nearby code and look for tModLoader hooks. tModLoader hooks have code like `PlayerHooks.Something` or `ItemLoader.Something`, so lets search for that. Luckily, this result is right next to the hook!

![](http://i.imgur.com/UyxK0l6.png)

Now let's look in the [documentation](http://blushiemagic.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod_player.html#a48027ba9e2e4188d3b73b49fa60071f2)! As you can see, the documentation also agrees, this is the hook to use to spawn dust and other effects.

The last 3 results are in Projectile.cs. They do the same thing that earlier results did, but for melee projectiles instead of melee items. (Dust, Hit NPC, Hit PvP) The corresponding hooks we use to replicate this behavior is in: ModPlayer.ModifyHitNPCWithProj, ModPlayer.ModifyHitPvpWithProj, and GlobalProjectile.PostAI.

Remember to adapt the code you find to the context the hooks give you. Replace "this" with the correct object variable, "Main.npc[i]" with "target", and so on.

## Example: Projectile or NPC AI code
This guide teaches how to adapt vanilla AI code for new uses. In Beginner level coding, we sometimes use vanilla aiStyles for our ModProjectiles or ModNPC. In Intermediate level coding, we take that a step further by setting aiType to mimic the specifics of an aiStyle or even use PostAI or PreAI to add additional code to vanilla aiStyles for our particular thing. This is not always enough, so in this tutorial, we will learn how to investigate vanilla source code to find the information or code snippets we need to completely customize the things in our Mod. 

## Example: Item and Projectile: Shadowbeam Staff clone
Shadowbeam is a pretty neat weapon that shoots a projectile that bounces several times instantaneously. Lets make a weaker version of the Shadowbeam Staff and then attempt to customize the look of the projectile.

### Weaker Shadowbeam Staff clone
The first step in our journey is cloning the vanilla item. This is seen a few times in ExampleMod, but below is the code.

    using Microsoft.Xna.Framework;
    using Terraria.ID;
    using Terraria.ModLoader;
    
    namespace ExampleMod.Items
    {
    	class ShadowbeamStaffClone : ModItem
    	{
    		public override string Texture { get { return "Terraria/Item_" + ItemID.ShadowbeamStaff; } }
    
    		public override void SetDefaults()
    		{
    			item.CloneDefaults(ItemID.ShadowbeamStaff);
    			item.color = Color.Purple;
    		}
    	}
    }

![](https://i.imgur.com/ADBy27n.png)    
Huh, something is off, the staff isn't held right. Lets find the value of `ItemID.ShadowbeamStaff` in ItemID.cs. We find that it is 1444. Lets now search the source for 1444.
![](https://i.imgur.com/RvhdTQC.png)     
Out of these matching results, we see an NPC loot drop, prefix assignment, SetDefaults, ItemID, and 2 pieces of code that we haven't seen before. The one in AchievementInitializer sounds like it is related to achievements, so lets look at this last result. This last result is a crazy mess due to the decompilation process, but we can see a switch statement here that has a goto. Goto might be new to you, so read [this](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/goto) first. Now that we know what goto is, lets click on the label that the goto is assigned and press F12 on the keyboard to follow it.
![](https://i.imgur.com/864MpVV.png)    
Ahah! We forgot to set Item.staff to true. If you studied ExampleMod's ExampleStaff, you would have already known about this. Lets add this to our code in SetStaticDefaults and also take this opportunity to reduce the damage of our weapon:

    using Microsoft.Xna.Framework;
    using Terraria;
    using Terraria.ID;
    using Terraria.ModLoader;
    
    namespace ExampleMod.Items
    {
    	class ShadowbeamStaffClone : ModItem
    	{
    		public override string Texture { get { return "Terraria/Item_" + ItemID.ShadowbeamStaff; } }
    
    		public override void SetStaticDefaults()
    		{
    			Item.staff[item.type] = true;
    		}
    
    		public override void SetDefaults()
    		{
    			item.CloneDefaults(ItemID.ShadowbeamStaff);
    			item.color = Color.Purple;
    			item.damage = 8; // Down from 53
    		}
    	}
    }

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
Now that we've found all the relevant code, now we need to investigate how the color is decided. This might be a good time to check what other projectiles use projectile aiStyle 48. Searching for `this.aiStyle = 48` reveals that HeatRay, UFOLaser, MagnetSphereBolt, and ShadowBeamHostile all also share aiStyle 48. We know that HeatRay is yellow, but Shadowbeam Staff is purple. Knowing this, we know that the code for the color must be within an if statement within the AI code. Going back to the 4th result, the result in the AI method, we look for anything that looks like code related to color. Ahah! We find the code `int num448 = Dust.NewDust(vector33, 1, 1, 173, 0f, 0f, 0, default(Color), 1f);` and look up the 173rd dust in the [dust spritesheet](https://github.com/blushiemagic/tModLoader/wiki/Basic-Dust):    
![](https://i.imgur.com/FLhIi1G.png)    
Yep, we've figured out why the Shadowbeam Staff projectile is purple, it is because dust 173 is spawned in AI. Let's remember to change this to spawn dust 178, a lime green projectile, for our modified clone. If you look at the rest of the code for aiStyle 48, you'll probably notice that there is nothing limiting the number of bounces. Through some trial and error, you'll discover that projectile.extraUpates and projectile.timeLeft are responsible. A timeLeft of 300 would normally stick around for 5 seconds, but because of extraUpdates being 100, it actually only lives for 3 ticks/frames. Lets change timeLeft to 200 for our weaker clone, this will shorten the reach of our weapon.   

Now that we've found all relevant pieces of code in the source code, lets make a ModProjectile clone of the vanilla ShadowBeamFriendly projectile. Lets first also remember to add in `item.shoot = mod.ProjectileType<ShadowbeamStaffCloneProjectile>();` to our ModItem so our weapon won't shoot the vanilla projectile. Remember, when adopting vanilla code, be sure to change `this.` and `base.` things to the correct thing, in this case `projectile.`. Also remember not to copy over `this.aiStlye = 48` since we will be copying over and editing the vanilla AI code instead. Also, rename things like num447 with the F2 button in Visual Studio. Below is the complete code after taking into account all the vanilla code:

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
		// Remove this and the item.color line in SetDefaults once you make your own sprite.
		public override string Texture { get { return "Terraria/Item_" + ItemID.ShadowbeamStaff; } }

		public override void SetStaticDefaults()
		{
			Item.staff[item.type] = true;
		}

		public override void SetDefaults()
		{
			item.CloneDefaults(ItemID.ShadowbeamStaff);
			item.color = Color.Purple;
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
		public override string Texture { get { return "Terraria/Projectile_" + ProjectileID.ShadowBeamFriendly; } }

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

### Custom Flail
Making a flail, you might have noticed that the range of the flail is hard to customize. (TODO)