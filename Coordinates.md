# Coordinates
Many modders get confused when working with Tiles, Projectiles, or Drawing code because they don't understand the various Coordinates in Terraria. This guide will teach you.

## Common
Common to all coordinates is the fact that the X axis goes up from left to right and the Y axis goes up from top to bottom. **Down is positive Y, up is negative Y**. All coordinates start in the top left of what they represent.

## World Coordinates
Players, NPC, Projectiles, and Dust all reside in world coordinates. World coordinates are typically represented as a `Vector2`, meaning that the `X` and `Y` components of world coordinates are float values, meaning non-whole number values like 3943.23 and 2334.213 are valid values. If you are using 100% screen zoom, each pixel on your monitor is equal to 1 unit of world space. The world coordinate of 0, 0 resides on on the very top left corner of the world. 

## Tile Coordinates
Tiles are in tile coordinates. Tile coordinates are only used in Tile related methods and the `Main.tile` array. Tile coordinates are typically represented as a `Point` or `Point16`, meaning that `X` and `Y` components of tile coordinates must be whole numbers/ints.

## Screen Coordinates
Screen coordinates (actually window coordinates) are used for things like drawing on the screen and user interface elements. A common mistake with manually drawing entities is to draw them using the position of the entity. This doesn't work because the draw code only accepts screen space coordinates, not world space coordinates. Make sure to subtract Main.screenPosition from whatever you are trying to draw to draw it correctly.

### Drawing Tiles
Tiles are drawn not necessarily drawn to the screen all the time. If you are using `ModTile.SpecialDraw` or `PostDraw` or `PreDraw`, use `Vector2 zero = Main.drawToScreen ? Vector2.Zero : new Vector2(Main.offScreenRange);` in your code and add `zero` to all calls to `spriteBatch.Draw`. The reason for this is to accommodate the shift in drawing coordinates that occurs when using the different Lighting modes. Press F9 to change lighting modes quickly to verify your code works for all lighting modes.

## Converting Coordinates
### Tile <--> World
Each tile is occupies a 16x16 area of world coordinate space. If you multiply a tile coordinate by 16, you end up with a world coordinate pointing to the top left corner of that tile. You can use Vector2.ToWorldCoordinates to simplify your code. Point.ToWorldCoordinates automatically adds 8 to both X and Y, returning world coordinates of the center of the tile.

```cs
// Finding the tile exactly behind the player.
Point tileLocation = Main.LocalPlayer.Center.ToTileCoordinates();
Tile tile = Main.tile[tileLocation.X, tileLocation.Y];
```

### World <--> Screen
When drawing tiles manually, you need to be aware of `Main.drawToScreen`, mentioned above.

## Relevant Fields
### Main.MouseWorld
World Coordinates of the current mouse position. Unfortunately this doesn't work with zoom.

### Main.MouseScreen
Screen Coordinates of the mouse. `Main.mouseX` and `Main.mouseY` are the same.
