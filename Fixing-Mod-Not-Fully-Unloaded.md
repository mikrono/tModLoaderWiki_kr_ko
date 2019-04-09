# Introduction

This guide goes over debugging your mod in Visual Studio in an effort to determine why your mod isn't fully unloading during a mod reload. Proper unload code ensures that mods properly reset their values and don't use excess RAM.

![](https://i.imgur.com/EmXS4eb.png)

# Unload Basics
As you know, c# utilizes managed memory, meaning we can usually ignore memory considerations as we can trust that the garbage collector will clean up objects that are no longer being referenced. For Mods, there are many common pitfalls that will result in objects being referenced after tModLoader attempts to unload the Mod, causing a mod to continue consuming RAM/memory after being disabled. In an effort to fix these issues, starting in tModLoader v0.11 the Mods menu will inform users of mods that fail to unload properly. 

[ExampleMod.Unload](https://github.com/blushiemagic/tModLoader/blob/master/ExampleMod/ExampleMod.cs#L143) shows many common things that modders need to remember to "unload" (set to null). The main culprit is usually static references to any tModLoader class. Make sure to set those to null. Static references to XNA classes like Texture2D will also cause issues.

# Debug
After you have made sure to null out all references you can think of, you may find that your mod is still failing to unload. To fix this, we will use Visual Studio. First, however, make sure to disable all other mods, as some mods maintain references to other mods. If disabling all other mods fixes the issue, the issue is with a different mod that is preventing your mod from unloading. Tell them to fix their mod.

