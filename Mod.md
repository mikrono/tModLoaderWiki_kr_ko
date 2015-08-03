# Mod

Mod is an abstract class that you will override. It serves as a central place from which the mod's contents are stored. It provides methods for you to use or override.

## Properties

### public string Name

Stores the name of the mod. This name serves as the mod's identification, and also helps with saving everything your mod adds.

### public ModProperties Properties

Stores the properties of the mod.

## Methods

### public abstract void SetModInfo(out string name, ref ModProperties properties)

Override this method in order to give your mod a name and set its properties.

### public abstract void Load()

Override this method to add most of your content to your mod. Here you will call other methods such as AddItem.

### public virtual void AddRecipes()

Override this method to add recipes to the game. It is recommended that you do so through instances of ModRecipe, since it provides methods that simplify recipe creation.

### public void AddItem(string name, ModItem item, string texture)

Adds a type of item to your mod with the specified internal name. This method should be called in Load. You can obtain an instance of ModItem by overriding it then creating an instance of the subclass. The texture parameter follows the same format for texture names of ModLoader.GetTexture.

### public ModItem GetItem(string name)

Gets the ModItem instance corresponding to the name. Because this method is in the Mod class, conflicts between mods are avoided.

### public int ItemType(string name)

Gets the internal ID / type of the ModItem corresponding to the name.

### public void SetGlobalItem(GlobalItem globalItem)

Sets the GlobalItem instance for this mod.

### public GlobalItem GetGlobalItem()

Gets the GlobalItem instance for this mod.

### public int AddEquipTexture(ModItem item, EquipType type, string texture, string armTexture = "", string femaleTexture = "")

Adds an equipment texture of the specified type to your mod. You can then get the ID for your texture by calling EquipLoader.GetEquipSlot. If the EquipType is EquipType.Body, make sure that you also provide an armTexture and a femaleTexture. Returns the ID / slot that is assigned to the equipment texture.

### public void AddDust(string name, ModDust dust, string texture = "")

Adds a type of dust to your mod with the specified name. Create an instance of ModDust normally, preferably through the constructor of an overriding class. Leave the texture as an empty string to use the vanilla dust sprite sheet.

### public void AddTile(string name, ModTile tile, string texture)

Adds a type of tile to the game with the specified name and texture.

### public ModTile GetTile(string name)

Gets the ModTile of this mod corresponding to the given name. Returns null if no ModTile with the given name is found.

### public int TileType(string name)

Gets the type of the ModTile of this mod with the given name. Returns 0 if no ModTile with the given name is found.

### public void SetGlobalTile(GlobalTile globalTile)

Sets the GlobalTile instance for this mod.

### public GlobalTile GetGlobalTile()

Gets the GlobalTile instance for this mod.

### public void SetGlobalNPC(GlobalNPC globalNPC)

Sets the GlobalNPC instance for this mod.

### public GlobalNPC GetGlobalNPC()

Gets the GlobalNPC instance for this mod.