# Basic NPC Spawning
This guide will teach the basics of spawning enemies.

# Basic Ideas
There are a few ideas you need to understand first:

## Balance
It is hard to guess a good value for the return of ModNPC.SpawnChance. We don't want our NPC spawning too often compared to vanilla NPC. Usually a value of 0.1f or something smaller is good, but you should use [Modders Toolkit's](https://forums.terraria.org/index.php?threads/modders-toolkit-a-mod-for-modders-doing-modding.55738/) NPC Spawn Tool to compare your NPC's spawn rate to vanilla and other Mod's NPCs.

## Terraria Spawning
Terraria spawns NPC by first deciding on a position to spawn the NPC, and then asking each NPC if they would like to spawn at that position. Each time Terraria decides to spawn an NPC, it will be in conjunction with a Player object (NPCSpawnInfo.player). In Multiplayer, the server handles all spawning decisions. If you make a custom biome in your mod and notice that spawning doesn't work correctly in multiplayer, your ModPlayer.SendCustomBiomes and related hooks need to be implemented correctly so the server knows the correct values of the custom biome booleans, so that it can make the correct decisions.

## Return values
The ModNPC.SpawnChance hook returns a float. Google that is you don't understand. The ModNPC.CanTownNPCSpawn hook returns a bool.

## ModNPC.SpawnChance
This is the main focus of this guide. All naturally spawning non-boss, non-townNPC ModNPC classes should override this hook:

    public override float SpawnChance(NPCSpawnInfo spawnInfo)
    {
    	// Code goes here
    }

## ModNPC.CanTownNPCSpawn
Only for townNPC ModNPC. Note that this class returns a bool not a float. Use this to let your townNPC spawn after certain conditions, such as defeating bosses, have been met.

    public override bool CanTownNPCSpawn(int numTownNPCs, int money)
    {
    	// Code goes here
    }

## Conditionals (if-else)
Spawning NPC boils down to making a decision whether or not to spawn our ModNPC. Read up on if-else [here](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/if-else).

### Not Operator (!)
Read up [here](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/logical-negation-operator).

    if(Main.dayTime) // if day time

    if(!Main.dayTime) // if night time

### And and OR (&& and ||)
Read up on [&&](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/conditional-and-operator) and [||](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/conditional-or-operator). Note that && has higher precedence than ||. We will use this to combine conditions.

### Ternary
A more compact version of an if-else conditional is a ternary. Read up on it [here](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/conditional-operator). Basically it changes: 

    if (condition)
    {
    	return .1f;
    }
    else
    {
    	return 0f;
    }
To this:

    return condition ? .1f : 0f;

# NPCSpawnInfo
NPCSpawnInfo is a struct that contains all the info pertaining to the spawn position that Terraria wishes to spawn an NPC. See [documentation](http://blushiemagic.github.io/tModLoader/html/struct_terraria_1_1_mod_loader_1_1_n_p_c_spawn_info.html) for the fields. We will use the values from this struct to guide our logic and arrive at a final decision.

## Player Biomes
Use the player object passed in inside NPCSpawnInfo rather than Main.LocalPlayer to use player biomes in spawn logic. [List of Zone Booleans](http://blushiemagic.github.io/tModLoader/html/struct_terraria_1_1_mod_loader_1_1_n_p_c_spawn_info.html#a894868167c60f17bea09fba0aea811a8)

    if(spawnInfo.player.ZoneJungle) // Vanilla Biome aka Zone

    if(spawnInfo.player.GetModPlayer<ExamplePlayer>().ZoneExample) // Mod Biome

# Other Values
In addition to NPCSpawnInfo, we can also use other fields in our SpawnChance logic:
* `Main.dayTime` - true during the day, false at night
* `NPC.downedGolemBoss` and [others](https://github.com/blushiemagic/tModLoader/wiki/Useful-Vanilla-Fields#npc-fields) - true if the named boss has been defeated in this world
* `Main.hardMode` - true if in hard mode
* `Main.expertMode` - true if in expert mode
* `Main.time` - A value between 0 (4:30 AM) and 54000 (7:30 PM) during the day and between 0 (7:30 PM) and 32400 (4:30 AM) during the night. Use with Main.dayTime. Main.time usually increments by 1 each tick. Each in-game hour is 3600 ticks. 
  * Example: `Main.dayTime && Main.time < 18000.0` - Morning between 4:30 AM and 9:30 AM (because 18000/3600 == 5) 
* `Main.raining` - true if currently raining
* `NPC.AnyNPCs(NPCID.IceGolem)` - true if any Ice Golem is in the world. Use with ! to prevent duplicate spawns of mini-bosses.
  * `NPC.AnyNPCs(mod.NPCType<PartyZombie>())` or `NPC.AnyNPCs(mod.NPCType("PartyZombie"))` - same, but for modded NPC
* `NPC.CountNPCS(NPCID.AngryNimbus) < 2` - true if less than 2 of the NPC exist in the world
* `TileID.Sets.Conversion.Sand[spawnInfo.spawnTileType]` - true if the spawn tile is any type of sand tile. There are other sets in TileID.Sets.Conversion that might be useful
* `Math.Abs(spawnInfo.spawnTileX - Main.spawnTileX) > Main.maxTilesX / 3` - true if spawn tile is on one of the outer thirds of the map
* `NPC.waveNumber` - the wave number during an event
* Need More? Ask for help from us on Discord and we can add more to this list.

# SpawnCondition
SpawnCondition is a class that contains a set of ready-to-use fields that mimic the logic of various Vanilla NPC spawn conditions. See [Documentation](http://blushiemagic.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_spawn_condition.html) for available SpawnConditions. Using SpawnCondition fields can simplify you SpawnChance logic. For example, a daytime slime can easily be implemented like this:

    return SpawnCondition.OverworldDaySlime.Chance * 0.1f;

as opposed to

    return Main.dayTime && info.spawnTileY <= Main.worldSurface ? 0.1f : 0f;

# Examples
Each of the following examples will be as if they were inside a ModNPC.SpawnChance method:

    public override float SpawnChance(NPCSpawnInfo spawnInfo)
    {
    	// Example Code Here
    }

### Spawn on my ModTile
    return spawnInfo.spawnTileType == mod.TileType<Tiles.CrystalBlock>() ? .1f : 0f;
### Spawn if player in custom Biome/Zone
    return spawnInfo.player.GetModPlayer<CrystalPlayer>().ZoneCrystal ? .1f : 0f;
### Spawn in Jungle Temple
    return spawnInfo.spawnTileType == TileID.LihzahrdBrick && spawnInfo.lihzahrd ? .1f : 0f;
    // or
    return SpawnCondition.JungleTemple.Chance * 0.1f;
### Spawn during Solar Eclipse
    return spawnInfo.spawnTileY <= Main.worldSurface && Main.dayTime && Main.eclipse
    // or
    return SpawnCondition.SolarEclipse.Chance * 0.05f; // Remember to test this value for balance
### Player standing on Sunplate tile
    // Here I show off 2 ways of converting a boolean to an int. (False is 0, True is 1)
    return (Main.tile[spawnInfo.playerFloorX, spawnInfo.playerFloorY].type == TileID.Sunplate).ToInt() * 0.2f;
    // or
    return Convert.ToInt32(Main.tile[spawnInfo.playerFloorX, spawnInfo.playerFloorY].type == TileID.Sunplate) * 0.2f; // using System;

# Combining Snippets
Just like in Examples above, we combine pieces of logic to construct our final decision. See !, &&, and || above.

# Common Errors
### CS0161 '[ClassName].SpawnChance(NPCSpawnInfo)': not all code paths return a value
This means your code has a chance to not return a value. For example.

    if (spawnInfo.granite)
        return 0.2f;
would need to be fixed like this:

    if (spawnInfo.granite)
        return 0.2f;
    return 0f;

### CS0029 Cannot implicitly convert type 'bool' to 'float'
This means you probably forgot to use your logic to decide on a value to return. See above.

# Relevant References
* [ModNPC.SpawnChance Documentation](http://blushiemagic.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod_n_p_c.html#ae7713bbbd313012944b958e8eafc35e0)
* [NPCSpawnInfo Documentation](http://blushiemagic.github.io/tModLoader/html/struct_terraria_1_1_mod_loader_1_1_n_p_c_spawn_info.html)
* [SpawnCondition Documentation](http://blushiemagic.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_spawn_condition.html)

# Not covered in Basic level
* ModNPC.CheckConditions - Custom TownNPC House conditions (like Truffle)
* Mod Boss Booleans - See ExampleMod for proper syncing and usage
* Using ModNPC.SpawnNPC - Manipulating NPC at spawn
* GlobalNPC.EditSpawnRate - Manipulate max spawns and spawn rate (Water Candle)
* GlobalNPC.EditSpawnRange
* GlobalNPC.EditSpawnPool - Manipulate Spawn Pool after populated with choices
* GlobalNPC.SpawnNPC