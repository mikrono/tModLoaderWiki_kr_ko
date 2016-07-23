# ModCactus

This class represents a type of modded cactus. The cactus will share a tile ID with the vanilla cacti (80), so that the cacti can freely convert between each other if the sand below is converted. This class encapsulates a function for retrieving the cactus's texture, the only difference between each type of cactus. Use ModTile.SetModCactus or GlobalTile.AddModCactus to make a tile able to grow this kind of cactus.

## Methods

### public abstract Texture2D GetTexture()

Return the texture that represents the tile sheet used for drawing this cactus.