# EntityInfo

There are three classes called ItemInfo, ProjectileInfo, and NPCInfo. You can override any of these three classes to add extra information to items, projectiles, or NPCs respectively, similar to how ModPlayer can be used to add extra information to players.

## Properties

### public Mod mod

The mod that has added this type of information storage.

### public string Name

The name of this type of information storage.

## Methods

### public virtual bool Autoload(ref string name)

Allows you to automatically add an ItemInfo, a ProjectileInfo, or an NPCInfo instead of using the respective Add method in Mod. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name. Use this to either force or stop an autoload, or change the name that identifies this type of information storage.

# Subclasses

## ItemInfo

### public virtual ItemInfo Clone()

Returns a clone of this ItemInfo. By default this will return a memberwise clone; you will want to override this if your ItemInfo contains object references.

## ProjectileInfo

### public virtual ProjectileInfo Clone()

Returns a clone of this ProjectileInfo. By default this will return a memberwise clone; you will want to override this if your ItemInfo contains object references.

## NPCInfo

### public virtual NPCInfo Clone()

Returns a clone of this NPCInfo. By default this will return a memberwise clone; you will want to override this if your ItemInfo contains object references.