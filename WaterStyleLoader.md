# WaterStyleLoader

## Properties

### public static int WaterStyleCount

The number of water styles that exist in the game, both vanilla and modded.

## Methods

### public static ModWaterStyle GetWaterStyle(int style)

Returns the ModWaterStyle with the given ID.

### public static void ChooseWaterStyle(ref int style)

### public static void UpdateLiquidAlphas()

### public static void DrawWatersToScreen(bool bg)

### public static void DrawWaterfall(WaterfallManager waterfallManager, SpriteBatch spriteBatch)

### public static void LightColorMultiplier(int style, ref float r, ref float g, ref float b)