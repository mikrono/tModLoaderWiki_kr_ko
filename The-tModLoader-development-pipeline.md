`WIP`

# How is tModLoader developed?
First of all, tModLoader is a modification to the vanilla source that facilitates both modding development for mods and loading (playing) with mods. It uses a custom built patcher (by chicken-bones) that can manage the source using patches. The patcher uses [ILSpy](https://github.com/icsharpcode/ILSpy) internally to decompile the vanilla client which allows access to modifications using an IDE such as [visual studio](https://visualstudio.microsoft.com/)

The patching system allows tML to be open-source as it does not disclose vanilla information nor does it host the decompiled source. Developers will have to manually decompile and patch their local source to work on the tML codebase. The patcher can do this by first decompiling the vanilla client, and then making separate managed projects in which one will have the patches applied. This will result in the user having access to the tML modified codebase locally.

# How istModLoader updated?
Everytime vanilla updates, the patches for tModLoader become broken. This is due to changes in the vanilla codebase. For every update, the developers will have to manually go through patches to figure out what changed and how to fix the patch. This is all done manually.

# How does tModLoader facilitate modding needs?
tModLoader exposes a variety of classes and methods that can be used by modders to create their own content. It can then "build" a modders' mod and register its changed in the vanilla code. Modders get access to this by referencing the modded assembly (tModLoader) in their IDE. Every mod has one entry point, a "Mod" class, and from there can add any content that is desired.

tModLoader provides an Object-Oriented design approach for modders to build content, which is facilitated by classes such as [ModItem](https://github.com/blushiemagic/tModLoader/blob/master/patches/tModLoader/Terraria.ModLoader/ModItem.cs), [ModProjectile](https://github.com/blushiemagic/tModLoader/blob/master/patches/tModLoader/Terraria.ModLoader/ModProjectile.cs), [ModPlayer](https://github.com/blushiemagic/tModLoader/blob/master/patches/tModLoader/Terraria.ModLoader/ModPlayer.cs) and so forth. Modders can use these classes in their mod to add these respective subjects to their mod. For example using the ModItem class modders can add their own items to the game.

Once a mod codebase is made, it can be built using the in-game mod source menu which will package the mod into a .tmod file. This file can then be spread to other players so they can play the mod.

# How does tModLoader facilitate playing the game with mods?
tModLoader is not only a tool to build mods, it also allows playing them. It does so by providing a Mod Browser (to look for- and download mods) and providing in-game functionality to load in and apply mods to the game (as well as unloading them). Players only need a '.tmod' file to get started, which is tModLoader's specific package format for mods.