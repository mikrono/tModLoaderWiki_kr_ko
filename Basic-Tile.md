# Basic Tile
This guide serves to explain the basics of Tiles.

# What is a Tile?
It is important to clearly understand tiles in your mind. Starting out, you might confuse or conflate tiles and items. Items are things in your inventory. Tiles are blocks in the world. Many items in the game place tiles, but aside from the item placing the tile and the tile returning the item, there is no enforced connection between an item and the tile it places. That said, most tiles modders will add will have 1 or many corresponding items. 

If you are curious about the `Tile` class itself, such as you would find in `Main.tile[]`, please see [Tile Class Documentation](Tile-Class-Documentation)

## Tile-Item Pairing
An Item will place a specific Tile when `item.createTile` is set to the TileType of the ModTile. If a Tile has multiple styles, setting `item.placeStyle` allows you to specify that style. The ModTile can return the ModItem as well. For 1x1 tiles, simply set `drop = ModContent.ItemType<ItemName>());` in `ModTile.SetDefaults`. For larger tiles and tiles with multiple styles, use `ModTile.KillMultiTile` combined with `Item.NewItem` to spawn the appropriate Item to the player. See [Multiple Styles](#multiple-styles) for more details.

# Making a Tile
To add a tile to Terraria, we must first create a "class" that "inherits" from ModTile. To do so, make a .cs file in your mod's source directory (My Games\Terraria\ModLoader\Mod Sources\MyModName) and then open that file in your text editor. Paste the following into that file, replacing `NameHere` with the internal name of your tile and `ModNamespaceHere` with your mod's foldername/namespace. (A common mistake is to use apostrophes or spaces in internal names, don't do this, the computer won't understand.)

```cs
using Microsoft.Xna.Framework;
using Terraria;
using Terraria.ModLoader;

namespace ModNamespaceHere
{
	public class NameHere : ModTile
	{
		public override void SetDefaults()
		{
			Main.tileSolid[Type] = true;
			Main.tileMergeDirt[Type] = true;
			Main.tileBlockLight[Type] = true;
			Main.tileLighted[Type] = true;
			dustType = ModContent.DustType<Sparkle>();
			drop = ModContent.ItemType<ExampleBlock>();
			AddMapEntry(new Color(200, 200, 200));
			// Set other values here
		}
	}
}
```

Now that you have a .cs file, bring in your texture file (a .png image file that you have made) and put it in the folder with this .cs file. Make sure read [Autoload](https://github.com/tModLoader/tModLoader/wiki/Basic-Autoload) so you know how to satisfy what the computer expects for its filename and folder structure.

# Framed vs FrameImportant Tiles
There are 2 different types of Tiles. One type is the regular tiles that are 1x1 (width of 1, height of 1) and adjust themselves as you place similar tiles next to them. These will be referred to as "Framed" tiles in this guide. The other type are the tiles that do not change automatically, which we will call "FrameImportant" tiles. These tiles are usually larger than 1x1 so another name for them could be "MultiTiles".   

Here is an example of the sprite of a Framed tile:    
![](https://i.imgur.com/vtH5d8n.png)

Here is an example of the sprite of a FrameImportant tile:    
![](https://i.imgur.com/s9ZtC9n.png)    

Not all 1x1 tiles are Framed tiles, here is an example of a 1x1 FrameImportant tile. If you recall, this `MetalBars` tile doesn't change when placed next to other tiles like Framed tiles do:    
![](https://i.imgur.com/bqoyLqT.png)

As you might have noticed, FrameImportant tiles can have multiple unique tile "styles". You might have also noticed some empty space in the spritesheets, this is padding. These concepts will be explored later.

# Coordinates
Tile coordinates are 1/16th the size of World coordinates. Remember this if you ever need to do anything involving Tiles and anything else such as NPC, Player, or Projectile.

# Padding
When making a sprite, it is important to know proper dimensions. For the most part, every individual tile will be 16x16 pixels with 2 pixels of padding to the right and below each, for a total of 18x18.

# SetDefaults
Now that we've gone through some preliminary info, lets focus on SetDefaults and what to put in it. This section will explain most of the common items in SetDefaults. The point of SetDefaults is to define how the tile acts, such as is it solid, can things stand on it, and does lava kill it. Consulting similar tiles that you wish to emulate in ExampleMod is probably better than trying to do this from scratch. Many of the lines below set something to true, this means the default value is false. Don't bother including lines setting the value to the default, it just clutters your code.

For this guide, many gifs will refer to this tile sprite:    
![](https://i.imgur.com/b009P8f.png)    

## Main.tileSolid[Type] = true;
Setting this to true means the tile will be solid. Projectiles will collide with it and NPC and Players can stand on it.    
True:    
![](https://thumbs.gfycat.com/VelvetyComposedBellfrog-size_restricted.gif)    
False (default):    
![](https://thumbs.gfycat.com/LinedWatchfulElectriceel-size_restricted.gif)    

## Main.tileSolidTop[Type] = true;
Many tiles, such as placed bars, tables, anvils, etc can be stood on but aren't solid. These tiles all set this value.    
True:    
![](https://thumbs.gfycat.com/PartialMedicalCaterpillar-size_restricted.gif)       

## Main.tileTable[Type] = true;
Some tiles, such as bottles, can only be placed on "tables". Be sure to set this for flat topped furniture tiles.    
True:    
![](https://thumbs.gfycat.com/CostlyFluffyArabianwildcat-size_restricted.gif)    
False (default):    
![](https://thumbs.gfycat.com/HighlevelMagnificentBlacklab-size_restricted.gif)   

## Main.tileMergeDirt[Type] = true;
This tile will merge with dirt using the extra sprites provided in the spritesheet.    
true:    
![](https://i.imgur.com/4RvCh3p.png)    
false (default):    
![](https://i.imgur.com/6UjS77f.png)    

## Main.tileSpelunker[Type] = true;	
## Main.tileShine[Type] = true;
## Main.tileShine2[Type] = true;
## Main.tileValue[Type] = true;
These are related to Metal Detector and ore shining. See [ExampleOre](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/Tiles/ExampleOre.cs)

## Main.tileBlockLight[Type] = true;	
If set to true, light is blocked by this tile and the light will decrease as it passes through.     
![](https://i.imgur.com/BVIl2Fl.png)    

## Main.tileLighted[Type] = true;
TODO

## Main.tileLavaDeath[Type] = true;
Set to True if you'd like your tile to die if hit by lava.    
![](https://thumbs.gfycat.com/OblongEsteemedDoctorfish-size_restricted.gif)    

## Main.tileWaterDeath[Type] = true;
Set to True if you'd like your tile to die if hit by water.    

## Main.tileNoAttach[Type] = true;
Prevents tiles from attaching to this tile.    
True:    
![](https://thumbs.gfycat.com/AcidicSparseBumblebee-size_restricted.gif)    
False (default):    
![](https://thumbs.gfycat.com/AshamedHoarseAnt-size_restricted.gif)    

### Main.tileCut[Type] = true;
The tile can be destroyed by weapons. See [ExampleCutTile.cs](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/Tiles/ExampleCutTile.cs)

## Other
These are more rarely used and won't be explained. See vanilla source code if you need hints with these.
### Main.tileBouncy[Type] = true;
### Main.tileAlch[Type] = true;
### Main.tileStone[Type] = true;
### Main.tileAxe[Type] = true;
### Main.tileHammer[Type] = true;
### Main.tileNoSunLight[Type] = true;
### Main.tileDungeon[Type] = true;
### Main.tileLargeFrames[Type] = true;
### Main.tileRope[Type] = true;
### Main.tileBrick[Type] = true;
### Main.tileMoss[Type] = true;
### Main.tileNoFail[Type] = true;
### Main.tileObsidianKill[Type] = true;
### Main.tilePile[Type] = true;
### Main.tileBlendAll[Type] = true;
### Main.tileGlowMask[Type] = true;
### Main.tileContainer[Type] = true;
### Main.tileSign[Type] = true;
### Main.tileMerge[Type][otherType] = true;
### Main.tileSand[Type] = true;
### Main.tileFlame[Type] = true;
### Main.tileFrame[Type] = true;
### Main.tileFrameCounter[Type] = true;

## Main.tileFrameImportant[Type] = true;
This changes a Framed tile to a FrameImportant tile. The frame important part of the name suggest that the frame is important, but what is frame? Frame is the coordinates within the spritesheet that the current tile should draw. For Framed tiles, the frame is never saved since the coordinate frame of a Framed tile is calculated when the world is loaded. For FrameImportant tiles, the world needs to save those coordinates, hence, "important". For modders, just remember to set this to true when you make a tile that uses a TileObjectData, or basically all tiles that aren't like dirt, ores, or other basic building tiles. See [TileObjectData](#tileobjectdata) below for details.
	
## ModTile fields: dustType, drop, adjTiles, etc
These are explained in the [documentation](http://tmodloader.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod_tile.html#pub-attribs). 

## AddToArray(ref TileID.Sets.RoomNeeds.????);
Used to make a ModTile act as a lightsource, chair or table for the purposes of housing. Some examples:    
```cs
AddToArray(ref TileID.Sets.RoomNeeds.CountsAsTable);
AddToArray(ref TileID.Sets.RoomNeeds.CountsAsDoor);
AddToArray(ref TileID.Sets.RoomNeeds.CountsAsTorch);
AddToArray(ref TileID.Sets.RoomNeeds.CountsAsChair);
```    

## AddMapEntry			
AddMapEntry is for setting the color and optional text associated with the Tile when viewed on the map. Explore ExampleMod and the documentation for more info.

<a name="tileobjectdata"></a>
# TileObjectData or FrameImportant/MultiTiles
If a ModTile is not a Framed tile, it must have `Main.tileFrameImportant[Type] = true;` and the `TileObjectData` in `SetDefaults`. FrameImportant tiles can be any size, from 1x1 to anything bigger. They can also have many different "styles". Each style can also have "alternates" which are alternate placements of the particular style. 

For this guide, many gifs will refer to this tile sprite:    
![](https://i.imgur.com/b009P8f.png)    

## Multiple Styles
You can take advantage of tile styles to simplify your code and avoid code repetition. Using this, you can have 1 ModTile file that places several styles. Each item that places this tile will have the same `item.createTile` but will have different `item.placeStyle` to differentiate which style to place. See [BossTrophy](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/Tiles/BossTrophy.cs) for an example of the code and notice how [BunnyTrophy](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/Items/Placeable/BunnyTrophy.cs) and [PuritySpiritTrophy](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/Items/Placeable/PuritySpiritTrophy.cs) items use the same createTile but different placeStyle values. Notice how `frameX` is used in `ModTile.KillMultiTile` to choose which item to spawn when the tile is mined. See [StyleHorizonal](#stylehorizonal), [StyleMultiplier](stylemultiplier) and [StyleWrapLimit](#stylewraplimit) below for more information.

![](https://i.imgur.com/O923oDq.png)    

## Basic TileObjectData.newTile structure
In `SetDefaults` we use `TileObjectData.newTile` to define properties of our tile. We typically start with `TileObjectData.newTile.CopyFrom(TileObjectData.Style???);`, make a few changes such as `TileObjectData.newTile.Something = SomeValue;`, then finish off the TileObjectData by calling `TileObjectData.addTile(Type);`. Doing this out of order will lead to errors.

## CopyFrom
Use this to utilize an existing template. The names are self explanatory usually.
Existing Templates include:   
```cs
StyleSwitch
StyleTorch
Style4x2
Style2x2
Style1x2
Style1x1
StyleAlch
StyleDye
Style2x1
Style6x3
StyleSmallCage
StyleOnTable1x1 // placeable on tables only, like bottles
Style1x2Top // "Hangs" from attaching to tiles above.
Style1xX
Style2xX
Style3x2
Style3x3
Style3x4
Style3x3Wall
```

Typically, you'll want to start out by copying a template, and modifying it as needed.
For example, [MonsterBanner.cs](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/Tiles/MonsterBanner.cs#L17) first does:
`TileObjectData.newTile.CopyFrom(TileObjectData.Style1x2Top);`    
....and then it makes adjustments such as:     
```cs
TileObjectData.newTile.Height = 3; // because the template is for 1x2 not 1x3
TileObjectData.newTile.CoordinateHeights = new int[]{ 16, 16, 16 }; // because height changed
```
....and finally calls:    
```TileObjectData.addTile(Type);```

## Width
Modifies the width of the tiles in tile coordinates:    
`TileObjectData.newTile.Width = 3;`    
![](http://i.imgur.com/ZjvSjOV.png)    
`TileObjectData.newTile.Width = 2;`     
![](http://i.imgur.com/4Qqx0mC.png)    
`TileObjectData.newTile.Width = 1;`    
![](http://i.imgur.com/8dudSXH.png)    

## Height
Modifies the height of the tiles in tile coordinates:    
`TileObjectData.newTile.Height = 3;`    
![](http://i.imgur.com/ZjvSjOV.png)    
`TileObjectData.newTile.Height = 2;`     
![](http://i.imgur.com/IqzM18z.png)    
`TileObjectData.newTile.Height = 1;`    
![](http://i.imgur.com/ypcuohX.png)    

## Origin
Modifies which part of the tile is centered on the mouse, in tile coordinates, from the top right corner:    
`TileObjectData.newTile.Origin = new Point16(0, 0); // default`    
![](https://thumbs.gfycat.com/DeficientNastyImperialeagle-small.gif)    
`TileObjectData.newTile.Origin = new Point16(2, 0); // To the right 2 tiles`     
Note how the cursor is placing the tile using the area marked "3" which is 2 to the right:    
![](https://thumbs.gfycat.com/BouncyBriskBurro-small.gif)     

## CoordinateHeights
This int array defines how tall each row of individual tiles within the tile should be. This array must be exactly the same number of elements as the value of Height or errors will happen. Note that these values don't include the padding pixels.

Basically, all values should be 16 in most cases. Use 18 on the bottom so that the texture can extend a little into the ground below. Here is a closeup of the texture, note how there is white there, this is to illustrate why we use 18 on a bottom tile sometimes.    
![](http://i.imgur.com/XMHqvfy.png)     
`TileObjectData.newTile.CoordinateHeights = new int[] { 16, 16, 18 }; // Extend into grass tiles.` 
   
Here we see the white pixels peeking out from behind the grass. Grass tiles don't completely cover their 16x16 area, leaving tiny holes. Correctly doing this will help the tile look like it's actually there and sitting in the soil, obviously modders should draw the sprite correctly and not with white. (note that it seems solid must be false for this.)    
![](http://i.imgur.com/CDzrin7.png)    
`TileObjectData.newTile.CoordinateHeights = new int[] { 16, 16, 16 }; // Don't extend into grass.`    
Notice how the grass doesn't completely cover its area, so our tile seems to float a little.    
![](http://i.imgur.com/PMBuMum.png)    

### Non 16 or 18 values
Values other than 16 or 18 are possible, but are very rarely done, usually just for 1x1 tiles (since bigger heights would just draw over each other for larger tiles). The coral tile, for example, is 24x26 (each style, excluding padding of course.) The code defines the follow for this effect:
```cs
TileObjectData.newTile.CoordinateHeights = new int[]
{
	26
};
TileObjectData.newTile.CoordinateWidth = 24;
TileObjectData.newTile.DrawYOffset = -8;
```    

![](https://i.imgur.com/65wwveE.png)       
![](https://i.imgur.com/ku0wQH7.png)    

## CoordinateWidth 
Unlike CoordinateHeights, all tiles in a tile share the same width, so this is an int not an int array. If you aren't copying a style, make sure to set it to 16.
`TileObjectData.newTile.CoordinateWidth = 16;`    

## CoordinatePadding 
This is the padding between tiles in the tile spritesheet. Adds a padding of 2 pixels to right and bottom of each area in the spritesheet. By convention, stick to 2. Do this or weird artifacts will appear.
`TileObjectData.newTile.CoordinatePadding = 2;`

## AnchorBottom/AnchorLeft/AnchorRight/AnchorTop
Anchors define required neighboring tiles for the placement of the tile to be valid. The most common anchor is AnchorBottom anchored to solid tiles for the full width of the tile, but more complex anchors can be made, such as an anchor only covering half of the side of the tile or anchoring to the left or right. Usually you get appropriate anchors for free when you use `CopyFrom`, but be aware that you'll need to fix anchors if you change the width or height of the TileObjectData if that width or height is used in an Anchor:     
`TileObjectData.newTile.AnchorBottom = new AnchorData(AnchorType.SolidTile, TileObjectData.newTile.Width, 0);`

Here is an example of a custom AnchorBottom. The 2nd variable in the AnchorData constructor is the width and the 3rd is the start of the tiles that require the anchor. If the 1st or 2nd block under this is broken, the tile will break as well, but if the 3rd block is broken it will not break. The 1st Variable is a BitMask describing the types or tiles valid for the anchor.
`TileObjectData.newTile.AnchorBottom = new AnchorData(AnchorType.SolidTile, TileObjectData.newTile.Width - 1, 0);`    
![](https://thumbs.gfycat.com/InferiorAfraidAbyssiniangroundhornbill-small.gif)

Here is an example of an AnchorTop that requires the tile above to be empty. Place a tile above Coral and you'll see the coral break because of this code:    
`TileObjectData.newTile.AnchorTop = new AnchorData(AnchorType.EmptyTile, TileObjectData.newTile.Width, 0);`

[ExampleCutTile.cs](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/Tiles/ExampleCutTile.cs) shows a custom `AnchorTop` as well as clearing out a copied AnchorBottom.

## StyleHorizontal
By default, tile styles are oriented vertically on the spritesheet:     
![](https://i.imgur.com/nx4asBg.png)     

It may be more convenient to place them horizontally. To do this:    
`TileObjectData.newTile.StyleHorizontal = true;`     
![](https://i.imgur.com/MzfnM3l.png)    
 
## StyleWrapLimit
If you are making a lot of styles, you should be aware that the maximum size for a sprite is 2048x2048. If you have a lot of styles for a big tile, you might run into this limit. StyleWrapLimit makes the image wrap around to the next row/column to continue placing styles. In the Banner tile sprite (Tiles_91.xnb), `TileObjectData.newTile.StyleWrapLimit = 111;` means that styles 0 to 110 are on the first line, styles 111 to 221 are on the next, and so on. 111 different styles each 18 pixels wide is 1998 pixels, keeping this sprite below the 2048 pixel limit.    
![](https://i.imgur.com/pdI4S2N.png)    

## StyleMultiplier
Used to give room for animation frames, growth stages, or tile states in the spritesheet.

## RandomStyleRange
Coral also randomly places a style:    
`TileObjectData.newTile.RandomStyleRange = 6;`    
![](https://i.imgur.com/y23Gc7T.png)    

## UsesCustomCanPlace
Should always be true. If you copied a template it will already be true, but be sure you set it if you aren't copying from a template.

## Wires, Toggles, Changing Frame
Sometimes we use extra frames in the spritesheet to allow our tile to toggle between off and on. The placement of extra sprites depends on StyleLineSkip, if necessary, and StyleHorizontal. These extra "states" for our tiles should still be the same style if set up correctly. See [ExampleLamp.cs](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/Tiles/ExampleLamp.cs) to see how HitWire changes the frameX to change which sprite is drawn.    
![](https://i.imgur.com/Xq13Slr.png)     

## Other
There are many more not yet explained in this guide. Decompile Terraria and look in `TileObjectData.Initialize` to figure out how they are used:     
```cs
AnchorWall
AnchorValidTiles
AnchorInvalidTiles
AnchorAlternateTiles
AnchorValidWalls
WaterDeath
LavaDeath
WaterPlacement
LavaPlacement
HookCheck
HookPostPlaceEveryone
HookPostPlaceMyPlayer
HookPlaceOverride
Direction
FlattenAnchors
CoordinatePaddingFix
DrawFlipHorizontal
DrawFlipVertical
DrawStepDown
StyleLineSkip
```

## addTile(Type);
Be sure to call this or your mod won't load properly.     
`TileObjectData.addTile(Type);`    

## newSubTile and newAlternate
### Conditional Behavior
You may have noticed that things like `Main.tileWaterDeath` are indexed by the tile type. You may have also remembered that both Cursed Torch and Ichor Torch work underwater and are not destroyed when touched by water. If you look in the code, you'll see that Cursed Torch and Ichor Torch are the same tile type as all the other torches. How is this possible? This is possible through `TileObjectData`. `TileObjectData` is a data structure that allows different properties to be applied to different "styles" or "alternates" of the same tile type. Doing this type of conditional behavior is best learned from studying the source and will not be explained further in this guide. Just be aware that it is possible.

# Animation
Do not change frameX or frameY of the tile for animation. The tile and its values should stay the same as it is animating. [VoidMonolith.cs](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/Tiles/VoidMonolith.cs) shows changing state and animating a tile. [ExampleAnimatedTile.cs](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/Tiles/ExampleAnimatedTile.cs) shows more animated tile options.

# Full Examples
## Framed Tile

## FrameImportant Tile

## Relevant References
* [Vanilla TileIDs](https://github.com/tModLoader/tModLoader/wiki/Vanilla-Tile-IDs)
* [ModTile Documentation](http://tmodloader.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod_tile.html)

## Not covered in Basic level
There are other aspects of ModTiles that will be covered in more advanced guides:
* SubTile -- Advanced -- Allows a single tile to have different properties for different styles of the same tile. For example, the Obsidian Lantern is lava proof even though all the other Lanterns aren't.
* Alternate -- Advanced -- Allows a single tile style to have different placement and anchoring options.  For example, Torches have a left and right alternative in addition to the normal.