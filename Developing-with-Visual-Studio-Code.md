- **This guide does not cover Visual Studio, but Visual Studio Code.**
- **Visual Studio Code can not be used to debug, but takes less hard drive space than Visual Studio.**
- **If you have space on your computer, it is highly recommended to get Visual Studio instead.**
- **This guide is not meant to learn C# fundamentals**

# Prerequisites
1. Download [Visual Studio](https://code.visualstudio.com/). 
1. Install Visual Studio by running the downloaded installer. Allow the program to run and accept licence agreements as you would normally do. The default options should be suitable.
1. Launch Visual Studio Code
1. Install the C# Extension
    1. You should see something similar to the following: [Welcome Page](https://i.imgur.com/YwNh3x6.png)
    1. Click the Extensions button: [Button Location](https://i.imgur.com/fqMhVKd.png)
    1. Type `c#` into the search bar, click the "C# for Visual Studio Code (powered by OmniSharp)" extension, then click the install button: [Install C# Extension Steps](https://i.imgur.com/tysRolo.png)
    1. Wait for the installation to complete. The output window will show several files being downloaded and installed. These 3 files are about 140 MB total, so this step might take a while depending on your internet speed. After all 3 files are downloaded and installed, it will say "Finished": [Successful Install Output](https://i.imgur.com/qcDMLVe.png) 

# Creating a Mod
Follow the v0.11 instructions in [Basic tModLoader Modding Guide](Basic-tModLoader-Modding-Guide)

# Open your Mod Source
1. It is very important that you installed the C# Extension listed in [Prerequisites](#prerequisites)
1. Open Visual Studio Code
1. Click `Open Folder`. If you don't see it, click `File->Open Folder`. Navigate to `%UserProfile%\Documents\My Games\Terraria\ModLoader\Mod Sources\TutorialMod` and click `Select Folder`: [Example](https://i.imgur.com/lCaN4aP.png)
1. You should now see various files available to edit. Open `TutorialMod.cs` and you should be able to start writing code.
1. You can create new folders and .cs files by right clicking in empty space in the Explorer pane: [New File](https://i.imgur.com/B6fh4JD.png)

# Build your Mod
Building your mod from Visual Studio Code can be convenient. If you have tModLoader open, you'll need to make sure the mod is disabled and unloaded before building.

## One Time Steps
These steps need to be done the first time you build the mod:
1. Click `Terminal->Run Build Task...`: [Image](https://i.imgur.com/J2AMh2x.png)
1. Click `Configure Build Task...` in the menu that appears: [Image](https://i.imgur.com/hsWTsUJ.png)
1. Click `Create tasks.json file` in the menu that appears: [Image](https://i.imgur.com/JHqC4PE.png)
1. Click the `MSBuild` option in the menu that appears: [Image](https://i.imgur.com/qg4chlH.png)

If you have done the `One Time Steps` above, you are now ready to build your mod. To build your mod, simply click `Terminal->Run Build Task...` and then `build` in the menu that appears. A successful build will look like this: [Successs](https://i.imgur.com/FjolklJ.png)

## Common Build Issues
* If you get an error relating to "project.assets.json not found. Run a NuGet package restore to generate this file.", then go to `View->Terminal`, run the `dotnet restore` command in the terminal that appears. Now you can run the build again.  

* If you get an error relating to "System.IO.IOException: The process cannot access the file TutorialMod.tmod because it is being used by another process.", you need to disable the mod in tModLoader and reload mods so that the file can be edited.

# Why Use Visual Studio Code
Please read the [Why Use an IDE](Why-Use-an-IDE) page. Aside from debugging, most of the features apply to Visual Studio Code. If you aren't seeing errors being underlined or fixes being suggested,  you might have skipped installing the [C# Extension](#prerequisites).

# Debugging
You can't debug in Visual Studio Code. To debug, use [Visual Studio](Developing-with-Visual-Studio).