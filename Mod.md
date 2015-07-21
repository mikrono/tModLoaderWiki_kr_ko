# Mod

Mod is an abstract class that you will override. It serves as a central place from which the mod's contents are stored. It provides methods for you to use or override.

## Properties

### public string Name

Stores the name of the mod. This name serves as the mod's identification, and also helps with saving everything your mod adds.

### public string Version

Currently has no use.

### public string Author

Currently has no use.

## Methods

### public abstract void SetModInfo(out string name, ref string version, ref string author)

Override this method in order to give your mod a name, version, and author.

### public abstract void Load()

Override this method to add most of your content to your mod. Here you will call other methods such as AddItem.

### public virtual void AddRecipes()

Override this method to add recipes to the game. It is recommended that you do so through instances of ModRecipe, since it provides methods that simplify recipe creation.

### public void AddItem(string name, ModItem item, string texture)

Adds a type of item to your mod with the specified name. This method should be called in Load. You can obtain an instance of ModItem by overriding For those of you familiar with tAPI, keep in mind that the internal name and display name are the same thing in tModLoader. The texture parameter follows the same format for texture names of ModLoader.GetTexture.

### public ModItem GetItem(string name)

Gets the ModItem instance corresponding to the name. Because this method is in the Mod class, conflicts between mods are avoided.

### public int ItemType(string name)

Gets the internal ID / type of the ModItem corresponding to the name.

### public void SetGlobalItem(GlobalItem globalItem)

Sets the GlobalItem instance for this mod.

### public GlobalItem GetGlobalItem()

Gets the GlobalItem instance for this mod.

### public void AddEquipTexture(EquipType type, string texture, string armTexture = "", string femaleTexture = "")

Adds an equipment texture of the specified type to your mod. You can then get the ID for your texture by calling EquipLoader.GetEquipSlot. If the EquipType is EquipType.Body, make sure that you also provide an armTexture and a femaleTexture.