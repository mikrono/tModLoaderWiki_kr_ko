The following is a list of Open Source Mods (not all are technically "Open Source" in the traditional sense, by the way.). These mods' sources are available for a variety of reasons, mostly to facilitate and encourage community contributions, but I have listed them here for two reasons. 

1. The first reason is that modding is difficult, and learning how to do everything by oneself can prove hard. Joining an existing mod is a great way to quickly see your ideas come to life without having to know everything about Terraria modding. If you are interested in contributing to one of these mods, feel free to fork the mod, make your changes, and then submit a pull request to the main branch. Once you have gained the trust of the mod owner, you may be allowed into the team and given direct access to the mod. Please note that not all open source mods desire random contributions. Try to get a feel for what the mod needs, either by noticing bug reports on their Issue tracker or on their homepage. You can also look for `TODO` notes in the code or noted elsewhere for ideas of things to implement. If you are contributing art or sounds, it might be a good idea to communicate with the mod owner as well to get art direction before you waste your efforts on something that wasn't desired. If in doubt, contact the mod owner on the Terraria Community Forums.

2. The second reason I have listed these mods is because there are still a lot of things the tutorials and ExampleMod do not cover. Search these mods and look for code that might help you. Remember, just because a Mod is Open Source, that doesn't mean you can copy all the code and call it your own. Read the licence the code is licenced under for details. For the most part, however, you should be using Open Source mods as a guide for learning how to structure your mod. (You'll be shunned in the modding community if your mod is just a copy/paste conglomeration of other mods' code.)

**Remember, you can't just copy code from these mods unless the licence they are under specifically allows it. If there is no licence, the owner reserves all ownership and copyright of the code, so you need to contact the owner and ask permission to utilize certain parts. Don't worry, most will be happy to allow you to use portions of their code, but you still need to ask.**

