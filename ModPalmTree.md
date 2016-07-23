# ModPalmTree

This class represents a type of modded palm tree. The palm tree will share a tile ID with the vanilla palm trees (323), so that the trees can freely convert between each other if the sand below is converted. This class encapsulates several functions that distinguish each type of palm tree from each other. Use ModTile.SetModPalmTree or GlobalTile.AddModPalmTree to make a tile able to grow this kind of palm tree.

## Methods

### public virtual int CreateDust()

Return the type of dust created when this palm tree is destroyed. Returns 215 by default.

### public virtual int GrowthFXGore()

Return the type of gore created to represent leaves when this palm tree grows on-screen. Returns -1 by default.

### public abstract int DropWood()

The ID of the item that is dropped in bulk when this palm tree is destroyed.

### public abstract Texture2D GetTexture()

Return the texture that represents the tile sheet used for drawing this palm tree.

### public abstract Texture2D GetTopTextures()

Return the texture containing the possible tree tops that can be drawn above this palm tree.