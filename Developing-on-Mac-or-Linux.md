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
