# ModWaterStyle

Represents a style of water that gets drawn, based on factors such as the background. This is used to determine the color of the water, as well as other things as determined by the hooks below.

## Properties

### public Mod mod

The mod that added this style of water.

### public string Name

The internal name of this water style.

### public int Type

The ID of the water style.

## Methods

### public virtual bool Autoload(ref string name, ref string texture, ref string blockTexture)

Allows you to automatically add a ModWaterStyle instead of using Mod.AddWaterStyle. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name, and texture is initialized to the namespace and overriding class name with periods replaced with slashes. BlockTexture is initialized to texture with "_Block" added at the end. Use this to either force or stop an autoload, change the name that identifies this type of ModWaterStyle, and/or change the texture paths used by this ModWaterStyle.

### public virtual bool ChooseWaterStyle()

Whether the conditions have been met for this water style to be used. Typically Main.bgStyle is checked to determine whether a water style should be used. Returns false by default.

### public abstract int ChooseWaterfallStyle()

The ID of the waterfall style the game should use when this water style is in use.

### public abstract int GetSplashDust()

The ID of the dust that is created when anything splashes in water.

### public abstract int GetDropletGore()

The ID of the gore that represents droplets of water falling down from a block.

### public virtual void LightColorMultiplier(ref float r, ref float g, ref float b)

Allows you to modify the light levels of the tiles behind the water. The light color components will be multiplied by the parameters.

### public virtual Color BiomeHairColor()

Allows you to change the hair color resulting from the biome hair dye when this water style is in use.