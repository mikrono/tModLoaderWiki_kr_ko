# GlobalNPC

This class allows you to modify and use hooks for all NPCs, including vanilla mobs. Create an instance of an overriding class then call Mod.SetGlobalNPC to use this.

## Properties

### public Mod mod

The mod to which this GlobalNPC belongs.

## Methods

### public virtual bool PreNPCLoot(NPC npc)

Allows you to determine whether or not the NPC will drop anything at all. Return false to stop the NPC from dropping anything. Returns true by default.

### public virtual void NPCLoot(NPC npc)

Allows you to add drops to an NPC when it dies.