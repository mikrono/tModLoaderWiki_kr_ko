# Read First

When inter-operating with other mods, there are several things to note. The biggest thing to note is that the Mod you wish to interact with may or may not actually be loaded. Calling methods, accessing fields, or trying to use Items from mods that aren't loaded will cause problems. You can avoid the issue altogether if you make your mod "strongly" depend on the other mod, but this means that your mod can't be loaded without the other mod present. An alternative to a strong dependency is a weak dependency. Weak dependencies are the most difficult to do correctly but also the most powerful. A 3rd option is less powerful than the other 2 but much easier and relies on Mod.Call, a simple way of passing messages between mods. This option requires the mod you wish to operate with to support it. A 4th option is using reflection, and is a bad approach. A 5th option is for simple things like recipes. These options are explained from simplest to strongest below.

# Simple Cross Mod content. Recipes, Items, and Tiles (Intermediate)

The easiest form of cross-mod content is utilizing items or tiles in recipes, shops, or drops. The first thing to note is that the mod may or may not exist. To determine if it exists, we first ask tModLoader for the Mod object.
```cs
ModLoader.TryGetMod("ExampleMod", out Mod exampleMod);
```
The exampleMod object is now either Null or a valid reference to the Mod class of ExampleMod. There are two ways to determine whether a mod is loaded when using `ModLoader.TryGetMod`, you can either use the boolean value returned by the method or check if the `Mod` instance is null. In this example, it's easier to check if the method returned `true`. We will add an item to our Town NPC's shop.
```cs
public override void SetupShop(Chest shop, ref int nextSlot)
{
    // other code
    if (ModLoader.TryGetMod("ExampleMod", out Mod exampleMod))
    {
        shop.item[nextSlot].SetDefaults(exampleMod.ItemType("ExampleWings"));
        nextSlot++;
        // Add more items to the shop from Example Mod
    }
    // other code
```
As you can see, we can use the exampleMod object and invoke the ItemType method to get the correct type for that item. Note that "ExampleWings" corresponds to the internal name of an item, so it may be necessary to ask the other modder so you can get the correct internal name of the items you wish to use. See [Determining Internal Names](#determining-internal-names) below for more approaches. Also be aware that if the mod you are referencing changes the internal name, your mod will break until you fix it. It is recommended that mods expecting cross-mod content refrain from changing fields, methods, and namespaces other mod expect to remain consistent.

Similar code can be used for NPC loot and recipes.

## Recipe Example
See [here](https://github.com/tModLoader/tModLoader/wiki/Intermediate-Recipes#cross-mod-recipes) for a recipe example.

## Determining Internal Names
If you use the Helpful Hotkeys mod and enable the Show Developer Info setting, then use the Query Mod Origin hotkey while hovering over a modded entity in-game, you can determine the internal name of that entity. Another approach is setting a breakpoint after the `ModLoader.GetMod` method call and inspecting the resulting Mod object. For example, you could inspect the `items` dictionary to find the `Name` field of the `ModItem` you are interested.
Another mod you can use is "Which Mod Is This From": Enable everything in its config and it tells you the internal name of that entity on hovering.

# Call, aka Mod.Call (Intermediate)

Call is a method that requires cooperation from both the Called mod and the Calling mod. The Called mod will publish details on the variety of messages they accept and Calling mods wishing to inter-operate with those mods conform to the message format to send messages to the Called mod. To teach this concept, we will inter-operate with the most popular mod utilizing Call: Boss Checklist. (This mod lets us add our mod's bosses to a neat checklist. Very useful for players to know which boss to fight next.) First, we will find the homepage of Boss Checklist. From the mod browser, we are lead to [this page](https://forums.terraria.org/index.php?threads/boss-checklist-in-game-progression-checklist.50668/#post-1123050). Reading this page, we find 2 messages that we can send to Boss Checklist. The page also instructs us to do Call in PostSetupContent, but this could be different for other mods. We now use the same technique as we did earlier and call ModLoader.GetMod to check if the mod is loaded. We must also make sure to follow the message format perfectly or risk errors. Let's now do the code as if we wanted to add Boss Checklist support for ExampleMod:
```cs
public override void PostSetupContent()
{
    Mod bossChecklist = ModLoader.GetMod("BossChecklist");
    if(bossChecklist != null)
    {
        bossChecklist.Call("AddBossWithInfo", "Abomination", 5.5f, (Func<bool>)(() => ExampleWorld.downedAbomination), "Use a [i:" + ModContent.ItemType<Items.Abomination.FoulOrb>() + "] in the underworld after Plantera has been defeated");
        bossChecklist.Call("AddBossWithInfo", "Purity Spirit", 15.5f, (Func<bool>)(() => ExampleWorld.downedPuritySpirit), "Kill a [i:" + ItemID.Bunny + "] in front of [i:" + ModContent.ItemType<Items.Placeable.ElementalPurge>() + "]");
    }
}
```
Now, if we build our mod, we will see that both of our bosses are added to the Boss Checklist checklist. 

Mod.Call is very useful, and is a very easy way for mods to communicate with each other. For info on how to implement receiving Mod.Call so other mods can interact with your mod, see the source code for Boss Checklist or other open source mods.

# Strong References, aka modReferences (Expert)

Strong references are easy, but prevent your mod from being loaded if the referenced mod isn't loaded. Strong References represent a hierarchy of mods where it would make no sense for the referencing mod to be enabled without the referenced mod. To begin, let's imagine we want to reference ExampleMod. First, add a line of "modReferences = ExampleMod" to your build.txt. Second, make sure ExampleMod is downloaded and enabled. Third, we can code as normal, doing whatever you want. As an example, let's make a hotkey that sets Abomination and Purity spirit to defeated. First, we should add a using statement: `using ExampleMod;`. Next, in our ModPlayer.ProcessTriggers, where hotkeys should be processed, we can do this:
```cs
public override void ProcessTriggers(TriggersSet triggersSet)
{
    if (MyMod.ToggleChecklistHotKey.JustPressed)
    {
         ExampleMod.ExampleWorld.downedAbomination = true;
         ExampleMod.ExampleWorld.downedPuritySpirit = true;
    // ...
```
This example is extremely simple, but all manner of things can be done with a mod reference. Calling methods, accessing public variables, using the generic versions of ItemType and NPCType, and so on.

Note: To properly code in Visual Studio, VS needs a reference to the .dll file contained within the .tmod file. Use the menus in tModLoader to extract the mod and find the .dll files. If no .dll file is extracted, the mod author has chosen not to allow it to be unpacked, so ask them nicely for it. Add the reference by right clicking `Dependencies` in the solution explorer, then clicking `Add Reference`, then `Browse...`, then finding the dll and finally clicking `Add`. 

# Weak References, aka weakReferences (Expert)

Weak References have the same capabilities as Strong references, but they don't have the restriction that the referenced mod must be enabled to work. They do, however, necessitate much more careful programming. The process is largely the same, but instead of a "modReferences = ExampleMod" line in build.txt there is a "weakReferences = ExampleMod@0.10" line. The 0.10 thing there is the minimum version that you expect your code to work. For example, in 0.10, a new field might have been added. If you reference this in your mod, but the loaded version is 0.9, the game will crash. This version portion of the weakReference prevents your mod from loading with older versions of the mod. 

Weak References necessitate careful programming. For example, if you have the code "ExampleMod.ExampleWorld.downedAbomination = true;" in a method that is called, but ExampleMod isn't loaded, the game will crash. With Weak References, you have to make sure that variables and classes that might not be loaded are never seen by the virtual machine as it runs the c# code. Some examples:
```cs
shop.item[nextSlot].SetDefaults(ModContent.ItemType<ExampleItem>());
nextSlot++;

// now for a cross-content item
if (BossChecklist.instance.thoriumLoaded)
{
    if (ThoriumMod.ThoriumWorld.downedScout)
    {
        shop.item[nextSlot].SetDefaults(ItemID.Gel);
        nextSlot++;
    }
}
```
In this example, the game will crash. You might think that the check for thoriumLoaded being true would prevent the game from crashing, but what happens is the .Net runtime will try to understand all the code mentioned in this method as it is invoked, and since it can't make sense of ThoriumMod.ThoriumWorld.downedScout, it will crash.

Here is a solution that does work, moving the potentially unresolvable code to a property, effectively preventing the runtime from ever having to know about ThoriumMod.ThoriumWorld.downedScout unless that mod is actually loaded:
```cs
if (BossChecklist.instance.thoriumLoaded)
{
    if (ThoriumModDownedScout)
    {
        shop.item[nextSlot].SetDefaults(ItemID.Gel);
        nextSlot++;
    }
}

public bool ThoriumModDownedScout
{
    get { return ThoriumMod.ThoriumWorld.downedScout; }
}
```
Of course, this relies on thoriumLoaded being correctly set. In Mod.Load, I suggest setting that static bool like this:
```cs
thoriumLoaded = ModLoader.HasMod("ThoriumMod");
```
Weak References are hard, but a neat to do. Many things, however, are much better off handled with Mod.Call. I hope this guide will help you choose the best approach to cross-mod content.

## Testing
You may mistakenly think that your weak reference is working because you disabled the weakly referenced mod and your mod still loads. This is a false positive. To properly test weak references, you must disable the referenced mod and then close and re-open tModLoader. 

## Recommendations
The best practice is to put all code that directly uses a weakReference (potentially optional one), in a separate class. This can make sure the JIT (Just-In-Time) compiler never has to resolve such references when they aren't available, preventing a crash.

## 1.4 Specific Instructions
On 1.4 tModLoader, you'll additionally need to annotate these methods/properties/classes to allow your mod to load. See [JIT Exception weak references](https://github.com/tModLoader/tModLoader/wiki/JIT-Exception#weak-references) for more info.

### ExtendsFromMod
If you are inheriting from a base class in the mod you are weakly referencing, you can use the `[ExtendsFromMod(...)]` attribute to specify that the mod should not be autoloaded or considered at all when mods inspect other mods. Here is an example:    

```cs
[ExtendsFromMod("ExampleMod")]
public class ExampleItemSubclass : ExampleItem
{
}
```
If your `ModX` class depends on another mod being loaded, but not necessarily inheriting from classes in that mod, you can still use `ExtendsFromMod` to prevent it from being loaded.  

# No References, aka reflection (Expert)

Using reflection to do cross mod is not ideal. For one, reflection relies on strings for accessing classes and fields of the target mod. As the target mod updates and changes, your mod will fail as well. This option is not cooperative and can be inefficient. This approach will not be explained here as it is a poor choice. It is much better if you work together with the author(s) of the other mod(s), so they can open up their mod for modifications you want to make with your mod. Using Github together is your best bet!