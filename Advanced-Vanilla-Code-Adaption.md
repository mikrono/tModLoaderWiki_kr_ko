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

### Custom Flail
Making a flail, you might have noticed that the range of the flail is hard to customize. (TODO)