# Basic Tile Entity
This guide serves to explain common things shared among all tile entities.  
This guide assumes that you know how to create files, create classes and know what it means for a class to "inherit from" another class.

# What is a Tile Entity?
It's important to recognize that there's a distinction between Tiles and Tile Entities.  While tiles are at fixed locations within a world and must be accessed via the `Main.tile` variable, tile entities store their tile position within the world and can be accessed through various means.

# Prerequisites
For simplifying later code in this guide, the following class is used.  
Feel free to put this class in your mod in a `TileUtils.cs` file.
```cs
using Terraria;
using Terraria.DataStructures;
using Terraria.ModLoader;
using Terraria.ObjectData;

namespace MyMod
{
	public static class TileUtils
	{
		/// <summary>
		/// Atttempts to find the top-left corner of a multitile at location (<paramref name="x"/>, <paramref name="y"/>)
		/// </summary>
		/// <param name="x">The tile X-coordinate</param>
		/// <param name="y">The tile Y-coordinate</param>
		/// <returns>The tile location of the multitile's top-left corner, or the input location if no tile is present or the tile is not part of a multitile</returns>
		public static Point16 GetTopLeftTileInMultitile(int x, int y)
        {
			Tile tile = Main.tile[x, y];

			int frameX = 0;
			int frameY = 0;

			if (tile.HasTile)
            {
				int style = 0, alt = 0;
				TileObjectData.GetTileInfo(tile, ref style, ref alt);
				TileObjectData data = TileObjectData.GetTileData(tile.TileType, style, alt);

				if (data != null)
                {
					int size = 16 + data.CoordinatePadding;

					frameX = tile.TileFrameX % (size * data.Width) / size;
					frameY = tile.TileFrameY % (size * data.Height) / size;
				}
			}

			return new Point16(x - frameX, y - frameY);
		}

		/// <summary>
		/// Uses <seealso cref="GetTopLeftTileInMultitile(int, int)"/> to try to get the entity bound to the multitile at (<paramref name="i"/>, <paramref name="j"/>).
		/// </summary>
		/// <typeparam name="T">The type to get the entity as</typeparam>
		/// <param name="i">The tile X-coordinate</param>
		/// <param name="j">The tile Y-coordinate</param>
		/// <param name="entity">The found <typeparamref name="T"/> instance, if there was one.</param>
		/// <returns><see langword="true"/> if there was a <typeparamref name="T"/> instance, or <see langword="false"/> if there was no entity present OR the entity was not a <typeparamref name="T"/> instance.</returns>
		public static bool TryGetTileEntityAs<T>(int i, int j, out T entity) where T : TileEntity
        {
			Point16 origin = GetTopLeftTileInMultitile(i, j);

			// TileEntity.ByPosition is a Dictionary<Point16, TileEntity> which contains all placed TileEntity instances in the world
			// TryGetValue is used to both check if the dictionary has the key, origin, and get the value from that key if it's there
			if (TileEntity.ByPosition.TryGetValue(origin, out TileEntity existing) && existing is T existingAsT)
            {
				entity = existingAsT;
				return true;
			}

			entity = null;
			return false;
		}
	}
}
```

