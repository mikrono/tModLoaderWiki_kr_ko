# Installation
### The following files were missing and could not be installed:.....
![](http://i.imgur.com/TSKfacG.png)  
Solution: You didn't unzip the files prior to installation. Don't know how to unzip? -> [Google](http://www.google.com)

### System.DllNotFoundException: Unable to load DLL'CSteamworks'.....
![](http://i.imgur.com/ZbbskuQ.png)  
Solution: You didn't run the installer, or you are trying to run tModLoader from somewhere other than the Steam Terraria Install directory. Or, you have the GOG version and you downloaded the wrong file.

### System.EntryPointNotFoundException: Unable to find an entry point named 'Init' in DLL 'CSteamworks'.
![](https://i.imgur.com/lp7yQQj.png)  
Solution: You tried to launch tModLoader 0.10+ with Terraria 1.3.4.4 or earlier files. Please use Steam to Verify game integrity (so that your Terraria is updated to 1.3.5+) and then reinstall the latest tModLoader.
[Video of how to verify game integrity](https://gfycat.com/PlaintiveFickleCamel)

# Load Mod
### "Failed to resolve assembly: 'Terraria, Version=1.3.5.1, Culture=neutral, PublicKeyToken=null'"
This seems to happen when users rename the tModLoader exe to something other than Terraria.exe. Rename it back to Terraria.exe and it should still work.

### "A Mod is crashing when I try to open tModLoader"
Open up the tModLoader save directory and delete the offending .tmod file: `%UserProfile%\Documents\My Games\Terraria\ModLoader\Mods` (Windows)

### OutOfMemoryException
This error means that tModLoader does not have enough RAM to load all the mods that you are trying to load. Large mods that add lots of items are the main culprit. You may have to cut down on the number of large mods you are trying to load at the same time. You can also try loading Small or Medium worlds instead of Large. Another possibility is that you have other large 32 bit programs running. If you can close them, do so. Press Ctrl+Shift+Escape to bring up the Task Manager. In the Task Manager's Processes tab, look for processes that take up a large amount of memory that also have "*32" at the end of their name. Anything taking more than 100,000 K is a good candidate. Also make sure that you are on 64 bit Windows and that you actually have more than 4 GB of RAM.

Technical Explanation: 
On Windows, Terraria is a 32 bit process, meaning it has to share up to 4GB of Ram with all other 32 bit programs that are running. Since this memory is shared, any way you can free up memory helps. Note that the so-called "4GB" patch is already applied, do not tell people to apply it, it does nothing. Also, if you are on 32 bit Windows, there isn't much you can do.

# Players/Worlds
### "HELP, all my players and worlds are gone!"
tModLoader saves are kept separate from vanilla Terraria saves. You can copy back and forth between save locations, but be aware that you will lose Modded Tile and Items if you use tModloader worlds/characters in vanilla.
Solutions: Copy from `%UserProfile%\Documents\My Games\Terraria` to `%UserProfile%\Documents\My Games\Terraria\ModLoader` (Windows)

# Mod Browser
### "Mod Browser Offline", "I can't download mods"
![](https://i.imgur.com/JTtOMbq.png)

If you are on Mac or Linux, the Mod Browser doesn't work yet.  
Otherwise, the Mod Browser is out of bandwidth. Visit Mod Homepages for alternate download. DO NOT post in the forums asking when the Mod Browser will be back, it's annoying and useless.
Most Mods are found here: [Terraria Community Forums](https://forums.terraria.org/index.php?forums/client-server-mods-tools.116/)
Web-based Mod Browser for Mac/Linux users: [Mod Browser](http://javid.ddns.net/tModLoader/DirectModDownloadListing.php)