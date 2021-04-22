# Why Use an IDE
This page will hopefully convince you to abandon making mods in Notepad. Be sure to click the links below to watch the videos. All info presented corresponds to Visual Studio, but other IDEs have similar functionality.

# Syntax Error Checking    
When you make a syntax error, you'll see red underlines under the bad code, similar to a spell check.    
![](https://i.imgur.com/dwskGkr.png)    
You can hover over the error to see an explanation.    
![](https://i.imgur.com/a80jrK8.png)    
Seeing the explanation above, we can fix the code easily by changing `20.5f` to just `20`.

## Simple Misspellings
Visual Studio can fix simple misspellings as well. 
[Watch this in action.](https://gfycat.com/DemandingFeminineArthropods)    

## Show Potential Fixes
Sometimes Visual Studio can guess how to fix your error. For example, below we forget to add `using Terraria.ID;` to our ModItem class.    
![](https://i.imgur.com/dq7v87C.png)    
Clicking `Show potential fixes` or pressing the hotkey shows the following options.
![](https://i.imgur.com/BivfnXZ.png)     
Clicking the first option adds the appropriate using statement to our code. We can fix errors without trying to build in-game.

### Video
[Watch this in action.](https://gfycat.com/AbleAcademicBagworm)

# Autocomplete / Intellisense

## Field names
A common problem with new modders is that they don't know all the Terraria variable names or method names. For example, the various biomes/zones have names that aren't always what you might expect. You'll still need a little experience to know which class to look for these variables, but Intellisense helps immensely. In the video below, we see Intellisense suggesting the available Zones and downed bosses. Notice how Plantera being defeated is `downedPlantBoss`. This is why Intellisense is so valuable to a self-sufficient modder. [Watch this in action.](https://gfycat.com/UnitedFaintGrouper)    
![](https://i.imgur.com/CkGOuta.png)

### ItemIDs, NPCIDs, ProjectilesIDs, etc...
While you can waste your time looking up various vanilla IDs in our [reference pages](https://github.com/tModLoader/tModLoader/wiki/Vanilla-Item-IDs), it is much easier to just let Autocomplete do the work for us! Simply start typing your guess for the ID name and you will quickly see suggestions. You can select a suggestion to autocomplete it. How nice is that?    
![](https://i.imgur.com/8VpUJFF.png)

## Method names and parameters
Method names are also suggested in the same way. In addition, you can see the method parameters, both their names and types. For example, I'd like to add a buff to an enemy when I hit him with my sword. Typing `target.AddBuff(` will bring up the parameters, helping me complete my call to the AddBuff method. [Watch this in action.](https://gfycat.com/UnknownQuarterlyChamois)      
![](https://i.imgur.com/Oml4sUU.png)     

## Override
An extremely common task when making mods is to override various hooks such as SetDefaults or UpdateAccessory. Remembering the parameters to these hooks is sometimes hard to do. By using Visual Studio, you can easily write "override" followed by a space, followed by a few letters of the method you want to override, and then enter to easily override hooks. As a bonus, any missing using references will automatically be added to your code.
[Watch this in action.](https://gfycat.com/AdorableFluidDunlin)     
Before:    
![](https://i.imgur.com/uOQMDHx.png)    
After:    
![](https://i.imgur.com/KpQARfo.png)     

## Cross Mod
By using the extract mod feature, `[Modname]_[Modversion].dll` (and optionally a `[Modname]_[Modversion].xml` documentation file if it exists) will be placed in `Terraria\ModLoader\references\mods`. This file can be added as a dll reference to your .csproj if your mod has a strong or weak reference to another mod (you'll still need the build.txt entries). This path is consistent and accessible from the Mod Sources folder, meaning that the reference added to .csproj is portable for all contributors for your mod, provided they extract the referenced mod on their computer. By doing this, you can enjoy auto-complete for the API provided by the mods you reference as well. Read more on that [here](https://github.com/tModLoader/tModLoader/wiki/Expert-Cross-Mod-Content).

# Debug
With an IDE, such as VS, you can debug your mod. Debugging is the best way to pinpoint issues in your mod and solve them. While debugging, you can set 'breakpoints' which will make the application 'stop' and pause itself when it gets to that point. During this pause you can inspect what is happening, by for example looking at the values of variables in scope. You can quickly resume the application, or step over code to try to find the cause of a problem. To learn more about debugging, see its [own guide](Learn-How-To-Debug).

## Edit And Continue
With Edit And Continue (or eac) you can edit code after you've hit a breakpoint and see the effect of your edits in game immediately, rather than having to rebuild your mod manually. This is extremely useful for positioning projectile spawns or tweaking item defaults. Be sure to read [edit and continue](https://github.com/tModLoader/tModLoader/wiki/Developing-with-Visual-Studio#edit-and-continue) in order to use this feature. Note that things like `ModItem.SetStaticDefaults`, `ModTile.SetDefaults`, or other code might only run when the mod is loading, so they cannot benefit from eac.

### Simple Example
In this video, we see 2 Example Gun spawned. After the first is spawned, a breakpoint is set in SetDefaults. Once it is hit, the game is paused and Visual Studio comes to the front. Then, `item.damage` is changed from 40 to 80. We press continue and we can hover over the 2 items and see that the second item was spawned with 80 damage. [Watch this in action.](https://gfycat.com/CriminalGrossBasenji)   

### Another Example
In the [Basic Projectile: Drawing and Collision](https://github.com/tModLoader/tModLoader/wiki/Basic-Projectile#drawing-and-collision) guide, an example is shown using a breakpoint to test values for `ModProjectile.SetDefaults`: [Video](https://gfycat.com/WebbedUntimelyHarborseal)

# Documentation
tModLoader will automatically provide documentation XMLdoc files, you can see documentation for a method when you hover it. 

## Mod Documentation
As of v0.11.5, modders making mods with API to be used by other mods can include [ModName].xml files in their mod and they will be automatically extracted alongside their .dll file and placed in `ModLoader\references\mods` for easy collaboration (provided `hideCode` is not true, which would be counter-productive for a mod expecting to be referenced by other mods.). XMLdoc files for mods can be generated in Visual Studio during a regular build and should be named [ModName].xml and placed in the root of the mod's source folder.

# Find All References (Ctrl-K, R)
Find all references makes it easy to locate all usages of a variable in your whole project. Simply right click on a variable and click `Find all references`. This is very useful to see where you assign and use your variables.
[Watch this in action.](https://gfycat.com/IdioticFatalCockerspaniel)    
![](https://i.imgur.com/QCUdJdc.png)    

# Go To Definition (F12, Ctrl-Left Mouse Click)
By clicking on a variable or classname, you can press F12 (or Ctrl-Left Mouse Click) and Visual Studio will bring you right to where that class or variable is declared. If you do this on a class not in your code, such as the Terraria class `Dust` or the XNA class `Rectangle`, you will be led to a page with just the variables and methods, not the actual code. [Watch this in action.](https://gfycat.com/FaithfulBestIslandwhistler)    

# Rename (F2)
Imagine you name a variable or class something stupid and want to change it. Without an IDE, you would have to manually check each file the variable is used in and edit the text. Even something like "Find and Replace" would be error prone if you use similar names for other variables. Using the Rename function, you can easily rename classes and variables. Be aware that if you are using methods like `mod.ItemType("Name")` rather than `mod.ItemType<Name>()`, this won't work. This is also a great reason to use the <> type methods for ItemType and NPCType, it is much easier to maintain. In the video, we rename a variable called `examplePet` to something better, `airplanePet`. 
[Watch this in action.](https://gfycat.com/SphericalUnsteadyDutchsmoushond)    
![](https://i.imgur.com/PInjsqm.png)    

# Parameter Info (Ctrl-Shift-Space)
Some Terraria methods have many many parameters. It can be hard to remember the order and types of those parameters. If you'd like to see the parameters of something like Item.NewItem or Projectile.NewProjectile, simeply place your cursor inside the method and press `Ctrl-Shift-Space`. You can navigate left and right and watch the bold parameter change. Press up and down to switch between overloads of the same method. [Watch this in action.](https://gfycat.com/BarrenShorttermAsianpiedstarling)  [2nd Video](https://gfycat.com/BlissfulAccurateKiwi)   
![](https://i.imgur.com/D0f2NM6.png)    

# Goto (Ctrl-,)
An extremely useful shortcut, Goto allows you to quickly navigate to and class, method, or field in your code. Simply type `Ctrl-,` and the Go to All window will pop up. Type the name or part of the name and select the item to quickly navigate to the code. You can also type the first letter of each word in a name to find it. For example, typing `eqf` will suggest `ExampleQuestFish`. Visual Studio will smartly handle misspellings and partial matches as well. The video shows just how quick this feature can be and how easy it is to quickly navigate to other places in your code. [Watch this in action.](https://gfycat.com/AromaticLazyFrogmouth)    
![](https://i.imgur.com/TUthjgE.png)    

# Quick Actions and Refactorings (Ctrl-.)
This context sensitive hotkey is useful in several situations. The suggestions depend on where the cursor is or what text is selected. Here are some useful actions:
## Introduce Local for All Occurrences
If you are repeating yourself a lot in your code accessing the same object via fields and methods, sometimes your code can get a bit repetitive. Making a local variable and re-using that variable makes the code much cleaner. If you highlight the first occurrence of repeated object access, you can use this action to quickly introduce a local variable that you can then give an appropriate name to. [Watch this in action.](https://gfycat.com/devotedgreedyirishwaterspaniel)

# Format Document (Ctrl-E, D or Edit->Advanced->Format Document)
Properly formatted code is key for readability. Make sure to run this command before asking for help with your code as nothing is more annoying that poorly formatted code. [Watch this in action.](https://gfycat.com/DearZanyComet)    

# Structure Guide Lines
Sometimes, especially in Projectile and NPC AI, you might get lost within your code. This is especially true with nested if statements. Hover over the dotted lines to quickly see where you currently are in your code. For example, here I easily see at a glance where in the logic of my AI I currently am.         
![](https://i.imgur.com/nVR628X.png)    

# Code Folding
Folding code by pressing the `-` or `+` buttons can be useful if you need a wider perspective on your code.    
![](https://i.imgur.com/BcgwF5H.png)    

# Goto Brace (Ctrl-])
Using this hotkey is useful when navigating vanilla code with large blocks of code within braces. 
[Watch this in action.](https://gfycat.com/RaggedScarceHochstettersfrog)   

# C# Interactive
Did you know that `3/2` in c# is `1` not `1.5`? Sometimes you might want to experiment with some c# syntax, but don't want to make a mod just to test the code. The c# interactive window is very useful for this. You can do whatever you want in the c# interactive, each command will execute it after you press enter. `Alt-UpArrow` can be used to bring up previous statements. [Watch this in action.](https://gfycat.com/FarDismalGordonsetter)   
![](https://i.imgur.com/F6YABM2.png)    

# Visual Studio Live Share
This feature lets you invite someone into your project and collaboratively work on the code. Useful for mentoring other developers on your mod team or getting help on a tricky problem. Pair with Discord chat for voice chat.