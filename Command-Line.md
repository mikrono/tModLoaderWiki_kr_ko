In addition to all the Command Line and Server Config settings available in [vanilla Terraria](https://terraria.gamepedia.com/Server), tModLoader adds a few which are listed below.

### savedirectory [pathToSaveDirectoryFolder]
Server and Client Command Line Argument    
Specifies the save directory. The save directory will usually be `%UserProfile%\Documents\My Games\Terraria\ModLoader​` on Windows, `/Users/account/Library/Application Support/Terraria/ModLoader` on Mac, and `~/.local/share/Terraria/ModLoader` OR `$XDG_DATA_HOME/Terraria/ModLoader` on Linux. The directory `/ModLoader` will automatically be appended to the provided path. Use this argument to specify a different save directory. The save directory is the parent directory of the worlds directory and mods directory, so you can use only this parameter rather than both of those.

### tmlsavedirectory [pathToSaveDirectoryFolder]
Server and Client Command Line Argument    
Same as savedirectory above except `/ModLoader` won't automatically be appended to the provided path.

### modpack [modpackname]
Server and Client Command Line Argument, Server Config.  
Specifies a modpack to use by name. Omit the .json extension. The mods specified in the modpack (and only them) will be loaded. Useful for quickly launching a specified set of mods for people who might be doing multiple concurrent play-throughs with different sets of mods.

### modpath [pathToModsFolder]
Server Command Line Argument, Server Config.  
Specifies the folder to look for .tmod files. Useful for hosted servers as typically the default folder location is no accessable on dedicated hosts.

### skipselect
Client Command Line Argument  
Useful for modders who wish to quickly test their mod. This flag will automatically select the 1st player and world in the list, allowing the modder to launch directly into the game. Very useful when building and launching from Visual Studio.

### build [pathToModSource]
Server Client Command Line Argument  
Builds the mod specified by the folder from source, useful for modders.

### eac
Server Client Command Line Argument (Windows Only)  
Enables Edit and Continue, useful for modders.

# Usage
For dedicated server configs, follow the example shown in the serverconfig.txt file in the Terraria install directory. (for example, "modpath=/mymods/". Basically, [settingname]=[settingvalue]) 

Otherwise, use either a script (.bat on windows, .sh on linux/mac) to specify a command line argument. (for example "Terraria.exe -modpack friendsplaythrough") Windows users can also add parameters to shortcut files by adding to the end of "Target". 

# Examples
## Windows Shortcut
Make a shortcut to the tModLoader exe file by either copy and then paste shortcut or by right click dragging the file to another place and selecting "create shortcuts here". Next, right click properties and edit the Target appropriately.
### Launch Windows Client with a specific Mod Pack
See picture:
![](https://i.imgur.com/N8FM1ba.png)
## Dedicated Host
Edit the server config file that the dedicated host is using. Not every host is the same. You'll need to make sure that tModLoaderServer is being launched with either the command line argument or the server config line for the path to Mods folder. If you wish to use a Mods folder in the same folder as the tModLoaderServer executable, I believe you can use a relative path like "modpath=./Mods". You'll also need to set savedirectory since some mods store mod specific data in the save directory. (savedirectory is a command line only parameter.) Add `-savedirectory ./` to the command that launches the server, which can be found usually on the website for your host.

### Dedicated Hosts known to work
NodeCraft - Follow the [instructions](https://nodecraft.com/support/games/terraria/tmodloader-server-guide)
GameServers - Add `-savedirectory ./` to server command line

## Visual Studio
### Build/Edit/Test mods
Setting the Post Build event to `"C:\Program Files (x86)\Steam\steamapps\common\Terraria\tModLoaderServer.exe" -build "$(ProjectDir)\" -eac "$(TargetPath)"` will allow you to easily build and run your mod in Edit and Continue mode, facilitating efficient tweaking, modding, and testing. Add -skipselect so you don't have to go through the menus for an even easier time. See [Developing with Visual Studio](https://github.com/blushiemagic/tModLoader/wiki/Developing-with-Visual-Studio) for more information.
