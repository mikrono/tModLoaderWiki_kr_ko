# These instructions not up-to-date
With the introduction of the in-game mod generator, many of these instructions might be incorrect. I think you can just skip the steps pertaining to creating a project and configuring it and use the in-game mod generator, but this guide needs to be fixed. The Visual Studio and Visual Studio Code guides are up-to-date.

# Introductory

Rider is an IDE similar to Visual Studio, for C# (and other languages) The software is made by JetBrains, whereas Visual Studio is made by Microsoft. Rider is more lightweight than VS, but also features intellisense and code debugging. It also allows opening an existing VS project.

## Table of Contents
- [Introductory](#introductory)
  * [Start](#start)
  * [Opening an existing VS project](#opening-an-existing-vs-project)
  * [Creating a new Rider project](#creating-a-new-rider-project)
  * [Adding project references](#adding-project-references)
  * [Setting up the mod class](#setting-up-the-mod-class)
  * [Setting up mod compilation, debugging and eac](#setting-up-mod-compilation-debugging-and-eac)
  * [Custom MSBuild steps](#custom-msbuild-steps)
  * [Viewing file structure](#viewing-file-structure)
  * [Optimal view setup](#optimal-view-setup)
  * [Keeping track of TODOs](#keeping-track-of-todos)
  * [No distraction mode](#no-distraction-mode)
  * [Code analysis, inspection settings](#code-analysis-inspection-settings)
  * [Using the right MSBuild version](#using-the-right-msbuild-version)
  * [Split view: working on multiple files simultaneously](#split-view-working-on-multiple-files-simultaneously)

## Start
First, install Rider. Unlike VS, Rider is a paid product, but a free 30 day trial is available. Download here: https://www.jetbrains.com/rider/

[Back to TOC](#table-of-contents)

## Opening an existing VS project
This one is easy. In the main menu, press 'Open Solution or Project' and simply open either the .csproj or .sln file.
If you don't know how to setup the solution, it is recommended to use the [mod skeleton generator](http://javid.ddns.net/tModLoader/generator/ModSkeletonGenerator.html)

[Back to TOC](#table-of-contents)

## Creating a new Rider project
In the main menu, press 'New Solution'. Firstly, you need to select the right type of application. On the left hand menu, look for .NET (**NOT .NET CORE**) and select 'Class Library'.

![](https://i.imgur.com/72ZhWAf.png)

You will see the right hand menu changed.

In the field 'Solution name', give it the internal name of your mod. Something like 'MyMod'. I'd like to refer to the [naming convention tips](Basic-tModLoader-Modding-Guide#naming-convention) on our wiki to learn more about using the proper names. You can give 'Project Name' the same name (it might do so automatically)

![](https://i.imgur.com/fOlcLQz.png)

In the field 'Solution directory', you'll want to change it to the Mod Sources documents. For most people, this is located at: `%userprofile%/Documents/My Games/Terraria/ModLoader/Mod Sources`

![](https://i.imgur.com/2RpsVtA.png)

Put this address into your explorer window and press enter.

![](https://i.imgur.com/IYWW8a9.png)

Copy this path to the field in the Rider window.

![](https://i.imgur.com/TCT759p.png)

Make sure to tick the 'Put solution and project in the same directory'
![](https://i.imgur.com/DcXllwE.png)

If you wish to make use of our [browser benefits when using git](Mod-Browser#getting-started) you should check the 'Create Git repository' checkbox.
![](https://i.imgur.com/yCBcw0D.png)

You should leave the 'Type' field as 'Class Library' and the 'Language' field as 'C#', but you should change the 'Framework' field to '.Net Framework v4.5

![](https://i.imgur.com/peWz7bN.png)

Your final window should look similar to this: (except the "Solution directory already exists")

![](https://i.imgur.com/IRfMWrq.png)

Now press create which will create the new project.
If you've selected to create a GIT repository, a window will pop up asking to add files to git. 
**Uncked all of these files, and check 'Remember, don't ask again'** 

![](https://i.imgur.com/2dtJ122.png)

Since tML does not support C#7 yet, it is recommended to set the language level to 6. To do so, right click the project and click 'Properties' to open the properties window. On the application tab, navigate to the Language section and set it to 'C# 6.0'. This will avoid Rider recommending c#7 language features that will not compile for tML.

![](https://i.imgur.com/L8YTOhO.png)

Now that your initial setup is done. We can proceed to adding the references.

[Back to TOC](#table-of-contents)

## Adding project references
In your solution explorer, navigate to 'References' and right click, then press on 'Add reference'. This opens a new window. Press on the button below 'Add From...'. Now navigate to your Terraria install directory, and proceed to add Terraria.exe (tModLoader modified .exe!)

![](https://i.imgur.com/dy4NLci.png)

You will also need the XNA references. However, these can be hard to find. To give you a little help, you can try pasting this handcrafted search string in your explorer window:
`search-ms:displayname=Search%20Results%20in%20GAC_32&crumb=filename%3A~<Microsoft.XNA%20OR%20System.Generic.String%3AMicrosoft.XNA&crumb=fileextension%3A~<Microsoft.XNA*.dll%20filename%3A~<Microsoft.XNA*.dll%20OR%20System.Generic.String%3AMicrosoft.XNA*.dll&crumb=location:C%3A%5CWindows%5CMicrosoft.NET%5Cassembly%5CGAC_32`
With a little luck, the XNA references are found as shown:

![](https://i.imgur.com/POzJM8t.png)

It is important that you **do not move or delete these files**. Instead, select all of them, and COPY them. It is recommended to copy them to a safe location you will use to reference these files in your mod projects. Once you've placed them in a safe location you can proceed adding them like the previous reference. 

[Back to TOC](#table-of-contents)

## Setting up the mod class
Next up, locate `Class1.cs` in the Solution Explorer and rename it to 'MyModName.cs'
Now in the class file, also rename Class1 to MyModName
Now have it inherit the class `Mod`, start typing `: Mod` and the intellisense should begin working:

![](https://i.imgur.com/IqeE0Ix.png)

The first class shown is correct. Simply press tab to insert the edits. If tab doesn't work, you can try ALT + ENTER

[Back to TOC](#table-of-contents)

## Setting up mod compilation, debugging and eac
**Notice! You should follow along this part, but this configuration will change slightly in the next section; it is important that you understand how it works. The modified setup is explained in the next section: ['Custom MSBuild steps'](#custom-msbuild-steps)**

To setup mod compilation, navigate to the Configuration dropdown, likely displaying 'Not configured' Drop it down and click 'Edit configurations'. If you wish to skip these steps you can alternatively download the premade run configuration [here (zip file)](https://cdn.discordapp.com/attachments/426125688148328469/444630048125747200/runConfigurations.zip). If you do, unzip it. You need to move the 'runConfigurations' folder to `Path/To/Project/.idea/.idea.MyModName/.idea/` Once done, follow the below steps (or simply open the configuration) and change the paths to match yours.

![](https://i.imgur.com/76EHBrN.png)

A new window pops up. Click the green plus sign and make a new '.NET Executable'. Name it 'tML compile' and make sure to uncheck 'Single instance only', unless you want only one instance of debugging to be up at once.

![](https://i.imgur.com/fopcCfX.png)

1. In 'Exe Path', set it to the path to your Terraria.exe (tModLoader)
2. In 'Working directory', set it to that folder. It should do so automatically.

![](https://i.imgur.com/DmxJOsF.png)

Next, navigate to the Before launch section below. First, you'll want it to build the project. Click the green plus icon and click 'Build Solution' like so:

![](https://i.imgur.com/vinOA1L.png)

Next click the icon again, but now add 'Run external tool'. This opens a new window. Press the green icon again, this opens a new 'Create Tool' window. There are a lot of settings that need to be configured right.

Set the settings like so:

* Set name to: `tML Server Compile`
* Set description to: `Compiles the mod using the server`
* Set program to the path to `tModLoaderServer.exe`
* Set the arguments to the following: `-build "$ProjectFileDir$"`
    * If you wish to enable eac, set it to the following:  `-build "$ProjectFileDir$" -eac " $ProjectFileDir$\bin\Debug\$ProjectName$.dll "` **Make sure to keep the spaces in the eac argument!!**
* Set working directory to your Terraria directory, where the server .exe is located.

The window should end up as shown:

![](https://i.imgur.com/R98SJgr.png)

Add this external tool. In the before launch section, **make sure 'Build Solution' goes before the external tool** as shown:

![](https://i.imgur.com/Soo7Ltu.png)

Now click apply and then OK. Now in the same dropdown, select your new configuration. When you debug, it will build the solution and your .tmod file automatically using the server, and then start debugging your mod.

**To actually debug, make sure to press the debug icon (F5) instead of the regular start icon! Otherwise you will not debug!**

![](https://i.imgur.com/VEMlSa1.png)

**In this configuration, your .tmod is not yet built if you use the regular Build task (CTRL + SHIFT + B). This is setup in the next section.**

[Back to TOC](#table-of-contents)

## Custom MSBuild steps
Since Rider simply uses MSBuild, we can provide custom MSBuild steps and events.
Unfortunately Rider does not feature a GUI for Pre/Post-build events, so we have to edit them in manually.
Locate your .csproj and open it in a rich text editor, like notepad++ or alternatively open it in Rider directly by double clicking it. Navigate towards the end of the file.

Now to have Rider actually build our .tmod file if we use Build task and not just debug, we need to create an "AfterBuild" target. We need to input an 'exec' property, which will execute a specified command in our shell. The following exec property will make the server build the .tmod file when we use Build (CTRL + B). Put this new property right before the project closing tag `</Project>`
```csproj
  <Target Name="AfterBuild">
	<Exec Command='D:\Apps\Steam\steamapps\common\Terraria\tModLoaderServer.exe -build "$([System.IO.Path]::GetFullPath("$(MSBuildProjectDirectory)"))"' />
  </Target>
```

![](https://i.imgur.com/DhMBqql.png)

The only important part is that you change the path to the server exe. It is important we pass the `"` quotation marks in the shell arguments, otherwise we will get errors. Therefore we use `'` to encapsulate the value. You do not have to worry about the build path, as it will find the correct path using the MSBuild Reserved property.

To explain the macro part a bit, here is what it does:
The `[System.IO.Path]::GetFullPath("path")` part basically tells MSBuild to call the function `GetFullPath` in the `System.IO.Path` namespace. This will get the appropriate path. We pass "`$(MSBuildProjectDirectory)`" as the value, this is a macro and reserved MSBuild property that corresponds to the absolute path of the directory where the project file is located, excluding the final backslash (which is want we want!!)

If you've added this AfterBuild target, we no should no longer want to build the project before launching the external tool, as we now have the AfterBuild target. Navigate to your existing configuration, and remove the build launch before the external tool. Leave the rest of the settings as-is, as shown:

![](https://i.imgur.com/WlNzvkS.png)

This will prevent the regular Build  task from happening. Instead, it compiles our mod using the server (like before) with eac support. Note eac is only supported on Windows. This new setup allows the mod to be built even when using the regular Build task, but avoids the mod being built twice when debugging.

[Back to TOC](#table-of-contents)

## Viewing file structure
To view file structure lik in VS studio (dropdowns for methods etc.)
Go to `View -> Tool Windows -> Structure`
This opens a structure view window as shown:

![](https://i.imgur.com/nwrPOGa.png)

Clicking on any method or field will make your editor scroll to it.

[Back to TOC](#table-of-contents)

## Optimal view setup
If you plan on working on large code, like tMLs, it is recommended to enable the 'View whitespaces' and 'Use soft wrap' options. Go to `View -> Active Editor`  and enable these options. Viewing whitespaces (indents) is important to ensure you don't use whitespaces but tabs of width 4.

[Back to TOC](#table-of-contents)

## Keeping track of TODOs
Rider has a useful window that can find things marked TODO in comments or throwing NotImplementedException()
To open this window, go to `View -> Tool Windows -> TODO`
This opens a new window as shown:

![](https://i.imgur.com/JU5DiOX.png)

[Back to TOC](#table-of-contents)

## No distraction mode
This is a useful tip when you want to write code without being distracted.
To use this mode, navigate to `View -> Enter Distraction Free Mode`
This gets rid of pretty much any side windows as shown:

![](https://i.imgur.com/JMLXqOx.png)

[Back to TOC](#table-of-contents)

## Code analysis, inspection settings
For your own benefits, it is recommend to enable code analysis and propagate code annotations.

Open the settings, by going to `File -> Settings` or using the `CTRL + ALT + S` shortcut.

Navigate to `Editor -> Inspection Settings`

On Inspection Settings, tick to enable `'Enable code analysis'` and `'Enable solution-wide analysis'`

![](https://i.imgur.com/is9hLMi.png)

Next, to enable code annotations propagation navigate to `'Code Annotations'` in the same dropdown, and tick `'Automatically propagate annotations'` as shown:

![](https://i.imgur.com/zeaXVWe.png)

You probably also want to disable the `'use 'var' (builtin types)'` hint, which can be annoying.

Navigate to `Inspection Severity -> C#`

Type in the search bar `Use preferred 'var'` and uncheck it as shown:

![](https://i.imgur.com/XxnqDJG.png)

If you are also annoyed by the `'Invert if to reduce nesting'`, it can be disabled as well:

![](https://i.imgur.com/537R8dV.png)

[Back to TOC](#table-of-contents)

## Using the right MSBuild version
It is recommended to use `MSBuild 15.0`

If for some reason the wrong version is used, you can navigate to the Settings (`CTRL + ALT + S`) and go to: `'Build, Execution, Deployment' -> 'Toolset and Build'`
 Inside the menu, find `'Use MSBuild version:'` and select version 15.0

![](https://i.imgur.com/Qnc3rnP.png)

[Back to TOC](#table-of-contents)

## Split view: working on multiple files simultaneously
Another useful feature in Rider is split views.

It can look something like the following:

![](https://i.imgur.com/gKgf9aa.png) 

To enable this feature navigate to `Window -> Editor Tabs -> Split Vertically`

[Back to TOC](#table-of-contents)


# How to set Target framework in Jetbrains Rider

If you get errors about the .NET Target framework, you can change the values by right clicking your Solution name and selecting 'properties'.

![](https://i.imgur.com/coM4bsX.png)

The Project Properties will now open up and the Target framework option is available to be changed.

![](https://i.imgur.com/zDwbi9y.png)

Click the option dots (...) and select the preferred Target framework. Any version above 4.5 will work for developing tModLoader mods.

![](https://i.imgur.com/nCZQ3I1.png)

Click OK -> OK and reboot Jetbrains Rider. After rebooting, load up your Solution and your Target framework should be correctly set. In order to verify this you can edit your configuration by clicking Edit Configuration in the top right corner of the Rider context menu.

![](https://i.imgur.com/g1CHKWW.png)

![](https://i.imgur.com/0LP7DNG.png)

Verify you can run your configuration by pressing Shift + F10, or by clicking the green arrow next to the edit configuration menu.

![](https://i.imgur.com/DpLIBbh.png)

[Back to TOC](#table-of-contents)