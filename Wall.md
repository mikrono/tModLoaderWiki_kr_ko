# What is a Wall?
Walls are fairly straightforward and creating a `ModWall` is mostly an exercise in following the existing patterns. Like tiles, walls are comprised of 2 parts, the `ModItem` that places the wall, and the `ModWall` itself.

## Wall-Item Pairing
An `Item` will place a specific `Wall` when `Item.createWall` is set to the `WallType` of the `ModWall`. The `ModWall` will typically return the `ModItem` as well. Simply set `ItemDrop = ModContent.ItemType<ItemName>());` in `ModWall.SetStaticDefaults`.

# Basic Example
`ExampleWall` in ExampleMod is a basic example. It has 4 files:
* [ExampleMod/Content/Walls/ExampleWall.cs](https://github.com/tModLoader/tModLoader/blob/1.4/ExampleMod/Content/Walls/ExampleWall.cs) is the actual `ModWall` class
  * Note: You can write `Item.DefaultToPlacableWall((ushort)ModContent.WallType<Walls.ExampleWall>());` in `ModWall.SetStaticDefaults` instead of all the existing code if you want to write simpler code.
* [ExampleMod/Content/Walls/ExampleWall.png](https://github.com/tModLoader/tModLoader/blob/1.4/ExampleMod/Content/Walls/ExampleWall.png) is the texture for the `ModWall`
* [ExampleMod/Content/Items/Placeable/ExampleWall.cs](https://github.com/tModLoader/tModLoader/blob/1.4/ExampleMod/Content/Items/Placeable/ExampleWall.cs) is the corresponding `ModItem` class that places the `ExampleWall` `ModWall`.
* [ExampleMod/Content/Items/Placeable/ExampleWall.png](https://github.com/tModLoader/tModLoader/blob/1.4/ExampleMod/Content/Items/Placeable/ExampleWall.png) is the texture for the `ExampleWall` `ModItem`.

# Texture
The texture for a wall has several sections for drawing the wall in different positions depending on which neighbors are the same wall type. Notice how for each orientation, there are 3 options. ExampleWall is based off of Gemspark walls since it is simple to comprehend. Consult other [existing wall textures](https://github.com/tModLoader/tModLoader/wiki/Intermediate-Prerequisites#vanilla-texture-file-reference) for more detailed examples.
![](https://github.com/tModLoader/tModLoader/blob/1.4/ExampleMod/Content/Walls/ExampleWall.png)

If `Main.wallLargeFrames` is used, a 4th option is added to the mix for each layout orientation to add even more variety. That template is shown here:    
![Wall_179](https://user-images.githubusercontent.com/4522492/191140372-60a897bf-f69c-4615-bab5-57cefc84066c.png)

## Animated
If the wall is animated, the pattern is repeated. This is only supported for non-`Main.wallLargeFrames` walls.
![Wall_225](https://user-images.githubusercontent.com/4522492/191140761-06536d43-3b87-4ffc-b036-d3b9cf1739f0.png)

# Wall Properties
In `ModWall.SetStaticDefaults`, various data can be assigned to affect the behavior of the wall.
### Main.wallHouse
If true, the wall is considered a safe wall, used for various things.
### Main.wallDungeon 
If true, the wall is considered a dungeon wall.
### Main.wallLight 
If true, light flows in from the background, such as fences and glass.
### Main.wallLargeFrames 
If 1, the "Phlebas" pattern is used (Plating-type walls). If 2, the "Lazure" pattern is used. These both use the 4th layout orientation option.
### Terraria.ID.WallID.Sets.BlendType
Assign to a known WallID to blend with similar walls.

# Animated
As seen above, the texture for animated walls has several copies of the template. The wall will animate in accordance with the timing and pattern provided in the `ModWall.AnimateWall` hook.

## Example
This example loops 7 frames of animation, switching frames every 10 frames.
```cs
public override void AnimateWall(ref byte frame, ref byte frameCounter) {
	frameCounter++;
	if (frameCounter >= 10) {
		frameCounter = 0;
		frame++;
		if (frame > 7)
			frame = 0;
	}
	// Or, more succinctly:
	if (++frameCounter >= 10) {
		frameCounter = 0;
		frame  = ++frame % 7;
	}
}
```

# Safe vs Unsafe
Many Terraria walls have safe and unsafe variants. The safe wall variant is the wall placed by the item the player receives when mining the unsafe wall. The unsafe wall is placed during world generation and usually can't be placed by the player. The unsafe wall is used in NPC Spawning calculations. This separation lets players mine walls and use them for decoration without risking enabling various enemies from specifically spawning in their creations. As a mod developer, this pattern is good to follow if NPC spawning logic takes wall types into account.

# Custom Framing
With `ModTile.WallFrame`, a modder can implement whatever framing logic they desire. This example shows weighting the 3rd option more than the other 2 options:    
```cs
TODO: Example here
```