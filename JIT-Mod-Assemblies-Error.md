# What are JIT Mod Assembly errors?

## User Instructions
The mod is broken, you'll need to wait for the mod to update

## Modders
You need to update your mod. Most of the time these errors happen because tModLoader changed and your mod has not updated. For example, if a tModLoader method changes, your mod will be broken because it is trying to use a method that no longer exists.

If you have special circumstances, such as methods or classes with weak references to other mods, you can use the `NoJIT` or `JITWhenModsEnabled` attributes. The `NoJIT` attribute on a class or method will skip the JIT process altogether. The `JITWhenModsEnabled` attribute on a class or method will skip the JIT process when the provided mods are not all enabled.