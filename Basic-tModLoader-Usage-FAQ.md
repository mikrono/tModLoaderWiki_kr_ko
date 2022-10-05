***
This Guide has been updated to 1.4. If you need to view the old 1.3 version of this wiki page, click [here](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-FAQ/8a86002d330cd4aa54d17012d31322c73bacd94a)
***

# Basic Troubleshooting Information
This guide will attempt to help users fix many common issues. Please search through this document for the error you are experiencing. If you can't find anything relevant, please try all the suggestions in [Generic Troubleshooting Steps](#generic-troubleshooting-steps). Do note that this guide mostly concerns issues with tModLoader itself. If you have an issue with a specific Mod or combination of Mods, see [Mod Troubleshooting Steps](#mod-troubleshooting-steps)

## Note on tModLoader 64 bit
Please do not attempt to install "tModLoader 64 bit" into tModLoader, it is no longer useful and will prevent the game from launching correctly. If you have previously installed it and are experiencing issues, your first troubleshooting step is to do a [fresh install](#fresh-install). 

## Note on Piracy
If you pirated Terraria, we can't help you. tModLoader won't work. Please don't bother us by asking how to get it to work.

# Generic Troubleshooting Steps
These steps should be followed if your specific issue is not found in the later sections of this guide.

## Fresh Install
A fresh install solves many issues preventing the game from launching. After doing a fresh install, **do not** place any other files in the install directory. First, open up the [install location](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#install).     

Make sure you opened the install folder, not the saves folder. The saves folder has folders named "Players" and "Worlds" while the install folder has folders such as "tModLoader-Logs" and "LaunchUtils". Now that the install folder is open select all the files and folders and delete them. Next, reinstall tModLoader:
* Steam install: In Steam right click on `tModLoader` in the library, then click on `Properties` click on `Local Files`. Click on `Verify integrity of game files...`. This should start a download of the now missing files. In the install folder, you should see the files start to appear again. 
* GOG install: Install as normal, to the same folder as before.

## Disable All Mods
If you are experiencing an issue, it is useful to confirm whether the issue is caused by a Mod or by tModLoader. Usually the cause of issues can be determined by reading the error messages in-game or by [Reading client.log](#reading-client.log), but if not, disabling all mods and confirming that the issue still happens or doesn't happen is useful. 

First, visit the mods menu and click `disable all` (if your issue is preventing you from getting to the main menu, you can either hold the shift key while the game is launching to skip loading mods or find "enabled.json" in the Mods folder in the saves folder and delete it). Close tModLoader and launch it again. Attempt to replicate the error you experienced before. If the error still happens, it is a tModLoader issue and you should try [Getting Support](#getting-support). If the error doesn't happen, you will need to use the [Flowchart](#flowchart) to identify the mod causing the issue. After

## Disable All Resource Packs
Resource packs can cause issues, disabling all resource packs and then testing again can rule out the resource packs as being the issue. To disable resource packs, you can visit the `Workshop->Use Resource Packs` menu and disable all packs. If you can't get to that menu, close the game, open up "\Documents\My Games\Terraria\tModLoader\config.json" in a text editor, find the "ResourcePacks" section, then change all true to false, save the file. 

## Flowchart
Sometimes a mod is causing issues, but you can't tell which mod is the problem. Use this flowchart to diagnose and determine the bad mod:    
![](https://cdn.discordapp.com/attachments/466247288331829249/481464717043564554/Untitled_Diagram1.png)

## Reading client.log
### Simple Explanation
Most bugs in tModLoader will appear as errors logged to the `client.log` file. Read the file to find mods that are potentially causing bugs.
* Close tModLoader, then reopen tModLoader and repeat the steps necessary to trigger the bug.
* Open the `client.log` file in a text editor, such as notepad. The `client.log` file is found in the [logs folder](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#logs)
  * To easily open the folder copy `%UserProfile%\Documents\My Games\Terraria\tModLoader\Logs\` to the clipboard with `ctrl-c` and paste it into the address bar in the file explorer with `ctrl-v`. Press enter and the file explorer will go to the folder.    
![](https://i.imgur.com/mdKWVXy.png)    
  * If a window pops up asking "How do you want to open this .log file?", find Notepad in the list, click it, click OK.    
![](https://i.imgur.com/Q7HPPLK.png)    
* Scroll down until you see the first exception in the file. It should look like the image below. Look for indentation followed by the word "at", these lines are the details of the exception. Each exception like this in the log is caused by a bug.
![](https://i.imgur.com/5rA3yR6.png)    
* Look through the exception to find the names of mods you have enabled. The first word after "at" on the lines and before the period is where the names of mods involved in the exception will be shown. Words like "Terraria", "Microsoft", and "MonoMod" can usually be ignored. In this example, the mod named "GadgetGalore" in the exception and is likely a broken mod.
  * If there are multiple mod names in a single stack trace, either mod could be at fault, or both, you'll have to do some testing.
* After finding a mod in an exception, disable it the next time you launch tModLoader and see if the same bug triggers. Repeat the above steps if there are additional mods that are causing bugs.
* If you need help reading your client.log, you can ask for help in the support-forum channel on the [tModLoader Discord chat](https://tmodloader.net/discord).

### Complicated Explanation
Many errors you might experience while playing tModLoader can be identified by reading the `client.log` file. (If you are experiencing an error that only happens in multiplayer, you'll need to read both `client.log` and `server.log`). By properly reading the log file, you can identify if an issue is caused by a bug in tModLoader or a bug in a mod you are using. If you are on 1.4, a mod that worked yesterday might be broken today. In any case, the logs are a good place to find the issue.

If you are experiencing a bug, first make sure all mods are up to date, then close tModLoader and reopen it. This will reset the log and make it easier to read. Once you experience the bug, close tModLoader and open the log file. The log file is found in `%UserProfile%\Documents\My Games\Terraria\ModLoader\Logs\client.log` (or `[tModLoaderInstallFolder]\tModLoader-Logs\client.log` for 1.4). Open the file in a normal text editor like notepad if it doesn't have a file association already. 

Next, you'll want to look through the file looking for words like "error" or "exception". Using `ctrl-f` to use the Find feature of your text editor. You'll typically want to focus on the earliest error you can find in the log file. Errors that come after earlier errors might be caused by those earlier errors, so fixing the earlier errors might fix the later errors. Once you find the word "error" or "exception", you should see something similar to this:    
![](https://i.imgur.com/5rA3yR6.png)     
In the above excerpt from `client.log`, we can see an error is being logged. The error begins at line 41, with a brief summary on line 42 shown in blue. After the error summary, there will be a "stack trace" that shows where in the code the error is happening. Take note of the indentation of the lines of the stack trace, shown in green. This indentation is very useful for quickly finding errors logged in the log file rather than using `crtl-f`. Next, you'll want to look through the stack trace for the names of mods you have enabled. The first word after "at" on the lines and before the period is the key word you'll want to look at. Words like "Terraria", "Microsoft", and "MonoMod" can usually be ignored, you want to find the mod or mods that show in the error. In this example, we see a mod named "GadgetGalore" in the stack trace and can make the assumption that it is the cause of the error. If there are multiple mod names in a single stack trace, either mod could be at fault, or both, you'll have to do some testing. Now that we found a mod that is erroring, you can disable the mod and try again. Not every error logged in the logs will an actual bug, so some experimentation may be required. If you need help reading your client.log, you can ask for help in the support channels on the [tModLoader Discord chat](https://tmodloader.net/discord).

# Getting Support
## tModLoader Issues
If your issue is not solved by this guide and you still need help first check [the tModLoader Issue Tracker](https://github.com/tModLoader/tModLoader/issues) and search for your issue to see if it has already been reported. If you find your issue, please add a :thumbsup: reaction to the issue to help us prioritize issues. To add the :thumbsup: reaction, click on the smile icon on the top right, then click on :thumbsup:: 

![](https://i.imgur.com/EDBwXtc.png)

It might also be useful to add a comment to the issue with `client.log` and other useful information you can provide to help identify the cause of the issue. There might also be a workaround in the comments that you can follow to fix the issue on your end.

If you do not find your issue in the issue tracker, you should visit the `#support-forum` channel on the [tModLoader discord](http://www.discord.me/tModLoader). Discord is a free chat service you can access in your browser. We will not provide support for support issues here on GitHub. Once you find the `#support-forum` channel, make a new thread and describe your issue. You'll also need to post all your logs by opening up the [logs folder](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#logs) and dragging the files into the chat. ([Video example](https://gfycat.com/CarefreeVastFrillneckedlizard)). Someone there should be able to help you.

## Mod Issues
If a specific mod is causing your issue, you should visit the [mod's workshop page](https://steamcommunity.com/app/1281930/workshop/). Read the `Description` and see if the mod maker has a preferred contact method for bugs, such as a `GitHub` or `Discord`. If there is one, use it. If not, making a comment on the workshop page with your issue should let the creator know. If the bug is game-breaking, you will have to disable the mod or wait for it to be fixed by the author.

# Launch Issues
### Multiple extensions for asset
This is caused by an incompatible resource pack. Currently some resource packs are incompatible with tModLoader. To fix this, open up `"\Documents\My Games\Terraria\tModLoader\config.json"` in a text editor, find the "ResourcePacks" section, then change all `true` to `false`, save the file. Now tModLoader should launch again.

### System.Threading.SynchronizationLockException
![](https://i.imgur.com/IkPqCo6.png)    
Solution. Disable BitDefender or disable the Safe Files feature of Bit Defender. Some more info has been collected [here](https://www.bitdefender.com/consumer/support/answer/2700/).

### Enabling mods freezes the game/Setting controls doesn't work properly.
This is most likely related to your antivirus blocking access to the `tModLoader` folder in the Documents directory. See the [below issue](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-FAQ#systemunauthorizedaccessexception-access-to-the-path-is-denied) for the process (but do this with the folder instead of the exe)

### Disk Write Error
If you try to install tModLoader through Steam and it gives you a message with "Disk Write Error" in it, it is usually caused by Avast. Disable it temporarily and install tModLoader.

### Controlled Folder Access
![](https://i.imgur.com/Zn40Ohq.png)    

Controlled Folder Access is a Windows 10+ security feature intended to prevent ransomware. It is a useful feature, but it will get in the way of tModLoader saving files to the Documents folder where game save files are typically installed. If you have this feature enabled, you can add an exception to tModLoader to allow it to work. You can also just disable the feature completely, but don't do that unless you know what you are doing.

The easiest way to add an exception for tModLoader is to press `OK` on the error, then click on the notification that appears:  
* If the notification doesn't appear within 5 seconds, you can visit the action center at the bottom right corner of the screen and find it there. If it is not there, search "Controlled folder access" in the start menu, click it, then click "Block History"    
![image](https://user-images.githubusercontent.com/4522492/176323144-fa85ff75-8759-414c-aeff-22ac0be0bf69.png)    

Click on the first result:    
![image](https://user-images.githubusercontent.com/4522492/176323195-14185202-e1be-4cf8-a62e-08b2c9a84a96.png)    

Click `Yes` when asked "Do you want to allow this app to make changes to your device":    
![image](https://user-images.githubusercontent.com/4522492/176323236-8f69f573-78be-4335-af93-5cd05cc51d56.png)    

You should now see information explaining that "dotnet.exe" was blocked from accessing the "My Games\Terraria\tModLoader" folder:    
![image](https://user-images.githubusercontent.com/4522492/176323400-53b3235e-7be1-4e62-8733-2de967532eb2.png)    

Click `Actions` and then `Allow on Device`:    
![image](https://user-images.githubusercontent.com/4522492/176323452-28e6c171-796e-49a9-87d1-77cbf1579733.png)    

You will once again be asked "Do you want to allow this app to make changes to your device", click `Yes`:    
![image](https://user-images.githubusercontent.com/4522492/176323236-8f69f573-78be-4335-af93-5cd05cc51d56.png)    

tModLoader should now be able to create the save files it needs. Launch the game again.

### System.UnauthorizedAccessException: Access to the path is denied.  
![](https://i.imgur.com/ZjhIvNo.png)

This issue can be caused by your antivirus or windows security settings. If you're using Windows Security (formerly Windows Defender) and are getting this error, then you will need to add "dotnet.exe" to your whitelist, for further instructions on how to do this continue reading below.

(If you follow the directions below and still can't solve the issue, a last resort is to tell tModLoader to save to the install directory instead. You can do this by creating a `savehere.txt` file in the install directory.)

![Right-Click and open security dashboard.](https://i.imgur.com/2Lj2Wrx.png)  
Right-Click Windows Security in your system tray and select "View Security Dashboard"

![Select "Virus & Threat protection"](https://i.imgur.com/Uc5a5WL.png)  
Left-Click "Virus & threat Protection"

![](https://i.imgur.com/OJNFSd5.png)  
From there, Left-Click "Manage settings" under "Virus & threat protection settings"

![Scroll down.](https://i.imgur.com/2nhsK3a.png)  
Scroll down until you find the "Controlled folder access" section, and then left-click "Manage Controlled folder access"

![](https://i.imgur.com/DnT3LLQ.png)  
Left-Click "Allow an app through controlled folder access"

![](https://i.imgur.com/Az03a4f.png)  
![](https://i.imgur.com/WLovfFc.png)  
Left-Click "Add an allowed app", and select "Recently blocked apps"

![](https://i.imgur.com/tsXmj1b.png)  
Scroll through the list until you find "dotnet.exe" (or "tModLoader.exe" if 1.3), and click the + and then close, after this you're done! (If you cannot find "dotnet.exe" (or "tModLoader.exe" if 1.3) on your list, then continue with the below steps)

![Extra Step 1](https://i.imgur.com/0ruiXoA.png)  
Back in the "Add allowed app" selection, left-click "Browse all apps"

![Extra Step 2](https://i.imgur.com/E7pnDZo.png)  
Navigate to wherever you installed your tModLoader (refer to video linked below on how to find an installation directory through Steam if you don't know how to do this) and double-click or select and left-click then open "dotnet/6.0.0/dotnet.exe" (or "tModLoader.exe" if 1.3) this will add the file to your whitelist. Done!

[How to find a game install location on Steam](https://gfycat.com/SelfreliantAssuredIsabellineshrike)  
Use the process shown in the above linked video to find your tModLoader install location, the gif is showing how to do this for Terraria, but you will need to do the same process except with tModLoader on Steam (you must have it installed first).

# Not Responding
When the game stops responding, that is indicative of the game logic being stuck in an infinite loop. This type of issue can be very hard to diagnose. A capable programmer can use a minidump file to investigate the cause of the issue. If the issue is in a mod, hopefully that modder will fix their mod, if it is in tModLoader we can work on fixing it.

## Minidump Instructions
If your tModLoader stops responding and goes white, you can provide us with a minidump file and it will help us debug the issue.    
![unknown (2)](https://user-images.githubusercontent.com/4522492/179609389-3eafa688-1039-4448-a35d-c1aef6f3d037.png)    
To do this, download https://download.sysinternals.com/files/Procdump.zip and extract the zip. Open a command prompt in that folder by typing `cmd` in the file explorer address bar and then pressing enter.    
![unknown (3)](https://user-images.githubusercontent.com/4522492/179609489-f7115dfc-1c27-4ec3-a945-1e1c5aa46f44.png)    
This will open a command prompt:    
![unknown (4)](https://user-images.githubusercontent.com/4522492/179609511-d507817b-8734-4db2-9b1e-77f71342758c.png)    
Next, open task manger by pressing ctrl-shift-escape. Click Details, then scroll down to the dotnet.exe that is Not Responding:    
![unknown (5)](https://user-images.githubusercontent.com/4522492/179609519-92cdfca1-0791-46ff-9bc2-9ae11d82325e.png)    
Take note of the PID. In this image, it is 20680, but yours will be different. Go back to the command prompt and type `procdump.exe -mm 20680`, except change 20680 to your number. After a few seconds it will be done:    
![unknown (6)](https://user-images.githubusercontent.com/4522492/179609528-13ab0f1e-eec9-4606-8f05-5f4966aa79eb.png)    
Back in the file explorer, you can see the .dmp file:    
![unknown (7)](https://user-images.githubusercontent.com/4522492/179609674-8ac1b5f4-27fd-48d6-a42d-1aad83e75de6.png)     
Find some way of uploading this file to us on the Discord support channel. We have nitro boosts so you should just be able to drag and drop the file into the support thread.

# Load Mod
### "A Mod is crashing when I try to open tModLoader"
You can skip loading mods by holding shift while tModLoader is loading until it reaches the main menu. Visit the `Workshop->Mods` menu to disable or delete the mod. If you need to directly delete a mod, you can unsubscribe on the [workshop](https://steamcommunity.com/app/1281930/workshop/). If you need to delete a manually installed a mod, open up the tModLoader [mods folder](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#mods) and delete the offending .tmod file.

### Begin cannot be called again until End has been successfully called
(Also applies to "Cannot access a disposed object" errors)    
![](https://i.imgur.com/jzbyghT.png)    
This error is an error usually caused by an unhandled error in a mod. This makes it hard for users to know which mod is broken. Much of the time this error will only happen after reloading mods (improperly unloaded Texture2D references), but it can happen due to other errors (for example, dividing by zero.) As a user of mods, it can be hard to figure out which mod is causing the issue. To determine the broken mod, follow the steps in [Reading client.log](#reading-client.log). The mod causing the error causing this crash should be in the last or second to last entry. If you find a mod mentioned in one of those errors, that is likely the mod causing the error. If in doubt, ask in #support-forum on the [tModLoader discord](http://www.discord.me/tModLoader). As a last resort, use the [flowchart](#flowchart).

### OutOfMemoryException
This error means that tModLoader does not have enough RAM to load all the mods that you are trying to load. Large mods that add lots of items are the main culprit. You may have to cut down on the number of large mods you are trying to load at the same time. You can also try loading Small or Medium worlds instead of Large. Another possibility is that you have other large programs running. If you can close them, do so. Press Ctrl+Shift+Escape to bring up the Task Manager. In the Task Manager's Processes tab, look for processes that take up a large amount of memory. Anything taking more than 100,000 K is a good candidate. Also make sure that you are on 64 bit Windows and that you actually have more than 4 GB of RAM.

Viewing Mods that use a lot of Memory:
If you are curious which mods are using your limited Ram, you can enable the "Show Mod Memory Estimates" option in `Settings->tModLoader Settings`. After enabling the setting, you will have to exit to the main menu and then close and reopen the game for the setting to take effect. Visit the Mods menu, make sure you enable and reload the mods you are curious about, and you should now see colorful graph at the top that shows how much memory each mod is using. Use this information to disable mods that take too much ram compared to how much you enjoy the content.

![](https://i.imgur.com/3Pl6tG2.png)

# Players/Worlds
### "HELP, all my players and worlds are gone!"
tModLoader saves are kept separate from vanilla Terraria saves. You can copy back and forth between save locations, but be aware that you will lose Modded Tile and Items if you use tModloader worlds/characters in vanilla.

Solutions: To copy from Terraria to tModLoader, use the `Migrate individual players...` button in the tModLoader player and world select menus. Note that if tModLoader is currently not up to date with Terraria, the players and worlds will be too new to open. Also note that files stored on the cloud will not show up, you'll need to take the file off the cloud first, see below.    
![](https://i.imgur.com/0CJ9FKM.png)    

To migrate a player or world from tModLoader to Terraria, you'll need to copy from the [tModLoader saves](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#saves) folder to the Terraria saves folder. (The Terraria saves folder should be the parent folder of the tModLoader folder.) For example, copy .wld files in `\Terraria\Worlds` to `\Terraria\tModLoader\Worlds`, and the same for the .plr files in the corresponding `Players` folders. For players you'll also want to grab the folder with the same name as well, since those are the maps.

<details><summary>Getting Players or Worlds from the Cloud</summary><blockquote>
    
You may notice that your player or world isn't in the folder, or maybe only .bak files are in the folder. This means that you have put that player or world onto the cloud. There are 2 ways to get the files. The first option is to open Terraria or tModLoader and simply click the `Move off cloud` button and then follow the above instructions. The second option is to copy the files from the local copy of cloud files steam keeps around and place them in their respective folder. These are found in the [local cloud saves folder](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#cloud)</blockquote>
    
</details>    

### "Cloud storage limit reached, unable to move to cloud"
tModLoader shares the cloud storage space with Terraria (about 150 MB of it). Exceeding this limit on Terraria + tModLoader combined will make you unable to move players and worlds to the cloud until sufficient storage is available. Easiest way is to "un-cloud" your vanilla Terraria worlds, they take up the most space. You can check how much storage is used in Steam: Right click `tModLoader` -> `Properties`, below the `Steam Cloud` section it says the amount of available storage. In the rare case where you used tModLoader cloud storage feature before steam release (0.11.7), you won't be able to get rid of these "orphaned" files the normal way. Try this method: [Video, Terraria App ID is 105600](https://youtu.be/JADsIv2RUSw).

# Mod Browser
### "Mod Browser Offline", "I can't download mods"
![](https://i.imgur.com/JTtOMbq.png)

Steam workshop sometimes goes offline for maintenance, try in a few hours or the next day.