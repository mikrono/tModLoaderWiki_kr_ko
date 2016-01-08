# PlayerDrawInfo

A struct that contains information that may help with PlayerLayer drawing.

## Fields

### public Player drawPlayer

The player that is being drawn.

### public Vector2 position

The position the player should be drawn in. Use this; **do not** use drawPlayer.position.

### public float shadow

The transparency of the player, where 0f is fully opaque and 1f is fully transparent.

### public Vector2 itemLocation

Similar to Player.itemLocation, but takes PlayerDrawInfo.position into account.

### public bool drawHands

Whether or not the player's hands underneath the armor should be drawn.

### public bool drawArms

Whether or not the player's arms underneath the armor should be drawn.

### public bool drawHeldProjInFrontOfHeldItemAndBody

Whether or not the held projectile is drawn in front of or behind the held item and arms.

### public bool drawHair

Whether or not the player's hair is drawn.

### public bool drawAltHair

Whether or not the player's alternate (hat) hair is drawn.

### public int hairShader

The ID of the shader (dye) on the player's hair.

### public int headArmorShader

The ID of the shader (dye) on the player's head armor.

### public int bodyArmorShader

The ID of the shader (dye) on the player's body armor.

### public int legArmorShader

The ID of the shader (dye) on the player's leg armor.

### public int handOnShader

The ID of the shader (dye) on the player's hand on accessory.

### public int handOffShader

The ID of the shader (dye) on the player's hand off accessory.

### public int backShader

The ID of the shader (dye) on the player's back accessory.

### public int frontShader

The ID of the shader (dye) on the player's front accessory.

### public int shoeShader

The ID of the shader (dye) on the player's shoe accessory.

### public int waistShader

The ID of the shader (dye) on the player's waist accessory.

### public int shieldShader

The ID of the shader (dye) on the player's shield accessory.

### public int neckShader

The ID of the shader (dye) on the player's neck accessory.

### public int faceShader

The ID of the shader (dye) on the player's face accessory.

### public int balloonShader

The ID of the shader (dye) on the player's balloon accessory.

### public int wingShader

The ID of the shader (dye) on the player's wings.

### public int carpetShader

The ID of the shader (dye) on the player's magic carpet.

### public Color hairColor

The color of the player's hair, with lighting and transparency taken into account.

### public Color eyeWhiteColor

The color of the whites of the player's eyes, with lighting and transparency taken into account.

### public Color eyeColor

The color of the player's eyes, with lighting and transparency taken into account.

### public Color faceColor

The color of the player's face, with lighting and transparency taken into account.

### public Color bodyColor

The color of the player's body skin, with lighting and transparency taken into account.

### public Color legColor

The color of the player's leg skin, with lighting and transparency taken into account.

### public Color shirtColor

The color of the player's shirt, with lighting and transparency taken into account.

### public Color underShirtColor

The color of the player's under-shirt, with lighting and transparency taken into account.

### public Color pantsColor

The color of the player's pants, with lighting and transparency taken into account.

### public Color shoeColor

The color of the player's shoes, with lighting and transparency taken into account.

### public Color upperArmorColor

The color of all armor and accessories on the upper third of the player, with lighting and transparency taken into account.

### public Color middleArmorColor

The color of all armor and accessories on the middle third of the player, with lighting and transparency taken into account.

### public Color mountColor

The color of the player's mount, with lighting and transparency taken into account.

### public Color lowerArmorColor

The color of all armor and accessories on the lower third of the player, with lighting and transparency taken into account.

### public int headGlowMask

The ID of the glow-mask on the player's head.

### public int bodyGlowMask

The ID of the glow-mask on the player's body.

### public int armGlowMask

The ID of the glow-mask on the player's arms.

### public int legGlowMask

The ID of the glow-mask on the player's legs.

### public Color headGlowMaskColor

The color of the glow-mask on the player's head.

### public Color bodyGlowMaskColor

The color of the glow-mask on the player's body.

### public Color armGlowMaskColor

The color of the glow-mask on the player's arms.

### public Color legGlowMaskColor

The color of the glow-mask on the player's legs.

### public SpriteEffects spriteEffects

The SpriteEffects that should be used to draw the player (how the sprite should be flipped).

### public Vector2 headOrigin

The point around which the player's head texture rotates.

### public Vector2 bodyOrigin

The point around which the player's body texture rotates.

### public Vector2 legOrigin

The point around which the player's leg texture rotates.

# PlayerHeadDrawInfo

A struct that contains information that may help with PlayerHeadLayer drawing.

## Fields

### public SpriteBatch spriteBatch

The SpriteBatch object that should be used to do all the drawing. This is the same as Main.spriteBatch.

### public Player drawPlayer

The player whose head is being drawn.

### public float alpha

The transparency in which the player should be drawn. 0 means fully transparent, while 1 means fully opaque.

### public float scale

The scale on the size in which the player should be drawn.

### public short hairShader

The ID of the shader (dye) on the player's hair.

### public int armorShader

The ID of the shader (dye) on the player's head armor.

### public Color eyeWhiteColor

The color of the whites of the player's eyes. Alpha has already been taken into account.

### public Color eyeColor

The color of the player's eyes. Alpha has already been taken into account.

### public Color hairColor

The color of the player's hair. Alpha has already been taken into account.

### public Color skinColor

The color of the player's skin. Alpha has already been taken into account.

### public Color armorColor

The color the player's armor should be shaded in. Alpha has already been taken into account.

### public SpriteEffects spriteEffects

The SpriteEffects that should be used to draw the player. (SpriteEffects.None or SpriteEffects.FlipHorizontal)

### public Vector2 drawOrigin

The point on the player's texture around which everything should be rotated.

### public bool drawHair

Whether the player's hair texture should be drawn.

### public bool drawHairAlt

Whether the player's alternate (hat) hair texture should be drawn.