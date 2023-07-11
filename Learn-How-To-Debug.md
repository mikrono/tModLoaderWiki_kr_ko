# What is Debugging?
Debugging is how modders investigate problems in their code in an attempt to fix them. The mod builds fine, but the mod does not behave as expected. Inevitably, something in your mod will not work as intended. When this happens, you'll need to "debug" the mod to determine where the issue is happening.

Debugging can take many forms, this guide will focus on 3 approaches: Isolation Debugging, Text Debugging, and Debugger Debugging. 

# Isolation Debugging
The first step when encountering a bug is to isolate it as much as possible. Testing with a new world and new player can rule out many sources of issues. Additionally, other mods are a big source of unknown behaviors, the bug you are seeing could potentially be a feature or a conflict with one of them. 

Disable all other mods and make sure the bug still exists, if it does, that means the problem is in your code, which means you can fix it. If the bug does not exist, you'll need to figure out which mod is causing the issue by loading individual mods with your mod. If the bug happens with only one other mod, it might be a bug from that mod. Collaborating with that modder to fix the issue might be necessary.

Once you know a bug is caused by your mod, you'll want to identify where the issue is coming from. The following sections will help you discover the issue. 

# Text Debugging
If you are not familiar with using a debugger like Visual Studio or just need to quickly visualize some data, then debugging via text can be useful. Do note that eventually you'll want to learn [Debugger Debugging](#Debugger-Debugging) because it is quicker and more powerful.

Text debugging, simply put, is printing text that you can read in an attempt to identify an issue. For example, if some math operation isn't giving the results you expect, printing the values out can help identify the issue. First, let's explore the text debugging methods available:

