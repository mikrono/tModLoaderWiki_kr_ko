# Advanced Prerequisites
Basically all the previous prerequisites. See Beginner and Intermediate prerequisite guides first. 

## tModLoader Source code
You may need to decompile tModLoader to understand the correct way to do something, or to figure out the recommended approach to what you are doing. Anyone attempting to do Advanced level guides probably needs their own copy of decompiled tModLoader.exe. 

### But wait, I found the code on Google!
No. That code is bad and will not work. It has tens of thousands of decompilation errors. Just decompile it yourself, it is easy. Read on.

### Simply Way
A simpler way is to download a recent release of [ilspy](https://github.com/icsharpcode/ILSpy/releases) (These instructions last tested with 6.0 Preview 3), unzip the zip to a folder, launch ILSpy.exe, use File->Open to open tModLoader.exe (typically found in `C:\Program Files (x86)\Steam\steamapps\common\tModLoader`), and finally press the save code button and navigate to a convenient empty folder (You'll want to choose a folder that you can easily find later, I suggest making a folder in the Saves directory: `C:\Documents\My Games\Terraria\ModLoader\DecompiledTModLoader`). (If you are on Linux or Mac, you will need to use [AvaloniaILSpy](https://github.com/icsharpcode/AvaloniaILSpy/releases) instead of ILSpy) This will take a minute or so depending on your computer. The folder that you saved the code to now has a `tModLoader.csproj` file, opening this file should open Visual Studio if you have it installed (highly recommended). If you do not have Visual Studio installed, you'll have to navigate the files individually through file explorer.

There are a few drawbacks to the simple way. The biggest drawback is you can't debug tModLoader itself. This is an advanced technique, but if you wish to do this later, come back to these instructions and read the Hard way below. The other drawback is you might find the code to have decompilation errors. With the latest version of ilspy, these are very limited and easily fixed.

![](http://i.imgur.com/ZeXH2p5.png)

### Hard Way
The best way to get the source code is to download the tModLoader repository and go through the [setup instructions](https://github.com/tModLoader/tModLoader/wiki/tModLoader-guide-for-contributors#getting-the-tmodloader-code-for-the-first-time). This will get you a completely functional solution that you can open in Visual Studio. This method will take more time and it's not usually required unless you need to debug Terraria code itself or contribute to tModLoader directly. 