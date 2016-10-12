# Mod

Mod is an abstract class that you will override. It serves as a central place from which the mod's contents are stored. It provides methods for you to use or override.

## Properties

### public TmodFile File

The TmodFile object created when tModLoader reads this mod.

### public Assembly Code

The assembly code this is loaded when tModLoader loads this mod.

### public virtual string Name

Stores the name of the mod. This name serves as the mod's identification, and also helps with saving everything your mod adds. By default this returns the name of the folder that contains all your code and stuff.

### public Version tModLoaderVersion

The version of tModLoader that was being used when this mod was built.

### public Version Version

This version number of this mod.

### public ModProperties Properties

Stores the properties of the mod.

### public ModSide Side

The ModSide that controls how this mod is synced between client and server.

### public string DisplayName

The display name of this mod in the Mods menu.

## Methods

### public virtual void Load()

Override this method to add most of your content to your mod. Here you will call other methods such as AddItem. This is guaranteed to be called after all content has been autoloaded.

### public virtual void PostSetupContent()

Allows you to load things in your mod after its content has been setup (arrays have been resized to fit the content, etc).

### public virtual void Unload()

This is called whenever this mod is unloaded from the game. Use it to undo changes that you've made in Load that aren't automatically handled (for example, modifying the texture of a vanilla item). Mods are guaranteed to be unloaded in the reverse order they were loaded in.

### public virtual void AddRecipeGroups()

Override this method to add recipe groups to this mod. You must add recipe groups by calling the RecipeGroup.RegisterGroup method here. A recipe group is a set of items that can be used interchangeably in the same recipe.

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

### public void AddItemInfo(string name, ItemInfo info)

Adds the given type of item information storage to the game, using the provided name.

### public int AddEquipTexture(ModItem item, EquipType type, string name, string texture, string armTexture = "", string femaleTexture = "")

