You can create a build.txt file in the top level of your mod's source directory to give it build properties. The file should be in the format of:

propertyName = property   
propertyName2 = element1, element2, element3

White space anywhere does not matter. However, each property must be on its own line, list items must be separated by commas, and property names must be on the left side of an equals sign.

Here is a list of properties:

displayName - The name your mod will display in on the mods menu

author - The author, which will be displayed on the mods menu

version - The version, which will be displayed on the mods menu

dllReferences - A list of dll references your mod has. You must place the dll files in the dllReferences folder both to build this mod and to load this mod. Name them by their file name without the extension.

modReferences - A list of mods that your mod depends on. Name them by their file name without the extension.

noCompile - Whether this mod should compile source code when being built or use dll files pre-compiled by the mod maker. By default this will be false if you do not include this property. If this is set to true, make sure to name your mod file "All.dll" if it does not use Microsoft.Xna.Framework. If it does use Microsoft.Xna.Framework, you will need two dll files: "Windows.dll" must reference Microsoft.Xna.Framework.dll and the Windows version of Terraria.exe, while "Other.dll" must reference FNA.dll and a non-Windows version of Terraria.exe.

homepage - A link to a website with your mod's information.

hideCode - If true, the unpacked mod that results from using tModReader will not include .dll files. These files are the compiled mod. Defaults to true.

hideResources - If true, the unpacked mod that results from using tModReader will not include resource files, such as .png files and .wav files. Use this if you would like to have your sounds and images freely available to other modders. Defaults to true.

includeSource - If true, the the unpacked mod that results from using tModReader will include .cs files. Use this if you would like to share your code freely. Defaults to false.

buildIgnore = A list of file patterns to ignore for building the mod. Useful for reducing .tmod file size. Use to ignore version control files, reference images, failed code experiments, and other files. Here are some examples: .git\*, referenceImages\*.png, .gitattributes, *.csproj. 