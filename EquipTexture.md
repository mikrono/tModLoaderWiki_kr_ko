# EquipTexture

This serves as a place for you to program behaviors of equipment textures. This is useful for equipment slots that do not have any item associated with them (for example, the Werewolf buff). Note that this class is purely for visual effects.

## Properties

### public string Texture

The name and folders of the texture file used by this equipment texture.

### public Mod mod

The mod that added this equipment texture.

### public string Name

The internal name of this equipment texture.

### public EquipType type

The type of equipment that this equipment texture is used as.

### public int Slot

The slot (internal ID) of this equipment texture.

### public ModItem item

The item that is associated with this equipment texture. Null if no item is associated with this.

## Methods

### public virtual void UpdateVanity(Player player, EquipType type)

Allows you to create special effects (such as dust) when this equipment texture is displayed on the player under the given equipment type. By default this will call the associated ModItem's UpdateVanity if there is an associated ModItem.

### public virtual bool IsVanitySet(int head, int body, int legs)

Returns whether or not the head armor, body armor, and leg armor textures make up a set. This hook is used for the PreUpdateVanitySet, UpdateVanitySet, and ArmorSetShadow hooks. By default this will return the same thing as the associated ModItem's IsVanitySet, or false if no ModItem is associated.

### public virtual void PreUpdateVanitySet(Player player)

Allows you to create special effects (such as the necro armor's hurt noise) when the player wears this equipment texture's vanity set. This hook is called regardless of whether the player is frozen in any way. By default this will call the associated ModItem's PreUpdateVanitySet if there is an associated ModItem.

### public virtual void UpdateVanitySet(Player player)

Allows you to create special effects (such as dust) when the player wears this equipment texture's vanity set. This hook will only be called if the player is not frozen in any way. By default this will call the associated ModItem's UpdateVanitySet if there is an associated ModItem.

### public virtual void ArmorSetShadows(Player player, ref bool longTrail, ref bool smallPulse, ref bool largePulse, ref bool shortTrail)

Allows you to determine special visual effects this vanity set has on the player without having to code them yourself. By default this will call the associated ModItem's ArmorSetShadows if there is an associated ModItem.