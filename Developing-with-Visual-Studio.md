# Prerequisites
1. Download [Visual Studio Community](https://visualstudio.microsoft.com/). It is free. Make sure you are **NOT** downloading Visual Studio **Code**, that won't work.
2. During install, check the `.NET desktop development` workload. Nothing else needs to be checked.
![](https://i.imgur.com/Y8uZ14h.png)

### I forgot to install .net desktop development
If you've forgotten this step, you can click on Tools->Get Tools and Features to bring up the installer again.    
![](https://i.imgur.com/EmqEsmH.png)    

# Creating a Project
This will let you use Visual Studio's code editing and Intellisense

(Alternatively, use [Jopojelly's Mod Generator tool](http://javid.ddns.net/tModLoader/generator/ModSkeletonGenerator.html) which sets everything up for you, **however: it is recommended you follow these steps so you know what's going on and how to do it**)

1. Open Visual Studio and go to New -> Project
1. Select the "Class Library" template
1. Choose a name for the mod. This is the internal name (and file name), not the display name. Do not use whitespaces eg. Example Mod -> "ExampleMod", if you really want to signify spaces then use underscores eg. Example Mod -> "Example_Mod" (**this however isn't recommended in C#!**)
1. Uncheck "Create directory for solution"
1. Click OK
![New Project Dialog](http://i.imgur.com/tQIfA3g.png)
1. Go to the Solution Explorer tab, right click on Solution -> ModName -> References and click Add Reference... ([example](https://i.imgur.com/oM30lfT.png))
1. Click Browse... and select the tModLoader executable (likely: `C:\Program Files (x86)\Steam\steamapps\common\Terraria\Terraria.exe`)
1. Add the line `buildIgnore = *.csproj, *.user, obj\*, bin\*, .vs\*` to your build.txt
1. Create a class extending `Terraria.ModLoader.Mod` and begin developing

You will also need to add the XNA library .dlls as references to your project.   This requires the [XNA Framework Redistributable](https://www.microsoft.com/en-us/download/details.aspx?id=20914) to be installed! Without it, you will not have the required DLLs.  
You can (most of the time) find these files here: `search-ms:displayname=Search%20Results%20in%20GAC_32&crumb=filename%3A~<Microsoft.XNA%20OR%20System.Generic.String%3AMicrosoft.XNA&crumb=fileextension%3A~<Microsoft.XNA*.dll%20filename%3A~<Microsoft.XNA*.dll%20OR%20System.Generic.String%3AMicrosoft.XNA*.dll&crumb=location:C%3A%5CWindows%5CMicrosoft.NET%5Cassembly%5CGAC_32` (Windows)
Paste this weird search string in your file browser pathbar like so 
![](https://i.imgur.com/zQo6j1X.png)    
[Video of finding XNA dll files](https://gfycat.com/CleanLastLeveret)

It is recommended to **!! copy !! (please COPY and paste, do not cut and paste)** these files some place safe and easily accessible. You can then add them as references by right clicking your references (in the solution explorer) and clicking 'Add reference' ([example](https://i.imgur.com/oM30lfT.png))

If you can't find the .dlls or you couldn't succeed in adding them as a reference, there's a neat trick you can try which will reference them automatically: 
1. Open the csproj file in a good text editor (like [Notepad++](https://notepad-plus-plus.org/))
1. Find the reference section and add these lines

`<Reference Include="Microsoft.Xna.Framework" />`  
`<Reference Include="Microsoft.Xna.Framework.Game" />`  
`<Reference Include="Microsoft.Xna.Framework.Graphics" />`  

If you already have several source files, and are having difficulty adding them, try dragging and dropping them into the Solution Explorer window.

# Building with Visual Studio
This will let you build your mod from within Visual Studio, so you don't have to rebuild from within the game, or have your sources in the Mod Sources folder

1. Right click on your mod project in the Solution Explorer and click Properties
1. Go to the Build Events tab
1. Add the following to the Post-build event command line
`"C:\Program Files (x86)\Steam\steamapps\common\Terraria\tModLoaderServer.exe" -build "$(ProjectDir)\"`

**Please keep in mind that Visual Studio DOES NOT abide to your buildIgnore rules, and your built .tmod file will be larger than if you would use the in-game build option. So, before you release your mod, ALWAYS build using the in-game menu.**

# Debugging
This will let you set breakpoints in your mod and inspect variables in Visual Studio.

1. Right click on your mod project in the Solution Explorer and click Properties
1. Go to the Debug tab
1. Set the Start Action to Start external program: `C:\Program Files (x86)\Steam\steamapps\common\Terraria\Terraria.exe`
1. Set the Working directory to `C:\Program Files (x86)\Steam\steamapps\common\Terraria`
1. Add the line `includePDB = true` to your build.txt

The `includePDB = true` line will not only enable debugging, but also line numbers in exception stack traces. However it will increase the size of your mod _and should be omitted from release builds_.

# Edit and Continue
This will let you edit the source code of your mod while at a breakpoint without restarting the game.

1. Simply add `-eac "$(TargetPath)"` to the end of your Post-build event command line

You may now edit your mod's code while tModLoader is running by pressing Ctrl+Alt+Break (Break All) in Visual Studio. Press F5 (Continue) when done to return to the game.

# FAQ
## Autocomplete isn't doing anything for me.
(Also: Autocomplete is changing things I don't want it to, like changing `item` to `Items`.)
1. Check that the tModLoader executable file is in your references. Make sure it's modded not vanilla.    
![](http://i.imgur.com/HvodIHV.png)
2. Check that your .cs files are actually in the project, and that you are opening the project not just individual .cs files. You can use the show all files toggle and then right click -> include in project to add missing source files to the project.    
![](http://i.imgur.com/dNMyROY.png)
3. Make sure you aren't opening .cs files from the File Explorer. You should always open the .csproj (or .sln) file and then once Visual Studio is open, open your .cs source files from the Solution Explorer. If you don't, you'll notice that the top left area says Miscellaneous Files, meaning the file doesn't belong to a project. If you see Miscellaneous Files, check step 2 again.    
![](https://i.imgur.com/bw41Wt4.png)    
After fixing:    
![](https://i.imgur.com/RY456Nb.png)
4. Make sure an installed .net framework is installed. Right click on the project and selec properties, then use the Target Framework dropdown to select an installed .net framework. 4.5 or anything 4 or higher should work.    
![](https://i.imgur.com/amGQghT.png)![](https://i.imgur.com/rH9Ca7q.png)    

## Error CS0246 The type or namespace name 'ReLogic' could not be found
You can download the dll file from [here](https://github.com/blushiemagic/tModLoader/tree/master/references). Place the file in a folder like `\My Games\Terraria\ModLoader\References` and then add it using `Add References...`

## Error CS0246 The type or namespace name 'Mod' (or 'ModLoader', 'ModItem', etc) could not be found
You have added a reference to the vanilla Terraria.exe, remove it and add a correct reference to the modded Terraria.exe.

## CS5001 Program does not contain a static 'Main' method suitable for an entry point
Make sure "Output type" is set to "Class Library" in the project properties.

## References to other classes stopped working! (such as : ModItem, : ModProjectile etc.)
Simply restart Visual Studio and it should fix itself.