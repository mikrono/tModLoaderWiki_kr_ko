# ModSurfaceBgStyle

Each background style determines in its own way how exactly the background is drawn. This class serves as a collection of functions for above-ground backgrounds.

## Properties

### public Mod mod

The mod that added this surface background style.

### public string Name

The internal name of this surface background style.

### public int Slot

The ID of this surface background style.

## Methods

### public virtual bool Autoload(ref string name)

Allows you to automatically add a ModSurfaceBgStyle instead of using Mod.AddSurfaceBgStyle. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name. Use this to either force or stop an autoload, or change the name that identifies this type of ModSurfaceBgStyle.

### public virtual bool ChooseBgStyle()

Whether or not the conditions have been met for this background style to draw its backgrounds. Returns false by default.

### public abstract void ModifyFarFades(float[] fades, float transitionSpeed)

Allows you to modify the transparency of all background styles that exist. In general, you should move the index equal to this style's slot closer to 1, and all other indexes closer to 0. The transitionSpeed parameter is what you should add/subtract to each element of the fades parameter. See the ExampleMod for an example.

### public virtual int ChooseFarTexture()

Allows you to determine which texture is drawn in the very back of the background. Mod.GetBackgroundSlot may be useful here, as well as for the other texture-choosing hooks.

### public virtual int ChooseMiddleTexture()

Allows you to determine which texture is drawn in the middle of the background.

### public virtual bool PreDrawCloseBackground(SpriteBatch spriteBatch)

Gives you complete freedom over how the closest part of the background is drawn. Return true for ChooseCloseTexture to have an effect; return false to disable tModLoader's own code for drawing the close background.

### public virtual int ChooseCloseTexture(ref float scale, ref double parallax, ref float a, ref float b)

Allows you to determine which texture is drawn in the closest part of the background. This also lets you modify the scale and parallax (as well as two unfortunately-unknown parameters).