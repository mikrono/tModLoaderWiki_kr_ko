# ModWorld

A ModWorld instance represents an extension of a World. You can store fields in the ModWorld classes to keep track of mod-specific information on the world. It also contains hooks to insert your code into the world generation process.

## Properties

### public Mod mod

The mod that added this type of ModWorld.

### public string Name

The name of this ModWorld. Used for distinguishing between multiple ModWorlds added by a single Mod.

### public virtual bool Autoload(ref string name)

Allows you to automatically add a ModWorld instead of using Mod.AddModWorld. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name. Use this to either force or stop an autoload, or change the name that identifies this type of ModWorld.

### public virtual void Initialize()

Called whenever the world is loaded. This can be used to initialize data structures, etc.

### public virtual void SaveCustomData(BinaryWriter writer)

Allows you to save custom data for this world. Useful for things like saving world specific flags. For example, if your mod adds a boss and you want certain NPC to only spawn once it has been defeated, this is where you would store the information that that boss has been defeated in this world. You are only able to save up to 64 KB of information (I don't imagine anyone will ever need more than that).

### public virtual void LoadCustomData(BinaryReader reader)

Allows you to load custom data you have saved for this player.

### public virtual void PreWorldGen()

Allows a mod to run code before a world is generated. 

### public virtual void PostWorldGen()

Use this method to place tiles in the world after world generation is complete. 
	
### public virtual void ModifyWorldGenTasks(List\<GenPass\> tasks, ref float totalWeight)

A more advanced option to PostWorldGen, this method allows you modify the list of Generation Passes before a new world begins to be generated. For example, removing the "Planting Trees" pass will cause a world to generate without trees. Placing a new Generation Pass before the "Dungeon" pass will prevent the the mod's pass from cutting into the dungeon.

### public virtual void ResetNearbyTileEffects()

Use this to reset any fields you set in any of your ModTile.NearbyEffects hooks back to their default values.

### public virtual void PostUpdate()

Use this method to have things happen in the world. In vanilla Terraria, a good example of code suitable for this hook is how Falling Stars fall to the ground during the night. This hook is called every frame.

### public virtual void TileCountsAvailable(int[] tileCounts)

Allows you to store information about how many of each tile is nearby the player. This is useful for counting how many tiles of a certain custom biome there are. The tileCounts parameter stores the tile count indexed by tile type.