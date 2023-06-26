# Intermediate Prerequisites
Before consulting any of the Intermediate-level guides, please read below so you are prepared for Intermediate-level concepts. Please also make sure that you have gone through the Basic Prerequisites and corresponding Basic level tutorials related to things you are interested in.

## Upgrade from Text Editor to an IDE
In Basic, we used a Text Editor to write some simple code, but using an IDE (Integrated Development Environment) extremely recommended for anyone attempting anything in the Intermediate-level guides.

We recommend installing the latest [Visual Studio Community](https://www.visualstudio.com/vs/community/). It is free and fully featured.

### Installation
Simply download from the above link and run the downloaded file to install. During install, you will be prompted to choose which capabilities and programming languages you want to install. Be sure to select ".Net Desktop Environment" so that you can write code in C#.

### IntelliSense
TODO
Video: https://gfycat.com/PowerlessEqualFeline

### Usage
To use Visual Studio, open the .csproj file that was included when we made our first mod in the Basic Guide. To make a new mod project, please see [Developing with Visual Studio](https://github.com/tModLoader/tModLoader/wiki/Developing-with-Visual-Studio)

### Advanced
See the Advanced Prerequisites to learn about Edit and Continue, Debugging, and Breakpoints.

## Inheritance
Please [read about inheritance](https://www.tutorialspoint.com/csharp/csharp_inheritance.htm)

Inheritance is used everywhere in tModLoader modding. If you look back on our sword we made during Basic, you'll notice that our sword inherits from ModItem.

## Hooks
A "Hook" is a method that is made available to modders so that they can achieve special effects for their mod. Hooks are virtual methods that modders override if they wish to use them. 

## Documentation
Learning to read [documentation](http://tmodloader.github.io/tModLoader/docs/1.4-stable/annotated.html) is essential to modding. For example, [Example Gun](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/Items/Weapons/ExampleGun.cs#L50) has an example that overrides the ModItem.Shoot method. We see from the method signature that Shoot returns a bool. The bool that we return has special meaning, but we won't know unless we look it up. Open up the Documentation and search for "Shoot" and then click the result, and then the sub-result for the ModItem class. You will be taken to a description. Read the description and learn what the parameters and return values represent. Looking back on that ExampleGun code, you should now see how the hook manages to change the bullet that is shot by using the Shoot hook.

### Virtual and Override
You may notice that the documentation lists all the hooks as virtual, but all the ExampleMod code has override. This is because we override virtual methods when we inherit from them. [Read about inheritance](https://www.tutorialspoint.com/csharp/csharp_inheritance.htm) again if you've forgotten.

## Vanilla Texture File Reference
Looking at vanilla texture files and seeing how they are laid out is very useful, especially when you start animating NPC. Terraria stores textures in .xnb files that we can't open normally. The best option is to just extract all the Texture files to .png files and keep them around in a folder so you can look at them whenever you need to.    

Instructions: Download [TConvert](https://forums.terraria.org/index.php?threads/tconvert-extract-content-files-and-convert-them-back.61706/) and run it. Fix the `Terraria Content Folder` path if needed, then click `Use Terraria`. Finally, use the folder icon for `Output Folder` to specify where you want to keep the extracted files.  I suggest creating a folder in the ModLoader folder so you can easily find it: `\Documents\My Games\Terraria\ModLoader\VanillaTextures\`. You can uncheck the options other than `Images` if you'd like. Now, click `Extract` and wait for it to finish. From now on, if you are curious how many frames an NPC has, you can simply find the png file corresponding to the NPCID and view it in your image viewer. 

Alternatively, you can also find Terraria sprites on [The Spriters Resource](https://www.spriters-resource.com/pc_computer/terraria/), but they may or may not be the original layout, dimensions, or filenames. Use the website only if you need a quick reference, the best option is to extract your current Terraria textures directly.