# EquipLoader

This serves as a central place to store equipment slots and their corresponding textures. You will use this to obtain the IDs for your equipment textures.

## Methods

### public static int GetEquipSlot(EquipType type, string texture)

Gets the equipment slot for the specified equipment type and corresponding to the given texture.

### public static sbyte GetAccessorySlot(EquipType type, string texture)

Same as GetEquipSlot except returns the slot as an sbyte for your convenience.