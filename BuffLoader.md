# BuffLoader

This serves as the central class from which buff-related functions are supported and carried out.

## Methods

### public static ModBuff GetBuff(int type)

Gets the ModBuff instance with the given type. If no ModBuff with the given type exists, returns null.

### public static void Update(int buff, Player player, ref int buffIndex)

### public static void Update(int buff, NPC npc, ref int buffIndex)

### public static bool ReApply(int buff, Player player, int time, int buffIndex)

### public static bool ReApply(int buff, NPC npc, int time, int buffIndex)

### public static bool LongerExpertDebuff(int buff)

### public static bool CanBeCleared(int buff)

### public static void ModifyBuffTip(int buff, ref string tip, ref int rare)