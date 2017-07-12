# Read First

When inter-operating with other mods, there are several things to note. The biggest thing to note is that the Mod you wish to interact with may or may not actually be loaded. Calling methods, accessing fields, or trying to use Items from mods that aren't loaded will cause problems. You can avoid the issue altogether if you make your mod "strongly" depend on the other mod, but this means that your mod can't be loaded without the other mod present. An alternative to a strong dependency is a weak dependency. Weak dependencies are the most difficult to do correctly but also the most powerful. A 3rd option is less powerful than the other 2 but much easier and relies on Mod.Call, a simple way of passing messages between mods. This option requires the mod you wish to operate with to support it. A 4th option is for simple things like recipes. These options are explained from simplest to strongest below.

# Simple Cross Mod content. Recipes, Items, and Tiles (Intermediate)

The easiest for of cross-mod content is utilizing items or tiles in recipes, shops, or drops. The first thing to note is that the mod may or may not exist. To determine if it exists, we first ask tModLoader for the Mod object.

    Mod exampleMod = ModLoader.GetMod("ExampleMod");

The exampleMod object is now either Null or a valid reference to the Mod class of ExampleMod. We want to check if exampleMod is null, and if it isn't we can do other things with it. In this example, we will add an item to our Town NPC's shop.

    public override void SetupShop(Chest shop, ref int nextSlot)
    {
        // other code
        Mod exampleMod = ModLoader.GetMod("ExampleMod");
        if (exampleMod != null)
        {
            shop.item[nextSlot].SetDefaults(exampleMod.ItemType("ExampleWings"));
            nextSlot++;
            // Add more items to the shop from Example Mod
        }
        // other code
As you can see, we can use the exampleMod object and invoke the ItemType method to get the correct type for that item. Note that "ExampleWings" corresponds to the internal name of an item, so it may be necessary to ask the other modder so you can get the correct internal name of the items you wish to use.

Similar code can be used for NPC loot and recipes.

# Call, aka Mod.Call (Intermediate)

Call is a method that requires cooperation from both the Called mod and the Calling mod. The Called mod will publish details on the variety of messages they accept and Calling mods wishing to inter-operate with those mods conform to the message format to send messages to the Called mod. To teach this concept, we will inter-operate with the most popular mod utilizing Call: Boss Checklist. (This mod lets us add our mod's bosses to a neat checklist. Very useful for players to know which boss to fight next.) First, we will find the homepage of Boss Checklist. From the mod browser, we are lead to [this page](https://forums.terraria.org/index.php?threads/boss-checklist-in-game-progression-checklist.50668/#post-1123050). Reading this page, we find 2 messages that we can send to Boss Checklist. The page also instructs us to do Call in PostSetupContent, but this could be different for other mods. We now use the same technique as we did earlier and call ModLoader.GetMod to check if the mod is loaded. We must also make sure to follow the message format perfectly or risk errors. Let's now do the code as if we wanted to add Boss Checklist support for ExampleMod:

    public override void PostSetupContent()
    {
        Mod bossChecklist = ModLoader.GetMod("BossChecklist");
        if(bossChecklist != null)
        {
            bossChecklist.Call("AddBossWithInfo", "Abomination", 5.5f, (Func<bool>)(() => ExampleWorld.downedAbomination), "Use a [i:" + ItemType<Items.Abomination.FoulOrb>() + "] in the underworld after Plantera has been defeated");
            bossChecklist.Call("AddBossWithInfo", "Purity Spirit", 15.5f, (Func<bool>)(() => ExampleWorld.downedPuritySpirit), "Kill a [i:" + ItemID.Bunny + "] in front of [i:" + ItemType<Items.Placeable.ElementalPurge>() + "]");
        }
    }

Now, if we build our mod, we will see that both of our bosses are added to the Boss Checklist checklist. 

Mod.Call is very useful, and is a very easy way for mods to communicate with each other. For info on how to implement receiving Mod.Call so other mods can interact with your mod, see the source code for Boss Checklist or other open source mods.

# Strong References, aka modReferences (Expert)

TODO

# Weak References, aka weakReferences (Expert)

TODO