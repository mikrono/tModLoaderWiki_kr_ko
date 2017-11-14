Autoload is a mechanism whereby tModLoader makes some assumptions about your mod in order to simplify the amount of work the modder has to do in their mod. In this Basic guide, we will focus on how Textures are autoloaded because that is the primary interest of beginner modders.

Items, Projectiles, and NPC all need a texture to work correctly. Any one of these without an associated texture will cause your mod to be unable to load. tModLoader can't magically know which texture file you want associated with which thing, but it can make an educated guess based on the pattern it knows.

This is the pattern:

Take the Mod Sources path: 
`C:\Documents\My Games\Terraria\ModLoader\Mod Sources`

Add the Namespace of the class, replacing '.' with '\\': 
`namespace ExampleMod.NPCs`
`C:\Documents\My Games\Terraria\ModLoader\Mod Sources\ExampleMod\NPCs`

Add the classname of the class, then append '.png':
`public class FlutterSlime : ModNPC`
`C:\Documents\My Games\Terraria\ModLoader\Mod Sources\ExampleMod\NPCs\FlutterSlime.png`

In more concise terms, look for the .png file with filename matching the classname of the thing in the folder designated by the namespace of the class.

Basically, just make sure the .cs file and .png file exist in the same directory and that the class in the .cs file matches the filename of the .png file. It is a good idea to also make sure the .cs file matches the classname and .png filename as well. Also, you must make sure that the folder path from the Mod Sources folder matches the namespace (with 
'.' becoming '\' in the folder structure.)