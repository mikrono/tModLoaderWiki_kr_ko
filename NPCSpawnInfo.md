# NPCSpawnInfo

A struct that stores information regarding where an NPC is naturally spawning and the player it is spawning around. This serves to reduce the parameter count for ModNPC.CanSpawn and GlobalNPC.EditSpawnPool.

## Fields

### public int spawnTileX

The x-coordinate of the tile the NPC will spawn above.

### public int spawnTileY

The y-coordinate of the tile the NPC will spawn above.

### public Player player

The player that this NPC is spawning around.

For convenience, here are the player zones, which are also useful for determining NPC spawn:
> ZoneDungeon  
> ZoneCorrupt  
> ZoneHoly  
> ZoneMeteor
> ZoneJungle  
> ZoneSnow  
> ZoneCrimson  
> ZoneWaterCandle  
> ZonePeaceCandle  
> ZoneTowerSolar  
> ZoneTowerVortex  
> ZoneTowerNebula  
> ZoneTowerStardust  
> ZoneDesert  
> ZoneGlowshroom  
> ZoneUndergroundDesert  
> ZoneSkyHeight  
> ZoneOverworldHeight  
> ZoneDirtLayerHeight  
> ZoneRockLayerHeight  
> ZoneUnderworldHeight  
> ZoneBeach  
> ZoneRain  
> ZoneSandstorm  

### public int playerFloorX

The x-coordinate of the tile the player is standing on.

### public int playerFloorY

The y-coordinate of the tile the player is standing on.

### public bool sky

Whether or not the player is in the sky biome, where harpies and wyverns spawn.

### public bool lihzahrd

Whether or not the player is inside the jungle temple, where Lihzahrds spawn.

### public bool playerSafe

Whether or not the player is in front of a player-placed wall or in a large town. If this is true, enemies that can attack through walls should not spawn (unless an invasion is in progress).

### public bool invasion

Whether or not there is an invasion going on and the player is near it.

### public bool water

Whether or not the tile the NPC will spawn in contains water.

### public bool granite

Whether or not the NPC will spawn on a granite block or the player is near a granite biome.

### public bool marble

Whether or not the NPC will spawn on a marble block or the player is near a marble biome.

### public bool spiderCave

Whether or not the player is in a spider cave or the NPC will spawn near one.

### public bool playerInTown

Whether or not the player is in a town. This is used for spawning critters instead of monsters.

### public bool desertCave

Whether or not the player is in front of a desert wall or the NPC will spawn near one.

### public bool planteraDefeated

Whether Plantera is defeated and the world is in hardmode. This isn't needed (it's easy to find by yourself), but it's a local flag in NPC.SpawnNPC, so it is included for completeness.

### public bool safeRangeX

Whether or not the NPC is horizontally within the range near the player in which NPCs cannot spawn. If this is true, it also means that it is vertically outside of the range near the player in which NPCs cannot spawn.