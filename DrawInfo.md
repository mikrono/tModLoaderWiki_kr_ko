# PlayerDrawInfo

A struct that contains information that may help with PlayerLayer drawing.

## Fields

### public Player drawPlayer

The player that is being drawn.

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