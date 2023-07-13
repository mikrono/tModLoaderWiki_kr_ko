***
This Guide has been updated to 1.4. If you need to view the old 1.3 version of this wiki page, click [here](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-FAQ/8a86002d330cd4aa54d17012d31322c73bacd94a)
***

# Basic Troubleshooting Information
This guide will attempt to help users fix many common issues. Please search through this document for the error you are experiencing. You can use `ctrl-F` to use your web browser to search this page for a word (such as an exception name or error message). If you can't find anything relevant, please try all the suggestions in [General Troubleshooting Steps](#general-troubleshooting-steps). If nothing there helps, you can pursue direct support in the [Getting Support section](#getting-support). Do note that this guide mostly concerns issues with tModLoader itself. If you have an issue with a specific Mod or combination of Mods, see [Mod Troubleshooting Steps](#mod-issues)

## Note on tModLoader 64 bit
Please do not attempt to install "tModLoader 64 bit" into tModLoader, it is no longer useful and will prevent the game from launching correctly. If you have previously installed it and are experiencing issues, your first troubleshooting step is to do a [fresh install](#fresh-install). You will also need to clear out the `Launch Options` by following [these instructions](#failed-to-start-process-for-tmodloader-the-system-cannot-find-the-path-specified).

## Note on Piracy
If you pirated Terraria, we can't help you. tModLoader won't work. Please don't bother us by asking how to get it to work.

# General Troubleshooting Steps
These steps should be followed if your specific issue is not found in the later sections of this guide.

## Switch to Stable tModLoader or to Preview tModLoader
If you are on `1.4.4-preview` tModLoader, there is a greater possibility of tModLoader breaking, so you should switch back to `None` (Stable tModLoader) to see if that solves your problem. Conversely, sometimes bugs are fixed in `1.4.4-preview`, so you can switch to `1.4.4-preview` from `None` to test and see if the upcoming monthly release has a fix for your issue.

<details><summary>Switching tModLoader branches</summary><blockquote>

In Steam, right click on `tModLoader`, select `Properties`, select `Betas`, then select `1.4.4-preview` or `None` in the dropdown. Ignore the access code section. Close the Properties window, you should see tModLoader downloading an update, once it finishes you can launch it.    
![image](https://i.imgur.com/aoy3HEp.png)     

([Video instructions](https://gfycat.com/AcidicDangerousAmericanwirehair))     

If you switched to `1.4.4-preview` and your issue is resolved, remember that `1.4.4-preview` is mostly for mod makers and testers and you may find that some mods you use are now broken. The fixes being tested in `1.4.4-preview` will arrive in stable tModLoader near the start of each month (unless there is an extended preview window). If you can avoid the issue, waiting until the start of next month might be better than staying on `1.4.4-preview`.

</blockquote></details>

## Verify Game Integrity
Verifying game integrity will restore all files installed by tModLoader in the install folder to their original state. In Steam right click on `tModLoader` in the library, then click on `Properties` click on `Local Files`. Click on `Verify integrity of game files...`. ([Video instructions](https://gfycat.com/ScaredOrdinaryBangeltiger)). This should start a download of the now missing files. Once that is done, try tModLoader again to see if you issue is resolved. If it isn't, or you are on GOG, follow the [Fresh Install](#fresh-install) instructions.

## Verify Game Integrity Terraria
tModLoader loads files from the Terraria install folder. Do the same steps above but for Terraria.

## Fresh Install
A fresh install solves many issues preventing the game from launching. After doing a fresh install, **do not** place any other files in the install directory. First, open up the [install location](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#install).     

Make sure you opened the install folder, not the saves folder. The saves folder has folders named "Players" and "Worlds" while the install folder has folders such as "tModLoader-Logs" and "LaunchUtils". Now that the install folder is open select all the files and folders and **delete them all**. Next, reinstall tModLoader:
* Steam install: In Steam right click on `tModLoader` in the library, then click on `Properties` click on `Local Files`. Click on `Verify integrity of game files...`. This should start a download of the now missing files. In the install folder, you should see the files start to appear again. 
* GOG install: Install as normal, to the same folder as before.

### Fresh Install Terraria
tModLoader loads files from the Terraria install folder. Do the same steps above but for Terraria.

## Disable All Mods
If you are experiencing an issue, it is useful to confirm whether the issue is caused by a Mod or by tModLoader. Usually the cause of issues can be determined by reading the error messages in-game or by [Reading client.log](#reading-clientlog), but if not, disabling all mods and confirming that the issue still happens or doesn't happen is useful. 

First, visit the mods menu and click `disable all` (if your issue is preventing you from getting to the main menu, you can either hold the shift key while the game is launching to skip loading mods or find "enabled.json" in the Mods folder in the saves folder and delete it). Close tModLoader and launch it again. Attempt to replicate the error you experienced before. If the error still happens, it is a tModLoader issue and you should try [Getting Support](#getting-support). If the error doesn't happen, you will need to use the [Flowchart](#flowchart) to identify the mod causing the issue. After

## Disable All Resource Packs
Resource packs can cause issues, disabling all resource packs and then testing again can rule out the resource packs as being the issue. To disable resource packs, you can visit the `Workshop->Use Resource Packs` menu and disable all packs. If you can't get to that menu, close the game, open up `\Documents\My Games\Terraria\tModLoader\config.json` in a text editor, find the "ResourcePacks" section, then change all true to false, save the file. 

## Reset config.json
The `config.json` holds user preference information. Sometimes a buggy value in it can cause issues. You can delete or rename the file to reset preferences to default values to test if it was causing your issue. Open up [the saves folder](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#saves) and find `config.json`. Rename `config.json` to `configOld.json`. Renaming it lets you restore it later if you want. When you launch tModLoader, you should see the `Select Language` menu, if you don't, you might have edited the wrong file. If doing this doesn't change anything for your issue, you can close tModLoader and rename the file back to `config.json` after deleting the newly generated `config.json`.

You can do the same for the `input profiles.json` file if your issue is related to keyboard, mouse, or gamepad issues.

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
Many errors you might experience while playing tModLoader can be identified by reading the `client.log` file. (If you are experiencing an error that only happens in multiplayer, you'll need to read both `client.log` and `server.log`). By properly reading the log file, you can identify if an issue is caused by a bug in tModLoader or a bug in a mod you are using. Since tModLoader updates to new versions every month, a mod that worked yesterday might be broken today. In any case, the logs are a good place to find the issue.

If you are experiencing a bug, first make sure all mods are up to date, then close tModLoader and reopen it. This will reset the log and make it easier to read. Once you experience the bug, close tModLoader and open the log file. The log file is the `client.log` file found in the [logs folder](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#logs). Open the file in a normal text editor like notepad if it doesn't have a file association already. 

Next, you'll want to look through the file looking for words like "error" or "exception". Using `ctrl-f` to use the Find feature of your text editor. You'll typically want to focus on the earliest error you can find in the log file. Errors that come after earlier errors might be caused by those earlier errors, so fixing the earlier errors might fix the later errors. Once you find the word "error" or "exception", you should see something similar to this:    
![](https://i.imgur.com/5rA3yR6.png)     
In the above excerpt from `client.log`, we can see an error is being logged. The error begins at line 41, with a brief summary on line 42 shown in blue. After the error summary, there will be a "stack trace" that shows where in the code the error is happening. Take note of the indentation of the lines of the stack trace, shown in green. This indentation is very useful for quickly finding errors logged in the log file rather than using `crtl-f`. Next, you'll want to look through the stack trace for the names of mods you have enabled. The first word after "at" on the lines and before the period is the key word you'll want to look at. Words like "Terraria", "Microsoft", and "MonoMod" can usually be ignored, you want to find the mod or mods that show in the error. In this example, we see a mod named "GadgetGalore" in the stack trace and can make the assumption that it is the cause of the error. If there are multiple mod names in a single stack trace, either mod could be at fault, or both, you'll have to do some testing. Now that we found a mod that is erroring, you can disable the mod and try again. Not every error logged in the logs will an actual bug, so some experimentation may be required. If you need help reading your client.log, you can ask for help in the support channels on the [tModLoader Discord chat](https://tmodloader.net/discord).

# Getting Support
If everything in this guide fails to solve your issue, you may need direct support.   

Support issues fall into two categories: [Issues specific to a mod](#mod-issues) and [tModLoader Support](#tmodLoader-support)

## tModLoader Support
If you need basic support to get the game running, see [Discord Support](#discord-support), otherwise start at [GitHub Issue Tracker](#github-issue-tracker)
### GitHub Issue Tracker
If your issue is not solved by this guide and you still need help first check [the tModLoader Issue Tracker](https://github.com/tModLoader/tModLoader/issues) and search for your issue to see if it has already been reported. If you find your issue, please add a :thumbsup: reaction to the issue to help us prioritize issues. To add the :thumbsup: reaction, click on the smile icon on the top right, then click on :thumbsup:: 

![](https://i.imgur.com/EDBwXtc.png)

It might also be useful to add a comment to the issue with `client.log` and other useful information you can provide to help identify the cause of the issue. There might also be a workaround in the comments that you can follow to fix the issue on your end.

### Discord Support
If you do not find your issue in the issue tracker, you should visit the `#support-forum` channel on the [tModLoader discord](http://www.discord.me/tModLoader). Discord is a free chat service you can access in your browser. We will not provide support for support issues here on GitHub. Once you find the `#support-forum` channel, make a new thread and describe your issue. You'll also need to post all your logs by opening up the [logs folder](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#logs) and dragging the files into the chat. ([Video example](https://gfycat.com/CarefreeVastFrillneckedlizard)). Someone there should be able to help you.

## Mod Issues
If a specific mod is causing your issue, you should visit the [mod's workshop page](https://steamcommunity.com/app/1281930/workshop/). Read the `Description` and see if the mod maker has a preferred contact method for bugs, such as a `GitHub` or `Discord`. If there is one, use it. If not, making a comment on the workshop page with your issue should let the creator know. If the bug is game-breaking, you will have to disable the mod or wait for it to be fixed by the author.

### It has been detected that this mod was built for `tModLoader vX.Y`
This message is a boiler-plate message, it is purely informational. If the version starts with `v0.X`, then you are trying to load a legacy mod and it won't work, you need to delete the mod and find it or a replacement on the current mod browser. If the version starts with `v202X`, then you should look at the next paragraph in the error message for the real error. If multiple mod names show up in the error, it may be a mod incompatibility. The author will be able to figure it out.

### JITException 
If you see this, the mod might be out of date. tModLoader updates every month and sometimes those updates will break mods. If it is the start of a month, the author might just be slow in updating. You will have to disable this mod until it is fixed. If you manually installed a mod, the mod will be purple on the `Mods` menu and you should delete it and use the workshop version.

# Launch Issues
### hostpolicy.dll error
Follow the instructions in the next section.

### Black windows appears and closes but nothing else
There are a variety of issues that can cause this issue. If none of these suggestions work, please come to the [tModLoader Discord Support](#discord-support)

**Launch tModLoader Manually**    
Sometimes launching manually can bypass certain launch issues. Open up the [tModLoader install folder](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#install) and double click on "start-tModLoaderServer.bat". If this works, you should still try to fix the issue through other steps here or in [tModLoader Support](#tmodLoader-support) since launching this way is inconvenient.

If, when you try this, an error message shows "Windows cannot find '[InstallFolderHere]/start-tModLoader.bat'. Make sure you typed the name correctly, and then try again.", then somehow your Windows has been corrupted and has a bad file association for `.bat` files. This issue will prevent your computer from running any `.bat` file, which might affect other programs, so it would be good to fix. Users have reported that following the steps in [this article](https://www.winhelponline.com/blog/bat-files-do-not-run-when-double-clicked-fix-association/) have worked for them.

**Fresh Install**    
Follow the [Fresh Install](#fresh-install) and [Fresh Install Terraria](#fresh-install-terraria) steps. This may seem like a lot of effort but it solves a large portion of issues.

**Delete `dotnet` folder**    
Deleting the `dotnet` folder in the [tModLoader install folder](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#install) might help. If the issue persists, check that your antivirus isn't quarantining any of the files. The `dotnet\6.0.0\shared\Microsoft.NETCore.App\6.0.0` folder should have 223 items in it. If it doesn't, ensure that your OS is 64 bit and install the [dependencies](https://learn.microsoft.com/en-us/dotnet/core/install/windows?tabs=net60#additional-deps) listed for your OS before deleting the `dotnet` folder and trying again.

Note: If your console window indicates that it is having trouble downloading dotnet on GOG, then you can manually download [dotnet 6.0.0](https://dotnetcli.azureedge.net/dotnet/Runtime/6.0.0/dotnet-runtime-6.0.0-win-x64.zip) and place it in the `LaunchUtils` folder in the [install folder](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#install). The file should be named `dotnet-runtime-6.0.0-win-x64.zip`. Delete the `dotnet` folder once again and try launching again.

If you are still having issues, and `dotnet\6.0.0\dotnet.exe` or the mentioned 223 files are not all there, you can manually install the included dotnet. Delete the `dotnet` folder, then make the `dotnet` folder again and open it. In that folder, make a folder named `6.0.0` and open it. In a separate file explorer window, open the `LaunchUtils\dotnet-runtime-6.0.0-win-x64.zip` file, you should see 2 folders and 3 files contained within. Drag all of those folders and files into the `6.0.0` folder you created earlier. 

If you are still having issues with dotnet, some users have reported that installing the [dotnet sdk](https://dotnet.microsoft.com/en-us/download/dotnet/thank-you/sdk-6.0.402-windows-x64-installer) to their computer after deleting the `dotnet` folder fixed the issue. Our guess is that the full installer fixes some issues that were preventing our installer from working.

**Check Natives.log**    
In the [logs folder](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#logs), open up `Natives.log`. 
* If it says "An assembly specified in the application dependencies manifest (Microsoft.NETCore.App.deps.json) was not found" or something similar, double check that you followed all the steps in **Delete `dotnet` folder** suggestion above.    
* If it says "System.AccessViolationException: Attempted to read or write protected memory.", follow the [Fresh Install](#fresh-install) and [Fresh Install Terraria](#fresh-install-terraria) steps.

**Disable Proton**    
Do not use Proton on Linux, it will not work. If you did, you'll have to [Delete dotnet folder](#delete-dotnet-folder) after disabling Proton for tModLoader.

**Check launch.log**    
Open up `launch.log` found in the [logs folder](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#logs). If the word "FAudio" shows up in your log, you might want to try the [IAudioClient-workaround.zip](https://github.com/tModLoader/tModLoader/issues/2863#issuecomment-1221975856)

**Disable RGB Keyboard Features**    
Even if you don't think you have RGB keyboard, this can help. Follow the [RGB Keyboard Bug (Port 53664)](#rgb-keyboard-bug-port-53664) instructions.

**Restart Computer**    
Restarting the computer solves many issues tModLoader users face. It may seem trite, but it works, give it a try.

### An error occurred while updating tModLoader (missing executable)    
![image](https://user-images.githubusercontent.com/4522492/194225008-48810974-7292-4b5b-974a-e03363b4383f.png)    
This is usually solved by restarting the computer after confirming that you have switched to the `None` branch for tModLoader and verifying game integrity, and following the next section.

### Failed to start process for tModLoader: "The system cannot find the path specified"
![image](https://user-images.githubusercontent.com/4522492/194225054-b0d9cac7-10fd-4864-87e9-0d16c9418ed1.png)    
This is caused by previously installing `tModLoader 64 bit`. First do a [fresh install](#fresh-install) if you haven't already, then right click on `tModLoader` in `Steam` and select `Properties`. Make sure `Launch Options` is completely empty, then close the window.    
![image](https://user-images.githubusercontent.com/4522492/194367542-7f6d2700-542e-4a14-8074-68fb9a9c7677.png)     

### Failed to start process for tModLoader: "The operation completed successfully." 0x0
![failiuyre](https://github.com/tModLoader/tModLoader/assets/4522492/5b6c8c50-ec14-42d2-bc4c-32474b500e99)    
We've seen this reported when the file association for `.bat` files was broken. Open up the install folder and look at the `start-tModLoader.bat` file. The icon should look similar to this:    
![image](https://github.com/tModLoader/tModLoader/assets/4522492/0e2e487e-de0b-4214-af13-b6a4f09fcc66)    
If it doesn't, the file association for `.bat` files has been changed. This shouldn't happen, you might want to scan for viruses. After that, you'll want to use Google to figure out how to fix the `.bat` file association for your Windows version.

### Failed to Load Asset (AssetLoadException)
![image](https://user-images.githubusercontent.com/4522492/194373688-06a6aedc-ca27-4fe5-9587-7971a6e6c09e.png)    
This is usually caused by your Terraria install being out of date. First, launch Terraria and confirm that it is updated to the [latest version](https://terraria.fandom.com/wiki/PC_version_history) by looking in the bottom right corner. Next, follow the [Verify Game Integrity Terraria](#verify-game-integrity-terraria) instructions. If this doesn't solve the issue, it might be a mod causing this issue.

### NoSuitableGraphicsDeviceException: Could not find d3dcompiler_47.dll
If you are on Windows 7, you'll need to install the [directX package from Microsoft](https://support.microsoft.com/en-us/topic/update-for-the-d3dcompiler-47-dll-component-on-windows-server-2012-windows-7-and-windows-server-2008-r2-769c6690-ed30-4dee-8bf8-dfa30e2f8088). Restart your computer after running and finishing the installer.

### Multiple extensions for asset
This is caused by an incompatible resource pack. Currently some resource packs are incompatible with tModLoader. To fix this, open up `\Documents\My Games\Terraria\tModLoader\config.json` in a text editor, find the "ResourcePacks" section, then change all `true` to `false`, save the file. Now tModLoader should launch again. The issue will be fixed eventually, thumbs up the [issue](https://github.com/tModLoader/tModLoader/issues/2913) to prioritize this issue being fixed.

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

<details><summary>Adding an exception</summary>

The easiest way to add an exception for tModLoader is to press `OK` on the error, then click on the notification that appears:  
* If the notification doesn't appear within 5 seconds, you can visit the action center at the bottom right corner of the screen and find it there. If it is not there, search "Controlled folder access" in the start menu, click it, then click "Block History"    
![image](https://user-images.githubusercontent.com/4522492/176323144-fa85ff75-8759-414c-aeff-22ac0be0bf69.png)    

Click on the first result:    
![image](https://user-images.githubusercontent.com/4522492/176323195-14185202-e1be-4cf8-a62e-08b2c9a84a96.png)    

Click `Yes` when asked "Do you want to allow this app to make changes to your device":    
![image](https://user-images.githubusercontent.com/4522492/176323236-8f69f573-78be-4335-af93-5cd05cc51d56.png)    

You should now see information explaining that "dotnet.exe" was blocked from accessing the [saves folder](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#saves) (The saves folder is typically `"My Games\Terraria\tModLoader"`):    
![image](https://user-images.githubusercontent.com/4522492/176323400-53b3235e-7be1-4e62-8733-2de967532eb2.png)    

Click `Actions` and then `Allow on Device`:    
![image](https://user-images.githubusercontent.com/4522492/176323452-28e6c171-796e-49a9-87d1-77cbf1579733.png)    

You will once again be asked "Do you want to allow this app to make changes to your device", click `Yes`:    
![image](https://user-images.githubusercontent.com/4522492/176323236-8f69f573-78be-4335-af93-5cd05cc51d56.png)    

tModLoader should now be able to create the save files it needs. Launch the game again.

</details>

If you followed the directions and still can't solve the issue, a last resort is to tell tModLoader to save to the [install directory](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#install) instead. You can do this by creating a `savehere.txt` file in the [install directory](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#install).

### System.UnauthorizedAccessException: Access to the path is denied.  
![](https://i.imgur.com/ZjhIvNo.png)

This issue can be caused by your antivirus or windows security settings. If you're using Windows Security (formerly Windows Defender) and are getting this error, then you will need to add "dotnet.exe" to your whitelist, for further instructions on how to do this continue reading below.

First, follow the instructions in the [Controlled Folder Access](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-FAQ#controlled-folder-access) section above, if that doesn't work then follow the steps below.

<details><summary>Adding "dotnet.exe" to your whitelist</summary>

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

![image](https://github.com/tModLoader/tModLoader/assets/4522492/78d94913-578e-4fbb-94de-138720efef86)    

Scroll through the list until you find "dotnet.exe", and click the + and then close, after this you're done! (If you cannot find "dotnet.exe" on your list, then continue with the below steps)

![image](https://github.com/tModLoader/tModLoader/assets/4522492/0335069e-4058-4400-9ca9-0fdc61b50a14)    

Back in the "Add allowed app" selection, left-click "Browse all apps"

![image](https://github.com/tModLoader/tModLoader/assets/4522492/e2e5a8b0-738e-456f-af52-2e839979adef)    

Navigate to the [tModLoader install folder](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#install) and then navigate to and select "dotnet/6.0.0/dotnet.exe", then press `Open`. This will add the file to your whitelist. Done!

</details>

# Audio Issues
### IAudioClient workaround
Try the [IAudioClient workaround](https://github.com/tModLoader/tModLoader/issues/2863#issuecomment-1221975856).

## Audio Troubleshooting
Audio issues are particularly hard to identify, try these troubleshooting steps if your issue is not listed. For each of these make sure tModLoader is closed while you try these changes:

**Fresh Install**    
Manually installed wave banks can cause issues, following the [Fresh Install](#fresh-install) and [Fresh Install Terraria](#fresh-install-terraria) steps will ensure that those files are removed. You might not remember manually installing wavebanks, but other things like installing `tModLoader 64 bit` could do this incidentally.

**Disable All Resource Packs**    
Follow the [Disable All Resource Packs](#disable-all-resource-packs) instructions. 

**Change audio devices**    
For example if you are using headphones, switch to your speakers. If you don't have speakers, try plugging some in. Try launching tModLoader.

**Unplug Controllers**    
Some controllers have speakers in them that might be causing issues with our audio playback code, try unplugging them and then launching tModLoader.

**Reset config.json**    
Follow the [Reset config.json](#reset-configjson) instructions. 

**Try the IAudioClient workaround**    
Try the [IAudioClient workaround](#iaudioclient-workaround).

**Set Volume to 0**    
If audio issues prevent the game from launching, open up [the saves folder](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#saves) and find `config.json`, open the file in a text editor. Find "VolumeMusic", "VolumeAmbient", and "VolumeSound". For each of these, change the number to `0`. Save the file. If this works, the issue is audio related, other steps in this guide might be able to restore audio so you can turn the volumes back up.

# Not Responding
When the game stops responding, that is indicative of the game logic being stuck in an infinite loop. This type of issue can be very hard to diagnose. A capable programmer can use a minidump file to investigate the cause of the issue. If the issue is in a mod, hopefully that modder will fix their mod, if it is in tModLoader we can work on fixing it.

## Minidump Instructions
If your tModLoader stops responding and goes white, you can provide us with a minidump file and it will help us debug the issue.    
![unknown (2)](https://user-images.githubusercontent.com/4522492/179609389-3eafa688-1039-4448-a35d-c1aef6f3d037.png)    

<details><summary>Expand for Minidump Instructions</summary><blockquote>

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

</blockquote></details>

# Load Mod
### "A Mod is crashing when I try to open tModLoader"
You can skip loading mods by holding shift while tModLoader is loading until it reaches the main menu. Visit the `Workshop->Mods` menu to disable or delete the mod. If you need to directly delete a mod, you can unsubscribe on the [workshop](https://steamcommunity.com/app/1281930/workshop/). If you need to delete a manually installed a mod, open up the tModLoader [mods folder](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#mods) and delete the offending .tmod file.

### Begin cannot be called again until End has been successfully called
(Also applies to "Cannot access a disposed object" errors)    
![](https://i.imgur.com/jzbyghT.png)    
This error is an error usually caused by an unhandled error in a mod. This makes it hard for users to know which mod is broken. Much of the time this error will only happen after reloading mods (improperly unloaded Texture2D references), but it can happen due to other errors (for example, dividing by zero.) As a user of mods, it can be hard to figure out which mod is causing the issue. To determine the broken mod, follow the steps in [Reading client.log](#reading-clientlog). The mod causing the error causing this crash should be in the last or second to last entry. If you find a mod mentioned in one of those errors, that is likely the mod causing the error. If in doubt, ask in #support-forum on the [tModLoader discord](http://www.discord.me/tModLoader). As a last resort, use the [flowchart](#flowchart).

### OutOfMemoryException
This error means that tModLoader does not have enough RAM to load all the mods that you are trying to load. Large mods that add lots of items are the main culprit. You may have to cut down on the number of large mods you are trying to load at the same time. You can also try loading Small or Medium worlds instead of Large. Another possibility is that you have other large programs running. If you can close them, do so. Press Ctrl+Shift+Escape to bring up the Task Manager. In the Task Manager's Processes tab, look for processes that take up a large amount of memory. Anything taking more than 100,000 K is a good candidate. Also make sure that you are on 64 bit Windows and that you actually have more than 4 GB of RAM.

Viewing Mods that use a lot of Memory:
If you are curious which mods are using your limited Ram, you can enable the "Show Mod Memory Estimates" option in `Settings->tModLoader Settings`. After enabling the setting, you will have to exit to the main menu and then close and reopen the game for the setting to take effect. Visit the Mods menu, make sure you enable and reload the mods you are curious about, and you should now see colorful graph at the top that shows how much memory each mod is using. Use this information to disable mods that take too much ram compared to how much you enjoy the content.

![](https://i.imgur.com/3Pl6tG2.png)

# Players/Worlds
### "(LaterVersion)" on player or world
![dotnet_2022-10-05_11-04-59](https://user-images.githubusercontent.com/4522492/194119673-44d0ad77-613b-4e6e-8198-20213d2b45c4.png)    
This means that you migrated a player or world from a version of Terraria newer than the version of Terraria your version of tModLoader was built for. When Terraria updates, it takes a while for tModLoader to update, you'll have to be patient and wait for tModLoader to update to use that player or world.

### "(Unknown error)" on player
![unknown (11)](https://user-images.githubusercontent.com/4522492/194117229-e0345d6f-7852-4f5d-916f-7a3a0e01b4d3.png)    
This means that tModLoader can't load the player for some reason. We are constantly looking into causes of this. To fix this issue, you'll need to [restore the player from a backup](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#world-and-player-backups). If you notice that your player consistently corrupts after restoring backups, it may be a broken mod. You can try [Getting Support](#getting-support), someone may be able to diagnose the issue.

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

### Can't redownload mod
If you find that you can't redownload a mod in-game, you'll need to close the game, then visit the [tModLoader workshop](https://steamcommunity.com/app/1281930/workshop/). There, find the mod you are interested and subscribe. If you are already subscribed, unsubscribe and then subscribe again. Finally, restart steam by fully closing it (Steam->Exit), then launching it again. You may also need to follow the [Verify Game Integrity](#verify-game-integrity) steps to force steam to redownload mods you subscribed to.

# Save Data File Issues
### Windows 10+ Onedrive
By default, Microsoft has forced Onedrive on to the 'My Documents' folder, which can mess with some games.

Error Message: The Cloud File Provider is Not Running    
![image](https://github.com/tModLoader/tModLoader/assets/59670736/2e17cd93-a44a-43f6-a169-c045d2c7f094)    

<details>
<Summary>Solution 1: Uninstall Onedrive.</Summary>

* On Windows Searchbar, search Programs, and select 'Add or Remove Programs'
* Uninstall Onedrive.
* Problem solved.

</details>

<details>
<summary>Solution 2: Get Onedrive up and running</summary>

* Follow the instructions in [Sync files with OneDrive in Windows](https://support.microsoft.com/en-gb/office/sync-files-with-onedrive-in-windows-615391c4-2bd3-4aae-a42a-858262e42a49) to turn on OneDrive. 
* Then turn off 'Documents' syncing in Manage Access menu    
![image](https://github.com/tModLoader/tModLoader/assets/59670736/bc96bafd-4f03-450a-9797-5fd322e8a4bf)    
* And/or turn on 'Always keep on this device:    
![image](https://github.com/tModLoader/tModLoader/assets/59670736/e2a24cec-5ec2-4a7b-adf5-cf638c720b5d)    

</details>

### Unable to find my tModLoader 1.4.3 save data OR Auto-migration of files failed.
Do this for files on your computer, referencing example of copying files from 'default' to 1.4.3:
![image](https://github.com/tModLoader/tModLoader/assets/59670736/aced141b-3e5e-4df1-aff3-b6e8f097fa52)

For Steam Cloud related files, unfortunately, there isn't a quick fix.
Please download your files manually per https://www.howtogeek.com/428491/how-to-download-your-save-games-from-steam-cloud/

### JsonReader Exception '0x00', Config.json
Error Message: You basically have a corrupted config.json.    
![image](https://github.com/tModLoader/tModLoader/assets/59670736/5ffb39ae-8677-4630-88c4-8e06e7aa0839)    

Replace Config.Json in the My Games/Terraria/tModLoader folder with the one provided here:    
[config.zip](https://github.com/tModLoader/tModLoader/files/11992590/config.zip)     

Example of File Location    
![image](https://github.com/tModLoader/tModLoader/assets/59670736/dbe1c30b-09eb-4776-a5a1-943984735d4f)    

# Linux / Steam Deck
tModLoader is designed to run as a native Linux application. It is fully featured as a native program, and as such, does not require Proton.
Unfortunately, due to unknown causes, tModLoader doesn't work with Proton.

### Compatibility Options
This is one of the rare applications where you will need to set Compatibility Options to 'None'.
STEAM DECK: If you don't see the None option, you can install 'Steam Play None' to forcibly add the option.

### Flatpak, SteamDeck Steam, and related sandboxed applications
IF you want to develop mods, these will conflict with doing so.
You will need to launch the game outside of Steam, using either 'Non-Steam Game' library option or launching it from the terminal.

# Other Issues
### RGB Keyboard Bug (Port 53664)
Networking issues (`SocketExceptionFactory`, `WebException`, `HttpRequestException`) addressed to port `53664` are caused by RGB keyboard support bugs. You can attempt to repair the installation of your keyboard software, or disable the feature. Even if you don't think you have RGB keyboard, this can help. Close tModLoader. Open up [the saves folder](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#saves) and find `config.json`, open the file in a text editor. Find the "UseRazerRGB", "UseCorsairRGB", "UseLogitechRGB", and "UseSteelSeriesRGB" entries and change "true" to "false" for each of them, then save the file and finally attempt to open tModLoader.

### Low FPS
There are many things you should try:
1. First, make sure none of your mods are throwing errors in the log. See [Reading client.log](#reading-clientlog) to see how to check logs for error messages from mods.
2. Next, confirm that the issue happens even with 0 mods enabled.
3. Try changing "Frame Skip" settings
4. You can try a different graphics options. To do this, close the game and then in Steam, right click on `tModLoader`, select `Properties`, and then find the `Launch Options` box. Type `/gldevice:OpenGL` or `/gldevice:Vulkan` into the box and close the window. Launch the game and check your frame rate. Do these same steps to try the other option. If these have no effect, remove them in the same manner.      
![image](https://user-images.githubusercontent.com/4522492/194379953-0392ca08-8ac3-4610-8486-595620e3d1d7.png)    
    1. If you are using the GOG version of the game, you can edit `start-tModLoader.bat` and change the 3rd line to `LaunchUtils/busybox-sh.bat ./LaunchUtils/ScriptCaller.sh /gldevice:Vulkan %*`, save, then test by launching the file.
5. If using Steam, you can try the following:
    * Disable animated avatars in Friends List settings
    * Disable the steam overlay in In-Game settings
    * Launch Steam in "compact mode" [(Tutorial here)](https://techverse.net/how-to-enable-small-mode-on-steam/)
6. Come to [tModLoader Discord Support](#discord-support).

### Ghost Mouse 
If your mouse is invisible and still highlights buttons, you might be running into an issue with "Monect Virtual Controller". If your `client.log` has "Monect Virtual Controller" in it, follow [these steps](https://community.monect.com/d/39-fuk-monect-hid-device) to uninstall that. Plugging in a separate controller has also been reported to fix this problem if you don't have "Monect Virtual Controller".

Another solution is to close the game, then edit `config.json`, found in [the saves folder](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#saves), in a text editor and change the "InvisibleCursorForGamepad" and "SettingBlockGamepadsEntirely" settings to `false` and then save the file.