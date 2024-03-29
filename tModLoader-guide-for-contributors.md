___

**[I don't want to contribute to tModLoader, I want to play mods](tModLoader-guide-for-players)**

___

**[I don't want to contribute to tModLoader, I want to create mods](tModLoader-guide-for-developers)**

___

# Installation
If you still need to install tModLoader refer to the [tModLoader guide for players](tModLoader-guide-for-players).

## IDE
You will need an IDE to help develop tModLoader. We recommend Visual Studio.

## Git
If you've never used Git before, checkout our [guide on how to use it](https://github.com/tModLoader/tModLoader/wiki/Intermediate-Git-&-mod-management). If you ever come across something in this guide you don't recognize, just Google it. You should be easily able to find something relevant to your problem. You can also checkout [this little snippet](#further-online-assistance).
Further, if command line inherently scares you, additional tools such as [GitHub desktop](https://desktop.github.com/) and [Github Extension for VisualStudio](https://marketplace.visualstudio.com/items?itemName=GitHub.GitHubExtensionforVisualStudio) can provide most of the functionality you need in a user friendly interface.

# Code patcher
___
## Current steps for developer setup:**
* Pull the 1.4 branch. You HAVE to use git for this, downloading source code as a zip file won't work;

* [Install .NET 6.0 SDK;](https://dotnet.microsoft.com/download/dotnet/6.0) 

* Run the setup.bat that is in the local copy of the repository;

* Select your Terraria installation (the terraria.exe file) under Options -> Select Terraria

* Click the Setup button;

**NOTE: The decompilation doesn't work on Mac or Linux. You need Windows Terraria.exe.**

* When you're done, PR to 1.4.4, and *not* `master`

tModLoader uses its own code patcher. If you want to contribute to tModLoader, you will have to use this tool. We need to use a patches system, because we are not allowed to upload vanilla source code publicly. It also allows for relatively easy code maintenance. [Here's what the tool looks like](https://i.imgur.com/u9Yy1rl.png)

## Getting the tModLoader code for the first time
___
1. Install Terraria and tModLoader via Steam. Note: setup does not currently work for GoG
2. Fork this repository, then clone your fork onto your PC
3. Open setup.bat in the root folder
4. Click on 'Setup' (top left button)
    * If asked, select your vanilla 1.4.4.9 Terraria.exe (must be vanilla) from steam. It is recommended to do a copy of both Terraria.exe and TerrariaServer.exe and renaming them Terraria_1.4.4.9.exe and TerrariaServer_1.4.4.9.exe, so that when steam updates, you can still keep working on tModLoader
    * If you want to work on the 1.4.3 or 1.3 version of tModLoader, you need Terraria_1.4.3.6.exe and TerrariaServer_1.4.3.6.exe, or Terraria_1.3.5.3.exe and TerrariaServer_1.3.5.3.exe (Up to you to figure out how to get them)
5. When decompilation is complete, verify that you have these folders:
    * src/decompiled/
    * src/Terraria/
    * src/tModLoader/ (You will be primarily modifying files within this folder, through the patcher and tModLoader.sln file) 
6. To open up the tModLoader workspace, navigate to solutions/ and open "tModLoader.sln"

## Testing your Code
If you are testing bug fixes, simply debugging the WindowsDebug (1.3)/ Debug (1.4) configuration is all that is required. Note that when you launch tModLoader.sln on 1.3, LinuxRelease will be selected, you have to switch this each time you launch tModLoader.sln.

### The following applies to 1.3 tModLoader:
If you are testing a new hook/field/method, you'll need to run setup.bat again and run Setup Debugging after saving your changes in tModLoader.sln. This step will update your ModCompile folder so that it is in sync with your new changes, allowing mods to build properly. After that, make sure WindowsDebug is built and debug either ExampleMod where you are using the new hook or WindowsDebug.
If you get the "Failed to compile tModLoader.FNA.exe" Error after pressing `Setup Debugging`, and you intend to test a new hook/field/method, choose the MacRelease configuration and build that. Otherwise, ignore it.

## Committing your changes
___
1. Open setup.bat in the root folder
2. Click on 'Diff x' where x is your workspace
    * **Your workspace is tModLoader 99% of the time.** If it isn't, we imply you know what you're doing.
3. Create a new commit to commit the patches/ folder
    * Before you push your commit, please check our [contribution article](https://github.com/tModLoader/tModLoader/blob/master/.github/CONTRIBUTING.md). Thanks.

## Getting Example Mod into the ModSources folder
___
As you add features to tModLoader, you'll want to add examples of using those features to Example Mod. Example Mod, however, needs to be located in the ModSources folder to build and test the mod. While you could copy and paste the folder back and forth between your ModSources folder and this repository folder whenever you wish to push your changes, a better approach can be achieved by using a symbolic link. A symbolic link allows a single folder to exist in 2 places at once. By creating a symbolic link in ModSources pointing to the ExampleMod folder in this repository, you can easily keep ExampleMod up to date and push changes to Github. Here is the command for creating the symbolic link on Windows. 
1. Open the Command Prompt as Administrator by right clicking on it in the start menu and selecting "Run as Administrator" 
2. Find the path to both your ModSources folder and the ExampleMod folder within your local copy of this repo.
3. Make sure the ModSources folder doesn't already have an old ExampleMod folder, delete it if it exists.
4. Run the command using your folder paths: (Below is just an example)
```cmd
mklink /D "C:\Documents\My Games\Terraria\tModLoader\ModSources\ExampleMod" "C:\Users\MyNameHere\Source\Repos\tModLoader\ExampleMod"
```
![](https://i.imgur.com/UmiWFha.png)    
5. You should see a message "symbolic link created for ..." in the command prompt. In ModSources, you'll see that the ExampleMod folder now has a little icon similar to desktop shortcuts. Now, you can edit ExampleMod and the changes will reflect wherever you cloned this repo to.    
![](https://i.imgur.com/pHVnAYN.png)  
6. To properly open ExampleMod.csproj, you need to navigate to `C:\Documents\My Games\Terraria\tModLoader\ModSources\ExampleMod` and open the ExampleMod.csproj file from File Explorer. Opening it from within Visual Studio with `File->Open->Project/Solution...` won't work, it will have the wrong working directory. Build ExampleMod once it is open to make sure there are no problems before starting.

Before you're about to make a contribution, please check [this article](https://github.com/tModLoader/tModLoader/blob/master/.github/CONTRIBUTING.md). Thanks in advance.

### Keeping your code up-to-date
___
**NOTE:** it is wise that you backup your edits before pulling latest patches, if you have any that you haven't committed yet. Applying the latest patches **will** delete any of your work not included in them.

**Setup** (do this if you're updating your code for the first time, it also requires that you have some kind of command line [git client](https://git-scm.com/downloads) installed)
1. Open a Git Bash window or whatever in the tML folder
2. Enter `git remote add upstream https://github.com/tModLoader/tModLoader/`
3. To ensure that it's been setup correctly, enter `git remote -v` and you should see something like this:
```
origin  https://github.com/*YOURUSERNAME*/tModLoader.git (fetch)
origin  https://github.com/*YOURUSERNAME*/tModLoader.git (push)
upstream        https://github.com/tModLoader/tModLoader (fetch)
upstream        https://github.com/tModLoader/tModLoader (push)
```

**Actually pulling**
1. Open up another shell window (if you want, enter `git remote -v` to make sure everything's as it should be)
2. Enter `git fetch upstream`
3. Then `git merge upstream/*branchtomerge*`
   * This will pull all the newest commits from *branchtomerge* into the branch that you have checked out
   * You should verify that you now have the latest patches, located in patches/
4. Open setup.bat in the root folder
5. Click on 'Regenerate Source' (bottom right corner)
   * After this process you can open solutions/tModLoader.sln as usual with the updated code

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
If you would like to contact us or tModLoader users, it's best to join our [Discord server](https://tmodloader.net/discord). Discord is a chat and voice application.