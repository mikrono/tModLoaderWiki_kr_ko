# public abstract class DrawLayer\<InfoType\>

This class represents a layer of the drawing of an object, using a certain type of InfoType to help with its drawing.

## Fields

### public readonly string mod

The name of the mod to which this DrawLayer belongs.

### public readonly string Name

The name which identifies this DrawLayer.

### public readonly DrawLayer\<InfoType\> parent

The parent of this DrawLayer. If the parent is not drawn, this layer will not be drawn either. Defaults to null, which skips the parent check.

### public readonly Action\<InfoType\> layer

The delegate of this method, which can either do the actual drawing or add draw data, depending on what kind of layer this is.

### public bool visible

Whether or not this DrawLayer should be drawn. For vanilla layers, this will be set to true before all drawing-related hooks are called. For modded layers, you must set this to true or false yourself.

## Constructors

### public DrawLayer(string mod, string name, Action\<InfoType\> layer)

Creates a DrawLayer with the given mod name, identifier name, and drawing action.

### public DrawLayer(string mod, string name, DrawLayer\<InfoType\> parent, Action\<InfoType\> layer)

Creates a DrawLayer with the given mod name, identifier name, parent layer, and drawing action.

## Methods

### public bool ShouldDraw(object layerList)

Whether or not this layer should be drawn. Returns false if visible is false. If layerList is of type List\<DrawLayer\<InfoType\>\> and parent is not null, this will also return false if the parent is not drawn.

### public void Draw(InfoType drawInfo)

Invokes this DrawLayer's layer action.

# public class PlayerLayer : DrawLayer\<PlayerDrawInfo\>

This class represents a DrawLayer for the player, and uses PlayerDrawInfo as its InfoType. Drawing should be done by adding Terraria.DataStructures.DrawData objects to Main.playerDrawData.

## Constructors

### public PlayerLayer(string mod, string name, Action\<PlayerDrawInfo\> layer)

Creates a PlayerLayer with the given mod name, identifier name, and drawing action.

### public PlayerLayer(string mod, string name, PlayerLayer parent, Action\<PlayerDrawInfo\> layer)

Creates a PlayerLayer with the given mod name, identifier name, parent layer, and drawing action.

## Fields

All vanilla PlayerLayers have the same Name as their field name.

### public static readonly PlayerLayer HairBack

Draws the player's hair. To be honest this layer seems kind of useless.

### public static readonly PlayerLayer MountBack

Draws the back textures of the player's mount. Also draws the player's magic carpet.

### public static readonly PlayerLayer MiscEffectsBack

Draws miscellaneous effects behind the player.

### public static readonly PlayerLayer BackAcc

Draws the player's back accessory and held item's backpack.

### public static readonly PlayerLayer Wings

Draws the layer's wings.

### public static readonly PlayerLayer BalloonAcc

Draws the player's balloon accessory.

### public static readonly PlayerLayer Skin

Draws the player's body and leg skin.

### public static readonly PlayerLayer Legs

Draws the player's leg armor or pants and shoes.

### public static readonly PlayerLayer ShoeAcc

Draws the player's shoe accessory.

### public static readonly PlayerLayer Body

Draws the player's body armor or shirts.

### public static readonly PlayerLayer HandOffAcc

Draws the player's hand off accessory.

### public static readonly PlayerLayer WaistAcc

Draws the player's waist accessory.

### public static readonly PlayerLayer NeckAcc

Draws the player's neck accessory.

### public static readonly PlayerLayer Face

Draws the player's face and eyes.

### public static readonly PlayerLayer Hair

Draws the player's hair.

### public static readonly PlayerLayer Head

Draws the player's head armor.

### public static readonly PlayerLayer FaceAcc

Draws the player's face accessory.

### public static readonly PlayerLayer MountFront

Draws the front textures of the player's mount. Also draws the pulley if the player is hanging on a rope.

### public static readonly PlayerLayer ShieldAcc

Draws the player's shield accessory.

### public static readonly PlayerLayer SolarShield

Draws the player's solar shield if the player has one.

### public static readonly PlayerLayer HeldProjBack

Draws the player's held projectile if it should be drawn behind the held item and arms.

### public static readonly PlayerLayer HeldItem

Draws the player's held item.

### public static readonly PlayerLayer Arms

Draws the player's arms (including the armor's arms if applicable).

### public static readonly PlayerLayer HandOnAcc

Draws the player's hand on accessory. Also draws the player's held item if the player is in the middle of using a claw item.

### public static readonly PlayerLayer HeldProjFront

Draws the player's held projectile if it should be drawn in front of the held item and arms.

### public static readonly PlayerLayer FrontAcc

Draws the player's front accessory.

### public static readonly PlayerLayer MiscEffectsFront

Draws miscellaneous effects in front of the player.

# public class PlayerHeadLayer : DrawLayer<PlayerHeadDrawInfo>

This class represents a DrawLayer for the player's map icon, and uses PlayerDrawHeadInfo as its InfoType. Drawing should be done directly through drawInfo.spriteBatch.

## Constructors

### public PlayerHeadLayer(string mod, string name, Action<PlayerHeadDrawInfo> layer)

Creates a PlayerHeadLayer with the given mod name, identifier name, and drawing action.

### public PlayerHeadLayer(string mod, string name, PlayerHeadLayer parent, Action<PlayerHeadDrawInfo> layer)

Creates a PlayerHeadLayer with the given mod name, identifier name, parent layer, and drawing action.

## Fields

All vanilla PlayerHeadLayers have the same Name as their field name.

### public static readonly PlayerHeadLayer Head

Draws the player's face and eyes.

### public static readonly PlayerHeadLayer Hair

Draws the player's hair.

### public static readonly PlayerHeadLayer AltHair

Draws the player's alternate (hat) hair.

### public static readonly PlayerHeadLayer Armor

Draws the player's head armor.

### public static readonly PlayerHeadLayer FaceAcc

Draws the player's face accessory.