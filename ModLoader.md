# ModLoader

This serves as the central class which loads mods. It contains many static fields and methods related to mods and their contents.

## Fields

### public static readonly string ModPath

The file path in which mods are stored.

### public static readonly string ModSourcePath

The file path in which mod sources are stored. Mod sources are the code and images that developers work with.

### public static readonly string DllPath

The file path in which .dll files referenced by mods should be stored.

## Methods

### public static Mod GetMod(string name)

Gets the instance of the Mod with the specified name.

### public static Texture2D GetTexture(string name)
Gets the texture with the specified name. The name is in the format of "ModFolder/OtherFolders/FileNameWithoutExtension".
Note: Texture2D is in the Microsoft.Xna.Framework.Graphics namespace.