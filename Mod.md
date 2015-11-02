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

### public virtual void Load()

Override this method to add most of your content to your mod. Here you will call other methods such as AddItem. This is guaranteed to be called after all content has been autoloaded.

### public virtual void PostSetupContent()

Allows you to load things in your mod after its content has been setup (arrays have been resized to fit the content, etc).

### public virtual void Unload()

This is called whenever this mod is unloaded from the game. Use it to undo changes that you've made in Load that aren't automatically handled (for example, modifying the texture of a vanilla item). Mods are guaranteed to be unloaded in the reverse order they were loaded in.

### public virtual void AddCraftGroups()

Override this method to add craft groups to this mod. You must add craft groups by calling the AddCraftGroup method here.

### public void AddCraftGroup(string name, string displayName, params int[] items)

Adds a craft group to this mod with the specified internal name, display name, and items.

### public CraftGroup GetCraftGroup(string name)

Gets the CraftGroup object of this mod with the corresponding internal name.

### public virtual void AddRecipes()

Override this method to add recipes to the game. It is recommended that you do so through instances of ModRecipe, since it provides methods that simplify recipe creation.

### public void AddItem(string name, ModItem item, string texture)

Adds a type of item to your mod with the specified internal name. This method should be called in Load. You can obtain an instance of ModItem by overriding it then creating an instance of the subclass. The texture parameter follows the same format for texture names of ModLoader.GetTexture.

### public ModItem GetItem(string name)

Gets the ModItem instance corresponding to the name. Because this method is in the Mod class, conflicts between mods are avoided. Returns null if no ModItem with the given name is found.

### public int ItemType(string name)

Gets the internal ID / type of the ModItem corresponding to the name. Returns 0 if no ModItem with the given name is found.

### public void AddGlobalItem(string name, GlobalItem globalItem)

Adds the given GlobalItem instance to this mod with the provided name.

### public GlobalItem GetGlobalItem(string name)

Gets the GlobalItem instance with the given name from this mod.

### public int AddEquipTexture(ModItem item, EquipType type, string texture, string armTexture = "", string femaleTexture = "")

Adds an equipment texture of the specified type to your mod. You can then get the ID for your texture by calling EquipLoader.GetEquipSlot. If the EquipType is EquipType.Body, make sure that you also provide an armTexture and a femaleTexture. Returns the ID / slot that is assigned to the equipment texture.

### public void AddDust(string name, ModDust dust, string texture = "")

Adds a type of dust to your mod with the specified name. Create an instance of ModDust normally, preferably through the constructor of an overriding class. Leave the texture as an empty string to use the vanilla dust sprite sheet.

### public ModDust GetDust(string name)

Gets the ModDust of this mod corresponding to the given name. Returns null if no ModDust with the given name is found.

### public int DustType(string name)

Gets the type of the ModDust of this mod with the given name. Returns 0 if no ModDust with the given name is found.

### public void AddTile(string name, ModTile tile, string texture)

Adds a type of tile to the game with the specified name and texture.

### public ModTile GetTile(string name)

Gets the ModTile of this mod corresponding to the given name. Returns null if no ModTile with the given name is found.

### public int TileType(string name)

Gets the type of the ModTile of this mod with the given name. Returns 0 if no ModTile with the given name is found.

### public void AddGlobalTile(string name, GlobalTile globalTile)

Adds the given GlobalTile instance to this mod with the provided name.

### public GlobalTile GetGlobalTile(string name)

Gets the GlobalTile instance with the given name from this mod.

### public void AddWall(string name, ModWall wall, string texture)

Adds a type of wall to the game with the specified name and texture.

### public ModWall GetWall(string name)

Gets the ModWall of this mod corresponding to the given name. Returns null if no ModWall with the given name is found.

### public int WallType(string name)

Gets the type of the ModWall of this mod with the given name. Returns 0 if no ModWall with the given name is found.

### public void AddGlobalWall(string name, GlobalWall globalWall)

Adds the given GlobalWall instance to this mod with the provided name.

### public GlobalWall GetGlobalWall(string name)

Gets the GlobalWall instance with the given name from this mod.

### public void AddProjectile(string name, ModProjectile projectile, string texture)

Adds a type of projectile to the game with the specified name and texture.

### public ModProjectile GetProjectile(string name)

