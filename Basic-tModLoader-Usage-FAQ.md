# Note on Piracy
If you pirated Terraria, we can't help you. tModLoader won't work. Please don't bother us by asking how to get it to work.

### Unable to find an entry point named 'SteamAPI_ManualDispatch_Init' in DLL 'steam_api64'
This is caused by pirating other games. Some poorly packaged pirated games put a `steam_api64.dll` file in strange locations on your computer. These broken files are located on the PATH and will be loaded before tModLoader has a chance to load it's own copy. We've seen various reports of `steam_api64.dll` being found on the PATH in places like `C:\Windows\System32`. To determine where, open up a command prompt from the start menu by typing "command prompt" and clicking on the result. Next, type `where steam_api64.dll` and press enter. If the result is `INFO: Could not find files for the given pattern(s).`, then you have some other issue and should come to the Discord and seek support. If you see a result like `c:\Windows\System32\` or other suspicious paths, you will need to fix the issue. Usually this means navigating to the folder and renaming the file from `steam_api64.dll` to `steam_api64_backup.dll`. This should fix the issue, but if it doesn't come to Discord.

### System.DllNotFoundException: Unable to load DLL '\steam\steamapps\common\tModLoader\Libraries\Native\Windows\FNA3D.dll' or one of its dependencies: The specified procedure could not be found. (0x8007007F)
This is caused by FNA3D.dll or SDL2.dll on the PATH.

### Multiple extensions for asset
This is caused by an incompatible resource pack. Currently some resource packs are incompatible with tModLoader. To fix this, open up `"D:\Documents\My Games\Terraria\tModLoader\config.json"` in a text editor, find the "ResourcePacks" section, then change all `true` to `false`, save the file. Now tModLoader should launch again.

### Screen is blurry or resolution resets to 1536x864
We are looking into a solution, but something you can do as a temporary solution is to change your display scale back to 100%. To do this, right click on your desktop and click `Display Settings`. Next, scroll down to `Scale and Layout` and find the 1st item labeled `Change the size of text, apps, and other items`, make it 100%, then launch the game.    
![](https://i.imgur.com/9WNLQYr.png)    

### System.Threading.SynchronizationLockException
![](https://i.imgur.com/IkPqCo6.png)    
Solution. Disable BitDefender or disable the Safe Files feature of Bit Defender. Some more info has been collected [here](https://www.bitdefender.com/consumer/support/answer/2700/).

### System.IO.IOException: Cannot create a file when that file already exists.
![](https://i.imgur.com/Wjv2nx2.png)    
Solution. Delete the logs folder. If it comes back, you can try renaming the `ModLoader` folder to ModLoaderOld or completely reinstalling Terraria.

### Enabling mods freezes the game/Setting controls doesn't work properly.
This is most likely related to your antivirus blocking access to the `ModLoader` folder in the Documents directory (`tModLoader` in 1.4). See the [below issue](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-FAQ#systemunauthorizedaccessexception-access-to-the-path-is-denied) for the process (but do this with the folder instead of the exe)

### Disk Write Error
If you try to install tModLoader through Steam and it gives you a message with "Disk Write Error" in it, it is usually caused by Avast. Disable it temporarily and install tModLoader.

### System.UnauthorizedAccessException: Access to the path is denied.  
![](https://i.imgur.com/ZjhIvNo.png)

This issue can be caused by your antivirus or windows security settings. If you're using Windows Security (formerly Windows Defender) and are getting this error, then you will need to add tModLoader.exe to your whitelist, for further instructions on how to do this continue reading below.

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
Scroll through the list until you find "tModLoader.exe", and click the + and then close, after this you're done! (If you cannot find "tModLoader.exe" on your list, then continue with the below steps)

![Extra Step 1](https://i.imgur.com/0ruiXoA.png)  
Back in the "Add allowed app" selection, left-click "Browse all apps"

![Extra Step 2](https://i.imgur.com/E7pnDZo.png)  
Navigate to wherever you installed your tModLoader (refer to video linked below on how to find an installation directory through Steam if you don't know how to do this) and double-click or select and left-click then open "tModloader.exe" this will add the file to your whitelist. Done!

[How to find a game install location on Steam](https://gfycat.com/SelfreliantAssuredIsabellineshrike)  
Use the process shown in the above linked video to find your tModLoader install location, the gif is showing how to do this for Terraria, but you will need to do the same process except with tModLoader on Steam (you must have it installed first).

# Load Mod
### "Failed to resolve assembly: 'Terraria, Version=1.3.5.1, Culture=neutral, PublicKeyToken=null'"
This seems to happen when users rename the tModLoader exe to something other than Terraria.exe. Rename it back to Terraria.exe and it should still work.

### "A Mod is crashing when I try to open tModLoader"
Open up the tModLoader save directory and delete the offending .tmod file: `%UserProfile%\Documents\My Games\Terraria\ModLoader\Mods` (Windows)

### Begin cannot be called again until End has been successfully called
(Also applies to "Cannot access a disposed object" errors)    
![](https://i.imgur.com/jzbyghT.png)    
This error is an error usually caused by an unhandled error in a mod. This makes it hard for users to know which mod is broken. Much of the time this error will only happen after reloading mods (improperly unloaded Texture2D references), but it can happen due to other errors (for example, dividing by zero.) As a user of mods, it can be hard to figure out which mod is causing the issue. To determine the broken mod, follow these steps: (If you are on a version prior to 0.11: In Settings->tModLoader Settings, make sure "Always log exceptions" is true. After that, reload mods. After that, enter the game and proceed to play until the error happens again. When this happens, open `%UserProfile%\Documents\My Games\Terraria\ModLoader\Logs\Logs.txt` and look at the file.) If you are on version 0.11+, open `%UserProfile%\Documents\My Games\Terraria\ModLoader\Logs\client.log` and look at the file. Scroll down to the very bottom and you should hopefully see this error as the last entry. Right above that is hopefully another error. Look for the name of any mod you are using in this 2nd to last error message. If you find one, that is likely the mod causing the error. If in doubt, ask in #support on the [tModLoader discord](http://www.discord.me/tModLoader). As a last resort, use the [flowchart](#flowchart).

### OutOfMemoryException
This error means that tModLoader does not have enough RAM to load all the mods that you are trying to load. Large mods that add lots of items are the main culprit. You may have to cut down on the number of large mods you are trying to load at the same time. You can also try loading Small or Medium worlds instead of Large. Another possibility is that you have other large programs running. If you can close them, do so. Press Ctrl+Shift+Escape to bring up the Task Manager. In the Task Manager's Processes tab, look for processes that take up a large amount of memory. Anything taking more than 100,000 K is a good candidate. Also make sure that you are on 64 bit Windows and that you actually have more than 4 GB of RAM.

Do note that there is also an alternative 64-bit version of TML that was made to solve this specific issue. Though any issue related to it must be reported to them and not the TML Team.

Technical Explanation: 
On Windows, Terraria is a 32 bit process, meaning it can only use up to 4GB of Ram. It also has a bit less than that due to the .Net framework overhead, limiting the amount of Ram it can use even further. Note that the so-called "4GB" patch is already applied, do not tell people to apply it, it does nothing. Also, if you are on 32 bit Windows, there isn't much you can do.

Viewing Mods that use a lot of Memory:
If you are curious which mods are using your limited Ram, you can enable the "Show Mod Memory Estimates" option in `Settings->tModLoader Settings`. After enabling the setting, you will have to exit to the main menu and then close and reopen the game for the setting to take effect. Visit the Mods menu, make sure you enable and reload the mods you are curious about, and you should now see colorful graph at the top that shows how much memory each mod is using. Use this information to disable mods that take too much ram compared to how much you enjoy the content.

![](https://i.imgur.com/3Pl6tG2.png)

# Players/Worlds
### "HELP, all my players and worlds are gone!"
tModLoader saves are kept separate from vanilla Terraria saves. You can copy back and forth between save locations, but be aware that you will lose Modded Tile and Items if you use tModloader worlds/characters in vanilla.
Solutions: Copy from `%UserProfile%\Documents\My Games\Terraria` to `%UserProfile%\Documents\My Games\Terraria\ModLoader` (Windows). For example, copy .wld files in `\Terraria\Worlds` to `\Terraria\ModLoader\Worlds`, and the same for the .plr files in Players folders. For players you'll also want to grab the folder with the same name as well, since those are the maps.

You may notice that your vanilla player or world isn't in that folder, or maybe only .bak files are in that folder. This means that you have put that player or world onto the cloud. There are 2 ways to get them into tModLoader. The first option is to open vanilla Terraria and simply click the "Move off cloud" button and then follow the above instructions. The second option is to copy the files from the local copy of cloud files steam keeps around and place them in their respective ModLoader folder (`tModLoader` in 1.4) (`%UserProfile%\Documents\My Games\Terraria\ModLoader`). These are located in: `C:\Program Files (x86)\Steam\userdata\[some number here]\105600\remote`

### "Cloud storage limit reached, unable to move to cloud"
tModLoader shares the cloud storage space with Terraria (about 150 MB of it). Exceeding this limit on Terraria + tModLoader combined will make you unable to move players and worlds to the cloud until sufficient storage is available. Easiest way is to "un-cloud" your vanilla Terraria worlds, they take up the most space. You can check how much storage is used in Steam: Right click "tModLoader" -> "Properties" -> "Updates", at the bottom it says the amount of available storage. In the rare case where you used tModLoader cloud storage feature before steam release (0.11.7), you won't be able to get rid of these "orphaned" files the normal way. Try this method: [Video, Terraria App ID is 105600](https://youtu.be/JADsIv2RUSw).

# Mod Browser
### "Mod Browser Offline", "I can't download mods"
![](https://i.imgur.com/JTtOMbq.png)

If you are on Mac or Linux, the Mod Browser doesn't work yet.  
Otherwise, the Mod Browser is out of bandwidth. Visit Mod Homepages for alternate download. DO NOT post in the forums asking when the Mod Browser will be back, it's annoying and useless.
Most Mods are found here: [Terraria Community Forums](https://forums.terraria.org/index.php?forums/client-server-mods-tools.116/)
Web-based Mod Browser for Mac/Linux users: [Mod Browser](http://javid.ddns.net/tModLoader/DirectModDownloadListing.php)

# Flowchart
Sometimes a mod is causing issues, but you can't tell which mod is the problem. Use this flowchart to diagnose and determine the bad mod:    
![](https://cdn.discordapp.com/attachments/466247288331829249/481464717043564554/Untitled_Diagram1.png)

# Reading client.log
## Simple Explanation
Most bugs in tModLoader will appear as errors logged to the `client.log` file. Read the file to find mods that are potentially causing bugs.
* Close tModLoader, then reopen tModLoader and repeat the steps necessary to trigger the bug.
* Open the `client.log` file in a text editor, such as notepad. The `client.log` file is found at `%UserProfile%\Documents\My Games\Terraria\ModLoader\Logs\client.log` (or `[tModLoaderInstallFolder]\tModLoader-Logs\client.log` for the 1.4 alpha. To get to your install directory on Steam - right click tModLoader in the library, then hover over Manage and click on Browse local files.)
  * To easily open the folder copy `%UserProfile%\Documents\My Games\Terraria\ModLoader\Logs\` (1.3 only) to the clipboard with `ctrl-c` and paste it into the address bar in the file explorer with `ctrl-v`. Press enter and the file explorer will go to the folder.    
![](https://i.imgur.com/6jtjyVC.png)    
  * If a window pops up asking "How do you want to open this .log file?", find Notepad in the list, click it, click OK.    
![](https://i.imgur.com/Q7HPPLK.png)    
* Scroll down until you see the first exception in the file. It should look like the image below. Look for indentation followed by the word "at", these lines are the details of the exception. Each exception like this in the log is caused by a bug.
![](https://i.imgur.com/5rA3yR6.png)    
* Look through the exception to find the names of mods you have enabled. The first word after "at" on the lines and before the period is where the names of mods involved in the exception will be shown. Words like "Terraria", "Microsoft", and "MonoMod" can usually be ignored. In this example, the mod named "GadgetGalore" in the exception and is likely a broken mod.
  * If there are multiple mod names in a single stack trace, either mod could be at fault, or both, you'll have to do some testing.
* After finding a mod in an exception, disable it the next time you launch tModLoader and see if the same bug triggers. Repeat the above steps if there are additional mods that are causing bugs.
* If you need help reading your client.log, you can ask for help in the support channels on the [tModLoader Discord chat](https://tmodloader.net/discord).

## Complicated Explanation
Many errors you might experience while playing tModLoader can be identified by reading the `client.log` file. (If you are experiencing an error that only happens in multiplayer, you'll need to read both `client.log` and `server.log`). By properly reading the log file, you can identify if an issue is caused by a bug in tModLoader or a bug in a mod you are using. If you are on the 1.4 alpha, a mod that worked yesterday might be broken today. In any case, the logs are a good place to find the issue.

If you are experiencing a bug, first make sure all mods are up to date, then close tModLoader and reopen it. This will reset the log and make it easier to read. Once you experience the bug, close tModLoader and open the log file. The log file is found in `%UserProfile%\Documents\My Games\Terraria\ModLoader\Logs\client.log` (or `%UserProfile%\Documents\My Games\Terraria\tModLoader\Logs\client.log` for the 1.4 alpha). Open the file in a normal text editor like notepad if it doesn't have a file association already. 

Next, you'll want to look through the file looking for words like "error" or "exception". Using `ctrl-f` to use the Find feature of your text editor. You'll typically want to focus on the earliest error you can find in the log file. Errors that come after earlier errors might be caused by those earlier errors, so fixing the earlier errors might fix the later errors. Once you find the word "error" or "exception", you should see something similar to this:    
![](https://i.imgur.com/5rA3yR6.png)     
In the above excerpt from `client.log`, we can see an error is being logged. The error begins at line 41, with a brief summary on line 42 shown in blue. After the error summary, there will be a "stack trace" that shows where in the code the error is happening. Take note of the indentation of the lines of the stack trace, shown in green. This indentation is very useful for quickly finding errors logged in the log file rather than using `crtl-f`. Next, you'll want to look through the stack trace for the names of mods you have enabled. The first word after "at" on the lines and before the period is the key word you'll want to look at. Words like "Terraria", "Microsoft", and "MonoMod" can usually be ignored, you want to find the mod or mods that show in the error. In this example, we see a mod named "GadgetGalore" in the stack trace and can make the assumption that it is the cause of the error. If there are multiple mod names in a single stack trace, either mod could be at fault, or both, you'll have to do some testing. Now that we found a mod that is erroring, you can disable the mod and try again. Not every error logged in the logs will an actual bug, so some experimentation may be required. If you need help reading your client.log, you can ask for help in the support channels on the [tModLoader Discord chat](https://tmodloader.net/discord).

# Old Issues
### The following files were missing and could not be installed:.....
![](http://i.imgur.com/TSKfacG.png)  
Solution: You didn't unzip the files prior to installation. Don't know how to unzip, or don't have software for it? -> Try [7-Zip](https://www.7-zip.org). It's free and open source, like tModLoader.

### System.DllNotFoundException: Unable to load DLL'CSteamworks'.....
![](http://i.imgur.com/ZbbskuQ.png)  
Solution: You didn't run the installer, or you are trying to run tModLoader from somewhere other than the Steam Terraria Install directory. Or, you have the GOG version and you downloaded the wrong file. If you pirated Terraria, you can't use tModLoader.

### System.EntryPointNotFoundException: Unable to find an entry point named 'Init' in DLL 'CSteamworks'.
![](https://i.imgur.com/lp7yQQj.png)  
Solution: You tried to launch tModLoader 0.10+ with Terraria 1.3.4.4 or earlier files. Please use Steam to Verify game integrity (so that your Terraria is updated to 1.3.5+) and then reinstall the latest tModLoader.
[Video of how to verify game integrity](https://gfycat.com/PlaintiveFickleCamel)

### System.EntryPointNotFoundException: Unable to find an entry point named 'InitSafe' in DLL 'CSteamworks'.
![](https://i.imgur.com/rZoXMGi.png)  
Solution: Same as above. (Don't try to install other things such as other stand alone mods.)

### Host and Play: The system cannot find the file specified
![](https://i.imgur.com/Vm2b78X.png)    
Simply put, you didn't follow the install instructions. Check the install directory, usually "C:\Program Files (x86)\Steam\steamapps\common\Terraria" (NOT "C:\Documents\My Games\Terraria\ModLoader"!), and make sure that tModLoaderServer.exe has been copied into the folder. If not, take the file from the tModLoader zip file and place it there.