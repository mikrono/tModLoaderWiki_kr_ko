___

**[I don't want to contribute to tModLoader, I want to play mods](tModLoader-guide-for-players)**

___

**[I don't want to contribute to tModLoader, I want to create mods](tModLoader-guide-for-developers)**

___

## Installation
If you still need to install tModLoader refer to the [tModLoader guide for players](tModLoader-guide-for-players).

## IDE
You will need an IDE to help develop tModLoader. We recommend Visual Studio.

## Code patcher
___
**Huge thanks to [ChickenBones](https://github.com/Chicken-Bones) for creating the code patcher!**

**NOTE: The decompilation doesn't work on Mac or Linux. You need Windows.**

tModLoader uses its own code patcher. If you want to contribute to tModLoader, you will have to use this tool. We need to use a patches system, because we are not allowed to upload vanilla source code publicly. It also allows for relatively easy code maintenance. [Here's what the tool looks like](https://i.imgur.com/u9Yy1rl.png)

### Getting the tModLoader code for the first time
___
1. Clone this repository
    * (Temporary Extra Step) Download Terraria 1.3.5.2 from steam:
        * In your web browser or file explorer, paste `steam://open/console` and hit enter. (You may have to confirm that you want to open Steam Client Bootstrapper) This will bring up the steam console prompt
        * Paste `download_depot 105600 105601 4963288974433434036` into the prompt and press enter. ([Image](https://i.imgur.com/fg2WVRM.png)) This will start a download of the Terraria 1.3.5.2 files
        * Wait until your download has finished. The console won't give much feedback, and the download might be slower than normal steam downloads, but you should see network activity in Library->Downloads. You'll know it is done when you see `Depot download complete : "C:\Program Files (x86)\Steam\steamapps\content\app_105600\depot_105601" (10431 files, manifest 4963288974433434036)` in the steam console window. This can take an hour or more because of the slow download server.
        * You'll need to use this version of Terraria when you select vanilla Terraria.exe in step 3 below. You can leave the files in `C:\Program Files (x86)\Steam\steamapps\content\app_105600\depot_105601` or move them to a convenient location.
2. Open setup.bat in the root folder
    * If setup.bat won't open, you must unblock all the files in the cloned repository
    * If you get an error that mentions File Cannot be Found, you might need to make sure `msbuild` is on your `PATH`. For a VS 2019 install, this should be in `C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Current\Bin`, so add that to your `PATH`. If you don't know how to edit your `PATH`, google it. Make sure to restart setup.bat after editing your PATH for it to take effect.
3. Select your vanilla terraria.exe (must be vanilla) ([See example](https://i.imgur.com/MccGyvB.png)) Remember that this needs to be the 1.3.5.2 version of Terraria that we downloaded earlier in step 1.
4. Click on 'Setup' (top left button)
    * **Warning:** decompilation can take several hours to complete depending on your hardware. It's also likely that your computer **completely freezes** during the process, mainly once it hits NPC.cs It is recommended that you enable the 'Single Decompile Thread' option ([See example](https://i.imgur.com/6mBbZnQ.png)) if you don't have very high end hardware. It's unwise to even attempt a decompile if you have less than 8 GB RAM. Having an SSD, powerful CPU and a high amount of RAM will significantly speed up the decompilation process.
5. When decompilation is complete, verify that you have these folders:
    * src/decompiled/
    * src/merged/
    * src/Terraria/
    * src/tModLoader/
6. To open up the tModLoader workspace, navigate to solutions/ and open tModLoader.sln

## Getting Example Mod into the Mod Sources folder
___
As you add features to tModLoader, you'll want to add examples of using those features to Example Mod. Example Mod, however, needs to be located in the Mod Sources folder to build and test the mod. While you could copy and paste the folder back and forth between your Mod Sources folder and this repository folder whenever you wish to push your changes, a better approach can be achieved by using a symbolic link. A symbolic link allows a single folder to exist in 2 places at once. By creating a symbolic link in Mod Sources pointing to the ExampleMod folder in this repository, you can easily keep ExampleMod up to date and push changes to Github. Here is the command for creating the symbolic link on Windows. 
1. Open the Command Prompt as Administrator by right clicking on it in the start menu and selecting "Run as Administrator" 
2. Find the path to both your Mod Sources folder and the ExampleMod folder within your local copy of this repo.
3. Make sure the Mod Sources folder doesn't already have an old ExampleMod folder, delete it if it exists.
4. Run the command using your folder paths: (Below is just an example)
```cmd
mklink /D "C:\Documents\My Games\Terraria\ModLoader\Mod Sources\ExampleMod" "C:\Users\MyNameHere\Source\Repos\tModLoader\ExampleMod"
```
![](https://i.imgur.com/UmiWFha.png)    
5. You should see a message "symbolic link created for ..." in the command prompt. In Mod Sources, you'll see that the ExampleMod folder now has a little icon similar to desktop shortcuts. Now, you can edit ExampleMod and the changes will reflect wherever you cloned this repo to.    
![](https://i.imgur.com/pHVnAYN.png)  

Before you're about to make a contribution, please check [this article](https://github.com/tModLoader/tModLoader/blob/master/CONTRIBUTING.md). Thanks in advance.

### Keeping your code up-to-date
___
**NOTE:** it is wise that you backup your edits before pulling latest patches, if you have any that you haven't committed yet. Applying the latest patches **will** delete any of your work not included in them.

1. Pull all newer commits from this repository
   * You should verify that you now have the latest patches, located in patches/
2. Open setup.bat in the root folder
3. Click on 'Regenerate Source' (bottom right corner)
   * After this process you can open solutions/tModLoader.sln as usual with the updated code

### Committing your changes
___
1. Open setup.bat in the root folder
2. Click on 'Diff x' where x is your workspace
    * Your workspace is tModLoader 99% of the time. If it isn't, we imply you know what you're doing.
3. Create a new commit to commit the patches/ folder
    * Before you push your commit, please check our [contribution article](https://github.com/tModLoader/tModLoader/blob/master/CONTRIBUTING.md). Thanks.

### HELP! I accidentally committed on a wrong branch!
Simply stash changes and checkout.
___
1. Open in git shell/bash or whatever
2. Run `git stash save` or `git stash` (should default to save)
3. Run `git checkout -b xxxx`
    * Replace xxxx by branch name
    * Omit -b if not creating a new branch
4. Run `git stash pop`

## Further online assistance
If you would like to contact us or tModLoader users, it's best to join our [Discord server](https://discord.me/tmodloader). Discord is a chat and voice application we've used for over 2 years to communicate with each other. It's similar to IRC chats. To use Discord you will need to register a new account on [their website](https://discordapp.com/).