Gets the ModProjectile of this mod corresponding to the given name. Returns null if no ModProjectile with the given name is found.

### public int ProjectileType(string name)

Gets the type of the ModProjectile of this mod with the given name. Returns 0 if no ModProjectile with the given name is found.

### public void AddGlobalProjectile(string name, GlobalProjectile globalProjectile)

Adds the given GlobalProjectile instance to this mod with the provided name.

### public GlobalProjectile GetGlobalProjectile(string name)

Gets the GlobalProjectile instance with the given name from this mod.

### public void AddNPC(string name, ModNPC npc, string texture)

Adds a type of NPC to the game with the specified name and texture.

### public ModNPC GetNPC(string name)

Gets the ModNPC of this mod corresponding to the given name. Returns null if no ModNPC with the given name is found.

### public int NPCType(string name)

Gets the type of the ModNPC of this mod with the given name. Returns 0 if no ModNPC with the given name is found.

### public void AddGlobalNPC(string name, GlobalNPC globalNPC)

Adds the given GlobalNPC instance to this mod with the provided name.

### public GlobalNPC GetGlobalNPC(string name)

Gets the GlobalNPC instance with the given name from this mod.

### public void AddNPCHeadTexture(int npcType, string texture)

Assigns a head texture to the given town NPC type.

### public void AddBossHeadTexture(string texture)

Assigns a head texture that can be used by NPCs on the map.

### public void AddGore(string texture, ModGore modGore = null)

Adds the given texture to the game as a custom gore, with the given custom gore behavior. If no custom gore behavior is provided, the custom gore will have the default vanilla behavior.

### public void AddSound(SoundType type, string soundPath, ModSound modSound = null)

Adds the given sound file to the game as the given type of sound and with the given custom sound playing. If no ModSound instance is provided, the custom sound will play in a similar manner as the default vanilla ones.

### public void AddMusicBox(int musicSlot, int itemType, int tileType, int tileFrameY = 0)

Allows you to tie a music ID, and item ID, and a tile ID together to form a music box. When music with the given ID is playing, equipped music boxes have a chance to change their ID to the given item type. When an item with the given item type is equipped, it will play the music that has musicSlot as its ID. When a tile with the given type and Y-frame is nearby, if its X-frame is >= 36, it will play the music that has musicSlot as its ID. This method must be called in the PostSetupContent hook.

### public void AddMount(string name, ModMountData mount, string texture, IDictionary\<MountTextureType, string\> extraTextures = null)

Adds the given mount to the game with the given name and texture. The extraTextures dictionary should optionally map types of mount textures to the texture paths you want to include.

### public ModMountData GetMount(string name)

Gets the ModMountData instance of this mod corresponding to the given name. Returns null if no ModMountData has the given name.

### public int MountType(string name)

Gets the ID of the ModMountData instance corresponding to the given name. Returns 0 if no ModMountData has the given name.

### public int GetGoreSlot(string name)

Shorthand for calling ModGore.GetGoreSlot(this.FileName(name)).

### public int public int GetSoundSlot(SoundType type, string name)

Shorthand for calling SoundLoader.GetSoundSlot(type, this.FileName(name)).

### public string FileName(string fileName)

Shorthand for this.Name + "/" + fileName.

### public byte[] GetFileBytes(string name)

Shorthand for calling ModLoader.GetFileBytes(this.FileName(name)).

### public bool FileExists(string name)

Shorthand for calling ModLoader.FileExists(this.FileName(name)).

### public Texture2D GetTexture(string name)

Shorthand for calling ModLoader.GetTexture(this.FileName(name)).

### public bool TextureExists(string name)

Shorthand for calling ModLoader.TextureExists(this.FileName(name)).

### public void AddTexture(string name, Texture2D texture)

Shorthand for calling ModLoader.AddTexture(this.FileName(name), texture).

### public SoundEffect GetSound(string name)

Shorthand for calling ModLoader.GetSound(this.FileName(name)).

### public bool SoundExists(string name)

Shorthand for calling ModLoader.SoundExists(this.FileName(name)).

### public virtual void ChatInput(string text)

Allows you to make anything happen whenever the player for this game inputs a message into the chat.

### public virtual void UpdateMusic(ref int music)

Allows you to determine what music should currently play.