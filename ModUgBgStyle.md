# ModUgBgStyle

Each background style determines in its own way how exactly the background is drawn. This class serves as a collection of functions for underground backgrounds.

## Properties

### public Mod mod

The mod that added this underground background style.

### public string Name

The internal name of this underground background style.

### public int Slot

The ID of this underground background style.

## Methods

### public virtual bool Autoload(ref string name)

Allows you to automatically add a ModUgBgStyle instead of using Mod.AddUgBgStyle. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name. Use this to either force or stop an autoload, or change the name that identifies this type of ModUgBgStyle.

### public virtual bool ChooseBgStyle()

Whether or not the conditions have been met for this background style to draw its backgrounds. Returns false by default.

### public abstract void FillTextureArray(int[] textureSlots)

Allows you to determine which textures make up the background by assigning their background slots/IDs to the given array. Mod.GetBackgroundSlot may be useful here. Index 0 is the texture on the border of the ground and sky layers. Index 1 is the texture drawn between rock and ground layers. Index 2 is the texture on the border of ground and rock layers. Index 3 is the texture drawn in the rock layer. The border images are 160x16 pixels, and the others are 160x96, but it seems like the right 32 pixels of each is a duplicate of the far left 32 pixels.