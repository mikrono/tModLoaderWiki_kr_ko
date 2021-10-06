# Introductory

Rider is a lightweight IDE, developed by JetBrains. It is similar to Visual Studio but has a different layout more similar to those found in Intellij and PyCharm. If you have previously used other JetBrains IDEs you may have a significantly easier time using Rider than you would Visual Studios. Rider is easier to run than Visual Studio, while still having intellisense and code debugging.

## Table of Contents
- [Introductory](#introductory)
  * [Start](#start)
  * [Opening an existing VS project](#opening-an-existing-vs-project)
  * [Creating a new Rider project](#creating-a-new-rider-project)
  * [Adding project references](#adding-project-references)
  * [Viewing file structure](#viewing-file-structure)
  * [Optimal view setup](#optimal-view-setup)
  * [Keeping track of TODOs](#keeping-track-of-todos)
  * [No distraction mode](#no-distraction-mode)
  * [Code analysis, inspection settings](#code-analysis-inspection-settings)
  * [Split view: working on multiple files simultaneously](#split-view-working-on-multiple-files-simultaneously)
  * [How to set Target framework in Jetbrains Rider](#how-to-set-target-framework-in-jetbrains-rider)

## Start
First, install Rider. Unlike VS, Rider is a paid product, but a free 30 day trial is available. Download here: https://www.jetbrains.com/rider/

JetBrains' entire suite of IDEs is also avaliable through [Github's education pack](https://education.github.com/pack) requiring only a recognized school email. Sign up [here](https://education.github.com/benefits?type=student).

[Back to TOC](#table-of-contents)

## Opening an existing VS project
After launching Rider, simply select "Open Solution or Project". Then navigate to your project's `.csproj` or `.sln` file and open it. Rider will open a new window and load the project.

[Back to TOC](#table-of-contents)

## Creating a new Rider project
It is recommended that you create your project files through tModLoader itself. This can be done by following the steps laid out in the [Basic tModLoader Modding Guide](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Modding-Guide). After creating a new mod skeleton through tML, your mod folder will be located in your Mod Sources directory found here: `%userprofile%/Documents/My Games/Terraria/ModLoader/Mod Sources` follow the steps above and paste this path into your file explorer to find the mod folder. Your project will be generated with the proper build settings so there is no need to change them.

Once you open your mod's main file, you can move on to adding references.

[Back to TOC](#table-of-contents)

## Adding project references
On the left side of the window will be a panel labeled explorer, this is your Solutions Explorer. Near the top will be a drop down menu. By default it will be set to Solution, if it is not click it and select Solution from the drop down menu. Now the explorer should show your solution. From the explorer, right click the main tab with a green C# on it, and your mods name written to its right. A menu should appear. Click Add, and then Add Reference.

![](https://i.imgur.com/E9Ky5nJ.png)

A new window will appear, select 'Add From...' a file explorer should appear. There are five files which must be added as refernces. First is tModLoader itself. In the file explorer navigate to tModLoader's install directory (if installed through steam it will likely be found here: `C:\Program Files (x86)\Steam\steamapps\common\tModLoader`, if is installed elsewhere, you can find it by right clicking tModLoader in your steam library then selecting Properties... > Local Files > Browse Local FIles...). Once you have navigated to the install directory, double click tModLoader.exe to add it as a project reference.

You will also need the XNA references. However, these can be hard to find. To give you a little help, you can try pasting this handcrafted search string in your explorer window:
`search-ms:displayname=Search%20Results%20in%20GAC_32&crumb=filename%3A~<Microsoft.XNA%20OR%20System.Generic.String%3AMicrosoft.XNA&crumb=fileextension%3A~<Microsoft.XNA*.dll%20filename%3A~<Microsoft.XNA*.dll%20OR%20System.Generic.String%3AMicrosoft.XNA*.dll&crumb=location:C%3A%5CWindows%5CMicrosoft.NET%5Cassembly%5CGAC_32`
With a little luck, the XNA references are found as shown:

![](https://i.imgur.com/POzJM8t.png)

It is important that you **do not move or delete these files**. Instead, select all of them, and COPY them. It is recommended to copy them to a safe location you will use to reference these files in your mod projects. Once you've placed them in a safe location you can proceed adding them like the previous reference. 

[Back to TOC](#table-of-contents)

## Viewing file structure
To view file structure like in VS studio (dropdowns for methods etc.)
Go to `View -> Tool Windows -> Structure`
This opens a structure view window as shown:

![](https://i.imgur.com/hrNR8X3.png)

Clicking on any method or field will make your editor scroll to it.

To return the regular file explorer simply select the Explorer tab on the left side.

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

![](https://i.imgur.com/9UXxkvH.png)

Next, to enable code annotations propagation navigate to `'Code Annotations'` in the same dropdown, and tick `'Automatically propagate annotations'` as shown:

![](https://i.imgur.com/zeaXVWe.png)

You probably also want to disable the `'use 'var' (builtin types)'` hint, which can be annoying.

Navigate to `Inspection Severity -> C#`

Type in the search bar `Use preferred 'var'` and uncheck it as shown:

![](https://i.imgur.com/XxnqDJG.png)

If you are also annoyed by the `'Invert if to reduce nesting'`, it can be disabled as well:

![](https://i.imgur.com/537R8dV.png)

[Back to TOC](#table-of-contents)

## Split view: working on multiple files simultaneously
Another useful feature in Rider is split views.

It can look something like the following:

![](https://i.imgur.com/gKgf9aa.png) 

To do this, right click the tab you wish to split and then select "Split Vertically" or "Split Horizontally", you can do this multiple times however readability quicky declines.

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

## Todo
### Debugging
### Common Issues