## Text Debugging Methods
The following are typical approaches for text debugging in tModLoader. With all of these approaches, make a string from the data you want to output. For example, if you have a variable called `total`, you might want to make a string `$"The Total is: {total}."` and pass that into any of these methods. To learn more about creating a string from text and data, see this [C# String Interpolation](https://www.w3schools.com/cs/cs_strings_interpol.php) tutorial.

### Main.NewText
This method outputs text to the in-game chat window. This is simple and straightforward. The text will be easily visible and will disappear after some time.   

`Main.NewText($"Using Main.NewText: Current health: {player.statLife}.");`:    
![image](https://github.com/tModLoader/tModLoader/assets/4522492/6eacdbe3-9b39-4e0e-8c54-1fc7b0aefdc8)        

### Mod.Logger.Info
This method outputs text to the [logs file](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#logs). A client outputs to `client.log` and the server process outputs to `server.log`. This approach will persist the data, so it might be more useful to use this approach if you need to track something over time.     

`Mod.Logger.Info($"Using Mod.Logger.Info: Current health: {player.statLife}.");`:    
![image](https://github.com/tModLoader/tModLoader/assets/4522492/6e2daa44-0442-4500-9dcf-7814b73c1cb9)    

### Console.WriteLine
If you are familiar with programming, using `Console.WriteLine` for text debugging should be familiar with you. This approach, however, is not very useful for debugging mods. The console is hidden, so there is no way to see the text. Use one of the other approaches. 

### CombatText.NewText
This approach spawns floating text. The text quickly disappears, but because the text spawns at a specified location, it can be useful for situations where that is helpful. The method requires passing in a Rectangle for the location and a Color: 

`CombatText.NewText(player.getRect(), Color.Purple, $"Using CombatText.NewText: Current health: {player.statLife}.");`:    
![image](https://github.com/tModLoader/tModLoader/assets/4522492/a725aab3-7450-4343-9025-2058088fdd39)    

### Dust.QuickDust, Dust.QuickBox, Dust.QuickDustLine
Not text, but spawning dust at specific world coordinates can be useful for code dealing with world locations. Use this to verify [geometry calculations](https://github.com/tModLoader/tModLoader/wiki/Geometry) in Projectile and NPC AI code.    

```cs
Dust.QuickDust(player.TopLeft, Color.Red);
Dust.QuickDust(player.BottomRight, Color.Blue);
```
![image](https://github.com/tModLoader/tModLoader/assets/4522492/97ef7aa3-7d1b-4d71-a481-9afde3de11c6)    

## How to use Text Debugging
Now that we know how to output text, lets go through an example. This exercise will help teach how to use Text Debugging. Consider the following code in a healing potion item similar to [ExampleHealingPotion](https://github.com/tModLoader/tModLoader/blob/1.4.4/ExampleMod/Content/Items/Consumables/ExampleHealingPotion.cs):
```cs
public override void GetHealLife(Player player, bool quickHeal, ref int healValue) {
	int numberOfDiamonds = player.CountItem(ItemID.DiamondRing);
	float healMultiplier = numberOfDiamonds / 10;
	healMultiplier = Utils.Clamp(0, healMultiplier, 10);
	healValue = (int)(10 * healMultiplier);
}
```
The intent of this code is to heal the player 100 health plus 10 health for every Diamond Ring in the player's inventory, up to a max of 200 health total. At first glance, this code seem correct, but it actually always heals for 100 health because of bugs. Lets add some text debugging to identify the issues:
```cs
public override void GetHealLife(Player player, bool quickHeal, ref int healValue) {
	int numberOfDiamonds = player.CountItem(ItemID.DiamondRing);
	Main.NewText($"numberOfDiamonds: {numberOfDiamonds}.");
	float healMultiplier = numberOfDiamonds / 10;
	Main.NewText($"healMultiplier initial: {healMultiplier}.");
	healMultiplier = Utils.Clamp(healMultiplier, 0f, 1f);
	Main.NewText($"healMultiplier after Clamp: {healMultiplier}.");
	healValue = 100 + (int)(100 * healMultiplier);
	Main.NewText($"healValue: {healValue}.");
}
```
This code results in the following output when the player is holding 5 Diamond Rings:    
![image](https://github.com/tModLoader/tModLoader/assets/4522492/9c403f63-7b85-43f5-afd9-aeb8cebc4094)    
For some reason the `CountItem` method is only counting up to 1. Lets look at the documentation for the method:    
![image](https://github.com/tModLoader/tModLoader/assets/4522492/4b2dcb64-1308-43ac-b97f-5e08df04f405)    
It seems that there is a 2nd parameter called `stopCountingAt`. We want to count up to 10, so lets fix this issue by changing that line to `int numberOfDiamonds = player.CountItem(ItemID.DiamondRing, 10);` and try again:    
![image](https://github.com/tModLoader/tModLoader/assets/4522492/48eb0b0b-f6e6-492e-af6a-ba3e6236c7cd)    
The value of `numberOfDiamonds` is now correct, but `healMultiplier` is still incorrect. It seems that `float healMultiplier = numberOfDiamonds / 10;` results in a value of `0` when `numberOfDiamonds` is `1`! This is not what we want. This is caused by [integer division](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/arithmetic-operators#integer-division), we can fix this by changing `numberOfDiamonds / 10;` to `numberOfDiamonds / 10f;`. Lets do that fix and try again:    
![image](https://github.com/tModLoader/tModLoader/assets/4522492/0ec58e15-7cdc-43bb-9788-29b71281935f)    
It's all working correctly now. 

In this example we used text debugging to determine where bugs were happening. Using a similar approach, you should be able to locate logic errors in your mod's code. Text debugging is a useful tool, but can be quite tedious. Typing out code to output text to the chat isn't very efficient. Once you are familiar with the basic concept of debugging, you should advance to using [Debugger Debugging](#Debugger-Debugging) as soon as possible. 

## Clean Up
Once text debugging has served it's purpose, remember to remove text debugging code from your mod. Nothing is more annoying for your users than seeing strange text appear in the chat during seemingly random circumstances. 
 
There are some situations where the text debugging code might be useful later. One approach is to use a [preprocessor directive](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/preprocessor-directives) to hide the code in the release build of the mod. With this approach, the code will be present when debugging from Visual Studio, but will not be present in the mod when built in-game:    
```cs
#if DEBUG
	Main.NewText("This text will only show for debug builds");
#endif
```
Sometimes debug text can be useful if you are remotely helping a user of your mod diagnose an issue. Sending them a special build of the mod with the debug text can be useful, but another approach is to provide a config that toggles additional debug logging. For example, the `BossChecklist` mod uses [a config setting](https://github.com/search?q=repo%3AJavidPack%2FBossChecklist%20ModCallLogVerbose%20&type=code) to control if text will be logged. This toggle allows mods integrating with that mod a way to diagnose their `Mod.Call` code.

# Debugger Debugging
In the Text Debugging section, we learned to output specific values to the chat to identify and fix a couple of bugs. That process is tedious because the modder needs to rebuild the mod each time they make a change. In this section we will learn to leverage the power of a debugger (Visual Studio) to debug our mods. This section assumes the modder is using [Visual Studio](https://github.com/tModLoader/tModLoader/wiki/Developing-with-Visual-Studio) and has correctly set it up, please consult that guide to follow those prerequisites.

## Launching the Debugger
To debug, first make sure tModLoader is closed, then simply click the button labeled "Terraria" or press F5:

# Miscellaneous Info

Here are a couple example videos until someone writes this:

Discovering the exact cause of `Object reference not set to an instance of an object` errors: In this [video](https://gfycat.com/CluelessFineDarklingbeetle), we can easily diagnose where the error is happening. When the error happens in the game, Visual Studio is brought to the front and the line where the error occurs is highlighted. We can then hover over things and inspect their values. Here we notice that `instance` is null.

Now we can fix the error, we can use `Find all references` to confirm that yes, we forgot to set `instance` to a value. You might notice the green underline under `instance` warning us that `instance` is never assigned. We should have paid attention! Now we can easily fix the error: [video](https://gfycat.com/RedBlackCollardlizard)