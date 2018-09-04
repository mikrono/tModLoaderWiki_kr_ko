## About
This guide is yet another Harmony Transpiler Tutorial. This guide will use Terraria as the target game and will touch lightly upon Labels, Extension Methods, and AccessTools. If you'd like, you can skip right to the bottom and see these sections directly. Read the guide from start to finish if you'd like to follow the thought process of developing this Transpiler.

## Prerequisites
* This guide assumes you have gone through the [original tutorial](https://gist.github.com/pardeike/c02e29f9e030e6a016422ca8a89eefc9).
* [dnSpy](https://github.com/0xd4d/dnSpy) - We will use the compile functionality to help design our patch.
* Familiarity with Terraria is NOT required

## Goal
The goal that this guide will work toward is making the various bee related items in Terraria stronger. For those not familiar with Terraria, various items will spawn bees as weapons. If the player has the `strongBees` ability, bees will have a random chance of spawning as GiantBee instead. This patch will further augment the `strongBees` ability by giving it a chance to spawn a Beenade as well.

Here is how Bee weapons currently work ([video](https://gfycat.com/MagnificentLividHuia)):    
![](https://thumbs.gfycat.com/MagnificentLividHuia-size_restricted.gif)    

Notice how about half of the bees spawn as GiantBees.

## Original Method
Lets first look at the original method to see what changes we'll want to make to the IL code. Start up dnSpy and add the exe by going to File->Open and then browsing to Terraria.exe. Expand the tabs for Terraria, Terraria.exe, Terraria, and finally Player. Scroll down and click on the `beeType()` method. How did I know this was where I need to look? Experience. I followed the logic of the bee weapons in decompiled code and found that beeType is the method that makes the decisions I want to modify. If you are reading this, you probably have already identified a method that you wish to change since you are resorting to Transpiling. By now you should see this:      
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
This is the C# code for this method. If you look in Terraria.ID.ProjectileID, you'll see that 566 is GiantBee and 181 is Bee. Our plan is to change the chosen projectile to Beenade (183) with random chance. This is how the code we want would look:    
```cs
public int beeType()
{
	if (this.strongBees && Main.rand.Next(2) == 0)
	{
		this.makeStrongBee = true;
		if(Main.rand.NextBool(10)) // NextBool is an extension method from Terraria.Utils
			return 183;
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
Lets make sense of this now by following along. Be sure to click on individual instructions to open the [OpCode documentation](https://msdn.microsoft.com/en-us/library/system.reflection.emit.opcodes_fields(v=vs.110).aspx) directly from dnSpy:    
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
![](https://i.imgur.com/XJQROM0.png)    
You'll see some errors if you are missing assembly references or bad code. In our case, we are missing ReLogic.dll. [Download it](https://github.com/blushiemagic/tModLoader/blob/master/references/ReLogic.dll) and then add it to dnSpy via "Add Assembly Reference":    
![](https://i.imgur.com/U6f23SK.png)    
Click "Compile" if you need to and the window will close and you'll be back to the IL Code. Switch back to C# view and you might be surprised to find your changes aren't exactly maintained in the C# view. The logic is all the same, it is just the layout of our code has moved various pieces about.    
```cs
public int beeType()
{
	if (!this.strongBees || Main.rand.Next(2) != 0)
	{
		this.makeStrongBee = false;
		return 181;
	}
	this.makeStrongBee = true;
	if (Main.rand.NextBool(10))
	{
		return 183;
	}
	return 566;
}
```    
Do not worry, however, as our approach for this transpiler is to find the `return 566` in the instructions and add our instructions right before that. This can still be done even though the instructions have moved around a little in our temporarily modified copy of the instructions. Switching back to the IL view, lets find the code between `this.makeStrongBee = true;` and `return 566`. This code now contains the instructions for our if statement and returning 183. Lets annotate these instructions now:    
```cs
// push Player instance onto stack
IL_0015: ldarg.0
// push 1 or true onto stack
IL_0016: ldc.i4.1
// set Player.makeStrongBee
IL_0017: stfld     bool Terraria.Player::makeStrongBee

// NEW: push Main.rand onto stack
IL_001C: ldsfld    class Terraria.Utilities.UnifiedRandom Terraria.Main::rand
// NEW: push 10 onto stack
IL_0021: ldc.i4.s  10
// NEW: Call Utils.NextBool. Why Utils? Utils is the class that defines the NextBool extension method!
IL_0023: call      bool Terraria.Utils::NextBool(class Terraria.Utilities.UnifiedRandom, int32)
// NEW: If result is false, jump to instruction 30.
IL_0028: brfalse.s IL_0030

// NEW: Push 183, aka Beenade, onto stack
IL_002A: ldc.i4    183
// NEW: Return taking Beenade with it
IL_002F: ret

// Push 566, aka GiantBee, onto stack
IL_0030: ldc.i4    566
// Return taking GiantBee with it
IL_0035: ret
```
By annotating the new IL code, we can see that our logic is neatly contained all before the original `return 566` code. Now lets work on Harmony code. Finally!    

## Harmony Patch
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