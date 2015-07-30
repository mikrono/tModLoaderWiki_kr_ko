# GlobalTile

This class allows you to modify the behavior of any tile in the game. Create an instance of an overriding class then call Mod.SetGlobalTile to use this.

## Properties

### public Mod mod

The mod to which this GlobalTile belongs to.

## Methods

### public void AddToArray(ref int[] array, int type)

A convenient method for adding an integer to the end of an array. This can be used with the arrays in TileID.Sets.RoomNeeds.

### public virtual void SetDefaults()

Allows you to modify the properties of any tile in the game. Most properties are stored as arrays throughout the Terraria code.