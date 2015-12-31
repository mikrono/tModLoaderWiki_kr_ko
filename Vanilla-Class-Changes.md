# Item

## Properties

### public ModItem modItem

The ModItem instance that controls the behavior of this item. This property is null if this is not a modded item.

## Methods

### public void CloneDefaults(int type)

Allows you to copy the defaults of a different type of item.

# Projectile

## Properties

### public ModProjectile modProjectile

The ModProjectile instance that controls the behavior of this projectile. This property is null if this is not a modded projectile.

## Methods

### public void CloneDefaults(int type)

Allows you to copy the defaults of another type of projectile.

# NPC

## Properties

### public ModNPC modNPC

The ModNPC instance that controls the behavior of this NPC. This property is null if this is not a modded NPC.

## Methods

### public void CloneDefaults(int type)

Allows you to copy the defaults of another type of NPC.

# Player

## Methods

### public ModPlayer GetModPlayer(Mod mod, string name)

Gets the ModPlayer instance (with the given name and added by the given mod) associated with this player instance.