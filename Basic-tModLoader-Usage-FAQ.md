# Note on Piracy
If you pirated Terraria, we can't help you. tModLoader won't work. Please don't bother us by asking how to get it to work.

# Installation
### The following files were missing and could not be installed:.....
![](http://i.imgur.com/TSKfacG.png)  
Solution: You didn't unzip the files prior to installation. Don't know how to unzip? -> [Google](http://www.google.com)

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

### System.Threading.SynchronizationLockException
![](https://i.imgur.com/IkPqCo6.png)    
Solution. Disable BitDefender or disable the Safe Files feature of Bit Defender.

### System.IO.IOException: Cannot create a file when that file already exists.
![](https://i.imgur.com/Wjv2nx2.png)    
Solution. Delete the logs folder. If it comes back, you can try renaming the ModLoader folder to ModLoaderOld or completely reinstalling Terraria.

### Host and Play: The system cannot find the file specified
![](https://i.imgur.com/Vm2b78X.png)    
Simply put, you didn't follow the install instructions. Check the install directory, usually "C:\Program Files (x86)\Steam\steamapps\common\Terraria" (NOT "C:\Documents\My Games\Terraria\ModLoader"!), and make sure that tModLoaderServer.exe has been copied into the folder. If not, take the file from the tModLoader zip file and place it there.

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

Do note that there is also an alternative version of tmodloader that was made to solve this specific issue made by Dradonhunter11 and Rartrin which you can get there. Tough any issue related to it must be reported to them and not us.
Forum page link: https://forums.terraria.org/index.php?threads/1-3-tmodloader-fna-32bit-64bit-branch-of-tml.75644/

Technical Explanation: 
On Windows, Terraria is a 32 bit process, meaning it can only use up to 4GB of Ram. It also has a bit less than that due to the .Net framework overhead, limiting the amount of Ram it can use even further. Note that the so-called "4GB" patch is already applied, do not tell people to apply it, it does nothing. Also, if you are on 32 bit Windows, there isn't much you can do.

# Players/Worlds
### "HELP, all my players and worlds are gone!"
tModLoader saves are kept separate from vanilla Terraria saves. You can copy back and forth between save locations, but be aware that you will lose Modded Tile and Items if you use tModloader worlds/characters in vanilla.
Solutions: Copy from `%UserProfile%\Documents\My Games\Terraria` to `%UserProfile%\Documents\My Games\Terraria\ModLoader` (Windows). For example, copy .wld files in `\Terraria\Worlds` to `\Terraria\ModLoader\Worlds`, and the same for the .plr files in Players folders. For players you'll also want to grab the folder with the same name as well, since those are the maps.

You may notice that your vanilla player or world isn't in that folder, or maybe only .bak files are in that folder. This means that you have put that player or world onto the cloud. There are 2 ways to get them into tModLoader. The first option is to open vanilla Terraria and simply click the "Move off cloud" button and then follow the above instructions. The second option is to copy the files from the local copy of cloud files steam keeps around and place them in their respective ModLoader folder (`%UserProfile%\Documents\My Games\Terraria\ModLoader`). These are located in: `C:\Program Files (x86)\Steam\userdata\[some number here]\105600\remote`

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