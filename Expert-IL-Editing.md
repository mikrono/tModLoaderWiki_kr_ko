# Introduction
## About
This guide explores IL editing. IL editing is an expert level technique that can be very powerful. With IL editing, mods can essentially edit code anywhere in the Terraria code base without relying on tModLoader hooks. IL editing stands for Intermediate Language editing, essentially we are editing compiled code on demand. This can be useful for very obscure methods that don't warrant a tModLoader method. With IL editing, it is crucial that your code employs defensive programming techniques to properly anticipate the potential of other mods attempting to modify the same area in the code. IL editing is powerful, but always try to utilize the tModLoader hooks if possible as they can facilitate multiple mods better than IL editing. If you aren't scared yet, read on. 

### Limitations
Be aware that the Common Language Runtime (CLR) will in-line short methods at runtime, these methods cannot be edited using these techniques. What counts as short isn't well defined, but things like Properties are likely candidates.

## Prerequisites
* [dnSpy](https://github.com/0xd4d/dnSpy) - We will use the compile functionality to help design our patch.
* [Expert Prerequisites](https://github.com/blushiemagic/tModLoader/wiki/Expert-Prerequisites) - You will fail if you don't know how to debug.
* [Advanced Vanilla Code Adaption](https://github.com/blushiemagic/tModLoader/wiki/Advanced-Vanilla-Code-Adaption) - Familiarity with finding things in the Terraria source code is very helpful.
* Visual Studio or similar IDE is required.

## Code Layout
You can put patches anywhere you want, but `Mod.Load` or any of the `Autoload` methods are good candidates. 

# Example - Hive Pack Upgrade
## Goal
The goal that this guide will work toward is making the various bee related items stronger when wearing an upgrade to the [Hive Pack](https://terraria.gamepedia.com/Hive_Pack). Various items will spawn bees as weapons. If the player is wearing the Hive Pack, `player.strongBees` will be true and spawned bees will have a random chance of spawning as GiantBee instead. To implement our Hive Pack upgrade, we need to modify the code referencing `player.strongBees` to give it a chance to spawn a Beenade as well.

Here is how Bee weapons currently work while the Hive Pack is equipped ([video](https://gfycat.com/MagnificentLividHuia)):    
![](https://thumbs.gfycat.com/MagnificentLividHuia-size_restricted.gif)    

Notice how about half of the bees spawn as GiantBees.

## Original Method
Lets first look at the original method to see what changes we'll want to make to the IL code. Start up dnSpy and add the exe by going to File->Open and then browsing to Terraria.exe. Expand the tabs for Terraria, Terraria.exe, Terraria, and finally Player. Scroll down and click on the `beeType()` method. How did I know this was where I need to look? Experience. I followed the logic of the bee weapons in decompiled code and found that `Player.beeType` is the method that makes the decisions I want to modify. If you are reading this, you probably have already identified a method that you wish to change since you are resorting to IL editing. By now you should see this:      
![](https://i.imgur.com/9fOlM4n.png)    
```cs
public int beeType()
{
	if (this.strongBees && Main.rand.Next(2) == 0)
	{
		this.makeStrongBee = true;
		return 566;
	}
	this.makeStrongBee = false;
	return 181;
}
```
This is the C# code for this method. If you look in `Terraria.ID.ProjectileID`, you'll see that 566 is `GiantBee` and 181 is `Bee`. Our plan is to change the chosen projectile to `Beenade` (183) with random chance. This is how the code we want would look:    
```cs
public int beeType()
{
	if (this.strongBees && Main.rand.Next(2) == 0)
	{
		this.makeStrongBee = true;
		if(this.GetModPlayer<ExamplePlayer>().strongBeesUpgrade && Main.rand.NextBool(10))
			return ProjectileID.Beenade;
		return 566;
	}
	this.makeStrongBee = false;
	return 181;
}
```
This is great, but what now? First, lets use dnSpy to look at the IL Code for this method. In the Menu Bar, click the dropdown combo box and switch from C# to IL. This is what we see:    
```cs
// Token: 0x0600050A RID: 1290 RVA: 0x0023D2D0 File Offset: 0x0023B4D0
.method public hidebysig 
	instance int32 beeType () cil managed 
{
	// Header Size: 1 byte
	// Code Size: 47 (0x2F) bytes
	.maxstack 8

	/* 0x0023B4D1 02           */ IL_0000: ldarg.0
	/* 0x0023B4D2 7B280A0004   */ IL_0001: ldfld     bool Terraria.Player::strongBees
	/* 0x0023B4D7 2C1A         */ IL_0006: brfalse.s IL_0022

	/* 0x0023B4D9 7E19040004   */ IL_0008: ldsfld    class Terraria.Utilities.UnifiedRandom Terraria.Main::rand
	/* 0x0023B4DE 18           */ IL_000D: ldc.i4.2
	/* 0x0023B4DF 6F53090006   */ IL_000E: callvirt  instance int32 Terraria.Utilities.UnifiedRandom::Next(int32)
	/* 0x0023B4E4 2D0D         */ IL_0013: brtrue.s  IL_0022

	/* 0x0023B4E6 02           */ IL_0015: ldarg.0
	/* 0x0023B4E7 17           */ IL_0016: ldc.i4.1
	/* 0x0023B4E8 7D690B0004   */ IL_0017: stfld     bool Terraria.Player::makeStrongBee
	/* 0x0023B4ED 2036020000   */ IL_001C: ldc.i4    566
	/* 0x0023B4F2 2A           */ IL_0021: ret

	/* 0x0023B4F3 02           */ IL_0022: ldarg.0
	/* 0x0023B4F4 16           */ IL_0023: ldc.i4.0
	/* 0x0023B4F5 7D690B0004   */ IL_0024: stfld     bool Terraria.Player::makeStrongBee
	/* 0x0023B4FA 20B5000000   */ IL_0029: ldc.i4    181
	/* 0x0023B4FF 2A           */ IL_002E: ret
} // end of method Player::beeType
```
Lets make sense of this now by following along. Be sure to hover or click on individual instructions to open the [OpCode documentation](https://msdn.microsoft.com/en-us/library/system.reflection.emit.opcodes_fields(v=vs.110).aspx) directly from dnSpy:    
```cs
ldarg.0 pushes the 1st argument onto the stack, but this method has 0 arguments, what is going on? 
Well, non-static methods have the current instance as the 1st argument, 
it is there even though it isn't in the method parameters.
IL_0000: ldarg.0
Next, the value of strongBees is placed over the top item in the stack
IL_0001: ldfld     bool Terraria.Player::strongBees
If that value is false, jump to instruction 22
IL_0006: brfalse.s IL_0022

// Otherwise, lets place the static field Main.rand onto the stack
IL_0008: ldsfld    class Terraria.Utilities.UnifiedRandom Terraria.Main::rand
// and then push 2 onto the stack
IL_000D: ldc.i4.2
// and then call the Next method.
IL_000E: callvirt  instance int32 Terraria.Utilities.UnifiedRandom::Next(int32)
// if the result of that is non-zero, jump to instruction 22.
IL_0013: brtrue.s  IL_0022

// Load the Player instance onto the stack
IL_0015: ldarg.0
// Load 1 into the stack (remember, 1 is true)
IL_0016: ldc.i4.1
// Set Player.makeStrongBee to true
IL_0017: stfld     bool Terraria.Player::makeStrongBee
// Load 566 onto the stack
IL_001C: ldc.i4    566
// Return. Since 566 in on top of the stack it is the value returned
IL_0021: ret

// Load the Player instance onto the stack
IL_0022: ldarg.0
// Load 0 into the stack (remember, 0 is false)
IL_0023: ldc.i4.0
// Set Player.makeStrongBee to false
IL_0024: stfld     bool Terraria.Player::makeStrongBee
// Load 181 onto the stack
IL_0029: ldc.i4    181
// Return. Since 181 in on top of the stack it is the value returned
IL_002E: ret
```
Hopefully this annotated IL Code can help you make sense of things. If you are confused, it might be worth your while learning about stacks and other things related to how computers run instructions.    
Now that we've gotten an idea about the original method, let's use dnSpy to see how our changes will look. In dnSpy, right click on the method and click `Edit Method (C#)...`.     
![](https://i.imgur.com/vKEjicD.png)    
In the window that pops up, make the changes we decided on earlier and then click Compile.    
![](https://i.imgur.com/ojUkfez.png)    
You'll see some errors if you are missing assembly references or bad code. First off, we need to add `using Terraria.ID;` and `using ExampleMod` to the code. Then, we need to add the missing references for Relogic and ExamplePlayer. You'll find ReLogic.dll in `\Documents\My Games\Terraria\ModLoader\references`. You'll find ExampleMod.dll in `\Documents\My Games\Terraria\Modding\tModLoader\ExampleMod\bin\Debug`. Add both of these dlls to dnSpy via "Add Assembly Reference":    
![](https://i.imgur.com/U6f23SK.png)       
Now that we have fixed the errors, click "Compile" and the window will close and you'll be back to the IL Code. Switch back to C# view and you might be surprised to find your changes aren't exactly maintained in the C# view. The logic is all the same, it is just the layout of our code has moved various pieces about.    
```cs
public int beeType()
{
	if (!this.strongBees || Main.rand.Next(2) != 0)
	{
		this.makeStrongBee = false;
		return 181;
	}
	this.makeStrongBee = true;
	if (this.GetModPlayer<ExamplePlayer>().strongBeesUpgrade && Main.rand.NextBool(10))
	{
		return 183;
	}
	return 566;
}
```    
Do not worry, however, as our approach for this patch is to find the `return 566` in the instructions and add our instructions right before that. This can still be done even though the instructions have moved around a little in our temporarily modified copy of the instructions. Switching back to the IL view, lets find the code between `this.makeStrongBee = true;` and `return 566`. This code now contains the instructions for our if statement and returning 183. Lets annotate these instructions now:    
```cs
// push Player instance onto stack
IL_0015: ldarg.0
// push 1 or true onto stack
IL_0016: ldc.i4.1
// set Player.makeStrongBee
IL_0017: stfld     bool Terraria.Player::makeStrongBee

// NEW: push Player instance onto stack
IL_001C: ldarg.0
// NEW: Calls GetModPlayer
IL_001D: call      instance !!0 Terraria.Player::GetModPlayer<class [ExampleMod]ExampleMod.ExamplePlayer>()
// NEW: place the value of strongBeesUpgrade onto the stack
IL_0022: ldfld     bool [ExampleMod]ExampleMod.ExamplePlayer::strongBeesUpgrade
// NEW: If result is false, jump to instruction 3D. (short-circuiting)
IL_0027: brfalse.s IL_003D

// NEW: push Main.rand onto stack
IL_0029: ldsfld    class Terraria.Utilities.UnifiedRandom Terraria.Main::rand
// NEW: push 10 onto stack
IL_002E: ldc.i4.s  10
// NEW: Call Utils.NextBool. Why Utils? Utils is the class that defines the NextBool extension method!
IL_0030: call      bool Terraria.Utils::NextBool(class Terraria.Utilities.UnifiedRandom, int32)
// NEW: If result is false, jump to instruction 3D.
IL_0035: brfalse.s IL_003D

// NEW: Push 183, aka Beenade, onto stack
IL_0037: ldc.i4    183
// NEW: Return taking Beenade with it
IL_003C: ret

// Push 566, aka GiantBee, onto stack
IL_003D: ldc.i4    566
// Return taking GiantBee with it
IL_0042: ret
```
By annotating the new IL code, we can see that our logic is neatly contained all before the original `return 566` code. Now lets work on the patch code. Finally!    

## Patch Code
Since this IL editing will be fairly straightforward, we will detail 3 separate approaches to this patch. Hopefully the repetition will give insight into different ways to approach IL editing. The full code can be explored on (Add link here after pushing to github)

### Common Ideas
The first concept to explore is loading our patch. Since this patch pertains to a new ModItem in our mod, lets add the patch to `ModItem.Autoload`. Simply override `Autoload` and type `IL.Terraria.Player.beeType += HookBeeType;` and then allow Visual Studio to generate the HookBeeType method for us. If Visual Studio doesn't understand the `IL.Terraria` namespace, make sure to add a dll reference to the MonoMod and TerrariaHooks dlls found in `Documents\My Games\Terraria\ModLoader\references`. 

Next, we begin writing code. To start, we create a Cursor by writing `var c = il.At(0);`. A Cursor allows us to navigate the IL codes in a well organized manner. We need to use Cursor methods such as `TryGotoNext` and `GotoLabel` to navigate to the correct index within the list of IL instructions. We can't rely on hard-coded indexes because we need our patch to work properly when multiple patches edit the same method, or when different builds of tModLoader slightly change the IL instructions. Designing robust patch code is expected, as this is an Expert level technique. 

After creating a cursor, we need to advance the cursor to be pointing at the area of code we desire to edit. As we have discovered through compiling in dnSpy, we want to insert our code between the code that sets `makeStrongBee` to true and the code that returns 566. We can write `if (!c.TryGotoNext(i => i.MatchLdcI4(566)))` to advance the cursor to the next IL instructions that matches the OpCode of `ldc.i4` with the operand of `566`. If such an instruction is not found, we would want to exit our patch and possibly log our patch failure to the logs, otherwise, we continue onto our edits in any of the following approaches.

### Approach 1 - Modifying Evaluation Stack
This first approach is the simplest. In this approach, we can take advantage of the fact that while `return 566;` is a single line in c#, in IL instructions, it consists of 2 instructions, the first pushing 566 to the stack, and the 2nd returning from the method. By taking advantage of this, we can insert instructions in between those 2 instructions to achieve our desired behavior. In effect, we are changing `return 566;` to `return (this.GetModPlayer<ExamplePlayer>().strongBeesUpgrade && Main.rand.NextBool(10)) ? 183 : 566;`. The first line of code here moves the cursor down: `c.Index++;`. The cursor was pointing at the instruction pushing 566 to the stack earlier, so increasing the index places the cursor right on the ret OpCode. After this, we call `.Emit` on the cursor with an OpCode, which places the specified OpCode at the current cursor index and pushes all the other instructions down, similar to List.Insert. The instruction we provide is Ldarg_0, which will push the current Player instance onto the stack because this is a non-static method. At this point, the stack consists of the Player at the top and an int with the original return value below that.     
With an int and Player on the stack, we can now use `.EmitDelegate` to write c# code for the rest of our patch, greatly simplifying things. The generic types provided to the Delegate need to match up with the current stack, in order from bottom to top (oldest to most recently pushed). In this case, we will be using a `Func` which takes 2 parameters and returns 1 parameter. The 2 input parameters must be `int` and `Player` as those match the current stack. The output type will be `int` because it will go onto the stack after the int and Player are popped off. When our patch began, there was an int on the stack, so we need to make sure the stack is still the same when our patch completes so we don't crash the game. In our delegate, we simply put our conditional and use the provided original return value and Player instance to drive our logic. Here is the complete code:
```cs
// Start the Cursor at the start
var c = il.At(0);
// Try to find where 566 is placed onto the stack
if (!c.TryGotoNext(i => i.MatchLdcI4(566)))
	return; // Patch unable to be applied

// Move the cursor after 566 and onto the ret op.
c.Index++; 
// Push the Player instance onto the stack
c.Emit(Mono.Cecil.Cil.OpCodes.Ldarg_0);
// Call a delegate using the int and Player from the stack.
c.EmitDelegate<Func<int, Player, int>>((returnValue, player) =>
{
	// Regular c# code
	if (player.GetModPlayer<ExamplePlayer>().strongBeesUpgrade && Main.rand.NextBool(10) && Main.ProjectileUpdateLoopIndex == -1)
		return ProjectileID.Beenade;
	return returnValue;
});
// After the delegate, the stack will once again have an int and the ret instruction will return from this method
```

### Approach 2 - Labels and Branches
This next approach is very similar to approach 1, but aims to show how branching works via labels. If you remember from our exploration of the IL code above, IL instructions often use the `brfalse` OpCodes to conditionally jump to different instructions. When we make if statements in c#, the compiler implements those as jumps to different areas of code. With IL editing, we can define Labels that our branching code can jump to. The code is below. We can see that in effect, this approach more similarly matches the typical c# code approach by more closely mimicking the behavior of an if statement inserted before `return 566;` in the original code. While this is a simple example, using labels and branching may prove a useful skill.
```cs
// Make a label to use later
var label = il.DefineLabel();
// Push the Player instance onto the stack
c.Emit(Mono.Cecil.Cil.OpCodes.Ldarg_0);
// Call a delegate popping the Player from the stack and pushing a bool
c.EmitDelegate<Func<Player, bool>>(player =>
{
	if (player.GetModPlayer<ExamplePlayer>().strongBeesUpgrade && Main.rand.NextBool(10) && Main.ProjectileUpdateLoopIndex == -1)
		return true;
	return false;
});
// if the bool on the stack is false, jump to label
c.Emit(Mono.Cecil.Cil.OpCodes.Brfalse, label);
// Otherwise, push ProjectileID.Beenade and return
c.Emit(Mono.Cecil.Cil.OpCodes.Ldc_I4, ProjectileID.Beenade);
c.Emit(Mono.Cecil.Cil.OpCodes.Ret);
// Set the label to the current cursor, which is still the instruction pushing 566 (which is followed by Ret)
c.MarkLabel(label);
```

### Approach 3 - Direct OpCode





# Old tutorial below

If you skipped to here, note that we now know what instructions we want to add and where. Now we will make the Transpiler code. We'll begin with the basic framework, annotating our static class with `HarmonyPatch` Attributes to specify the target method:     
```cs
/// <summary>
/// Patches Player.beeType to return a Beenade 10% of the time if Player.strongBees is true
/// </summary>
[HarmonyPatch(typeof(Player))]
[HarmonyPatch("beeType")]
public class Player_beeType_Patcher
{
	static IEnumerable<CodeInstruction> Transpiler(IEnumerable<CodeInstruction> instructions)
	{
		var code = new List<CodeInstruction>(instructions);

		// We'll need to modify code here.

		return code;
	}
}
```
We now need to decide where to put our code. Since other patches might occur in this same method, we need to be careful to preserve functionality. We also need to be aware that updates to the game we are patching, Terraria, might also change the specific instructions our patch will see as input to the Transpiler. We are fairly confident that GiantBee won't change from 566, so lets use the return of 566 as our way of finding a suitable location to insert our additional logic. Using a for loop and checking opcodes and operands, we can easily find this location:    
```cs
int insertionIndex = -1;
for (int i = 0; i < code.Count - 1; i++) // -1 since we will be checking i + 1
{
	if (code[i].opcode == OpCodes.Ldc_I4 && (int)code[i].operand == 566 && code[i + 1].opcode == OpCodes.Ret)
	{
		insertionIndex = i;
		break;
	}
}
```
Having found an appropriate index, now we must add instructions. I'm putting them in a List for easy insertion later:    
```cs
// Create a List
var instructionsToInsert = new List<CodeInstruction>();

// To translate the opcode output we saw earlier in dnSpy, for the most part we can copy things straight over. Comments below explain exactly how.

// IL_001C: ldsfld    class Terraria.Utilities.UnifiedRandom Terraria.Main::rand
// To translate instruction, we need a FieldInfo. Typing Main.rand here would be an error. 
// Using AccessTools, we can easily derive the FieldInfo from Type and field name. 
// Using nameof instead of a string helps prevent typos.
instructionsToInsert.Add(new CodeInstruction(OpCodes.Ldsfld, AccessTools.Field(typeof(Main), nameof(Main.rand))));
// IL_0021: ldc.i4.s  10
// Make sure to cast to sbyte here because the documentation for Ldc_I4_S says it expects an int8. 
// Incorrect casts could cause issues.
instructionsToInsert.Add(new CodeInstruction(OpCodes.Ldc_I4_S, (sbyte)10));
// Even though we call the Extension method via Main.rand.NextBool, the method actually resides in Utils. 
// We use AccessTools again here, this time for MethodInfo.
// IL_0023: call      bool Terraria.Utils::NextBool(class Terraria.Utilities.UnifiedRandom, int32)
instructionsToInsert.Add(new CodeInstruction(OpCodes.Call, AccessTools.Method(typeof(Utils), "NextBool", new Type[] { typeof(UnifiedRandom), typeof(int) })));
// IL_0028: brfalse.s IL_0030
// What is IL_0030? How do we add this? We'll learn this next.
instructionsToInsert.Add(new CodeInstruction(OpCodes.Brfalse_S, ???));
// IL_002A: ldc.i4    183
instructionsToInsert.Add(new CodeInstruction(OpCodes.Ldc_I4, 183));
// IL_002F: ret
instructionsToInsert.Add(new CodeInstruction(OpCodes.Ret));
```
In our first pass here, we almost got everything translated into code, the only thing we still don't know how to do is make the `Brfalse_S` opcode jump to the original `ldc.i4` instruction for 183/GiantBee. For now, lets finish off the code before talking about Labels. Lets take this List of CodeInstruction and insert them into the code variable and then return it:    
```cs
if (insertionIndex != -1)
{
	code.InsertRange(insertionIndex, instructionsToInsert);
}
return code;
```
Now lets talk about Labels. You might assume that you'll have to do some math to calculate instruction sizes and edit the values of the jumps in various branching statements. Luckily, this is not the case. This is a case where Harmony augments the instructions provided to the Transpiler to simplify things. Each CodeInstruction that has code jumping to it contains an entry in its CodeInstruction.labels for each CodeInstruction jumping to it. Using this `Label` class, we can modify where jumps jump to without calculating resulting instruction indexes. For our example, we need to jump to the `return 183` code, but no Label currently points to that line of IL Code.
The first step is adding `ILGenerator il` to our Transpiler method parameters. After this, we can make a new label by calling `Label return566Label = il.DefineLabel();` We then need to add this label to the instruction that we want to jump to later. In our code, this will be the `code[i].labels.Add(return566Label);` line. Finally, we can use this label as the operand to our `Brfalse_S` opcode from before: `instructionsToInsert.Add(new CodeInstruction(OpCodes.Brfalse_S, return566Label));`     

Here is the final result:    
```cs
/// <summary>
/// Patches Player.beeType to return a Beenade 10% of the time if Player.strongBees is true
/// </summary>
[HarmonyPatch(typeof(Player))]
[HarmonyPatch("beeType")]
public class Player_beeType_Patcher
{
	static IEnumerable<CodeInstruction> Transpiler(IEnumerable<CodeInstruction> instructions, ILGenerator il)
	{
		var code = new List<CodeInstruction>(instructions);

		int insertionIndex = -1;
		Label return566Label = il.DefineLabel();
		for (int i = 0; i < code.Count - 1; i++) // -1 since we will be checking i + 1
		{
			if (code[i].opcode == OpCodes.Ldc_I4 && (int)code[i].operand == 566 && code[i + 1].opcode == OpCodes.Ret)
			{
				insertionIndex = i;
				code[i].labels.Add(return566Label);
				break;
			}
		}

		var instructionsToInsert = new List<CodeInstruction>();

		instructionsToInsert.Add(new CodeInstruction(OpCodes.Ldsfld, AccessTools.Field(typeof(Main), nameof(Main.rand))));
		instructionsToInsert.Add(new CodeInstruction(OpCodes.Ldc_I4_S, (sbyte)10));
		instructionsToInsert.Add(new CodeInstruction(OpCodes.Call, AccessTools.Method(typeof(Utils), "NextBool", new Type[] { typeof(UnifiedRandom), typeof(int) })));
		instructionsToInsert.Add(new CodeInstruction(OpCodes.Brfalse_S, return566Label));
		instructionsToInsert.Add(new CodeInstruction(OpCodes.Ldc_I4, 183));
		instructionsToInsert.Add(new CodeInstruction(OpCodes.Ret));

		if (insertionIndex != -1)
		{
			code.InsertRange(insertionIndex, instructionsToInsert);
		}
		return code;
	}
}
```    

Lets watch a Bee weapon in action after applying our patch ([video](https://gfycat.com/QuerulousParchedJaguar)):    
![](https://thumbs.gfycat.com/QuerulousParchedJaguar-size_restricted.gif)    

As a reminder, this is how it used to act ([video](https://gfycat.com/MagnificentLividHuia)):    
![](https://thumbs.gfycat.com/MagnificentLividHuia-size_restricted.gif)    

## Recap
### AccessTools
AccessTools provides easy access to FieldInfo and MethodInfo. If you are familiar with Reflection using System.Reflection, it is the same thing. 

### Extension Methods
You might assume that the IL code to call an extension method would need a MethodInfo from the extended class, but remember that it is the class that adds the extension method that contains the MethodInfo. The first parameter will be the original class. 

### Labels
Labels are a Harmony abstraction that simplifies jumps in branching code. This abstraction serves to simplify the work patches need to do to jump to the correct label. Since patches insert and delete IL Code, it would be very hard to properly maintain the jumps in branching code. With Harmony, the operand of branching opcodes are in fact replaced with Label instances. These Labels are swapped in in the place of the operands and are also added to the targeted CodeInstruction's `labels` field. Make sure to add `ILGenerator il` as a parameter to your Transpiler method so you can utilize `Label myLabel = il.DefineLabel();` to create a new Label. After creating a new Label, you need to add it to both the source and target CodeInstruction instances. For the CodeInstruction with the branching opcode, use the Label as the operand. For the CodeInstruction the opcode is branching to, be sure to add it to the labels field: `code[i].labels.Add(myLabel);`. After the Transpiler method executes, Harmony will swap the Labels out and calculate correct values for branching opcodes.

# Example - Lava Snail Statue Spawn
See [ExampleCritter.cs](https://github.com/blushiemagic/tModLoader/blob/master/ExampleMod/NPCs/ExampleCritter.cs) for another IL editing patch example. This example is much trickier as the method we want to patch is very large. The example is well commented and shows a more complex example of instruction targeting.

# Other Examples
The following examples from other mods can be explored as well:
* Add to this list