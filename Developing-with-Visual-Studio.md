- **This guide supports both VS 2017 and VS 2019 edition**
- **It does not cover Visual Studio Code.**
- **This guide is not meant to learn C# fundamentals**

# Prerequisites
1. Download [Visual Studio Community](https://visualstudio.microsoft.com/). 
    1. The community edition is free of charge. 
    1. You can use either 2017 or 2019 edition, but if you are using 2017 you must update to a recent update.
1. During installation, check the `.NET desktop development` workload. Nothing else needs to be checked. ([Example](https://i.imgur.com/Y8uZ14h.png)) 
    1. You may be tempted to uncheck items you think you don't need in the `Individual components` tab to save hard drive space or bandwidth. We have not currently determined which individual components are able to be unchecked for tModLoader modding. Many have unchecked things and have been unable to compile mods. If you get an error message similar to "The type 'Object' is defined in an assembly that is not referenced.", you may have this issue and should do the full workload install. 
    1. If you've forgotten this step, you can click on Tools->Get Tools and Features to bring up the installer again. ([Example](https://i.imgur.com/EmqEsmH.png))

# Creating a Project
First we must create a new project for our mod. To do this, follow the v0.11 instructions in [Basic tModLoader Modding Guide](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Modding-Guide#v011-instructions)

## Creating or Updating an existing project for an old mod
If you are updating a mod older than 0.11, you can use the "Upgrade .csproj file" button in the Mod Sources menu. This will create a fully working project for you to open and continue working on.    
![](https://i.imgur.com/woTP4Sf.png)

# Opening your Project
It is very important that you open the .csproj file from the file explorer. If you open individual .cs files, Visual Studio will be completely useless. To open the csproj, navigate to `Documents\My Games\Terraria\ModLoader\Mod Sources\[ModName]\` and double-click on `[ModName].csproj`. Make sure you aren't opening the `[ModName].csproj.user` or `[ModName].cs` files. This will likely happen if you do not have file extensions shown. Watch [this](https://gfycat.com/SleepyDisfiguredGuanaco) to see how to toggle file extensions. Now that Visual Studio is open, make sure you see the following:    
![](https://i.imgur.com/NreIujY.png)    
Click the `Solution Explorer` button to see the `.cs` files in your mod. You must open `.cs` files from here in order to correctly use Visual Studio. Let's make sure autocomplete is working. Open up `Items\TutorialSword.cs` and type `item.` on a new line after the existing `item.autoReuse = true;`. You should see the autocomplete popup show up:    
![](https://i.imgur.com/2WOzFND.png)    
Congratulations, you are ready to write code for your mod. Read below for info on building and debugging within Visual Studio.

# Building with Visual Studio
Building your mod within Visual Studio lets you quickly make sure the code you have written doesn't have any build errors. First, make sure tModLoader itself is closed, or at least that the mod you are building is unloaded. Next, simply select the Build->Build [Modname] menu item:    
![](https://i.imgur.com/Afx2wtd.png)    
If you have errors, you'll see them in the Error List. You can click on each error to see the offending code: 
![](https://i.imgur.com/eO0w0cn.png)    
If you do not have errors, you should see Build Succeeded on the bottom left. If you open the Output Panel (View->Output), you should see something similar to this:
![](https://i.imgur.com/mrMpzB3.png)    

**Please keep in mind that Visual Studio DOES NOT abide to your buildIgnore rules, and your built .tmod file will be larger than if you would use the in-game build option. So, before you release your mod, ALWAYS build using the in-game menu.**

# Debugging
Debugging is the main feature that sets Visual Studio apart from Visual Studio Code. This will let you set breakpoints in your mod and inspect variables in Visual Studio. To debug, simply click the button labeled "Terraria" or press F5:    
![](https://i.imgur.com/7GUHYCS.png)    
If you have errors, the Error List will appear, otherwise, the mod will build, be enabled automatically, and Terraria will start. (You may find that Full screen mode makes modding difficult, I suggest using Windowed mode while making mods.) You'll know it is working if you see this:
![](https://i.imgur.com/9vK1rvF.png)    
More information on how to debug and the benefits of debugging are found in the [Why Use an IDE](https://github.com/tModLoader/tModLoader/wiki/Why-Use-an-IDE#debug) page, please read it, it is extremely useful.

# Edit and Continue
While debugging, you can edit the source code and recompile the mod in-game without having to reload mods or restart the game. The [Why Use an IDE](https://github.com/tModLoader/tModLoader/wiki/Why-Use-an-IDE#edit-and-continue) page has more info on this topic, please read it, it is extremely useful.

# Next Steps
Now that you have setup Visual Studio, your mod development speed can increase greatly. Please read [Why Use an IDE](Why-Use-an-IDE) for more tips on how an IDE like Visual Studio greatly speed up your modding.

# Manual Setup
Setting up a project for your mod manually is not recommended. Modders should use the automatic mod project generation. The info in this section is for informational purposes only.

## Creating a Project
1. Open Visual Studio and go to New -> Project
1. Select the "Class Library" template
1. Set the _internal_ name for your mod. (No special characters allowed)
1. Uncheck "Create directory for solution" is using VS 2017, and check "Place solution and project in the same directory" if using VS 2019.
1. Make sure to set the location to the Mod Sources directory
1. Click OK
    1. Your output should look similar to [this](https://i.imgur.com/8JBLt1A.png). Note, your window may look different, this is the window for VS 2019. 
1. Go to the Solution Explorer tab, right click on Solution -> ModName -> References and click Add Reference... ([example](https://i.imgur.com/oM30lfT.png))
1. Click Browse... and select the tModLoader executable (likely: `C:\Program Files (x86)\Steam\steamapps\common\Terraria\Terraria.exe`)
1. Add a new txt file named `build.txt` to the root of your project
1. `buildIgnore = *.csproj, *.user, obj\*, bin\*, .vs\*` to your build.txt
1. Add the XNA references to your project. You should have these if you can play Terraria, otherwise see [here](https://www.microsoft.com/en-us/download/details.aspx?id=20914)
    1. Open the csproj file in a good text editor (like [Notepad++](https://notepad-plus-plus.org/))
    1. Find the reference section and add the following:
    ```
    <Reference Include="Microsoft.Xna.Framework" />
    <Reference Include="Microsoft.Xna.Framework.Game" />
    <Reference Include="Microsoft.Xna.Framework.Graphics" />
    ```
    1. VS should prompt to reload the project, do so. Otherwise, simply re-open the project.
1. Create a class extending `Terraria.ModLoader.Mod`

## Building with Visual Studio
This will let you build your mod from within Visual Studio, so you don't have to rebuild from within the game, or have your sources in the Mod Sources folder.

1. Right click on your mod project in the Solution Explorer and click Properties
1. Go to the Build Events tab
1. Add the following to the Post-build event command line
`"C:\Program Files (x86)\Steam\steamapps\common\Terraria\tModLoaderServer.exe" -build "$(ProjectDir)\"`
    1. Ensure the paths are enclosed by double quotes, change the path to your tModLoaderServer if you have it installed elsewhere.

# Debugging
This will let you set breakpoints in your mod and inspect variables in VS. Change the paths if you installed Terraria elsewhere.

1. Right click on your mod project in the Solution Explorer and click Properties
1. Go to the Debug tab
1. Set the Start Action to Start external program: `C:\Program Files (x86)\Steam\steamapps\common\Terraria\Terraria.exe`
1. Set the Working directory to `C:\Program Files (x86)\Steam\steamapps\common\Terraria`
    1. Add the line `includePDB = true` to your build.txt if you want to include line numbers in exception stack traces (useful for debugging). _However it will increase the size of your mod and should be omitted from release builds_.

# Edit and Continue
This will let you edit the source code and recompile the mod during debugging, without having to restart the game.

1. Simply add `-eac "$(TargetPath)"` to the end of your Post-build event command line

# Common issues
These issues usually only apply if you have not upgraded to the 0.11 .csproj. Please update your csproj in the Mod Sources menu first.

## Autocomplete isn't doing anything for me.
(Also: Autocomplete is changing things I don't want it to, like changing `item` to `Items`.)
1. Check that the tModLoader executable file is in your references. Make sure it's modded not vanilla.    
[Example](http://i.imgur.com/HvodIHV.png)
2. Check that your .cs files are actually in the project, and that you are opening the project not just individual .cs files. You can use the show all files toggle and then right click -> include in project to add missing source files to the project.    
[Example](http://i.imgur.com/dNMyROY.png)
3. Make sure you aren't opening .cs files from the File Explorer. You should always open the .csproj (or .sln) file and then once Visual Studio is open, open your .cs source files from the Solution Explorer. If you don't, you'll notice that the top left area says Miscellaneous Files, meaning the file doesn't belong to a project. If you see Miscellaneous Files, check step 2 again.    
[Example](https://i.imgur.com/bw41Wt4.png)    
After fixing:    
[Example](https://i.imgur.com/RY456Nb.png)
4. Make sure an installed .net framework is installed. Right click on the project and selec properties, then use the Target Framework dropdown to select an installed .net framework. 4.5 or anything 4 or higher should work.    
[Example](https://i.imgur.com/rH9Ca7q.png)   
5. Make sure the Solution Explorer is in solution view and not folder view. [Button Location](https://i.imgur.com/NIOP3fn.png)     

## The SDK 'Microsoft.NET.Sdk' specified could not be found.
This means you skipped installing the `.NET desktop development` workload mentioned at the top of this page. [Prerequisites](#Prerequisites)

## Error CS0246 The type or namespace name 'ReLogic' could not be found
Make sure you are using the updated .csproj and this error will automatically fix itself. See [Creating or Updating an existing project for an old mod](#creating-or-updating-an-existing-project-for-an-old-mod) to update your csproj.

## Error CS0246 The type or namespace name 'Mod' (or 'ModLoader', 'ModItem', etc) could not be found
You have added a reference to the vanilla Terraria.exe, remove it and add a correct reference to the modded Terraria.exe.

## CS5001 Program does not contain a static 'Main' method suitable for an entry point
Make sure "Output type" is set to "Class Library" in the project properties.

## References to other classes stopped working! (such as : ModItem, : ModProjectile etc.)
Simply restart Visual Studio and it should fix itself.
If it does not, you can attempt removing the .vs folder in your mod source root, then reloading the project.

## Failed to load pre-compiled edit and continue dll System.IO.FileNotFoundException: Could not find file 'C:\...\YYY.pdb'.
This error occurs when the project wasn't created with the mod skeleton generator. To fix this error, go under your project's properties (right-click the project under the Solution Explorer and click `Properties`), go to the `Build` tab, click the `Advanced` button and change the value of `Debugging Information` to `Full`.