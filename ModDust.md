# ModDust

This class represents a type of dust that is added by a mod. Only one instance of this class will ever exist for each type of dust you add.

## Properties

### public string Name

The internal name of this type of dust.

### public Texture2D Texture

The sprite sheet that this type of dust uses. Normally a sprite sheet will consist of a vertical alignment of three 10 x 10 pixel squares, each one containing a possible look for the dust.

### public Mod mod

The mod that added this type of dust.

## Methods

### public static int NewDust(Vector2 Position, int Width, int Height, Mod mod, string name, float SpeedX = 0f, float SpeedY = 0f, int Alpha = 0, Color newColor = default(Color), float Scale = 1f)

Creates a new modded dust. The parameters are the same as Terraria.Dust.NewDust, except instead of specifying the type, you must specify the mod and internal name. Note that Position, Width, and Height make up a rectangle which is the range of positions that dust can spawn in.

### public virtual bool Autoload(ref string name, ref string texture)

Allows you to automatically add a type of dust without having to use Mod.AddDust. By default returns the mod's Autoload property. Return true to automatically add the dust. Name will be initialized to the dust's class name, and Texture will be initialized to the dust's namespace and overriding class name with periods replaced with slashes. The name parameter determines the internal name and the texture parameter determines the texture path.

### public virtual void OnSpawn(Dust dust)

Allows you to modify a dust's fields when it is created.

### public virtual bool Update(Dust dust)

Allows you to customize how you want this type of dust to behave. Return true to allow for vanilla dust updating to also take place; will return true by default. Normally you will want this to return false.

### public virtual Color? GetAlpha(Dust dust, Color lightColor)

Allows you to override the color this dust will draw in. Return null to draw it in the normal light color; returns null by default. Note that the dust.noLight field makes the dust ignore lighting and draw in full brightness, and can be set in OnSpawn instead of having to return Color.White here.