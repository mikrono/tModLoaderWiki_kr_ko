# Advanced Prerequisites
Basically all the previous prerequisites. See Beginner and Intermediate prerequisite guides first. 

## tModLoader Source code
You may need to decompile tModLoader to understand the correct way to do something, or to figure out the recommended approach to what you are doing. Anyone attempting to do Advanced level guides probably needs their own copy of decompiled tModLoader.exe. The best way to get this is to download the tModLoader repository and go through the [setup instructions](https://github.com/tModLoader/tModLoader/wiki/tModLoader-guide-for-contributors#getting-the-tmodloader-code-for-the-first-time). This will net you a completely functional solution that you can open in Visual Studio. This method will take more time and it's not usually required unless you need to debug Terraria code itself or contribute to tModLoader directly. 

A simpler way is to download [ilspy](https://github.com/icsharpcode/ILSpy/releases/download/v3.0.1/ILSpy_binaries_Net46_Win_3.0.1.3459.zip), open it up, open up the tModLoader exe in it, and then press the save code button. This will take 20 minutes or so depending on your computer.

![](http://i.imgur.com/ZeXH2p5.png)

### ilspy notes
You may get things like this in your decompiled code:    
```cs
ref x = ref npc.ai[1];
x += 1f;
```    
This isn't correct c# syntax and will result in an error. This happens in the ilspy 3.0+ versions. You can change the code to `npc.ai[1] += 1f;`, but it might be easier to use an older but slower ilspy, [version 2.3.1](https://github.com/icsharpcode/ILSpy/releases/tag/2.3.1).

## But wait, I found the code on Google!
No. That code is bad and will not work. It has tens of thousands of decompilation errors. Just decompile it yourself.