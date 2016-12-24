# ModTileEntity

## Properties

### public Mod mod

The mod that added this ModTileEntity.

### public string Name

The internal name of this ModTileEntity.

### public int Type

The numeric type used to identify this kind of tile entity.

## Static Methods

### public static ModTileEntity GetTileEntity(int type)

Gets the base ModTileEntity object with the given type.

### public static int CountInWorld()

Returns the number of modded tile entities that exist in the world currently being played.

### public static void Initialize()

You should never use this. It is only included here for completion's sake.

### public static void NetPlaceEntity(int i, int j, int type)

You should never use this. It is only included here for completion's sake.

### public static ModTileEntity ConstructFromType(int type)

Returns a new ModTileEntity with the same class, mod, name, and type as the ModTileEntity with the given type. It is very rare that you should have to use this.

### public static ModTileEntity ConstructFromBase(ModTileEntity tileEntity)

Returns a new ModTileEntity with the same class, mod, name, and type as the parameter. It is very rare that you should have to use this.

## Methods

### public int Place(int i, int j)

A helper method that places this kind of tile entity in the given coordinates for you.

### public void Kill(int i, int j)

A helper method that removes this kind of tile entity from the given coordinates for you.

### public int Find(int i, int j)

Returns the entity ID of this kind of tile entity at the given coordinates for you.

### public override sealed void WriteExtraData(BinaryWriter writer, bool networkSend)

Don't use this. It is included only for completion's sake.

### public override sealed void ReadExtraData(BinaryReader reader, bool networkSend)

Don't use this. It is included only for completion's sake.

### public virtual bool Autoload(ref string name)

Allows you to automatically load a tile entity instead of using Mod.AddTileEntity. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name. Use this method to either force or stop an autoload, or change the default display name.

### public virtual TagCompound Save()

Allows you to save custom data for this tile entity.

### public virtual void Load(TagCompound tag)

Allows you to load the custom data you have saved for this tile entity.

### public virtual void NetSend(BinaryWriter writer)

Allows you to send custom data for this tile entity between client and server.

### public virtual void NetReceive(BinaryReader reader)

Receives the data sent in the NetSend hook.

### public abstract bool ValidTile(int i, int j)

Whether or not this tile entity is allowed to survive at the given coordinates. You should check whether the tile is active, as well as the tile's type and frame.

### public virtual int Hook_AfterPlacement(int i, int j, int type, int style, int direction)

This method does not get called by tModLoader, and is only included for you convenience so you do not have to cast the result of Mod.GetTileEntity.

### public virtual void OnNetPlace()

Code that should be run when this tile entity is placed by means of server-syncing.

### public virtual void PreGlobalUpdate()

Code that should be run before all tile entities in the world update.

### public virtual void PostGlobalUpdate()

Code that should be run after all tile entities in the world update.

### public virtual void OnKill()

This method only gets called in the Kill method. If you plan to use that, you can put code here to make things happen when it is called.