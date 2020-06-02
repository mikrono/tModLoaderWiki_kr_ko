

___
I don't want to just play mods, [I want to make them](https://github.com/tModLoader/tModLoader/wiki/tModLoader-guide-for-developers)

I don't want to just play mods, [I want to contribute to tModLoader](https://github.com/tModLoader/tModLoader/wiki/tModLoader-guide-for-contributors)
___

## Steam Installation
To install [tModLoader](https://store.steampowered.com/app/1281930/tModLoader) on Steam, you need to own [Terraria](https://store.steampowered.com/app/105600/Terraria) on Steam.
Simply browse for [tModLoader](https://store.steampowered.com/app/1281930/tModLoader/) on Steam and install it.
The tModLoader installation will exist alongside the vanilla installation, allowing you to play both vanilla and modded without the hassle of reinstalling vanilla.

### How to uninstall?
Simply right-click tModLoader -> Manage -> Uninstall.
Your vanilla install will be unharmed.

## Manual Installation
This installation is necessary for players who have purchased Terraria from GOG.

Installing tModLoader is relatively easy.

1. Goto the **[releases](https://github.com/tModLoader/tModLoader/releases)** page and download the tML release you want. (usually the **[latest](https://github.com/tModLoader/tModLoader/releases/latest)**)
2. Unzip the **contents** of the zip you downloaded to a folder named `tModLoader` either next to or nested inside the Terraria install folder. (GOG usually installs to `C:\GOG Games`, and Steam to `C:\Program Files (x86)\Steam\steamapps\common\Terraria`. See [this video](https://gfycat.com/SelfreliantAssuredIsabellineshrike) to find the steam installation location if you customized it.) If you don't know how to unzip a zip file, get someone who knows how to use a computer to help you.
    * **Option 1, side-by-side (Recommended)**:    
![](https://i.imgur.com/gmrBMSO.png)    
    * **Option 2, nested**:    
![](https://i.imgur.com/YWaqZPO.png)    
    * **DO NOT** install the tModLoader files directly into the Terraria folder.
3. Remove or Add the Steam files depending on which version of the game you own:
    1. If you are using the GOG version of Terraria, delete the Steam file from the folder you just extracted tModLoader into (These files might already be deleted from the zip you download):
        * Windows: steam_api.dll
        * Linux: lib/libsteam_api.so and lib64/libsteam_api.so
        * Mac: tModLoader.app/Contents/MacOS/osx/libsteam_api.dylib
    2. If you are using the Steam version of Terraria, if the Steam files are missing from the zip, copy them from your Terraria install to the tModLoader install:
        * Windows: steam_api.dll and CSteamworks.dll
        * Linux: lib/libsteam_api.so, lib/libCSteamworks.so, lib64/libsteam_api.so, and lib64/libCSteamworks.so
        * Mac: tModLoader.app/Contents/MacOS/osx/libsteam_api.dylib and tModLoader.app/Contents/MacOS/osx/CSteamworks
        * We're not positive, but you might also need to make a steam_appid.txt file in the install directory and put the text `1281930` in it and save the file.
5. Done. You can now make a desktop shortcut for tModLoader and launch tModLoader from that.

Tip: Here is an easy way to find where your Terraria files are located:

1. Locate Terraria in your Steam game library, right click it and click 'Properties'
2. Browse to the 'Local Files' tab and click on the 'Browse local files...' button
3. You are now in your Terraria folder (this is where you should install tModLoader)

### How to uninstall?

1. Open Steam, go to your game library section and locate Terraria.
2. Let Steam **[verify the integrity of game files](https://support.steampowered.com/kb_article.php?ref=2037-QEUH-3335)** for Terraria, this will reconfigure your game files to run vanilla.
4. Done. You can launch Terraria as usual.

If you use GOG, simply delete the tModLoader folder you made before. Your worlds and players will be saved.

## My worlds and characters disappeared when I installed!
**tModLoader DOES NOT use your vanilla world and player files.**
The in-game should give you the option to _copy over_ your original vanilla files. This option won't work for players and worlds you have used in Terraria 1.4.
You do not have to worry about your vanilla saves being modified; they will be copied for modded gameplay use. When you go back to vanilla, you will see your original saves.

tModLoader uses separate folders to store player (.plr) and world (.wld) files, mainly because it will store additional data for them. Your vanilla players and worlds will be stored in: `%UserProfile%\Documents\My Games\Terraria` (for Windows) in the _Players_ and _Worlds_ folders respectively.

If the automatic copy doesn't work, copy the "World" and "Player" folders from `%UserProfile%\Documents\My Games\Terraria` to `%UserProfile%\Documents\My Games\Terraria\tModLoader`.

## How do I download and play mods?
tModLoader comes with a mod browser. Refer to the [mod browser guide](Mod-Browser) to learn how to download and play mods.

## I have too little memory to run multiple mods!
Terraria and tModLoader are 32-bit applications. In short, this means they are only capable of utilizing up to ~4 GiB of RAM. With a lot of mods, you may run out of memory. The solution is using less mods, unfortunately. Alternatively, you can try using the unofficial 64-bit version of tModLoader.

## I use macOS Catalina. What do I do?
If you experience any problems, try using the 64-bit version of tModLoader or talk to us on Discord.