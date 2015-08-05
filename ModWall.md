# ModWall

This class represents a type of wall that can be added by a mod. Only one instance of this class will ever exist for each type of wall that is added. Any hooks that are called will be called by the instance corresponding to the wall type.

## Properties

### public Mod mod

The mod which has added this type of ModWall.

### public string Name

The name of this type of wall.

### public ushort Type

The internal ID of this type of wall.

## Methods

### public virtual bool Autoload(ref string name, ref string texture)

Allows you to modify the name and texture path of this wall when it is autoloaded. Return true to autoload this wall. When a wall is autoloaded, that means you do not need to manually call Mod.AddWall. By default returns the mod's autoload property.

### public virtual void SetDefaults()

Allows you to set the properties of this wall. Many properties are stored as arrays throughout Terraria's code.