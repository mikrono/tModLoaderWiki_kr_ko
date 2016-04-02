# ModTile

This class represents a _type_ of tile that can be added by a mod. Only one instance of this class will ever exist for each type of tile that is added. Any hooks that are called will be called by the instance corresponding to the tile type. This is to prevent the game from using a massive amount of memory storing tile instances.

## Properties

### public Mod mod

The mod which has added this type of ModTile.

### public string Name

The name of this type of tile.

### public ushort Type

The internal ID of this type of tile.

## Fields

These fields can be set from the SetDefaults method.

### public int soundType

The default type of sound made when this tile is hit. Defaults to 0.

### public int soundStyle

The default style of sound made when this tile is hit. Defaults to 1.

### public int dustType

The default type of dust made when this tile is hit. Defaults to 0.

### public int drop

The default type of item dropped when this tile is killed. Defaults to 0, which means no item.

### public int animationFrameHeight

The height of a group of animation frames for this tile. Defaults to 0, which disables animations.

### public float mineResist

A multiplier describing how much this block resists harvesting. Higher values will make it take longer to harvest. Defaults to 1f.

### public int minPick

The minimum pickaxe power required for pickaxes to mine this block. Defaults to 0.

### public bool disableSmartCursor

Whether or not the smart cursor function is disabled when the cursor hovers above this tile. Defaults to false.

### public int[] adjTiles

An array of the IDs of tiles that this tile can be considered as when looking for crafting stations.

### public int closeDoorID

The ID of the tile that this door transforms into when it is closed. Defaults to -1, which means this tile isn't a door.

### public int openDoorID

The ID of the tile that this door transforms into when it is opened. Defaults to -1, which means this tile isn't a door.

### public string chest

The default name of this chest that is displayed when this chest is open. Defaults to the empty string, which means that this tile isn't a chest. Setting this field will make the tile behave like a chest (meteors will avoid it, tiles underneath cannot be mined, etc.), but you will have to manually give it storage capabilities yourself. (See the example mod in the forums thread for something you can copy/paste.)

### public bool bed

Whether or not this tile is a valid spawn point. Defaults to false. If you set this to true, you will still have to manually set the spawn yourself in the RightClick hook.

### public bool platform

Whether or not this tile behaves like a platform. This is important in order to make the tile slope properly like a platform.

### public bool torch

Whether or not this tile behaves like a torch. If you are making a torch tile, then setting this to true is necessary in order for tile placement, tile framing, and the item's smart selection to work properly.

## Methods

### public void AddToArray(ref int[] array)

A convenient method for adding this tile's Type to the given array. This can be used with the arrays in TileID.Sets.RoomNeeds.

### public void AddMapEntry(Color color, string name = "")

Adds an entry to the minimap for this tile with the given color and display name. This should be called in SetDefaults.

### public void AddMapEntry(Color color, string name, Func\<string, int, int, string\> nameFunc)

Adds an entry to the minimap for this tile with the given color, default display name, and display name function. The parameters for the function are the default display name, x-coordinate, and y-coordinate. This should be called in SetDefaults.

### public virtual bool Autoload(ref string name, ref string texture)

Allows you to modify the name and texture path of this tile when it is autoloaded. Return true to autoload this tile. When a tile is autoloaded, that means you do not need to manually call Mod.AddTile. By default returns the mod's autoload property.

### public virtual void SetDefaults()

Allows you to set the properties of this tile. Many properties are stored as arrays throughout Terraria's code.

### public virtual void PostSetDefaults()

Allows you to override some default properties of this tile, such as Main.tileNoSunLight and Main.tileObsidianKill.

### public virtual bool KillSound(int i, int j)

Allows you to customize which sound you want to play when the tile at the given coordinates is hit. Return false to stop the game from playing its default sound for the tile. Returns true by default.

### public virtual void NumDust(int i, int j, bool fail, ref int num)

Allows you to change how many dust particles are created when the tile at the given coordinates is hit.

### public virtual bool CreateDust(int i, int j, ref int type)

Allows you to modify the default type of dust created when the tile at the given coordinates is hit. Return false to stop the default dust (the type parameter) from being created. Returns true by default.

### public virtual void DropCritterChance(int i, int j, ref int wormChance, ref int grassHopperChance, ref int jungleGrubChance)

Allows you to modify the chance the tile at the given coordinates has of spawning a certain critter when the tile is killed.

### public virtual bool Drop(int i, int j)

Allows you to customize which items the tile at the given coordinates drops. Return false to stop the game from dropping the tile's default item. Returns true by default.

### public virtual bool CanKillTile(int i, int j, ref bool blockDamaged)

Allows you to determine whether or not the tile at the given coordinates can be hit by anything. Returns true by default. blockDamaged currently has no use.

