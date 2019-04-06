- **This guide supports both VS 2017 and VS 2019 edition**
- **It does not cover Visual Studio Code.**
- **This guide is not meant to learn C# fundamentals**

# Prerequisites
1. Download [Visual Studio Community](https://visualstudio.microsoft.com/). 
    1. The community edition is free of charge. 
    1. You can use either 2017 or 2019 edition.
        1. Please note the 2019 edition performs better, but there is currently only a 32-bit release. This will use up approx. 800-1000 MiB of modding RAM.
1. During installation, check the `.NET desktop development` workload. Nothing else needs to be checked. ([Example](https://i.imgur.com/Y8uZ14h.png))<br>
    1. If you've forgotten this step, you can click on Tools->Get Tools and Features to bring up the installer again. ([Example](https://i.imgur.com/EmqEsmH.png))

# Creating a Project
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

# Building with Visual Studio
This will let you build your mod from within Visual Studio, so you don't have to rebuild from within the game, or have your sources in the Mod Sources folder.

1. Right click on your mod project in the Solution Explorer and click Properties
1. Go to the Build Events tab
1. Add the following to the Post-build event command line
`"C:\Program Files (x86)\Steam\steamapps\common\Terraria\tModLoaderServer.exe" -build "$(ProjectDir)\"`
    1. Ensure the paths are enclosed by double quotes, change the path to your tModLoaderServer if you have it installed elsewhere.

**Please keep in mind that Visual Studio DOES NOT abide to your buildIgnore rules, and your built .tmod file will be larger than if you would use the in-game build option. So, before you release your mod, ALWAYS build using the in-game menu.**

# See the build output from within VS.
You can view the output of the build process using the Output window, **this is especially useful if your build fails**

1. Navigate to View -> Output

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

## Error CS0246 The type or namespace name 'ReLogic' could not be found
You can download the dll file from [here](https://github.com/blushiemagic/tModLoader/tree/master/references). Place the file in a folder like `\My Games\Terraria\ModLoader\References` and then add it using `Add References...`

## Error CS0246 The type or namespace name 'Mod' (or 'ModLoader', 'ModItem', etc) could not be found
You have added a reference to the vanilla Terraria.exe, remove it and add a correct reference to the modded Terraria.exe.

## CS5001 Program does not contain a static 'Main' method suitable for an entry point
Make sure "Output type" is set to "Class Library" in the project properties.

## References to other classes stopped working! (such as : ModItem, : ModProjectile etc.)
Simply restart Visual Studio and it should fix itself.
If it does not, you can attempt removing the .vs folder in your mod source root, then reloading the project.