___

**[I don't just want to play mods, I want to create them](tModLoader-guide-for-developers)**

___

**[I don't just want to play mods, I want to contribute to tModLoader](tModLoader-guide-for-contributors)**

___

## Steam Installation
To install [tModLoader](https://store.steampowered.com/app/1281930/tModLoader) on Steam, you need to own [Terraria](https://store.steampowered.com/app/105600/Terraria) on Steam.
Simply browse for tModLoader on Steam and install it.
The tModLoader installation will exist alongside the vanilla installation, allowing you to play both vanilla and modded without the hassle of reinstalling vanilla.

**How to uninstall?** <br>
Simply right-click tModLoader -> Manage -> Uninstall.
Your vanilla install will be unharmed.
___

## Manual Installation
___
This installation is suitable for all players, including as non-steam players.

Installing tModLoader is relatively easy. If you want to ensure you can easily revert back to vanilla, you should make a backup copy of your Terraria.exe and TerrariaServer.exe, however if you use the installer jar it will create these backups for you (Windows only, Mac and Linux don't need backups as we don't replace the files).

1. Goto the **[releases](https://github.com/tModLoader/tModLoader/releases)** page and download the tML release you want. (usually the **[latest](https://github.com/tModLoader/tModLoader/releases/latest)**)
2. Unblock the .zip file _if needed_ ([See how to](https://thirtysix.zendesk.com/hc/en-us/articles/202921675-How-to-Unblock-a-File-Downloaded-from-an-Email-or-the-Internet))
3. Unzip the contents somewhere (usually documents or downloads folder)
4. You can install using one of two options, pick one:
    * **Option 1, manual install (Recommended)**: Open the extracted folder, **copy** the contents to your Terraria folder and let it overwrite files when asked. (replace files)
    * **Option 2, installer**: Run the installer .jar file (requires Java 8 or higher) (_Recommended to have launched vanilla Terraria at least once for the best install experience_)
5. Done. You can launch Terraria as usual.

Tip: Here is an easy way to find where your Terraria files are located:

1. Locate Terraria in your Steam game library, right click it and click 'Properties'
2. Browse to the 'Local Files' tab and click on the 'Browse local files...' button
3. You are now in your Terraria folder (this is where you should install tModLoader)

**How to uninstall?** <br>
Uninstallation of tModLoader is even easier than installation.

1. Open Steam, go to your game library section and locate Terraria.
2. Let Steam **[verify the integrity of game files](https://support.steampowered.com/kb_article.php?ref=2037-QEUH-3335)** for Terraria, this will reconfigure your game files to run vanilla.
4. Done. You can launch Terraria as usual.

If you use GOG, uninstall and reinstall. Your worlds and players will be saved.

## HELP! I've lost my Players / Worlds when I installed tModLoader, what do I do?
___
**tModLoader DOES NOT use your vanilla world and player files.** <br>
The in-game should give you the option to _copy over_ your original vanilla files.<br>
You do not have to worry about your vanilla saves being modified; they will be copied for modded gameplay use. When you go back to vanilla, you will see your original saves.<br>

tModLoader uses separate folders to store player (.plr) and world (.wld) files, mainly because it will store additional data for them. Your vanilla players and worlds will be stored in: `%UserProfile%\Documents\My Games\Terraria` (for Windows) in the _Players_ and _Worlds_ folders respectively.

If the automatic copy doesn't work, copy the "World" and "Player" folders from "%UserProfile%\Documents\My Games\Terraria" to "%UserProfile%\Documents\My Games\Terraria\tModLoader".

## How do I download and play mods?
tModLoader comes with a mod browser. Refer to the [mod browser guide](Mod-Browser) to learn how to download and play mods. If you use the official Steam installation, you may get access to Steam Workshop mods, **but as of yet there is no Steam Workshop support. This will come at a later time.**