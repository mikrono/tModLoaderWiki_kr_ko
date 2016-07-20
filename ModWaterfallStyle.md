# ModWaterfallStyle

Represents a style of waterfalls that gets drawn. This is mostly used to determine the color of the waterfall.

## Properties

### public Mod mod

The mod that added this style of waterfall.

### public string Name

The internal name of this waterfall style.

### public int Type

The ID of this waterfall style.

## Methods

### public virtual bool Autoload(ref string name, ref string texture)

Allows you to automatically add a ModWaterfallStyle instead of using Mod.AddWaterfallStyle. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name, and texture is initialized to the namespace and overriding class name with periods replaced with slashes. Use this to either force or stop an autoload, change the name that identifies this type of ModWaterStyle, or change the texture path used by this ModWaterfallStyle.

### public virtual void AddLight(int i, int j)

Allows you to create light at a tile occupied by a waterfall of this style.

### public virtual void ColorMultiplier(ref float r, ref float g, ref float b, float a)

Allows you to determine the color multiplier acting on waterfalls of this style. Useful for waterfalls whose colors change over time.