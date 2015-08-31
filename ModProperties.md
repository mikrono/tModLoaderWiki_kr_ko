# ModProperties

This is a struct that stores the properties of a mod.

## Fields

### public bool Autoload

Whether or not this mod will autoload content by default. Autoloading content means you do not need to manually add content through methods.

### public bool AutoloadGores

Whether or not this mod will automatically add images in the Gores folder as gores to the game, along with any ModGore classes that share names with the images. This means you do not need to manually call Mod.AddGore.

### public bool AutoloadSounds

Whether or not this mod will automatically add sounds in the Sounds folder to the game. Place sounds in Sounds/Item to autoload them as item sounds, Sounds/NPCHit to add them as npcHit sounds, and Sounds/NPCKilled to add them as npcKilled sounds. Sounds placed anywhere else will be added as custom sounds. Any ModSound classes that share the same name as the sound files will be bound to them. Setting this field to true means that you do not need to manually call AddSound.