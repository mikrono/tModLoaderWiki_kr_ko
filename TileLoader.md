# TileLoader

This serves as the central class from which tile-related functions are supported and carried out.

## Methods

### public static ModTile GetTile(int type)

Gets the ModTile instance with the given type. If no ModTile with the given type exists, returns null.

### public static void CheckModTile(int i, int j, int type)

### public static void DisableSmartCursor(Tile tile, ref bool disable)

### public static int OpenDoorID(Tile tile)

### public static int CloseDoorID(Tile tile)

### public static bool IsChest(int type)

### public static string ModChestName(int type)

### public static bool IsModBed(int type)

### public static bool IsPlatform(int type)

### public static bool IsTorch(int type)

### public static bool IsModMusicBox(Tile tile)

### public static bool KillSound(int i, int j, int type)

### public static void NumDust(int i, int j, int type, bool fail, ref int numDust)

### public static bool CreateDust(int i, int j, int type, ref int dustType)

### public static void DropCritterChance(int i, int j, int type, ref int wormChance, ref int grassHopperChance, ref int jungleGrubChance)

### public static bool Drop(int i, int j, int type)

### public static bool CanKillTile(int i, int j, int type, ref bool blockDamaged)

### public static void KillTile(int i, int j, int type, ref bool fail, ref bool effectOnly, ref bool noItem)

### public static void KillMultiTile(int i, int j, int frameX, int frameY, int type)

### public static void NearbyEffects(int i, int j, int type, bool closer)

### public static void ModifyLight(int i, int j, int type, ref float r, ref float g, ref float b)

### public static bool Dangersense(int i, int j, int type, Player player)

### public static void SetSpriteEffects(int i, int j, int type, ref SpriteEffects spriteEffects)

### public static void SetDrawPositions(int i, int j, ref int width, ref int offsetY, ref int height)

### public static void AnimateTiles()

### public static void SetAnimationFrame(int type, ref int frameY)

### public static bool PreDraw(int i, int j, int type, SpriteBatch spriteBatch)

### public static void PostDraw(int i, int j, int type, SpriteBatch spriteBatch)

### public static void RandomUpdate(int i, int j, int type)

### public static bool TileFrame(int i, int j, int type, ref bool resetFrame, ref bool noBreak)

### public static void MineDamage(int minePower, ref int damage)

### public static void PickPowerCheck(Tile target, int pickPower, ref int damage)

### public static bool CanPlace(int i, int j)

### public static void AdjTiles(Player player, int type)

### public static void RightClick(int i, int j)

### public static void MouseOver(int i, int j)

### public static void MouseOverFar(int i, int j)

### public static int AutoSelect(int i, int j, Player player)

### public static void HitWire(int i, int j, int type)

### public static bool Slope(int i, int j, int type)

### public static bool HasWalkDust(int type)

### public static void WalkDust(int type, ref int dustType, ref bool makeDust, ref Color color)