### public virtual void KillTile(int i, int j, ref bool fail, ref bool effectOnly, ref bool noItem)

Allows you to determine what happens when the tile at the given coordinates is killed or hit with a pickaxe. Fail determines whether the tile is mined, effectOnly makes it so that only dust is created, and noItem stops items from dropping.

### public virtual void KillMultiTile(int i, int j, int frameX, int frameY)

This hook is called exactly once whenever a block encompassing multiple tiles is destroyed. You can use it to make your multi-tile block drop a single item, for example.

### public virtual bool CanExplode(int i, int j)

Whether or not the tile at the given coordinates can be killed by an explosion (ie. bombs). Returns true by default; return false to stop an explosion from destroying it.

### public virtual void NearbyEffects(int i, int j, bool closer)

Allows you to make things happen when this tile is within a certain range of the player (around the same range water fountains and music boxes work). The closer parameter is whether or not the tile is within the range at which things like campfires and banners work.

### public virtual void ModifyLight(int i, int j, ref float r, ref float g, ref float b)

Allows you to determine how much light this block emits. Make sure you set Main.tileLighted[Type] to true in SetDefaults for this to work.

### public virtual bool Dangersense(int i, int j, Player player)

Allows you to determine whether this block glows red when the given player has the Dangersense buff.

### public virtual void SetSpriteEffects(int i, int j, ref SpriteEffects spriteEffects)

Allows you to determine whether or not the tile will draw itself flipped in the world.

### public virtual void SetDrawPositions(int i, int j, ref int width, ref int offsetY, ref int height)

Allows you to customize the position in which this tile is drawn. Width refers to the width of one frame of the tile, offsetY refers to how many pixels below its actual position the tile should be drawn, and height refers to the height of one frame of the tile. By default the values will be set to the values you give this tile's TileObjectData. If this tile has no TileObjectData then they will default to 16, 0, and 16, respectively.

### public virtual void AnimateTile(ref int frame, ref int frameCounter)

Allows you to animate your tile. Use frameCounter to keep track of how long the current frame has been active, and use frame to change the current frame.

### public virtual bool PreDraw(int i, int j, SpriteBatch spriteBatch)

Allows you to draw things behind the tile at the given coordinates. Return false to stop the game from drawing the tile normally. Returns true by default.

### public virtual void DrawEffects(int i, int j, SpriteBatch spriteBatch, ref Color drawColor)

Allows you to make stuff happen whenever the tile at the given coordinates is drawn. For example, creating dust or changing the color the tile is drawn in.

### public virtual void PostDraw(int i, int j, SpriteBatch spriteBatch)

Allows you to draw things in front of the tile at the given coordinates. This can also be used to do things such as creating dust.

### public virtual ushort GetMapOption(int i, int j)

Allows you to choose which minimap entry the tile at the given coordinates will use. 0 is the first entry added by AddMapEntry, 1 is the second entry, etc. Returns 0 by default.

### public virtual void RandomUpdate(int i, int j)

Called whenever the world randomly decides to update this tile in a given tick. Useful for things such as growing or spreading.

### public virtual bool TileFrame(int i, int j, ref bool resetFrame, ref bool noBreak)

Called whenever this tile updates due to being placed or being next to a tile that is changed. Return false to stop the game from carrying out its default TileFrame operations. Returns true by default.

### public virtual bool CanPlace(int i, int j)

Allows you to stop this tile from being placed at the given coordinates. Return false to block the tile from being placed. Returns true by default.

### public virtual void RightClick(int i, int j)

Allows you to make something happen when this tile is right-clicked by the player.

### public virtual void MouseOver(int i, int j)

Allows you to make something happen when the mouse hovers over this tile. Useful for showing item icons or text on the mouse.

### public virtual void MouseOverFar(int i, int j)

Allows you to make something happen when the mouse hovers over this tile, even when the player is far away. Useful for showing what's written on signs, etc.

### public virtual bool AutoSelect(int i, int j, Item item)

Allows you to determine whether the given item can become selected when the cursor is hovering over this tile and the auto selection hotkey is pressed.

### public virtual void HitWire(int i, int j)

Allows you to make something happen when a wire current passes through this tile.

### public virtual bool Slope(int i, int j)

Allows you to control how hammers slope this tile. Return true to allow it to slope normally. Returns true by default.

### public virtual bool HasWalkDust()

Whether or not this tile creates dust when the player walks on it. Returns false by default.

### public virtual void WalkDust(ref int dustType, ref bool makeDust, ref Color color)

Allows you to modify the dust created when the player walks on this tile. The makeDust parameter is whether or not to make dust; you can randomly set this to false to reduce the amount of dust produced.