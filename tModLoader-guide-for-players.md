

___
I don't want to just play mods, [I want to make them](https://github.com/tModLoader/tModLoader/wiki/tModLoader-guide-for-developers)

I don't want to just play mods, [I want to contribute to tModLoader](https://github.com/tModLoader/tModLoader/wiki/tModLoader-guide-for-contributors)
___

## Steam Installation
To install [tModLoader](https://store.steampowered.com/app/1281930/tModLoader) on Steam, you need to own [Terraria](https://store.steampowered.com/app/105600/Terraria) on Steam.
Simply browse for [tModLoader](https://store.steampowered.com/app/1281930/tModLoader/) on Steam and install it.
The tModLoader installation will exist alongside the vanilla installation, allowing you to play both vanilla and modded without the hassle of reinstalling vanilla. 
Note that tModLoader does NOT currently support Family Share due to an issue with Valve's implementation. Active dialogs are still being had to fix it, we ask for your patience!

By Default, 1.4 tModLoader will be installed by Steam.

To access 1.3 (Legacy tModLoader), use either the manual install instructions below (look for Release version 0.11.8.9) or take the following steps:
 Go to Library: 
![image](https://user-images.githubusercontent.com/59670736/169886058-3eb1b43c-a113-468b-8213-ef0bcccc8e01.png)
 Find tModLoader:
![image](https://user-images.githubusercontent.com/59670736/169886128-43f95278-a2a4-4b13-bb7c-1b670c81b657.png)
 Right Click tModLoader
![image](https://user-images.githubusercontent.com/59670736/169886174-dfe0612b-75d9-4469-8f13-be3649fd35fc.png)
 Click Properties to Open the Steam Game Control Panel
![image](https://user-images.githubusercontent.com/59670736/169886269-f3a0e854-bbe6-4c2f-ab4c-6980406fea51.png)
 Select Betas
![image](https://user-images.githubusercontent.com/59670736/169886307-e60be211-d331-443f-bcde-109f61c23323.png)
 In the dropdown, select "Public-1.3-beta"
![image](https://user-images.githubusercontent.com/59670736/169886370-5a340164-ccfe-4520-a3f5-515fd81671cf.png)
 Close the Prompt (no code required)
![image](https://user-images.githubusercontent.com/59670736/169886435-b9cd7a18-eeb9-46f8-a41f-3dc450be8702.png)


### How to uninstall?
Simply right-click tModLoader -> Manage -> Uninstall.
Your vanilla install will be unharmed.

### Steam Family Share Installation
For some reason, if you don't own Terraria and are instead using a family shared Terraria, tModLoader won't launch and will take you to the Steam store. We are looking into the issue, there is no known workaround for the 1.4 version.

On the 1.3 version, a workaround is to install tModLoader through steam as normal and then copy the `steam_appid.txt` file from the Terraria install folder to the tModLoader install folder, replacing the existing file. This does however break multiplayer.

## Manual Installation
This installation is necessary for players who have purchased Terraria from GOG or who otherwise want to install a particular version of tModLoader.

Installing tModLoader is relatively easy.

1. Goto the **[releases](https://github.com/tModLoader/tModLoader/releases)** page and download the tML release you want. (usually the **[latest](https://github.com/tModLoader/tModLoader/releases/latest)**)
2. Unzip the **contents** of the zip you downloaded to a folder named `tModLoader` either next to or nested inside the Terraria install folder. 
On 1.4, if the folder contains a 'Build' folder, you will need to remove this intermediate folder and bring the contents up one level. (GOG usually installs to `C:\GOG Games`, and Steam to `C:\Program Files (x86)\Steam\steamapps\common\Terraria`. 

See [this video](https://gfycat.com/SelfreliantAssuredIsabellineshrike) to find the steam installation location if you customized it.) (If you are on linux and own the game on GOG, the nested option inside `Terraria\game` is preferred) If you don't know how to unzip a zip file, get someone who knows how to use a computer to help you.

    * **Option 1, side-by-side (Recommended)**:    
![](https://i.imgur.com/gmrBMSO.png)    
    * **Option 2, nested**:    
![](https://i.imgur.com/YWaqZPO.png)    
    * **DO NOT** install the tModLoader files directly into the Terraria folder. This option not supported for GOG on Mac.
3. [This step applies to 1.3 ONLY] Remove or Add the Steam files depending on which version of the game you own:
    1. If you are using the GOG version of Terraria, delete the Steam file from the folder you just extracted tModLoader into (these files might already be deleted from the zip you downloaded):
        * Windows: `steam_api.dll`
        * Linux: `lib/libsteam_api.so` and `lib64/libsteam_api.so`
        * Mac: `tModLoader.app/Contents/MacOS/osx/libsteam_api.dylib`
    2. If you are using the Steam version of Terraria, if the Steam files are missing from the zip, copy them from your Terraria install to the tModLoader install:
        * Copy the `steam_appid.txt`, then, depending on your platform:
            * Windows: `steam_api.dll` and `CSteamworks.dll`
            * Linux: `lib/libsteam_api.so`, `lib/libCSteamworks.so`, `lib64/libsteam_api.so`, and `lib64/libCSteamworks.so`
            * Mac: `tModLoader.app/Contents/MacOS/osx/libsteam_api.dylib` and `tModLoader.app/Contents/MacOS/osx/CSteamworks`
5.  [1.4 ONLY] GoG users will need to install Steam if they haven't already. Our Mod Browser uses some of the steam install files to facilitate accessing the Steam Workshop. You should NOT need an account/be logged in for this to work.
6. Done. You can now make a desktop shortcut for tModLoader and launch tModLoader from that.

Tip: Here is an easy way to find where your Terraria files are located: ([video example](https://gfycat.com/SelfreliantAssuredIsabellineshrike))

1. Locate Terraria in your Steam game library, right click it and click 'Properties'
2. Browse to the 'Local Files' tab and click on the 'Browse local files...' button
3. You are now in your Terraria folder (this is where you should install tModLoader)

### Manual Installation Common Issues
Windows 1.3 only: If the game doesn't launch at all, you might not have .NET 4.5 or XNA 4.0 installed. Download and run both installers:
1. [Microsoft .NET Framework 4.5](https://www.microsoft.com/en-us/download/details.aspx?id=30653)
2. [Microsoft XNA Framework Redistributable 4.0](https://www.microsoft.com/en-us/download/details.aspx?id=20914)

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
Terraria and 1.3 tModLoader are 32-bit applications. In short, this means they are only capable of utilizing up to ~4 GiB of RAM. With a lot of mods, you may run out of memory. The solution is using less mods, unfortunately. Alternatively, you can try using the unofficial 64-bit version of 1.3 tModLoader.

1.4 tModLoader is 64 bit by default, which alleviates this problem.

## I use macOS Catalina. What do I do?
If you experience any problems, try using the 64-bit version of tModLoader or talk to us on Discord.