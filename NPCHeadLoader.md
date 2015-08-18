# NPCHeadLoader

This class serves as a central place from which NPC head slots are stored and NPC head textures are assigned. This can be used to obtain the corresponding slots to head textures.

## Fields

### public const int vanillaHeadCount

The number of vanilla town NPC head textures that exist.

### public const int vanillaBossHeadCount

The number of vanilla boss head textures that exist.

## Methods

### public static int GetHeadSlot(string texture)

Gets the index of the head texture corresponding to the given texture path.

### public static int GetBossHeadSlot(string texture)

Gets the index of the boss head texture corresponding to the given texture path.