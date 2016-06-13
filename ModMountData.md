# ModMountData

This class serves as a place for you to place all your properties and hooks for each mount. Create instances of ModMoundData (preferably overriding this class) to pass as parameters to Mod.AddMount.

## Properties

### public Mount.MountData mountData

The vanilla MountData object that is controlled by this ModMountData.

### public Mod mod

The mod which has added this ModMountData.

### public int Type

The index of this ModMountData in the Mount.mounts array.

### public string Name

The name of this type of mount.

## Methods

### public virtual bool Autoload(ref string name, ref string texture, IDictionary\<MountTextureType, string\> extraTextures)

Allows you to automatically load a mount instead of using Mod.AddMount. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name, texture is initialized to the namespace and overriding class name with periods replaced with slashes, and extraTextures is initialized to a dictionary containing all MountTextureTypes as keys, with texture + "_" + the texture type name as values. Use this method to either force or stop an autoload, change the default display name and texture path, and to modify the extra mount textures.

### public virtual void SetDefaults()

Allows you to set the properties of this type of mount.

### public virtual void UpdateEffects(Player player)

Allows you to make things happen when mount is used (creating dust etc.) Can also be used for mount special abilities.

### public virtual bool UpdateFrame(Player mountedPlayer, int state, Vector2 velocity)

Allows for manual updating of mount frame. Return false to stop the default frame behavior. Returns true by default.

### public virtual void UseAbility(Player player, Vector2 mousePosition, bool toggleOn)

Allows you to make things happen when the mouse is clicked while the mount is active.

### public virtual bool AimAbility(Player player, Vector2 mousePosition)

Allows you to make things happen when the mount ability is aiming (while charging).

### public virtual bool StartAbilityCharge(Player player)

Called when the mount ability starts charging.

### public virtual void StopAbilityCharge()

Called when the mount ability stops charging.