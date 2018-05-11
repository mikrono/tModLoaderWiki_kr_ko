# Introductory
Rider is an IDE similar to Visual Studio, for C# (and other languages) The software is made by JetBrains, whereas Visual Studio is made by Microsoft. Rider is more lightweight than VS, but also features intellisense and code debugging. It also allows opening an existing VS project.

# Start
First, install Rider. Unlike VS, Rider is a paid product, but a free 30 day trial is available. Download here: https://www.jetbrains.com/rider/

## Opening an existing VS project
This one is easy. In the main menu, press 'Open Solution or Project' and simply open either the .csproj or .sln file.
If you don't know how to setup the solution, it is recommended to use the [mod skeleton generator](http://javid.ddns.net/tModLoader/generator/ModSkeletonGenerator.html)

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

Your final window should look similar to this:

![](https://i.imgur.com/hF2Pw4V.png)

Now press create which will create the new project.
If you've selected to create a GIT repository, a window will pop up asking to add files to git. **Uncked all of these files, and check 'Remember, don't ask again'**

![](https://i.imgur.com/2dtJ122.png)

Since tML does not support C#7 yet, it is recommended to set the language level to 6. To do so, right click the project and click 'Properties' to open the properties window. On the application tab, navigate to the Language section and set it to 'C# 6.0'. This will avoid Rider recommending c#7 language features that will not compile for tML.

![](https://i.imgur.com/L8YTOhO.png)

Now that your initial setup is done. We can proceed to adding the references.

## Adding project references
In your solution explorer, navigate to 'References' and right click, then press on 'Add reference'. This opens a new window. Press on the button below 'Add From...'. Now navigate to your Terraria install directory, and proceed to add Terraria.exe (tModLoader modified .exe!)

![](https://i.imgur.com/dy4NLci.png)

You will also need the XNA references. However, these can be hard to find. To give you a little help, you can try pasting this handcrafted search string in your explorer window:
`search-ms:displayname=Search%20Results%20in%20GAC_32&crumb=filename%3A~<Microsoft.XNA%20OR%20System.Generic.String%3AMicrosoft.XNA&crumb=fileextension%3A~<Microsoft.XNA*.dll%20filename%3A~<Microsoft.XNA*.dll%20OR%20System.Generic.String%3AMicrosoft.XNA*.dll&crumb=location:C%3A%5CWindows%5CMicrosoft.NET%5Cassembly%5CGAC_32`
With a little luck, the XNA references are found as shown:

![](https://i.imgur.com/POzJM8t.png)

It is important that you **do not move or delete these files**. Instead, select all of them, and COPY them. It is recommended to copy them to a safe location you will use to reference these files in your mod projects. Once you've placed them in a safe location you can proceed adding them like the previous reference. 

## Setting up the mod class
Next up, locate `Class1.cs` in the Solution Explorer and rename it to 'MyModName.cs'
Now in the class file, also rename Class1 to MyModName
Now have it inherit the class `Mod`, start typing `: Mod` and the intellisense should begin working:

![](https://i.imgur.com/IqeE0Ix.png)

The first class shown is correct. Simply press tab to insert the edits. If tab doesn't work, you can try ALT + ENTER

## Setting up mod compilation, debugging and eac
To setup mod compilation, navigate to the Configuration dropdown, likely displaying 'Not configured' Drop it down and click 'Edit configurations'

![](https://i.imgur.com/76EHBrN.png)

A new window pops up. Click the green plus sign and make a new '.NET Executable'. Name it 'tML compile' and make sure to uncheck 'Single instance only'
![](https://i.imgur.com/fopcCfX.png)

In the 'Exe path', put the path to your Terraria.exe

Leave Program arguments empty. In working directory, set it to the directory your Terraria.exe belongs to (it might have done so automatically)

![](https://i.imgur.com/DmxJOsF.png)

Next, navigate to the Before launch section below. First, you'll want it to build the project. Click the green plus icon and click 'Build Solution' like so:

![](https://i.imgur.com/vinOA1L.png)

Next click the icon again, but now add 'Run external tool'. This opens a new window. Press the green icon again, this opens a new 'Create Tool' window. There are a lot of settings that need to be configured right.

Set the settings like so:

* Set name to: `tML Server Compile`
* Set description to: `Compiles the mod using the server`
* Set program to the path to tModLoaderServer.exe
* Set the arguments to the following: `-build "$ProjectFileDir$" -eac " $ProjectFileDir$\bin\Debug\MyModName.dll "`
    * **Make sure to change MyModName at the end to your mod name! Also make sure to keep the spaces in the eac argument!!**
* Set working directory to your Terraria directory, where the server .exe is located.

The window should end up as shown:

![](https://i.imgur.com/UYNGszR.png)

Add this external tool. In the before launch section, **make sure 'Build Solution' goes before the external tool** as shown:

![](https://i.imgur.com/Soo7Ltu.png)

Now click apply and then OK. Now in the same dropdown, select your new configuration. When you debug, it will build the solution and your .tmod file automatically using the server, and then start debugging your mod.

## Custom MSBuild steps
.... TODO ....