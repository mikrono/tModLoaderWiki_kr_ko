# Intermediate Prerequisites
Before consulting any of the Intermediate-level guides, please read below so you are prepared for Intermediate-level concepts. Please also make sure that you have gone through the Basic Prerequisites and corresponding Basic level tutorials related to things you are interested in.

## Upgrade from Text Editor to an IDE
In Basic, we used a Text Editor to write some simple code, but using an IDE (Integrated Development Environment) extremely recommended for anyone attempting anything in the Intermediate-level guides.

We recommend installing the latest [Visual Studio Community](https://www.visualstudio.com/vs/community/). It is free and fully featured.

### Installation
Simply download from the above link and run the downloaded file to install. During install, you will be prompted to choose which capabilities and programming languages you want to install. Be sure to select ".Net Desktop Environment" so that you can write code in C#.

### IntelliSense
TODO
Video: https://gfycat.com/PowerlessEqualFeline

### Usage
To use Visual Studio, open the .csproj file that was included when we made our first mod in the Basic Guide. To make a new mod project, please see [Developing with Visual Studio](https://github.com/bluemagic123/tModLoader/wiki/Developing-with-Visual-Studio)

### Advanced
See the Advanced Prerequisites to learn about Edit and Continue, Debugging, and Breakpoints.

## Inheritance
Please [read about inheritance](https://www.tutorialspoint.com/csharp/csharp_inheritance.htm)

Inheritance is used everywhere in tModLoader modding. If you look back on our sword we made during Basic, you'll notice that our sword inherits from ModItem.

## Hooks
A "Hook" is a method that is made available to modders so that they can achieve special effects for their mod. Hooks are virtual methods that modders override if they wish to use them. 

## Documentation
Learning to read [documentation](http://bluemagic123.github.io/tModLoader/html/annotated.html) is essential to modding. For example, [Example Gun](https://github.com/bluemagic123/tModLoader/blob/master/ExampleMod/Items/Weapons/ExampleGun.cs#L50) has an example that overrides the ModItem.Shoot method. We see from the method signature that Shoot returns a bool. The bool that we return has special meaning, but we won't know unless we look it up. Open up the Documentation and search for "Shoot" and then click the result, and then the sub-result for the ModItem class. You will be taken to a description. Read the description and learn what the parameters and return values represent. Looking back on that ExampleGun code, you should now see how the hook manages to change the bullet that is shot by using the Shoot hook.

### Virtual and Override
You may notice that the documentation lists all the hooks as virtual, but all the ExampleMod code has override. This is because we override virtual methods when we inherit from them. [Read about inheritance](https://www.tutorialspoint.com/csharp/csharp_inheritance.htm) again if you've forgotten.

