# Tile Class Documentation
This page lists methods and fields pertaining to the Tile class. If you are working directly with `Tile`s, you can consult this page for an understanding of various things. If you want to create a Tile in your mod, this is not relevant, see [Basic Tile Guide](https://github.com/blushiemagic/tModLoader/wiki/Basic-Tile) for that. See also [Main.tile[]](https://github.com/blushiemagic/tModLoader/wiki/Main-Class-Documentation#tile). It is good to remember that `Main.tile[]` can have null values in it, null values are empty/air tiles. Non-null values could also be empty/air tiles as well.

Index|
-----|
[Fields](#fields-and-properties)|
[Methods](#methods)|

# Fields and Properties
The `Tile` class is structured to be very compact and efficient. You usually don't want to modify many of these fields directly. All values default to 0.

| Field    | Type | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|----------|------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [type](#type)<a name="type"></a>| ushort | Corresponds to the TileID of this tile. Since the dirt tile has the value of 0, you need to check other values to know if this tile has a tile or not. `Tile.active()` can be used to check if the `Tile` is air or not. |
| [wall](#wall)<a name="wall"></a>| ushort | Corresponds to the WallID of this tile. A value of 0 means no wall. |
| [liquid](#liquid)<a name="liquid"></a>| byte | Represents how much liquid is in the tile. Ranges from 0, no liquid, to 255, filled with liquid. |
| [sTileHeader](#sTileHeader)<a name="sTileHeader"></a>| ushort | A bit field. Don't touch this directly, use the methods. Stores: Wires, Actuator, Slope, Color, and HalfBrick data. |
| [bTileHeader](#bTileHeader)<a name="bTileHeader"></a>| byte | A bit field. Don't touch this directly, use the methods. Stores: LiquidType, WallColor, and Wire4 data. |
| [bTileHeader2](#bTileHeader2)<a name="bTileHeader2"></a>| byte | A bit field. Don't touch this directly, use the methods. Stores: FrameNumber, wallFrameX, and WallFrameNumber data. |
| [bTileHeader3](#bTileHeader3)<a name="bTileHeader3"></a>| byte | A bit field. Don't touch this directly, use the methods. Stores: SkipLiquid, CheckLiquid, and wallFrameY data. |
| [frameX](#frameX)<a name="frameX"></a><br>[frameY](#frameY)<a name="frameY"></a>| short | Corresponds to the coordinates of the 16x16 (usually) area in the spritesheet this Tile uses. For a Framed tile, this value is set automatically as the world loads or other tiles are placed or mined nearby. See [Framed vs FrameImportant](https://github.com/blushiemagic/tModLoader/wiki/Basic-Tile#framed-vs-frameimportant-tiles) for more info. For FrameImportant tiles, this value will not change and will be saved and synced in Multiplayer. In either case, `frameX` and `frameY` correspond to the coordinates of the top left corner of the area in the spritesheet that this Tile should draw. Some Tiles such as Christmas Tree and Weapon Rack use the higher bits of these fields to do tile-specific behaviors. Modders should not attempt to do similar approaches, but should use `ModTileEntity`s |
| [collisionType](#collisionType)<a name="collisionType"></a>| int | Get only property. Unused. |
| [](#)<a name=""></a>| | |

# Methods
## public Tile()
Constructor, all values default to 0.

## public Tile(Tile copy)
Constructor that clones another Tile

## public object Clone()
Creates a clone of the Tile. `Tile clone = (Tile)Main.tile[x,y].Clone()`

## public void CopyFrom(Tile from)
Copies fields from another Tile.

## public void ClearEverything()
Clears all fields.

## public bool active()
## public void active(bool active)
Sets or checks if the Tile has a tile present or not. 

## public bool nactive()
Returns true is Tile is active but not actuated.

## public bool inActive()
## public void inActive(bool inActive)
Sets or checks if the tile is actuated.

## public void liquidType(int liquidType)
## public byte liquidType()
Sets or checks liquid type. 0 water, 1 lava, 2 honey. 

## public bool lava()
## public void lava(bool lava)
## public bool honey()
## public void honey(bool honey)
Sets or checks liquid type.

## public bool wire()
## public void wire(bool wire)
## public bool wire2()
## public void wire2(bool wire2)
## public bool wire3()
## public void wire3(bool wire3)
## public bool wire4()
## public void wire4(bool wire4)
Sets or checks presence of individual wires.

## public bool actuator()
## public void actuator(bool actuator)
Sets or checks presence of actuator on tile.

## public byte color()
## public void color(byte color)
Sets or checks tile color.

## public byte wallColor()
## public void wallColor(byte wallColor)
Sets or checks wall color.

## public byte frameNumber()
## public void frameNumber(byte frameNumber)
Framed tiles usually have 3 choices (alternate images), this sets or checks that value.

## public int wallFrameX()
## public void wallFrameX(int wallFrameX)
## public byte wallFrameNumber()
## public void wallFrameNumber(byte wallFrameNumber)
## public int wallFrameY()
## public void wallFrameY(int wallFrameY)
Various wall framing related methods. 

## public bool checkingLiquid()
## public void checkingLiquid(bool checkingLiquid)
## public bool skipLiquid()
## public void skipLiquid(bool skipLiquid)
Various Liquid movement related methods.

## public bool topSlope()
## public bool bottomSlope()
## public bool leftSlope()
## public bool rightSlope()
## public bool HasSameSlope(Tile tile)
## public bool halfBrick()
## public void halfBrick(bool halfBrick)
## public byte slope()
## public void slope(byte slope)
## public static void SmoothSlope(int x, int y, bool applyToNeighbors = true)
Various slope related methods.

## public override string ToString()
Prints debug info about a Tile.

## public void ClearTile()
## public bool isTheSameAs(Tile compTile)
## public int blockType()
## public void ResetToType(ushort type)
## internal void ClearMetadata()
## public Color actColor(Color oldColor)
Various methods.
