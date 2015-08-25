# NPCLoader

This serves as the central class from which NPC-related functions are carried out. It also stores a list of mod NPCs by ID.

## Fields

### public static readonly IList\<int\> blockLoot

Allows you to stop an NPC from dropping loot by adding item IDs to this list. This list will be cleared whenever NPCLoot ends. Useful for either removing an item or change the drop rate of an item in the NPC's loot table. To change the drop rate of an item, use the PreNPCLoot hook, spawn the item yourself, then add the item's ID to this list.

## Methods

### public static ModNPC GetNPC(int type)

Gets the ModNPC instance corresponding to the specified type.