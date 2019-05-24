# These instructions do not apply yet

For now, the only instructions are here: [Xamarin/MonoDevelop instructions](https://forums.terraria.org/index.php?threads/1-3-tmodloader-a-modding-api.23726/page-525#post-1001200)

Everything below this possible relates to tModLoader v0.11, but the instructions do not work yet.

### Note: Work in Progress, instructions are not completely working or tested yet. Currently there is no implementation to decompile Terraria on Linux/Mac. This means that you need to run the `Setup` tool to decompile Terraria under Windows. This will hopefully soon be implemented on Linux and Mac as well.

# Developing on Mac or Linux
tModLoader requires the Microsoft C# Compiler (`mcs`) to be installed.
On Windows, it's bundled with the .NET installation itself. On Mac and Linux however, it requires a separate package which might not be pre-installed on your system.

You can check if it is present by opening a terminal and executing the command `mcs`, you should see something like this:
```
error CS2008: No files to compile were specified
Compilation failed: 1 error(s), 0 warnings
```

Or you can simply try compiling and look for `mcs not found` errors.

Instructions on how to install the required packages are in the following section.

# Building mods inside of tModLoader

tModLoader has the ability to build mods that have no external dependencies/references within Terraria itself.
If you are new to modding or have no idea what external dependencies are, this is most likely what you want.

If you want to use external dependencies which allow for more "fancy" features and you know what you are doing you can look into [building outside of tModLoader](#building-outside-of-tmodloader)

## Installing mono
Having the Mono framework installed allows you to build mods directly within tModLoader under the `Mod Sources` menu option.

### Linux

On most Linux distributions the package is called `mono-complete` or simply `mono`.

If you are having trouble installing the package you can go to the [installation instructions](https://www.mono-project.com/download/stable/#download-lin) page on the mono-projects website for further assistance.

### Mac

Visit the Mono website and follow the [installation instructions](https://www.mono-project.com/download/stable/#download-mac) for Mac.

Apparently this install doesn't add the appropriate tools to the Path, so you may need to follow these instructions if you still can't build in game. Please update this page with any relevant information if you have any.

Edit: Some further instructions can be found in the answers section of this [Stackoverflow question](https://stackoverflow.com/questions/32542535/how-to-install-mono-on-macos-so-mono-works-in-terminal)

Untested.

# Building outside of tModLoader

If you would like to have a more flexible and generally better development environment, you should probably look into building mods outside of tModLoader. This implies using an IDE such as Visual Studio Code (The regular Visual Studio is not supported on Linux, although it is on Mac).

MonoDevelop/Xamarin (on which Visual Studio for Mac is based on) is the easiest method to get started.

The following instructions can be followed for getting a simple environment going: [Xamarin/MonoDevelop instructions](https://forums.terraria.org/index.php?threads/1-3-tmodloader-a-modding-api.23726/page-525#post-1001200)

# Known issues
## Mac
### The type initializer for 'System.Drawing.GDIPlus' threw an exception
On **Mac** after you've followed this guide you might find that you encounter this error when trying to build a mod that contains png files.

![](https://cdn.discordapp.com/attachments/103115427491610624/540334979343974410/Screen_Shot_2019-01-30_at_6.55.40_PM.png)

To fix this download [this zip file](https://cdn.discordapp.com/attachments/103115427491610624/540387967915655188/system.drawing_for_mac.zip) and extract the contents to where you installed tModLoader (usually it will be `Library/Application Support/Steam/steamapps/common/Terraria/Terraria.app/Contents/MacOS`), overwriting/merging any folders/files if asked.
