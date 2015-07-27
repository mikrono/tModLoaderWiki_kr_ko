You can create a build.txt file in the top level of your mod's source directory to give it build properties. The file should be in the format of:

propertyName = property   
propertyName2 = element1, element2, element3

White space anywhere does not matter. However, each property must be on its own line, list items must be separated by commas, and property names must be on the left side of an equals sign.

Here is a list of properties:

displayName - The name your mod will display in on the mods menu

author - The author, which will be displayed on the mods menu

version - The version, which will be displayed on the mods menu

dllReferences - A list of dll references your mod has. You must place the dll files in the Mod Sources folder to build, and in the Mods folder to load. Name them by their file name without the extension.

modReferences - A list of mods that your mod depends on. Name them by their file name without the extension.