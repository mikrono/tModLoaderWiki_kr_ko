- **This guide does not cover [Visual Studio](https://github.com/tModLoader/tModLoader/wiki/Developing-with-Visual-Studio), but Visual Studio Code.**
- **Visual Studio Code takes less space to store than Visual Studio, but is missing features.**
- **If you have space on your computer and are on Windows, it is highly recommended to get Visual Studio instead.**
- **This guide is not meant to learn C# fundamentals**

# Prerequisites

## .NET Coding Pack
You can install the ".NET Coding Pack" to install "Visual Studio Code", the "C# for Visual Studio Code" extension, and the ".NET 6 SDK" all at once. You can use this even if you installed some of these previously and only need some of the others. To do this, follow the instructions [here](https://code.visualstudio.com/docs/languages/dotnet#_net-coding-pack).
## Install Manually
1. Download [Visual Studio Code](https://code.visualstudio.com/). 
1. Install Visual Studio Code by running the downloaded installer. Allow the program to run and accept license agreements as you would normally do. The default options should be suitable.
1. Install the [.NET 6 SDK](https://aka.ms/vscDocs/dotnet/download) by downloading and following the instructions.
1. Launch Visual Studio Code
1. Install the C# Extension
    1. You should see something similar to the following: [Welcome Page](https://i.imgur.com/YwNh3x6.png)
    1. Click the Extensions button: [Button Location](https://i.imgur.com/fqMhVKd.png)
    1. Type `c#` into the search bar, click the "C# for Visual Studio Code (powered by OmniSharp)" extension, then click the install button: [Install C# Extension Steps](https://i.imgur.com/tysRolo.png)
    1. Wait for the installation to complete. The output window will show several files being downloaded and installed. These 3 files are about 140 MB total, so this step might take a while depending on your internet speed. After all 3 files are downloaded and installed, it will say "Finished": [Successful Install Output](https://i.imgur.com/qcDMLVe.png) 

# Creating a Mod
Follow the instructions in [Basic tModLoader Modding Guide](Basic-tModLoader-Modding-Guide)

# Open your Mod Source
1. It is very important that you installed the C# Extension and .NET 6 SDK listed in [Prerequisites](#prerequisites)
1. Open Visual Studio Code
1. Click `Open Folder`. If you don't see it, click `File->Open Folder`. Navigate to `%UserProfile%\Documents\My Games\Terraria\ModLoader\Mod Sources\TutorialMod`, if you're on Windows, or Navigate to `~\Library\Application Support\Terraria\ModLoader\Mod Sources\TutorialMod`, if you're on Mac, and click `Select Folder`: [Example](https://i.imgur.com/lCaN4aP.png)
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

If you have done the `One Time Steps` above, you are now ready to build your mod. To build your mod, simply click `Terminal->Run Build Task...` and then `build` in the menu that appears. A successful build will look like this: [Success](https://i.imgur.com/FjolklJ.png)

## Common Build Issues
* If you get an error relating to "project.assets.json not found. Run a NuGet package restore to generate this file.", then go to `View->Terminal`, run the `dotnet restore` command in the terminal that appears. Now you can run the build again.  

* If you get an error relating to "System.IO.IOException: The process cannot access the file TutorialMod.tmod because it is being used by another process.", you need to disable the mod in tModLoader and reload mods so that the file can be edited.

## No Error Checking or Autocomplete
You might need to wait for the "C# for Visual Studio Code (powered by OmniSharp)" extension to finish updating. It updates fairly often. See the output panel to view download progress. [Update Output](https://i.imgur.com/qcDMLVe.png) 

# Why Use Visual Studio Code
Please read the [Why Use an IDE](Why-Use-an-IDE) page. Most of the features apply to Visual Studio Code. If you aren't seeing errors being underlined or fixes being suggested, you might have skipped installing the [C# Extension](#prerequisites).

# Debugging
Debugging can be achieved in VSCode by manually configuring the launch configuration (`.vscode/launch.json`), see this example:
```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Launch Debug",
            "type": "coreclr",
            "request": "launch",
            "program": "/storage/Games/SteamLibrary/steamapps/common/tModLoader/tModLoader.dll",
            "cwd": "/storage/Games/SteamLibrary/steamapps/common/tModLoader/",
            "requireExactSource": false,
            "justMyCode": false,
            "suppressJITOptimizations": false // true, if the debugger is skipping lines or otherwise acting weirdly, will probably slow down the game significantly
        }
    ]
}
```
The `"program"` key corresponds to wherever your tModLoader.dll is located, if using Steam you can find it by right clicking tModLoader and clicking `"Browse local files"`. The `"cwd"` key is that same path, but without `tModLoader.dll` on the end. All the other keys should remain the same. Like is said in the example, `suppressJITOptimizations` may make the debugging experience better, but will also likely slow down the game a bit. Test it out yourself to see if it works well for you.