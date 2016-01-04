# WallLoader

This serves as the central class from which wall-related functions are supported and carried out.

## Methods

### public static ModWall GetWall(int type)

Gets the ModWall instance with the given type. If no ModWall with the given type exists, returns null.

### public static bool KillSound(int i, int j, int type)

### public static void NumDust(int i, int j, int type, bool fail, ref int numDust)

### public static bool CreateDust(int i, int j, int type, ref int dustType)

### public static bool Drop(int i, int j, int type, ref int dropType)

### public static void KillWall(int i, int j, int type, ref bool fail)

### public static void ModifyLight(int i, int j, int type, ref float r, ref float g, ref float b)

### public static void RandomUpdate(int i, int j, int type)

### public static void AnimateWalls()

### public static bool PreDraw(int i, int j, int type, SpriteBatch spriteBatch)

### public static void PostDraw(int i, int j, int type, SpriteBatch spriteBatch)