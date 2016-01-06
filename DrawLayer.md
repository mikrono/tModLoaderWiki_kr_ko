# public abstract class DrawLayer<InfoType>

This class represents a layer of the drawing of an object, using a certain type of InfoType to help with its drawing.

## Fields

### public readonly string mod

The name of the mod to which this DrawLayer belongs.

### public readonly string Name

The name which identifies this DrawLayer.

### public readonly DrawLayer<InfoType> parent

The parent of this DrawLayer. If the parent is not drawn, this layer will not be drawn either. Defaults to null, which skips the parent check.

### public readonly Action<InfoType> layer

The delegate of this method, which can either do the actual drawing or add draw data, depending on what kind of layer this is.

### public bool visible

Whether or not this DrawLayer should be drawn. For vanilla layers, this will be set to true before all drawing-related hooks are called. For modded layers, you must set this to true or false yourself.

## Constructors

### public DrawLayer(string mod, string name, Action<InfoType> layer)

Creates a DrawLayer with the given mod name, identifier name, and drawing action.

### public DrawLayer(string mod, string name, DrawLayer<InfoType> parent, Action<InfoType> layer)

Creates a DrawLayer with the given mod name, identifier name, parent layer, and drawing action.

## Methods

### public bool ShouldDraw(object layerList)

Whether or not this layer should be drawn. Returns false if visible is false. If layerList is of type List<DrawLayer<InfoType>> and parent is not null, this will also return false if the parent is not drawn.

### public void Draw(InfoType drawInfo)

Invokes this DrawLayer's layer action.

# public class PlayerLayer : DrawLayer<PlayerDrawInfo>

This class represents a DrawLayer for the player, and uses PlayerDrawInfo as its InfoType. Drawing should be done by adding Terraria.DataStructures.DrawData objects to Main.playerDrawData.

## Constructors

### public PlayerLayer(string mod, string name, Action<PlayerDrawInfo> layer)

Creates a PlayerLayer with the given mod name, identifier name, and drawing action.

### public PlayerLayer(string mod, string name, PlayerLayer parent, Action<PlayerDrawInfo> layer)

Creates a PlayerLayer with the given mod name, identifier name, parent layer, and drawing action.

# public class PlayerHeadLayer : DrawLayer<PlayerHeadDrawInfo>

This class represents a DrawLayer for the player's map icon, and uses PlayerDrawHeadInfo as its InfoType. Drawing should be done directly through drawInfo.spriteBatch.

## Constructors

### public PlayerHeadLayer(string mod, string name, Action<PlayerHeadDrawInfo> layer)

Creates a PlayerHeadLayer with the given mod name, identifier name, and drawing action.

### public PlayerHeadLayer(string mod, string name, PlayerHeadLayer parent, Action<PlayerHeadDrawInfo> layer)

Creates a PlayerHeadLayer with the given mod name, identifier name, parent layer, and drawing action.