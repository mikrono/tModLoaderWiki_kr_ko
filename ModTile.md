# ModTile

This class represents a _type_ of tile that can be added by a mod. Only one instance of this class will ever exist for each type of tile that is added. Any hooks that are called will be called by the instance corresponding to the tile type. This is to prevent the game from using a massive amount of memory storing tile instances.

## Properties

### public Mod mod

The mod which has added this type of ModTile.

### public string Name

The name of this type of tile.

### public ushort Type

The internal ID of this type of tile.

## Methods

### public void AddToArray(ref int[] array)

A convenient method for adding this tile's Type to the given array. This can be used with the arrays in TileID.Sets.RoomNeeds.

### public virtual bool Autoload(ref string name, ref string texture)

Allows you to modify the name and texture path of this tile when it is autoloaded. Return true to autoload this tile. When a tile is autoloaded, that means you do not need to manually call Mod.AddTile. By default returns the mod's autoload property.

### public virtual void SetDefaults()

Allows you to set the properties of this tile. Many properties are stored as arrays throughout Terraria's code.