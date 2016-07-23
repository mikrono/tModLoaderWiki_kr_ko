# ModTree

This class represents a type of modded tree. The tree will share a tile ID with the vanilla trees (5), so that the trees can freely convert between each other if the soil below is converted. This class encapsulates several functions that distinguish each type of tree from each other.

## Methods

### public virtual int CreateDust()

Return the type of dust created when this tree is destroyed. Returns 7 by default.

### public virtual int GrowthFXGore()

Return the type of gore created to represent leaves when this tree grows on-screen. Returns -1 by default.

### public virtual bool CanDropAcorn()

Whether or not this tree can drop acorns. Returns true by default.

### public abstract int DropWood()

The ID of the item that is dropped in bulk when this tree is destroyed.

### public abstract Texture2D GetTexture()

Return the texture that represents the tile sheet used for drawing this tree.

### public abstract Texture2D GetTopTextures()

Return the texture containing the possible tree tops that can be drawn above this tree.

### public abstract Texture2D GetBranchTextures()

Return the texture containing the possible tree branches that can be drawn next to this tree.