This guide will get you familiar with tModLoader modding and will help you make your first mod. Please read and follow [Basic Prerequisites](https://github.com/bluemagic123/tModLoader/wiki/Basic-Prerequisites) first so you have access to a capable text editor.

# Your First Mod
To start, we will make a very simple mod to get you familiar with how mods are created for tModLoader. To begin, please visit the [Mod Skeleton Generator](http://javid.ddns.net/tModLoader/generator/ModSkeletonGenerator.html) and fill out the input boxes. I suggest TutorialMod, Tutorial Mod, TutorialSword, and NewbieModder. 

![](http://i.imgur.com/B38HAI5.png)

Click the `Generate Mod Skeleton` button and you should see a .zip file download. Next, open up `Documents\My Games\Terraria\ModLoader\Mod Sources` or equivalent. If you do not have the Mod Sources folder, make a folder of that name. Next, we will take the folder contained within the .zip file we just downloaded and place it in the Mod Sources folder.

![](http://i.imgur.com/0zH7w65.png)

We now have a ready-made mod ready to be built. Start up tModLoader and click on the Mod Sources menu item. Now click the build and reload button.

![](http://i.imgur.com/jbCpSYc.png)

Now go in game, make a workbench and mine 10 dirt blocks, and you should see that you can craft a new sword!

![](http://i.imgur.com/UQb3tXq.png)

Wow! Amazing. But 50 damage isn't enough. We will now do our first actual programming. Open up the `Mod Sources\TutorialMod\Items\TutorialSword.cs` file in Notepad++. Find the line with `item.damage = 50;` and change 50 to 100. Also at this time, let's change `Tooltip.SetDefault("This is a modded sword.");` just for fun. Remember not to mess up the syntax that you learned in the Basic Prerequisites lesson. Now, save the file! Next, go in game, once again build and reload the mod and acquire the sword again. You should see the new damage and the new tooltip.

## Experiment a little

You can now experiment a little more by changing some of the other item values. Remember, you have to save your changes and build and reload to see your changes in-game.

## Next Steps

Now that you have a simple mod with a simple sword, it is time to branch out an learn other skills. Take it slow and experiment with something you want to learn. If you are seeking help from other modders on the Forum or Discord, it is best to phrase your question in terms of Vanilla things. For example, if you are curious how the Molten Fury changes Wooden Arrows to Flaming Arrows, it would be wise to ask "How does the Molten Fury change the projectile it shoots only when Wooden Arrows are used as ammo?" Phrasing your questions in this manner is most effective.

Continue reading to learn more about tModLoader and how to get better.

# How do tModLoader 'hooks' work?

Firstly, they are technically not hooks. We simply call them hooks because it is easy. A 'hook' is a function you can use as a modder. Which hooks are available depends on the class you're working in. For example your Mod class has a **[Load Hook](http://blushiemagic.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod.html#afbcbdc176a60f3da809842f683ff2e75)**, which is a function special to the Mod class. Every one of these functions is `virtual`, this means the function has a basic implementation but can be overridden by the modder if desired. This means when you want to use a hook, you _override_ it, this is why the word `virtual` is replaced with `override`. Since the _virtual_ (partial) implementation is overridden, that implementation will be lost. To see the standard implementation of hooks you should see the **[source documentation](http://blushiemagic.github.io/tModLoader/html/annotated.html)**.

# Learning from Example Mod

Example Mod is a mod made by the tModLoader developers to show off various modding capabilities. It would be wise to enable Example Mod and play around with it for a while. Use a mod like [Cheat Sheet](https://github.com/JavidPack/CheatSheet) to spawn in its various items and find something it does that you want to learn. Next, download the [Example Mod sourcecode](https://github.com/bluemagic123/tModLoader/releases) and place it in the Mod Sources folder as you did with TutorialMod. Find the thing you are interested and try to understand it. Hopefully it is easy to understand. You can also change things in Example Mod and build Example Mod in game.

If you can handle modifying simple things in Example Mod or Tutorial Mod, you should be comfortable with the mod building process. Feel free to explore other guides or help for the next steps.

## Learning in steps

If you are new to modding, you are probably also new to programming itself and possibly the c# language. It is recommended to start with easy things and work your way up the ladder of difficulty. One of the easiest things to do would be a sword that can be swung to deal damage as shown above, and one of the hardest would be to create a fully functional boss fight. Here is a few suggestions to get started:
* First, try to familiarize yourself with how tModLoader works. Read the section 'How do tModLoader 'hooks' work?'
* Next, try modifying the tutorial sword to deal more damage, have more knockback, faster speed etc. You can also make it shoot things by setting `item.shoot`, and control the speed with `item.shootSpeed`
* You need to understand that modding is programming, it is c# and it is being familiar with the vanilla code. This means you should definitely follow free online courses to get a better understanding of the c# language or programming itself. A useful resource might also be the [Quick Terraria-specific C# crash course](https://docs.google.com/document/d/1xRz3kFNbewb8DI29AKXuyi6O327IcxlgihZ7sdK_IuE/edit?usp=sharing). To get more familiar with the vanilla code, use our vanilla description pages (WIP)
* If you feel confident enough, try making your own projectile and having your sword interact with it.
* With more confidence, you can start trying other things, such as making an enemy NPC.

**Here is a few things that would be considered difficult for most modders, and it is advised you start with easier things before attempting these:**
* A fully functional boss
* Comprehensive UI
* Code abstraction
* Modding info for entities, and interacting with it
* Completely custom AI for a projectile, NPC, pet... etc.

## Slowly learning

There is many resources available so you can become a better modder. First of all, this wiki. On the right side of this page you can find the wiki menu, which houses links to various different tutorials and guides that you can use. These are all separated into their own difficulty level, from basic being the easiest and expert being the hardest. Secondly, a great source of information is the [modding section on TCF](https://forums.terraria.org/index.php?forums/general-mod-discussion.121/). And lastly, a variety of helpful tools and guides is available on the [homepage](Home) of this wiki.