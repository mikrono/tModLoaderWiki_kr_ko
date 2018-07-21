# Why Use an IDE
This page will hopefully convince you to abandon making mods in Notepad. Be sure to click the links below to watch the videos. All info presented corresponds to Visual Studio, but other IDEs have similar functionality.

# Syntax Error Checking    
When you make a syntax error, you'll see red underlines under the bad code, similar to a spell check.    
![](https://i.imgur.com/dwskGkr.png)    
You can hover over the error to see an explanation.    
![](https://i.imgur.com/a80jrK8.png)    
Seeing the explanation above, we can fix the code easily by changing `20.5f` to just `20`.

## Show Potential Fixes
Sometimes Visual Studio can guess how to fix your error. For example, below we forget to add `using Terraria.ID;` to our ModItem class.    
![](https://i.imgur.com/dq7v87C.png)    
Clicking `Show potential fixes` or pressing the hotkey shows the following options.
![](https://i.imgur.com/BivfnXZ.png)     
Clicking the first option adds the appropriate using statement to our code. We can fix errors without trying to build in-game.

### Video
[Watch this in action.](https://gfycat.com/AbleAcademicBagworm)

# Autocomplete / Intellisense

## Override
An extremely common task when making mods is to override various hooks such as SetDefaults or UpdateAccessory. Remembering the parameters to these hooks is sometimes hard to do. By using Visual Studio, you can easily write "override" followed by a space, followed by a few letters of the method you want to override, and then enter to easily override hooks. As a bonus, any missing using references will automatically be added to your code.
[Watch this in action.](https://gfycat.com/AdorableFluidDunlin)

# Documentation
Hover over any tModLoader hook and you can see documentation for that method.

# Find All References (Ctrl-K, R)
Find all references makes it easy to locate all usages of a variable in your whole project. Simply right click on a variable and click `Find all references`. This is very useful to see where you assign and use your variables.
[Watch this in action.](https://gfycat.com/IdioticFatalCockerspaniel)

# Go To Definition (F12)
By clicking on a variable or classname, you can press F12 and Visual Studio will bring you right to where that class or variable is declared. If you do this on a class not in your code, such as the Terraria class `Dust` or the XNA class `Rectangle`, you will be led to a page with just the variables and methods, not the actual code. [Watch this in action.](https://gfycat.com/FaithfulBestIslandwhistler)

# Rename (F2)
Imagine you name a variable or class something stupid and want to change it. Without an IDE, you would have to manually check each file the variable is used in and edit the text. Even something like "Find and Replace" would be error prone if you use similar names for other variables. Using the Rename function, you can easily rename classes and variables. Be aware that if you are using methods like `mod.ItemType("Name")` rather than `mod.ItemType<Name>()`, this won't work. This is also a great reason to use the <> type methods for ItemType and NPCType, it is much easier to maintain. In the video, we rename a variable called `examplePet` to something better, `airplanePet`. 
[Watch this in action.](https://gfycat.com/SphericalUnsteadyDutchsmoushond)

# Parameter Info (Ctrl-Shift-Space)

# Goto Brace (Ctrl-])

# Goto (Ctrl-,)
An extremely useful shortcut, Goto allows you to quickly navigate to and class, method, or field in your code. Simply type `Ctrl-,` and the Go to All window will pop up. Type the name or part of the name and select the item to quickly navigate to the code. You can also type the first letter of each word in a name to find it. For example, typing `eqf` will suggest `ExampleQuestFish`. Visual Studio will smartly handle misspellings and partial matches as well. The video shows just how quick this feature can be and how easy it is to quickly navigate to other places in your code. [Watch this in action.](https://gfycat.com/AromaticLazyFrogmouth)

# Structure Guide Lines
Sometimes, especially in Projectile and NPC AI, you might get lost within your code. This is especially true with nested if statements. Hover over the dotted lines to quickly see where you currently are in your code. For example, here I easily see at a glance where in the logic of my AI I currently am.         
![](https://i.imgur.com/nVR628X.png)    
# Code Folding

# Debug

# Edit And Continue