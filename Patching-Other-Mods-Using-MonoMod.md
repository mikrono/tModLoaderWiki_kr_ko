___
[New to IL editing? Read our other guide about IL editing first, click here!](Expert-IL-Editing)
___

## Introduction
This guide will explore the use of MonoMod to patch the code of other mods - a very useful tool for compatibility reasons, as well as a means to allow your mod to better interact with others. Using this, you are essentially given complete freedom of choice as to what changes you wish to make. It involves using techniques that will be familiar to experienced modders - `On` and `IL`, and I recommend trying it with vanilla before moving on to other mods, so you get a grasp of how it works. The expert IL guide can be found [here](https://github.com/tModLoader/tModLoader/wiki/Expert-IL-Editing).

## Word of Warning
Note that the third rule of the Mod Browser outlines that: **Your mod will not do anything else deemed illicit, inappropriate, or harmful.
(Yes, we check the decompiled source of mods from time to time. If your code is obfuscated to prevent easy checking we may ban you if we decide to.) This also includes irresponsible use of reflection, Cecil, or other code that prevents harmonious mod coexistence.** This means that it is prohibited to use any of the techniques mentioned in this guide to deliberately prevent another mod from working, or otherwise compromise it. If you do, we will find out and you will be banned from the browser. There are some exceptions, for example deliberately disabling/alter a mechanic because it would crash the game etc. But don't play around!

## Prerequisites
If you are going to be IL editing specifically, then the prerequisites for that are identical to those mentioned in the IL guide (linked above). However, as we are editing other mods, there are also a few more things that need to be done, and they will be listed below.

## Step 1: Creating an MMHook dll
An MMHook dll is a special reference file, that contains the `On` and `IL` variants of all the methods in your chosen mod. It is highly important that you have one, or it will make the process much more difficult. To acquire one, you must follow these steps:
* Get the dll of your chosen mod. You can do this by extracting ingame. Go to the Mods menu, and click the question mark icon on the mod of your choice, which will bring you to the Mod Info screen. Once you've done this, you will see an Extract button: ![](https://i.imgur.com/SxAm09D.png)
* Click it, and the extraction will begin. Once it's complete, the dll will be output to the path `C:\Users\%userprofile%\Documents\My Games\Terraria\ModLoader\references\mods`.


* Download MonoMod from [here](https://github.com/MonoMod/MonoMod/releases). Always download the latest version for `net40`, rather than `net35`. Extract it to a location of your choice.
* Copy the mod dll you extracted previously into the MonoMod folder, and drag and drop it on top of the `MonoMod.RuntimeDetour.HookGen` executable. It should look like this: ![](https://i.imgur.com/PNElana.png)
* This should open a CMD window, which will start creating your MMHook dll. The time taken will vary by hardware and mod size, but it shouldn't take more than a few minutes in most cases.
* Once it is done, you should find a dll in the same folder called `MMHook_NameOfMod_VersionNumber`: ![](https://i.imgur.com/KsAcmRD.png)

## Step 2: Adding the MMHook dll as a Reference
Now that you have the dll, you can add it to your mod. In build.txt, add the line `dllReferences = <Name of MMHook dll>`. The dll must be put in a `lib/` folder in the highest level of your mod's directory, as per the information on `dllReferences` states [here](https://github.com/tModLoader/tModLoader/wiki/build.txt). I also recommend adding `sortAfter = <Name of mod you are editing>`, which will mitigate the risk of your mod trying to apply patches before the other mod is loaded.

 In order to facilitate development, it is advisable to add it as a Visual Studio reference as well. You can do this by navigating to the drop down menu labelled `Dependencies` in the Solution Explorer, right-clicking it, and selecting `Add Reference`. This will give you the option to Browse, and use it to select your MMHook dll and add it as a VS reference.

It is also recommended to add your chosen mod as a reference in Visual Studio, because it will give you access to namespaces and types from that mod without having to use more complex methods such as reflection to get them, although it is entirely possible to do this using only weak references. 

Before beginning, confirm that you also have `MonoMod.RuntimeDetour` and `MonoMod.Utils` as VS references. If you don't, it's perfectly alright - they come with the copy of MonoMod that you downloaded, so simply add them and continue.

In order for your mod to function, the MMHook dll must be included in the mod's `lib/` folder when you publish. This may cause file size issues with editing larger mods, but it is unlikely to be a problem. If you really cannot include the MMHook dll, or do not want any strong references at all, then see the **Addendum** subtitle below. 

## Step 3: Develop!
All the preparations are out of the way, so let's get down to development. I will lay out 2 examples here, one that makes use of `On`, and the other making use of `IL`.

### Example 1: Modifying other mods using `On` hooking
Firstly, let's choose a mod to edit. I'm going to choose the Calamity mod, simply because I have its dll files on hand, but you can do this with any mod you like. Let's start with an empty Mod class:

![](https://i.imgur.com/pj8EMqS.png)

This obviously isn't going to be of much use to us, so let's add some simple code that will weak reference (guide: [cross mod content](https://github.com/tModLoader/tModLoader/wiki/Expert-Cross-Mod-Content)) calamity, such that our patches will only be applied if it is loaded:

![](https://i.imgur.com/XXp8JDC.png)

As you required the mod's dll to get the MMHook one, you will be able to view its source code. You can open the dll in [ILSpy](https://github.com/icsharpcode/ILSpy) and do `File > Save Code` to save a copy of the source. If you are IL editing, you can do the same thing with [dnSpy](https://github.com/0xd4d/dnSpy) to inspect IL code. Now that you have the source code, it's time to choose what you want to change. I have no particular goal in mind here, so I'll just pick something at random - let's say we want our mod to stop the Roxcalibur from spawning and replace it with the merchant NPC, if certain conditions are true.

You should have already found the method you desire, and noted its name, class and namespace. VS autocomplete can now do most of the work for us. Type in `On.` and it should bring up a list of namespaces. You can then simply type and navigate to the one you want. Once you've found the exact method you'd like to hook, type `+=` and VS will bring up a prompt to automatically generate a method. Press `TAB` to insert it:

![](https://i.imgur.com/4MCov6h.png)

The auto generated method should resemble this - the body containing only a `throw new NotImplementedException();`:

![](https://i.imgur.com/coyNORY.png)

The parameters of the method will always contain an `orig`, which is short for 'original' and represents the original method. Instance methods will also have a `self` object that represents their instance, although the method I am hooking does not have one, as it is static.

The name `WorldGenerationMethods_PlaceRoxShrine` is a little long-winded, so let's rename it to something simpler - `RoxShrinePatch`. Let's also add a boolean that controls whether we touch the method:

![](https://i.imgur.com/r4c6gbc.png)

Obviously, under real circumstances, this bool would not simply be set to true in our class - something else would likely control it, but this is just an example. Now let's add some more code to our method:

![](https://i.imgur.com/UPzGvhr.png)

The `orig()` call is extremely important here. As orig represents the original, we are going to simply call the original method if we are not applying the patch. If you do not intend to outright stop the method you hook from calling - **always remember to call orig!** Here, orig does not require any parameters, but you will need to supply the correct parameters with other methods (treat it as calling the `base` of a tML hook). If the return type of the method you hook is not void, you will need to write `return orig(...)` instead of simply calling it.

Now, you could write anything you wanted in the body of `if (doPatch)`, and it would execute when Calamity calls this method. As an example, I'll make it spawn a town NPC where it would normally spawn the Roxcalibur. To do this, I'll simply copy the code from Calamity and replace the tile placement with an NPC spawn. This is known as a method swap - because you are replacing a method with your own version of it.

Upon pasting the code in, it is likely that you may see errors; this is when you may realize it is a good idea to strong reference the mod you are editing, as reflection can be nasty.

![](https://i.imgur.com/yKyhlNq.png)

Due to my use of strong references, I can simply add a using directive to fix it. Once you have identified the exact code you want to change, it is simply a matter of replacing it. Luckily for us, this method is very small and it is easy to read, but some other methods are gargantuan and as a result, IL would be easier.

Aha! here's the line in question:

![](https://i.imgur.com/9r9bw0D.png)

Now that we've found it, commenting it out and replacing it was easy to do. Note that the coordinates for the NPC spawn were multiplied by 16, as we wish to convert from tile coordinates to world coordinates (see the [coordinates guide](https://github.com/tModLoader/tModLoader/wiki/Coordinates) for more details). The rest of the code is simply copied from the guide's spawning code in vanilla. Now that we're finished, let's build and run the mod, and generate a world to test out our changes. I'll use the Worldgen Previewer mod to watch the generation in real time.

And here it is! As expected, the merchant was spawned with the same conditions as the Roxcalibur shrine:

![](https://i.imgur.com/EFlh2YT.png)

The code was relatively simple, too - it only involved a small amount of [vanilla adaption](https://github.com/tModLoader/tModLoader/wiki/Advanced-Vanilla-Code-Adaption) and copying from the source of the mod in order to enact our method swap.

### Example 2: Modifying other mods using `IL` editing
IL editing is the next step up from `On` hooking, and it creates almost complete freedom - a modder who can use IL well is able to do almost anything. However, as a trade-off, it is more complex and harder to understand. You must at least have knowledge of how stacks work ([this](https://en.wikipedia.org/wiki/Stack_(abstract_data_type)) article can help), and without being able to read and understand the code of what you are trying to edit, you will fail.

Once again, let's select a method that we'd like to IL edit. I'm going to choose Calamity's lava geysers, with the aim of preventing them from occurring at all. Let's go back to our Mod class. Upon identifying which method controls the geysers, we can hook our IL edit method onto it:

![](https://i.imgur.com/gr758f0.png)

The autocomplete here is identical to `On` hooking, in that it will generate a method with a body that throws a `new NotImplementedException();` - however, the parameters are different. All IL edits will only have one parameter, an `ILContext`. Knowing what it is doesn't particularly matter, but in short it simply gives our `ILCursor` object the correct context. Let's begin by creating that `ILCursor`, as you always should:

![](https://i.imgur.com/TvcQFea.png)

This code instantiates a new `ILCursor` object in the context of the method we wish to edit. Although single-character variable names are usually not good convention, cursor objects usually use them. As you can see, I have also changed the name of the method containing our IL code to make it more readable.

In order to successfully IL edit, you need to be able to view the instructions. This is done by opening the mod dll of the mod you wish to edit in dnSpy and changing the mode from C# to IL. Select the desired method on the drop-down menu on the left to view its IL code.

Now that we have a cursor, the next step is to position the cursor in the correct position such that we can emit our instructions. After examining the `NearbyEffects` method's IL code, we can find this:

![](https://i.imgur.com/JNupHTZ.png)

654 is the `ProjectileID` that represents the geyser fire, and in this instruction, the value is being pushed onto the stack - the instruction `LdcI4` represents an `int32` being pushed. To move the cursor, you should always attempt to use defensive programming such that it's not going to conflict with other possible edits or crash users' games if it does not work. `TryGotoNext` is a good method to use:

![](https://i.imgur.com/ONT0v9c.png)

This is a lot to unpack, so let's work through it slowly. Firstly, we attempt to move the cursor to an instruction that matches our predicate. The predicate is indicated by the lambda operator, `i => i.MatchLdcI4(ProjectileID.Geyser)`. This predicate will look for the instruction where an OpCode of LdcI4 is present, and its value is 654. If the instruction matches the predicate, the cursor will stop and return true. However, if it returns false, meaning it cannot find the instruction that matches the predicate, then it will execute our failsafe - logging the failure and safely exiting the method.

Due to this, we can assume that in any code that runs after the if statement, the cursor will be in the correct position. As those reading should know, stacks have 2 principal actions: push and pop. Our goal here is to pop 654 off the stack, and push onto it a value that will cause a projectile to not spawn when used as a type parameter in a `NewProjectile` call; in this case 0. Let's add some code that will do that for us:

![](https://i.imgur.com/7qmRTG9.png)

In our new code, the cursor index is first incremented by 1, which will move it directly in front of the 'push 654' instruction. Next, we will pop the value 654 off the stack. `OpCodes` are crucial, as they tell the computer what you are actually doing to the stack. The full OpCode documentation can be found [here](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes?redirectedfrom=MSDN&view=netcore-3.1#fields). For small edits like these, you will only need simple instructions. More complex actions such as branching will only be required on larger edits. 

Now that we've removed 654 from the stack, we can push our own value in place of it, since the code still expects a value. If you remember from before, the value pushed used the instruction `LdcI4`, which we know is for an `int32`. Therefore, we need to push our new number on using the same OpCode:

![](https://i.imgur.com/idEomMZ.png)

And now, we should be finished! To confirm, let's test it ingame. Geyser projectiles only spawn in the underworld with Calamity's deathmode enabled, but they're fairly frequent, so let's see if any spawn:

![](https://i.imgur.com/IZnG5V8.png)

As expected, none spawned with the edit active. Usually, a lot of debugging is necessary before IL edits will work, as they often do not, the first time. Errors thrown by it are almost meaningless, and so your best chance if you are stuck is asking somebody else, or attempting to use a different approach such as the simpler `On`.

### Addendum
In rare cases, the method you want to IL edit may not be in any namespaces. This could happen under certain conditions like the method being private. If this happens, you can still IL edit using events and reflection:

![](https://i.imgur.com/7Yw5RII.png)

Here, `Limits` is a private method present in a Calamity class, and `value` is our IL edit. We use reflection to get its `MethodInfo`, and then we add our IL edits to that method using `HookEndpointManager`. To add your edit, simply do what you would normally do but instead of adding to a method in an IL namespace, you add to your event. The operand `+=` will call `add`, while `-=` will call `remove`. This method could also be used if you do not want to strong reference the mod you are editing, or include the MMHook dll in your mod's source, as all that is required is reflection to get the `MethodInfo`. Therefore, if you are proficient at reflection, it is possible to IL edit other mods using nothing but weak references.


