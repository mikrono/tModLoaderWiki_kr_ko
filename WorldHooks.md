# WorldHooks

This is where all ModWorld hooks are gathered and called.

## Methods

### public static void SendCustomData(BinaryWriter writer)

### public static void ReceiveCustomData(BinaryReader reader)

### public static void PreWorldGen()

### public static void ModifyWorldGenTasks(List<GenPass> passes, ref float totalWeight)

### public static void PostWorldGen()

### public static void ResetNearbyTileEffects()

### public static void PostUpdate()

### public static void TileCountsAvailable(int[] tileCounts)