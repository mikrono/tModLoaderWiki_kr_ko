___

**[I don't just want to play mods, I want to create them](tModLoader-guide-for-developers)**

___

**[I don't just want to play mods, I want to contribute to tModLoader](tModLoader-guide-for-contributors)**

___

## Installation
___
Installing tModLoader is relatively easy. If you want to ensure you can easily revert back to vanilla, you should make a backup copy of your Terraria.exe and TerrariaServer.exe, however if you use the installer jar it will create these backups for you (Windows only, Mac and Linux don't need backups as we don't replace the files).

1. Goto the **[releases](https://github.com/tModLoader/tModLoader/releases)** page and download the tML release you want. (usually the **[latest](https://github.com/tModLoader/tModLoader/releases/latest)**)
2. Unblock the .zip file _if needed_ ([See how to](https://thirtysix.zendesk.com/hc/en-us/articles/202921675-How-to-Unblock-a-File-Downloaded-from-an-Email-or-the-Internet))
3. Unzip the contents somewhere (usually documents or downloads folder)
4. You can install using one of two options, pick one:
    * **Option 1, manual install (Recommended)**: Open the extracted folder, **copy** the contents to your Terraria folder and let it overwrite files when asked. (replace files)
    * **Option 2, installer**: Run the installer .jar file (requires Java 8 or higher) (_Recommended to have launched vanilla Terraria at least once for the best install experience_)
    * **Option 3, Steam**: Download [Terraria](https://store.steampowered.com/app/105600/Terraria) through Steam, then install [tModLoader](https://store.steampowered.com/app/1281930/tModLoader].
5. Done. You can launch Terraria as usual. If you used Option 3, choose tModLoader in your Steam library.

Tip: Here is an easy way to find where your Terraria files are located:

1. Locate Terraria in your Steam game library, right click it and click 'Properties'
2. Browse to the 'Local Files' tab and click on the 'Browse local files...' button
3. You are now in your Terraria folder (this is where you should install tModLoader)

## Uninstallation (Standard)
___
Uninstallation of tModLoader is even easier than installation. This part covers how to do it when using Steam.

1. Open Steam, go to your game library section and locate Terraria.
2. Let Steam **[verify the integrity of game files](https://support.steampowered.com/kb_article.php?ref=2037-QEUH-3335)** for Terraria, this will reconfigure your game files to run vanilla.
4. Done. You can launch Terraria as usual.

## Uninstallation (Steam Version)
__
Uninstallation of tModLoader is even easier if you downloaded it from Steam. All you have to do is:

1. Open Steam, go to your game library section and locate tModLoader.
2. Uninstall tModloader.
3. Done.

## HELP! I've lost my Players / Worlds when I installed tModLoader, what do I do?
___
tModLoader uses separate folders to store player (.plr) and world (.wld) files, mainly because it will store additional data for them. Your vanilla players and worlds will be stored in: `%UserProfile%\Documents\My Games\Terraria` (for Windows) in the _Players_ and _Worlds_ folders respectively. (If you are using the cloud, you will need to get them from there first or they won't be shown. Do this by opening your vanilla client and moving them off the cloud first.) You can select, then copy and paste these folders into the ModLoader directory to migrate your players and worlds to tModLoader. The ModLoader directory is used by tModLoader to store all kinds of files, such as your installed mods. **Please note** that once you play tModLoader, it will create .tplr and .twld files as well as backup files (.bak) for your player and world files. To revert to a backup, simply remove the .plr or .wld file, then rename the .bak file to not include the .bak extension. (If your backup looks like this: playername.plr.bak, then it should be renamed to: playername.plr) If your explorer view is not showing file extensions, you can enable this by opening your 'View' menu and checking the 'File name extensions' checkbox. ([See example](https://i.imgur.com/CzP5yMA.png)) If you can not see this menu, you may need to unfold it by [clicking the down arrow](https://i.imgur.com/O8LqfGz.png) in the top right corner of your explorer view.

## How do I download and play mods?
tModLoader comes with a mod browser. Refer to the [mod browser guide](Mod-Browser) to learn how to download and play mods.