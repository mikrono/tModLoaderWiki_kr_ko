In addition to all the Command Line and Server Config settings available in [vanilla Terraria](https://terraria.gamepedia.com/Server), tModLoader adds a few which are listed below.

### modpack
Server and Client.
Specifies a modpack to use by name. Omit the .json extension. The mods specified in the modpack (and only them) will be loaded. Useful for quickly launching a specified set of mods for people who might be doing multiple concurrent play-throughs with different sets of mods.

### modpath
Server only.
Specifies the folder to look for .tmod files. Useful for hosted servers as typically the default folder location is no accessable on dedicated hosts.

# Usage
For dedicated server configs, follow the example shown in the serverconfig.txt file in the Terraria install directory. (for example, "modpath=/mymods/") 

Otherwise, use either a script (.bat on windows, .sh on linux/mac) to specify a command line argument. (for example "Terraria.exe -modpack=friendsplaythrough")