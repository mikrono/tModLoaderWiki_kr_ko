# Creating a Project
This will let you use Visual Studio's code editing and Intellisense

1. Open Visual Studio and go to New -> Project
1. Select the "Class Library" template
1. Choose a name for the mod. This is the internal name (and file name), not the display name. It is recommended that internal names have no spaces eg. "ExampleMod"
1. Uncheck "Create directory for solution"
1. Click OK
![New Project Dialog](http://i.imgur.com/tQIfA3g.png)
1. Go to the Solution Explorer tab, right click on Solution -> ModName -> References and click Add Reference...
1. Click Browse... and select the tModLoader executable (most likely C:\Program Files (x86)\Steam\steamapps\common\Terraria\Terraria.exe)
1. Add the line `buildIgnore = *.csproj, *.user, obj\*, bin\*, .vs\*` to your build.txt
1. Create a class extending `Terraria.ModLoader.Mod` and begin developing

If you want to use the Microsoft Xna Framework (you probably do) you need to add the DLLs as references. Unfortunately they're hard to find. Fortunately, there's a trick.

1. Open the csproj file in a text editor (like Notepad++)
1. Find the reference section and add the lines

|

    <Reference Include="Microsoft.Xna.Framework" />
    <Reference Include="Microsoft.Xna.Framework.Game" />
    <Reference Include="Microsoft.Xna.Framework.Graphics" />

If you already have several source files, and are having difficulty adding them, try dragging and dropping them into the Solution Explorer window.

# Building with Visual Studio
This will let you build your mod from within Visual Studio, so you don't have to rebuild from within the game, or have your sources in the Mod Sources folder

1. Right click on your mod project in the Solution Explorer and click Properties
1. Go to the Build Events tab
1. Add the following to the Post-build event command line
`"C:\Program Files (x86)\Steam\steamapps\common\Terraria\tModLoaderServer.exe" -build "$(ProjectDir)\"`

# Debugging
This will let you set breakpoints in your mod and inspect variables in Visual Studio

1. Right click on your mod project in the Solution Explorer and click Properties
1. Go to the Debug tab
1. Set the Start Action to Start external program: `C:\Program Files (x86)\Steam\steamapps\common\Terraria\Terraria.exe`
1. Set the Working directory to `C:\Program Files (x86)\Steam\steamapps\common\Terraria`
1. Add the line `includePDB = true` to your build.txt

The `includePDB = true` line will not only enable debugging, but also line numbers in exception stack traces. However it will increase the size of your mod and can be omitted from release builds.

# Edit and Continue
This will let you edit the source code of your mod while at a breakpoint without restarting the game

1. Simply add `-eac "$(TargetPath)"` to the end of your Post-build event command line

# FAQ
## Autocomplete isn't doing anything for me.
1. Check that the tModLoader executable file is in your references. Make sure it's modded not vanilla.    
![](http://i.imgur.com/HvodIHV.png)
2. Check that your .cs files are actually in the project, and that you are opening the project not just individual .cs files. You can use the show all files toggle and then right click -> include in project to add missing source files to the project.    
![](http://i.imgur.com/dNMyROY.png)

## CS5001 Program does not contain a static 'Main' method suitable for an entry point
Make sure "Output type" is set to "Class Library" in the project properties.