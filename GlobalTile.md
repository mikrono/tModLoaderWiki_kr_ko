# GlobalTile

This class allows you to modify the behavior of any tile in the game. Create an instance of an overriding class then call Mod.AddGlobalTile to use this.

## Properties

### public Mod mod

The mod to which this GlobalTile belongs to.

### public string Name

The name of this GlobalTile instance.

## Methods

### public void AddToArray(ref int[] array, int type)

A convenient method for adding an integer to the end of an array. This can be used with the arrays in TileID.Sets.RoomNeeds.

### public virtual bool Autoload(ref string name)

Allows you to automatically load a GlobalTile instead of using Mod.AddGlobalTile. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name. Use this method to either force or stop an autoload or to control the internal name.

### public virtual void SetDefaults()

Allows you to modify the properties of any tile in the game. Most properties are stored as arrays throughout the Terraria code.

### public virtual bool KillSound(int i, int j, int type)

Allows you to customize which sound you want to play when the tile at the given coordinates is hit. Return false to stop the game from playing its default sound for the tile. Returns true by default.

### public virtual void NumDust(int i, int j, int type, bool fail, ref int num)

Allows you to change how many dust particles are created when the tile at the given coordinates is hit.

### public virtual bool CreateDust(int i, int j, int type, ref int dustType)

Allows you to modify the default type of dust created when the tile at the given coordinates is hit. Return false to stop the default dust (the dustType parameter) from being created. Returns true by default.

### public virtual void DropCritterChance(int i, int j, int type, ref int wormChance, ref int grassHopperChance, ref int jungleGrubChance)

Allows you to modify the chance the tile at the given coordinates has of spawning a certain critter when the tile is killed.

### public virtual bool Drop(int i, int j, int type)

Allows you to customize which items the tile at the given coordinates drops. Return false to stop the game from dropping the tile's default item. Returns true by default.

### public virtual bool CanKillTile(int i, int j, int type, ref bool blockDamaged)

Allows you to determine whether or not the tile at the given coordinates can be hit by anything. Returns true by default. blockDamaged currently has no use.

### public virtual void KillTile(int i, int j, int type, ref bool fail, ref bool effectOnly, ref bool noItem)

Allows you to determine what happens when the tile at the given coordinates is killed or hit with a pickaxe. Fail determines whether the tile is mined, effectOnly makes it so that only dust is created, and noItem stops items from dropping.

### public virtual void NearbyEffects(int i, int j, int type, bool closer)

Allows you to make things happen when the tile is within a certain range of the player (around the same range water fountains and music boxes work). The closer parameter is whether or not the tile is within the range at which things like campfires and banners work.

### public virtual void ModifyLight(int i, int j, int type, ref float r, ref float g, ref float b)

Allows you to determine how much light the block emits. Make sure you set Main.tileLighted[type] to true in SetDefaults for this to work.

### public virtual void SetSpriteEffects(int i, int j, int type, ref SpriteEffects spriteEffects)

Allows you to determine whether or not a tile will draw itself flipped in the world.

### public virtual bool PreDraw(int i, int j, int type, SpriteBatch spriteBatch)

Allows you to draw things behind the tile at the given coordinates. Return false to stop the game from drawing the tile normally. Returns true by default.

### public virtual void PostDraw(int i, int j, int type, SpriteBatch spriteBatch)

Allows you to draw things in front of the tile at the given coordinates. This can also be used to do things such as creating dust.

### public virtual void RandomUpdate(int i, int j, int type)

Called for every tile the world randomly decides to update in a given tick. Useful for things such as growing or spreading.

### public virtual bool TileFrame(int i, int j, int type, ref bool resetFrame, ref bool noBreak)

Called for every tile that updates due to being placed or being next to a tile that is changed. Return false to stop the game from carrying out its default TileFrame operations. Returns true by default.

### public virtual bool CanPlace(int i, int j, int type)

Allows you to stop a tile from being placed at the given coordinates. Return false to block the tile from being placed. Returns true by default.

### public virtual int[] AdjTiles(int type)

Allows you to determine which tiles the given tile type can be considered as when looking for crafting stations.

### public virtual void RightClick(int i, int j, int type)

Allows you to make something happen when any tile is right-clicked by the player.

### public virtual void MouseOver(int i, int j, int type)

Allows you to make something happen when the mouse hovers over any tile. Useful for showing item icons or text on the mouse.

### public virtual void HitWire(int i, int j, int type)

Allows you to make something happen when a wire current passes through any tile.

### public virtual bool Slope(int i, int j, int type)

Allows you to control how hammers slope any tile. Return true to allow the tile to slope normally. Returns true by default.

### public virtual void AnimateTile()

Allows animating tiles that were previously static. Loading a new texture for the tile is required first. Use Main.tileFrameCounter to count game frames and Main.tileFrame to change animation frames.