# Making a Tile Entity
There are three components for making a tile entity.  First and foremost, you will need a `ModItem` that places a `ModTile`.  
The noteworthy line needed is the line in `SetDefaults` which sets `item.createTile` (`Item.createTile` in 1.4.4).  See [ExampleMod's Placeable Items](https://github.com/tModLoader/tModLoader/tree/1.3/ExampleMod/Items/Placeable) if you're using 1.3 tModLoader or [1.4.4 ExampleMod's Placeable Items](https://github.com/tModLoader/tModLoader/tree/1.4.4/ExampleMod/Content/Items/Placeable) if you're using 1.4.4 tModLoader.

## Making the Tile Entity
Setting up a ModTileEntity requires the usage of a few hooks.

### ValidTile(int i, int j)
This hook runs every frame and is used to automatically kill the tile entity if the hook returns `false`.  Below is a standard usage of `ValidTile()`:
```cs
public override bool IsTileValidForEntity(int x, int y)
{
    Tile tile = Main.tile[i, j];
    //The MyTile class is shown later
    return tile.HasTile && tile.TileType == ModContent.TileType<MyTile>();
}
```

### Hook_AfterPlacement(int i, int j, int type, int style, int direction, int alternate)
This hook is used by the `ModTile` which places the tile entity.  This hook is mainly used to place the tile entity.  Below is an example usage of `Hook_AfterPlacement()`:
```cs
public override int Hook_AfterPlacement(int i, int j, int type, int style, int direction, int alternate)
{
    if (Main.netMode == NetModeID.MultiplayerClient)
    {
        // Sync the entire multitile's area.  Modify "width" and "height" to the size of your multitile in tiles
        int width = 2;
        int height = 2;
        NetMessage.SendTileRange(Main.myPlayer, i, j, width, height);

        // Sync the placement of the tile entity with other clients
        // The "type" parameter refers to the tile type which placed the tile entity, so "Type" (the type of the tile entity) needs to be used here instead
        NetMessage.SendData(MessageID.TileEntityPlacement, number: i, number2: j, number3: Type);
    }

    // ModTileEntity.Place() handles checking if the entity can be placed, then places it for you
    // Set "tileOrigin" to the same value you set TileObjectData.newTile.Origin to in the ModTile
    Point16 tileOrigin = new Point16(1, 1);
    int placedEntity = Place(i - tileOrigin.X, j - tileOrigin.Y);
    return placedEntity;
}
```

### OnNetPlace()
This hook runs when the tile entity is placed and is usually used to inform other clients of its placement.  Below is a standard usage of the hook:
```cs
public override void OnNetPlace()
{
    if (Main.netMode == NetmodeID.Server)
    {
        NetMessage.SendData(MessageID.TileEntitySharing, number: ID, number2: Position.X, number3: Position.Y);
    }
}
```

The other hooks present in `ModTileEntity` are optional and will not be covered in this guide.

## Making the Tile
After making a normal `frameImportant` multitile (see the [Basic ModTile Guide](https://github.com/tModLoader/tModLoader/wiki/Basic-Tile) for more information), you only have to add a few lines in order to make it automatically place the tile entity.

### SetDefaults() / SetStaticDefaults()
Before the `TileObjectData.addTile(Type);` line, add the following:
```cs
// MyTileEntity refers to the tile entity mentioned in the previous section
TileObjectData.newTile.HookPostPlaceMyPlayer = new PlacementHook(ModContent.GetInstance<MyTileEntity>().Hook_AfterPlacement, -1, 0, false);

// This is required so the hook is actually called.
TileObjectData.newTile.UsesCustomCanPlace = true;
```

### KillMultiTile(int i, int j, int frameX, int frameY)
Add the following line to this hook:
```cs
// ModTileEntity.Kill() handles checking if the tile entity exists and destroying it if it does exist in the world for you
// The tile coordinate parameters already refer to the top-left corner of the multitile
ModContent.GetInstance<MyTileEntity>().Kill(i, j);
```

# Other Useful Things
Accessing the tile entity, opening a UI, etc. via a right-click of your multitile can be done as follows:
```cs
// This hook goes in your ModTile
public override bool NewRightClick(int i, int j)
{
    Player player = Main.LocalPlayer;

    // Should your tile entity bring up a UI, this line is useful to prevent item slots from misbehaving
    Main.mouseRightRelease = false;

    // The following four (4) if-blocks are recommended to be used if your multitile opens a UI when right clicked:
    if (player.sign > -1)
    {
        SoundEngine.PlaySound(SoundID.MenuClose);
        player.sign = -1;
        Main.editSign = false;
        Main.npcChatText = string.Empty;
    }
    if (Main.editChest)
    {
        SoundEngine.PlaySound(SoundID.MenuTick);
        Main.editChest = false;
        Main.npcChatText = string.Empty;
    }
    if (player.editedChestName)
    {
        NetMessage.SendData(MessageID.SyncPlayerChest, -1, -1, NetworkText.FromLiteral(Main.chest[player.chest].name), player.chest, 1f);
        palyer.editedChestName = false;
    }
    if (player.talkNPC > -1)
    {
        player.SetTalkNPC(-1);
        Main.npcChatCornerItem = 0;
        Main.npcChatText = string.Empty;
    }

    if (TileUtils.TryGetTileEntityAs(i, j, out MyTileEntity entity))
    {
        // Do things to your entity here
    }
}
```