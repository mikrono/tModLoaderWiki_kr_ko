# _Note: Work in Progress, instructions are not completely working or tested yet._

# Developing on Mac or Linux
tModLoader sometimes can't build mods in-game on Mac and Linux. This is because unlike on Windows, the compiler needed to compile code isn't necessarily present on the computer. This limitation is unfortunate, but there are workarounds.

You should notice `mcs not found` errors

# Install mono-complete
Please attempt this approach before attempting later approaches. This step installs the development files needed to compile c# code. 

## Linux

Visit the Mono website and follow the [install instructions](https://www.mono-project.com/download/stable/#download-lin).

Tested with Raspbian 9 and Ubuntu 14.

## Mac

Visit the Mono website and follow the [install instructions](https://www.mono-project.com/download/stable/#download-mac).

Apparently this install doesn't add the appropriate tools to the Path, so you may need to follow these instructions if you still can't build in game. Please update this page with any relevant information if you have any.

Untested.

# Build outside of tModLoader
Building mods outside of tModLoader is a little annoying, but it also forces you to use an IDE, which is a good skill to learn.

[Xamarin instructions](https://forums.terraria.org/index.php?threads/1-3-tmodloader-a-modding-api.23726/page-525#post-1001200)

### The type initializer for 'System.Drawing.GDIPlus' threw an exception
On **Mac** after you've followed this guide you might find that you encounter this error when trying to build a mod that contains png files.

![](https://cdn.discordapp.com/attachments/103115427491610624/540334979343974410/Screen_Shot_2019-01-30_at_6.55.40_PM.png)

To fix this download [this zip file](https://cdn.discordapp.com/attachments/103115427491610624/540387967915655188/system.drawing_for_mac.zip) and extract the contents to where you installed tModLoader (usually it will be `Library/Application Support/Steam/steamapps/common/Terraria/Terraria.app/Contents/MacOS`), overwriting/merging any folders/files if asked.