## Content Mods
These mods are mods that add _a lot_ of common things such as items, enemies, and bosses
* [Example Mod](https://github.com/tModLoader/tModLoader/tree/master/ExampleMod) -- Offers a wide variety of code examples covering Beginner to Expert.
* [Cave Story](https://github.com/JavidPack/CaveStory) -- Very open to contributions. 
* [Bluemagic's Mod](https://github.com/blushiemagic/ElementalUnleash) -- Good resource for learning how to code Boss AI
* [Magic Storage](https://github.com/blushiemagic/MagicStorage) -- A lot of UI, inheritance, and networking.
* [All The Walls](https://github.com/JavidPack/AllTheWalls) -- A good example of NOT using Autoload. Learn from this mod how to programmatically add items to write clean code.
* [Summoners Association](https://github.com/JavidPack/SummonersAssociation) -- Projectile and Minion related code.
* [The Luggage](https://github.com/JavidPack/TheLuggage) -- A pet that collects loot for you.
* [WeaponOut](https://github.com/Flashkirby/WeaponOut) -- Complicated items, custom graphics, config file system with JSON.
* [Miscellania](https://github.com/goldenapple3/Miscellania) -- Small content mod with a bit of everything, open to contributors.
* [Tremor](https://github.com/Jofairden/Tremor) -- Please be advised that the code in Tremor is mostly very sub-optimal, though slowly being improved by Jofairden and collaborators. If you are looking for example, try to use Tremor as a last resort instead of a first
* [Enigma](https://github.com/Laugic/Laugicality) -- Adds a new level-up system, a few bosses, weapons, and accessories. 
* [Kalciphoz's RPG Mod](https://github.com/Kalciphoz/kRPG) -- This mod aims to overhaul Terraria by adding RPG elements, and includes a leveling system, an item upgrade system, an elemental damage system, procedurally generated weapons, and much more. For more detailed information, see the [wiki](http://krpgmod.wikidot.com/)
* [Dragon Ball Terraria](https://github.com/NuovaPrime/DBZMOD) -- Adds a custom energy system with some UI elements, a custom damage type, quite a few unique weapons and transformational effects.
* [Antiaris Mod](https://github.com/zadum4ivii/Antiaris) -- Adds several generated structures, quest system and other content, including bosses, enemies & items.
* [Qwerty's Bosses and Items](https://github.com/qwerty3-14/QwertysRandomContent) -- Adds a lot of interesting things. Has a lot uses of custom npc AIs (including bosses), custom minion/sentry AIs, player layers and more.
* [Starflower](https://github.com/AlurienFlame/Starflower) -- Adds an herb that spawns on sky islands, a planter box to go with it, and three potions crafted from it.


## Other Mods
These mods cover everything except mods that are primarily about new content. Some change mechanics, others make quality of life changes, others do random things. These mods may be a bit harder to contribute to due to their nature.
* [Even More Modifiers](https://github.com/Jofairden/EvenMoreModifiers) -- A mod to add custom effects to the game that can affect weapons, accessories and armors. Showcases hacking armor items to allow them to be reforged as accessories. Also showcases UI. Has a solid open-ended framework designed so other mods can add their own content to its system. Is fully documented and showcases lots of abstraction and design practices. Showcases MP compatibility. Showcases use of glowmasks and shaders, also showcases optional hard-references (support with other mods) Showcases various uses for Attributes.
* [Boss Checklist](https://github.com/JavidPack/BossChecklist) -- UI, weak mod references, and using Call for cross-mod communication
* [HEROs Mod](https://github.com/JavidPack/HEROsMod) -- A lot of networking code. (Do not consult for UI)
* [Modders Toolkit](https://github.com/JavidPack/ModdersToolkit) -- A lot of UI. Very useful in-game to visualize the effects of code as well. In-game c# interpreter also very useful.
* [AutoTrash](https://github.com/JavidPack/AutoTrash) -- UI, a slot that auto trashes items you pick up.
* [Shorter Respawn](https://github.com/JavidPack/ShorterRespawn) -- Integrating with other mods through Call. (Cheat Sheet and HEROs Mod)
* [Item Checklist](https://github.com/JavidPack/ItemChecklist) -- UI, TagCompound, saving Items in TagCompounds.
* [Item Customizer](https://github.com/gamrguy/ItemCustomizer) -- Messing with shaders
* [Shader Lib](https://github.com/gamrguy/ShaderLib) -- Messing with shaders
* [Dye Easy](https://github.com/goldenapple3/DyeEasy) -- Automatically editing recipes in complicated ways (without using RecipeEditor)
* [Vanilla Tweaks](https://github.com/goldenapple3/VanillaTweaks) -- Various tweaks using GlobalItem, GlobalNPC etc, recipe editing, options system with JSON.
* [Wireless](https://github.com/goldenapple3/Wireless) -- Wiring related code, storing custom tile information using ModWorld.
* [Eternal Mods](https://github.com/Eternal-Team) -- Tile Entities, UI and a bunch of framework stuff.
* [Craftable Lunar Tools](https://github.com/Jofairden/CraftableLunarTools) -- Makes normally unobtainable lunar tools craftable.
* [Bouncy Coins](https://github.com/Jofairden/BouncyCoins) -- Makes coins bounce (also showcases weak dependency support with FK's Mod Settings Configurator)
* [Fast Start](https://github.com/Jofairden/FastStart) -- Start the game with a bit more gear than usual
* [The Deconstructor](https://github.com/Jofairden/TheDeconstructor) -- Deconstruct _any_ item back into its recipe (showcases custom UI, sound effects and more)
* [hamstar's mods](https://github.com/hamstar0) -- A set of 11 open source mods adding significant original gameplay elements or minor content additions.

## Too Advanced/Specific
These mods are _probably_ too specific or advanced to for most modders.
* [Alternate Dimensions](https://github.com/JavidPack/AlternateDimensions) -- An incomplete implementation of alternate dimensions. Knowledgeable and determined modders are encouraged to discuss a suitable implementation.
* [WorldGenPreviewer](https://github.com/JavidPack/WorldGenPreviewer) -- Hijacking the WorldGen screen, Reflection
* [Emoji Support](https://github.com/JavidPack/EmojiSupport) -- Add a Chat Tag Handler
* [Boss Expertise](https://github.com/goldenapple3/BossExpertise) -- Various Expert Mode related "hacks", options system with JSON, integration with Cheat Sheet.

## Ambitious
*[CLib](https://github.com/aberna01/CLib) -- Rewriting the update methods for all entities, loads of static method tools that you can use to save some time typing. Tries to aid TMod the best it can.