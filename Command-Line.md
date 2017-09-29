In addition to all the Command Line and Server Config settings available in [vanilla Terraria](https://terraria.gamepedia.com/Server), tModLoader adds a few which are listed below.

### modpack
Server and Client Command Line Argument, Server Config.  
Specifies a modpack to use by name. Omit the .json extension. The mods specified in the modpack (and only them) will be loaded. Useful for quickly launching a specified set of mods for people who might be doing multiple concurrent play-throughs with different sets of mods.

### modpath
Server Command Line Argument, Server Config.  
Specifies the folder to look for .tmod files. Useful for hosted servers as typically the default folder location is no accessable on dedicated hosts.

### skipselect
Client Command Line Argument  
Useful for modders who wish to quickly test their mod. This flag will automatically select the 1st player and world in the list, allowing the modder to launch directly into the game. Very useful when building and launching from Visual Studio.

# Usage
For dedicated server configs, follow the example shown in the serverconfig.txt file in the Terraria install directory. (for example, "modpath=/mymods/") 

Otherwise, use either a script (.bat on windows, .sh on linux/mac) to specify a command line argument. (for example "Terraria.exe -modpack=friendsplaythrough")