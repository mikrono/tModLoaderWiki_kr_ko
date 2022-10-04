- **Learn the basics of using tModLoader for both players and modders**
- If you run into problems, see the [Usage FAQ](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-FAQ)  

# Installing tModLoader
[Read our install instructions](tModLoader-guide-for-players)

# Installing Mods
- Mods are files with the ".tmod" extension. They go in the Mods directory.
  - Windows: `%UserProfile%\Documents\My Games\Terraria\ModLoader\Mods`
  - Linux: `~/.local/share/Terraria/ModLoader/Mods` OR `$XDG_DATA_HOME/Terraria`
  - Mac: `/Users/account/Library/Application Support/Terraria`
- **Most mods are hosted in the "Mod Browser". You can easily browse mods and use the download button to download mods. Downloaded mods aren't enabled by default.**
- If you use the Steam version, you can also get mods from the Steam Workshop **when it is available.**
- You can also find mods in other places and add them to the Mods folder, this is useful when the Mod Browser is down or the mod is too big for the Mod Browser.  

# Enabling Mods
Use the Mods menu to set the mods you wish to use, then click reload.

# Playing with non-tModLoader users
**tModloader will not connect to non-tModLoader Terraria servers.**
If you want to play together, you must either revert to vanilla, or your friends must install tModLoader.

You can learn more about running a modded server [here](Starting-a-modded-server).

# World and Player Backups
Modding games is inherently prone to bugs, and bugs could cause your world or player to be unplayable. Because of this, tModLoader maintains multiple backups of your world and player files. The player backups are in `Terraria\ModLoader\Players\Backups` and the world backups are in `Terraria\ModLoader\Worlds\Backups`. If you find that you can't load a world or player anymore, you can roll back to a previous save by restoring the files from one of these backups. You'll lose some progress since the last save, but it is better than losing everything.

## Restoring a Backup
In the backup folder, you'll find many zip files. The files are named with the date of the backup followed by the name of the player or world. These instructions will show the process for restoring a world backup, but the process is exactly the same for a player backup, except for folder and file names. For example, if your `Terraria\ModLoader\Worlds\Backups` folder has the files `2021-02-21-CoolTown.zip` and `2021-02-19-CoolTown.zip`, then you have 2 backups of the world "CoolTown", one from February 21st, and one from February 19th. If the current "CoolTown" isn't loading in tModLoader, you can attempt to restore backups in reverse chronological order until it finally works. First, let's make a backup of the current world files that aren't working. You want to do this since the issue could be a broken mod you are using, and you'll want to be able to restore to the latest save if that is the case. Find `WorldName.twld` and `WorldName.wld`, copy and paste them to a safe location, something like `Terraria\ModLoader\TempWorldBackup\` should be fine. Make sure you have "File Name Extensions" visible, so you are sure you are backup up the `.twld` and `.wld` files instead of the `.twld.bak` and `.wld.bak` files. If you can't find these files, you might have the world on the cloud, if so, move the world off the cloud first, then follow the steps.     
![](https://i.imgur.com/4m69DnM.png)    
Now that you have a backup of the latest save, open the newest .zip file for the world backup you wish to restore from the `Terraria\ModLoader\Worlds\Backups` folder and copy the files to the clipboard. Navigate back to the Worlds folder and paste. Make sure to say "replace files in the destination" when asked.
![](https://i.imgur.com/kesLiid.png)    
![](https://i.imgur.com/8QSH6sF.png)    
Now, open tModLoader and attempt to load the World. If it succeeds, great, if it fails, try again with the 2nd most recent backup. You can use cheat mods like Cheat Sheet or Heros Mod to restore lost items, if you wish. If you can't get backups to load, your issue is likely a broken mod. You can look at `Terraria\ModLoader\Logs\client.log` for hints as to which mod is erroring, or come to the Discord chat for help.

# Paths
Here are some useful default paths:

## Install
In Steam right click on `tModLoader` in the library, then hover over `Manage` and click on `Browse local files`. The folder that opens is the install folder. [Video example](https://giant.gfycat.com/SoulfulImpoliteBigmouthbass.mp4)

If you use GOG, the game is installed wherever you installed it, most likely `"C:\GOG Games\tModLoader"`. 

### Default Install Locations
Windows Install: `C:\Program Files (x86)\Steam\steamapps\common\tModLoader`    
Linux Install: `~/.local/share/Steam/steamapps/common/tModLoader` or `~/.steam/steam/steamapps/common/tModLoader`    
Mac Install: `Library/Application Support/Steam/steamapps/common/tModLoader/tModLoader.app/Contents/MacOS`    

## Saves
Windows Saves: `%UserProfile%\Documents\My Games\Terraria\tModLoader` (This is typically found in `C:\Documents\`)    
Linux Saves: `~/.local/share/Terraria/tModLoader/` or `$XDG_DATA_HOME/Terraria/tModLoader/`    
Mac Saves: `~/Library/Application support/Terraria/tModLoader/`    

## Mods
Your manually installed mods are stored in a subfolder of the [Saves](#saves) folder called `/Mods`. Workshop mods are stored in `C:\Program Files (x86)\Steam\steamapps\workshop\content\1281930`, but you shouldn't touch the folder and instead unsubscribe in steam.

## Logs
The Logs are found in a subfolder of the [Install](#install) folder called `/tModLoader-Logs`. If you are sharing logs for support with game-breaking issues, please post all log files. Just `client.log` and/or `server.log` (if relevant) is sufficient if the issue happens in-game. 