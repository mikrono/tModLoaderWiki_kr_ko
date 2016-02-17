# ModLoader

This serves as the central class which loads mods. It contains many static fields and methods related to mods and their contents.

## Fields

### public static readonly string version

The name and version number of tModLoader.

### public static readonly string ModPath

The file path in which mods are stored.

### public static readonly string ModSourcePath

The file path in which mod sources are stored. Mod sources are the code and images that developers work with.

### public static readonly string DllPath

The file path in which .dll files referenced by mods should be stored.

## Methods

### public static Mod GetMod(string name)

Gets the instance of the Mod with the specified name.

### public static string[] GetLoadedMods()

Returns an array containing the names of all loaded mods. The array entries will be in the reverse order in which the mods were loaded.

### public static byte[] GetFileBytes(string name)

Gets the byte representation of the file with the specified name. The name is in the format of "ModFolder/OtherFolders/FileNameWithExtension". Throws an ArgumentException if the file does not exist.

### public static bool FileExists(string name)

Returns whether or not a file with the specified name exists.

### public static Texture2D GetTexture(string name)

Gets the texture with the specified name. The name is in the format of "ModFolder/OtherFolders/FileNameWithoutExtension". Throws an ArgumentException if the texture does not exist. If a vanilla texture is desired, the format "Terraria/FileNameWithoutExtension" will reference an image from the "terraria/Content/Images" folder.
Note: Texture2D is in the Microsoft.Xna.Framework.Graphics namespace.

### public static bool TextureExists(string name)

Returns whether or not a texture with the specified name exists.

### public static void AddTexture(string name, Texture2D texture)

Adds the given texture with the specified name to the game so that it can be retrieved with GetTexture.

### public static SoundEffect GetSound(string name)

Gets the sound with the specified name. The name is in the same format as for texture names. Throws an ArgumentException if the sound does not exist. Note: SoundEffect is in the Microsoft.Xna.Framework.Audio namespace.

### public static bool SoundExists(string name)

Returns whether or not a sound with the specified name exists.