Adds an equipment texture of the specified type, internal name, and associated item to your mod. (The item parameter may be null if you don't want to associate an item with the texture.) You can then get the ID for your texture by calling EquipLoader.GetEquipTexture, and using the EquipTexture's Slot property. If the EquipType is EquipType.Body, make sure that you also provide an armTexture and a femaleTexture. Returns the ID / slot that is assigned to the equipment texture.

### public int AddEquipTexture(EquipTexture equipTexture, ModItem item, EquipType type, string name, string texture, string armTexture = "", string femaleTexture = "")

Adds an equipment texture of the specified type, internal name, and associated item to your mod. This method is different from the other AddEquipTexture in that you can specify the class of the equipment texture, thus allowing you to override EquipmentTexture's hooks. All other parameters are the same as the other AddEquipTexture.

### public EquipTexture GetEquipTexture(string name)

Gets the EquipTexture instance corresponding to the name. Returns null if no EquipTexture with the given name is found.

### public int GetEquipSlot(string name)

Gets the slot/ID of the equipment texture corresponding to the given name. Returns -1 if no EquipTexture with the given name is found.

### public sbyte GetAccessorySlot(string name)

Same as GetEquipSlot, except returns the number as an sbyte (signed byte) for your convenience.

### public void AddFlameTexture(ModItem item, string texture)

Assigns a flame texture to the given item added by your mod. Flame textures are drawn when held by the player if the item's "flame" field is set to true. Flame textures are currently only used for torches.

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

### public void AddProjectileInfo(string name, ProjectileInfo info)

Adds the given type of projectile information storage to the game, using the provided name.

### public void AddNPC(string name, ModNPC npc, string texture, string[] altTextures = null)

Adds a type of NPC to the game with the specified name and texture. Also allows you to give the NPC alternate textures.

### public ModNPC GetNPC(string name)

Gets the ModNPC of this mod corresponding to the given name. Returns null if no ModNPC with the given name is found.

### public int NPCType(string name)

Gets the type of the ModNPC of this mod with the given name. Returns 0 if no ModNPC with the given name is found.

### public void AddGlobalNPC(string name, GlobalNPC globalNPC)

Adds the given GlobalNPC instance to this mod with the provided name.

### public GlobalNPC GetGlobalNPC(string name)

Gets the GlobalNPC instance with the given name from this mod.

### public void AddNPCInfo(string name, NPCInfo info)

Adds the given type of NPC information storage to the game, using the provided name.

### public void AddNPCHeadTexture(int npcType, string texture)

Assigns a head texture to the given town NPC type.

### public void AddBossHeadTexture(string texture)

Assigns a head texture that can be used by NPCs on the map.

### public void AddPlayer(string name, ModPlayer player)

Adds a type of ModPlayer to this mod. All ModPlayer types will be newly created and attached to each player that is loaded.

### public void AddBuff(string name, ModBuff buff, string texture)

Adds a type of buff to the game with the specified internal name and texture.

### public ModBuff GetBuff(string name)

Gets the ModBuff of this mod corresponding to the given name. Returns null if no ModBuff with the given name is found.

### public int BuffType(string name)

Gets the type of the ModBuff of this mod corresponding to the given name. Returns 0 if no ModBuff with the given name is found.

### public void AddGlobalBuff(string name, GlobalBuff globalBuff)

Adds the given GlobalBuff instance to this mod using the provided name.

### public GlobalBuff GetGlobalBuff(string name)

Gets the GlobalBuff with the given name from this mod.

### public void AddMusicBox(int musicSlot, int itemType, int tileType, int tileFrameY = 0)

Allows you to tie a music ID, and item ID, and a tile ID together to form a music box. When music with the given ID is playing, equipped music boxes have a chance to change their ID to the given item type. When an item with the given item type is equipped, it will play the music that has musicSlot as its ID. When a tile with the given type and Y-frame is nearby, if its X-frame is >= 36, it will play the music that has musicSlot as its ID.

### public void AddMount(string name, ModMountData mount, string texture, IDictionary\<MountTextureType, string\> extraTextures = null)

Adds the given mount to the game with the given name and texture. The extraTextures dictionary should optionally map types of mount textures to the texture paths you want to include.

### public ModMountData GetMount(string name)

Gets the ModMountData instance of this mod corresponding to the given name. Returns null if no ModMountData has the given name.

### public int MountType(string name)

Gets the ID of the ModMountData instance corresponding to the given name. Returns 0 if no ModMountData has the given name.

### public void AddModWorld(string name, ModWorld modWorld)

Adds a ModWorld to this mod with the given name.

### public ModWorld GetModWorld(string name)

Gets the ModWorld instance with the given name from this mod.

### public void AddUgBgStyle(string name, ModUgBgStyle ugBgStyle)

Adds the given underground background style with the given name to this mod.

### public ModUgBgStyle GetUgBgStyle(string name)

Returns the underground background style corresponding to the given name.

### public void AddSurfaceBgStyle(string name, ModSurfaceBgStyle surfaceBgStyle)

Adds the given surface background style with the given name to this mod.

### public ModSurfaceBgStyle GetSurfaceBgStyle(string name)

Returns the surface background style corresponding to the given name.

### public int GetSurfaceBgStyleSlot(string name)

Returns the Slot of the surface background style corresponding to the given name.

### public void AddGlobalBgStyle(string name, GlobalBgStyle globalBgStyle)

Adds the given global background style with the given name to this mod.

### public GlobalBgStyle GetGlobalBgStyle(string name)

Returns the global background style corresponding to the given name.

### public void AddWaterStyle(string name, ModWaterStyle waterStyle, string texture, string blockTexture)

Adds the given water style to the game with the given name, texture path, and block texture path.

### public ModWaterStyle GetWaterStyle(string name)

Returns the water style with the given name from this mod.

### public void AddWaterfallStyle(string name, ModWaterfallStyle waterfallStyle, string texture)

Adds the given waterfall style to the game with the given name and texture path.

### public ModWaterfallStyle GetWaterfallStyle(string name)

Returns the waterfall style with the given name from this mod.

### public void AddGore(string texture, ModGore modGore = null)

Adds the given texture to the game as a custom gore, with the given custom gore behavior. If no custom gore behavior is provided, the custom gore will have the default vanilla behavior.

### public int GetGoreSlot(string name)

Shorthand for calling ModGore.GetGoreSlot(this.Name + '/' + name).

### public void AddSound(SoundType type, string soundPath, ModSound modSound = null)

Adds the given sound file to the game as the given type of sound and with the given custom sound playing. If no ModSound instance is provided, the custom sound will play in a similar manner as the default vanilla ones.

### public int GetSoundSlot(SoundType type, string name)

Shorthand for calling SoundLoader.GetSoundSlot(type, this.Name + '/' + name).

### public void AddBackgroundTexture(string texture)

Adds a texture to the list of background textures and assigns it a background texture slot.

### public int GetBackgroundSlot(string name)

Gets the texture slot corresponding to the specified texture name. Shorthand for calling BackgroundTextureLoader.GetBackgroundSlot(this.Name + '/' + name).

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

### public void RegisterHotKey(string name, string defaultKey)

Registers a hotkey with a name and defaultKey.

### public virtual void HotKeyPressed(string name)

Called when a hotkey is pressed. Check against the name to verify particular hotkey that was pressed.

### public ModPacket GetPacket(int capacity = 256)

Creates a ModPacket object that you can write to and then send between servers and clients.

### public virtual void HandlePacket(BinaryReader reader, int whoAmI)

Called whenever a net message / packet is received from a client (if this is a server) or the server (if this is a client). whoAmI is the ID of whomever sent the packet (equivalent to the Main.myPlayer of the sender), and reader is used to read the binary data of the packet.

### public virtual bool HijackGetData(ref byte messageType, ref BinaryReader reader, int playerNumber)

Allows you to modify net message / packet information that is received before the game can act on it.

### public virtual Matrix ModifyTransformMatrix(Matrix Transform)

Allows you to set the transformation of the screen that is drawn. (Translations, rotations, scales, etc.)

### public virtual void ModifyInterfaceLayers(List\<MethodSequenceListItem\> layers)

Allows you to modify the elements of the in-game interface that get drawn. MethodSequenceListItem can be found in the Terraria.DataStructures namespace.

### public virtual void PostDrawInterface(SpriteBatch spriteBatch)

Called after interface is drawn but right before mouse and mouse hover text is drawn. Allows for drawing interface.

**Note:** This hook should no longer be used. It is better to use the ModifyInterfaceLayers hook.

### public virtual void PostDrawFullscreenMap(ref string mouseText)

Called while the fullscreen map is active. Allows custom drawing to the map.

### public virtual void PostUpdateInput()

Called after the input keys are polled. Allows for modifying things like scroll wheel if your custom drawing should capture that.