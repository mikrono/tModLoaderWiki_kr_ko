# GlobalBgStyle

This class serves to collect functions that operate on any kind of background style, without being specific to one single background style.

## Properties

### public Mod mod

That mod that added this global background style.

### public string Name

The internal name of this global background style.

## Methods

### public virtual bool Autoload(ref string name)

Allows you to automatically add a GlobalBgStyle instead of using Mod.AddGlobalBgStyle. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name. Use this to either force or stop an autoload, or change the name that identifies this type of GlobalBgStyle.

### public virtual void ChooseUgBgStyle(ref int style)

Allows you to change which underground background style is being used.

### public virtual void ChooseSurfaceBgStyle(ref int style)

Allows you to change which surface background style is being used.

### public virtual void FillUgTextureArray(int style, int[] textureSlots)

Allows you to change which textures make up the underground background by assigning their background slots/IDs to the given array. Index 0 is the texture on the border of the ground and sky layers. Index 1 is the texture drawn between rock and ground layers. Index 2 is the texture on the border of ground and rock layers. Index 3 is the texture drawn in the rock layer. The border images are 160x16 pixels, and the others are 160x96, but it seems like the right 32 pixels of each is a duplicate of the far left 32 pixels.

### public virtual void ModifyFarSurfaceFades(int style, float[] fades, float transitionSpeed)

Allows you to modify the transparency of all background styles that exist. The style parameter is the current style that is being used.