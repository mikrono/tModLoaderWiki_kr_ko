# Basic Tile Entity
This guide serves to explain common things shared among all tile entities.  
This guide assumes that you know how to create files, create classes and know what it means for a class to "inherit from" another class.

# What is a Tile Entity?
It's important to recognize that there's a distinction between Tiles and Tile Entities.  While tiles are at fixed locations within a world and must be accessed via the `Terraria.Main.tile[,]` array (or methods that wrap around it, such as `Terraria.Framing.GetTileSafely(int, int)`), tile entities store their tile position within the world and can be accessed through various means.

# Prerequisites
For simplifying later code in this guide, the following class is used.  
Feel free to put this class in your mod in a `TileUtils.cs` file.
```cs
using Terraria;
using Terraria.DataStructures;
using Terraria.ModLoader;

namespace MyMod
{
	public static class TileUtils
	{
		/// <summary>
		/// Gets the top-left tile of a multitile
		/// </summary>
		/// <param name="i">The tile X-coordinate</param>
		/// <param name="j">The tile Y-coordinate</param>
		public static Point16 GetTileOrigin(int i, int j)
		{
			//Framing.GetTileSafely ensures that the returned Tile instance is not null
			//Do note that neither this method nor Framing.GetTileSafely check if the wanted coordiates are in the world!
			Tile tile = Framing.GetTileSafely(i, j);

			Point16 coord = new Point16(i, j);
			Point16 frame = new Point16(tile.frameX / 18, tile.frameY / 18);

			return coord - frame;
		}

		/// <summary>
		/// Uses <seealso cref="GetTileOrigin(int, int)"/> to try to get the entity bound to the multitile at (<paramref name="i"/>, <paramref name="j"/>).
		/// </summary>
		/// <typeparam name="T">The type to get the entity as</typeparam>
		/// <param name="i">The tile X-coordinate</param>
		/// <param name="j">The tile Y-coordinate</param>
		/// <param name="entity">The found <typeparamref name="T"/> instance, if there was one.</param>
		/// <returns><see langword="true"/> if there was a <typeparamref name="T"/> instance, or <see langword="false"/> if there was no entity present OR the entity was not a <typeparamref name="T"/> instance.</returns>
		public static bool TryGetTileEntityAs<T>(int i, int j, out T entity) where T : TileEntity{
			Point16 origin = GetTileOrigin(i, j);

			//TileEntity.ByPosition is a Dictionary<Point16, TileEntity> which contains all placed TileEntity instances in the world
			//TryGetValue is used to both check if the dictionary has the key, origin, and get the value from that key if it's there
			if(TileEntity.ByPosition.TryGetValue(origin, out TileEntity existing) && existing is T existingAsT){
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
The noteworthy line needed is the line in `SetDefaults` which sets `item.createTile` (`Item.createTile` in 1.4).  See [ExampleMod's Placeable Items](https://github.com/tModLoader/tModLoader/tree/1.3/ExampleMod/Items/Placeable) if you're using 1.3 tModLoader or [1.4 ExampleMod's Placeable Items](https://github.com/tModLoader/tModLoader/tree/1.4/ExampleMod/Content/Items/Placeable) if you're using 1.4 tModLoader.

## Making the Tile Entity
Setting up a ModTileEntity requires the usage of a few hooks.

### ValidTile(int i, int j)
This hook runs every frame and is used to automatically kill the tile entity if the hook returns `false`.  Below is a standard usage of `ValidTile()`:
```cs
public override bool ValidTile(int i, int j)
{
    Tile tile = Main.tile[i, j];
    //The MyTile class is shown later
    return tile.active() && tile.type == ModContent.TileType<MyTile>();
}
```

### Hook_AfterPlacement(int i, int j, int type, int style, int direction)
This hook is used by the `ModTile` which places the tile entity.  This hook is mainly used to place the tile entity.  Below is an example usage of `Hook_AfterPlacement()`:
```cs
public override int Hook_AfterPlacement(int i, int j, int type, int style, int direction)
{
    if (Main.netMode == NetModeID.MultiplayerClient)
    {
        //Sync the entire multitile's area.  Modify "width" and "height" to the size of your multitile in tiles
        int width = 2;
        int height = 2;
        NetMessage.SendTileRange(Main.myPlayer, i, j, width, height);

        //Sync the placement of the tile entity with other clients
        //The "type" parameter refers to the tile type which placed the tile entity, so "Type" (the type of the tile entity) needs to be used here instead
        NetMessage.SendData(MessageID.TileEntityPlacement, -1, -1, null, i, j, Type);
    }

    //ModTileEntity.Place() handles checking if the entity can be placed, then places it for you
    //Set "tileOrigin" to the same value you set TileObjectData.newTile.Origin to in the ModTile
    Point16 tileOrigin = new Point16(1, 1);
    int placedEntity = Place(i - tileOrigin.X, j - tileOrigin.Y);
    return placedEntity;
}
```

This hook is identical in usage in 1.4 tModLoader, except the definition was changed to the following:
```cs
public override int Hook_AfterPlacement(int i, int j, int type, int style, int direction, int alternate)
```

### OnNetPlace()
This hook runs when the tile entity is placed and is usually used to inform other clients of its placement.  Below is a standard usage of the hook:
```cs
public override void OnNetPlace()
{
    if (Main.netMode == NetmodeID.Server)
    {
        NetMessage.SendData(MessageID.TileEntitySharing, -1, -1, null, ID, Position.X, Position.Y);
    }
}
```

The other hooks present in `ModTileEntity` are optional and will not be covered in this guide.

## Making the Tile
After making a normal `frameImportant` multitile (see the [Basic ModTile Guide](https://github.com/tModLoader/tModLoader/wiki/Basic-Tile) for more information), you only have to add a few lines in order to make it automatically place the tile entity.

### SetDefaults() / SetStaticDefaults()
Before the `TileObjectData.addTile(Type);` line, add the following:
```cs
//MyTileEntity refers to the tile entity mentioned in the previous section
TileObjectData.newTile.HookPostPlaceMyPlayer = new PlacementHook(ModContent.GetInstance<MyTileEntity>().Hook_AfterPlacement, -1, 0, false);
```

### KillMultiTile(int i, int j, int frameX, int frameY)
Add the following line to this hook:
```cs
//ModTileEntity.Kill() handles checking if the tile entity exists and destroying it if it does exist in the world for you
Point16 origin = TileUtils.GetTileOrigin(i, j);
ModContent.GetInstance<MyTileEntity>().Kill(origin.X, origin.Y);
```

# Other Useful Things
Accessing the tile entity, opening a UI, etc. via a right-click of your multitile can be done as follows:
```cs
//This hook goes in your ModTile
public override bool NewRightClick(int i, int j)
{
    Player player = Main.LocalPlayer;

    //Should your tile entity bring up a UI, this line is useful to prevent item slots from misbehaving
    Main.mouseRightRelease = false;

    //The following four (4) if-blocks are recommended to be used if your multitile opens a UI when right clicked:
    if (player.sign > -1)
    {
        Main.PlaySound(11, -1, -1, 1);
        player.sign = -1;
        Main.editSign = false;
        Main.npcChatText = string.Empty;
    }
    if (Main.editChest)
    {
        Main.PlaySound(12, -1, -1, 1);
        Main.editChest = false;
        Main.npcChatText = string.Empty;
    }
    if (player.editedChestName)
    {
        NetMessage.SendData(MessageID.SyncPlayerChest, -1, -1, NetworkText.FromLiteral(Main.chest[player.chest].name), player.chest, 1f, 0f, 0f, 0, 0, 0);
        palyer.editedChestName = false;
    }
    if (player.talkNPC > -1)
    {
        player.talkNPC = -1;
        Main.npcChatCornerItem = 0;
        Main.npcChatText = string.Empty;
    }

    if (TileUtils.TryGetTileEntityAs(i, j, out MyTileEntity entity))
    {
        //Do things to your entity here
    }
}
```