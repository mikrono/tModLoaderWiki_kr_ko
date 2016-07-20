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

Allows you to determine which textures make up the background by assigning their background slots/IDs to the given array. Mod.GetBackgroundSlot may be useful here.