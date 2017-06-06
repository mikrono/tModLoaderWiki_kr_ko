# Basic tModLoader Modding Guide
This guide will get you familiar with tModLoader modding and will help you make your first mod. Please read and follow [Basic Prerequisites](https://github.com/bluemagic123/tModLoader/wiki/Basic-Prerequisites) first so you have access to a capable text editor.

# Your First Mod
To start, we will make a very simple mod to get you familiar with how mods are created for tModLoader. To begin, please visit the [Mod Skeleton Generator](http://javid.ddns.net/tModLoader/generator/ModSkeletonGenerator.html) and fill out the input boxes. I suggest TutorialMod, Tutorial Mod, TutorialSword, and NewbieModder. 

![](http://i.imgur.com/B38HAI5.png)

Click the "Generate Mod Skeleton" button and you should see a .zip file download. Next, open up "Documents\My Games\Terraria\ModLoader\Mod Sources" or equivalent. If you do not have the Mod Sources folder, make a folder of that name. Next, we will take the folder contained within the .zip file we just downloaded and place it in the Mod Sources folder.

![](http://i.imgur.com/0zH7w65.png)

We now have a ready-made mod ready to be built. Start up tModLoader and click on the Mod Sources menu item. Now click the build and reload button.

![](http://i.imgur.com/jbCpSYc.png)

Now go in game, make a workbench and mine 10 dirt blocks, and you should see that you can craft a new sword!

![](http://i.imgur.com/UQb3tXq.png)

Wow! Amazing. But 50 damage isn't enough. We will now do our first actual coding. Open up the "Mod Sources\TutorialMod\Items\TutorialSword.cs" file in Notepad++. Find the line with `item.damage = 50;` and change 50 to 100. Also at this time, let's change `Tooltip.SetDefault("This is a modded sword.");;` just for fun. Remember not to mess up the syntax that you learned in the Basic Prerequisites lesson. Now, save the file! Next, go in game, once again build and reload the mod and acquire the sword again. You should see the new damage and the new tooltip. Congratulations.

## Experiment a little

You can now experiment a little more by changing some of the other item values. Remember, you have to save your changes and build and reload to see your changes in-game.

## Next Steps

Now that you have a simple mod with a simple sword, it is time to branch out an learn other skills. Take it slow and experiment with something you want to learn. If you are seeking help from other modders on the Forum or Discord, it is best to phrase your question in terms of Vanilla things. For example, if you are curious how the Molten Fury changes Wooden Arrows to Flaming Arrows, it would be wise to ask "How does the Molten Fury change the projectile it shoots only when Wooden Arrows are used as ammo?" Phrasing your questions in this manner is most effective.

# Learning from Example Mod

Example Mod is a mod made by the tModLoader developers to show off various modding capabilities. It would be wise to enable Example Mod and play around with it for a while. Use a mod like Cheat Sheet to spawn in its various items and find something it does that you want to learn. Next, download the [Example Mod sourcecode](https://github.com/bluemagic123/tModLoader/releases) and place it in the Mod Sources folder as you did with TutorialMod. Find the thing you are interested and try to understand it. Hopefully it is easy to understand. You can also change things in Example Mod and build Example Mod in game.

If you can handle modifying simple things in Example Mod or Tutorial Mod, you should be comfortable with the mod building process. Feel free to explore other guides or help for the next steps.