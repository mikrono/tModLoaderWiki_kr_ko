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

# Creating a Project
Follow the v0.11 instructions in [Basic tModLoader Modding Guide](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Modding-Guide)

# Open your Project
1. It is very important that you installed the C# Extension listed in [Prerequisites](#prerequisites)
1. Open Visual Studio Code
1. Click `Open Folder`. If you don't see it, click `File->Open Folder`. Navigate to `%UserProfile%\My Games\Terraria\ModLoader\Mod Sources\TutorialMod` and click `Select Folder`: [Example](https://i.imgur.com/lCaN4aP.png)
1. You should now see various files available to edit. Open `TutorialMod.cs` and you should be able to start writing code.
1. You can create new folders and .cs files by right clicking in empty space in the Explorer pane: [New File](https://i.imgur.com/B6fh4JD.png)


and write some some junk, like "adassaf". After a second, you should see red underlines showing an error. [Error](https://i.imgur.com/rceyQWJ.png) If you do not see any error, you might have skipped installing the C# Extension. Delete "adassaf" to get rid of the error.

# Debugging
You can't debug in Visual Studio Code. To debug, use [Visual Studio](Developing-with-Visual-Studio).