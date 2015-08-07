# GlobalNPC

This class allows you to modify and use hooks for all NPCs, including vanilla mobs. Create an instance of an overriding class then call Mod.AddGlobalNPC to use this.

## Properties

### public Mod mod

The mod to which this GlobalNPC belongs.

### public string Name

The name of this GlobalNPC instance.

## Methods

### public virtual bool Autoload(ref string name)

Allows you to automatically load a GlobalNPC instead of using Mod.AddGlobalNPC. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name. Use this method to either force or stop an autoload or to control the internal name.

### public virtual void SetDefaults(NPC npc)

Allows you to set the properties of any and every NPC that gets created.

### public virtual bool PreNPCLoot(NPC npc)

Allows you to determine whether or not the NPC will drop anything at all. Return false to stop the NPC from dropping anything. Returns true by default.

### public virtual void NPCLoot(NPC npc)

Allows you to add drops to an NPC when it dies.