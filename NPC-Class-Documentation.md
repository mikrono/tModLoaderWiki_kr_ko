# NPC Class Documentation
This page lists methods and fields pertaining to the NPC class. This page is useful for understanding what to use in `ModNPC.SetDefaults` and `ModNPC.AI`.

Index|
-----|
[Fields](https://github.com/blushiemagic/tModLoader/wiki/NPC-Class-Documentation#fields-and-properties)|
[Methods](https://github.com/blushiemagic/tModLoader/wiki/NPC-Class-Documentation#methods)|

# Fields and Properties
You can assign these fields to give your ModNPC various values. Typically you'll want to refer to this page when writing code for `ModNPC.SetDefaults` and `ModNPC.AI`. Be sure to visit [Vanilla NPC Field Values](https://github.com/blushiemagic/tModLoader/wiki/Vanilla-NPC-Field-Values) to see what values vanilla npc use for these fields. All fields listed are public unless otherwise noted.

| Field    | Type | Default Value | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|----------|------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [width](#width)<a name="width"></a>| int | 0 | The width of the npc hitbox |
| [height](#height)<a name="height"></a>| int | 0 | The height of the npc hitbox |
| [aiStyle](#aistyle)<a name="aistyle"></a>| int | 0 | Determines which AI code to run. Many NPC share the same AI code by having the same aiStyle. ModNPC can utilize vanilla aiStyle values in addition to ModNPC.aiType to mimic vanilla behavior to some degree. If you wish to further customize the AI of a ModNPC, you should read [Advanced Vanilla Code Adaption](https://github.com/blushiemagic/tModLoader/wiki/Advanced-Vanilla-Code-Adaption). |
| [damage](#damage)<a name="damage"></a>| int | 0 | The amount of damage this NPC will deal on collision. Usually npc.damage is scaled by some factor when NPC spawn projectiles with Projectile.NewProjectile in AI code. For example, passing in `(int)(npc.damage * 0.5f)` as the Damage parameter. |
| [defDamage](#defdamage)<a name="defdamage"></a>| int | 0 | Stores the value of `damage` at the end of SetDefaults. Useful for scaling damage in AI code. |
| [defense](#defense)<a name="defense"></a>| int | 0 | How resistant to damge this NPC is. |
| [defDefense](#defdefense)<a name="defdefense"></a>| int | 0 | Stores the value of `defense` at the end of SetDefaults. Useful for scaling defense in AI code, like how King Slime changes defense as it gets smaller. |
| [lifeMax](#lifemax)<a name="lifemax"></a>| int | 0 | The maximum life of this NPC. |
| [life](#life)<a name="life"></a>| int | 0 | The current life of the NPC. Automatically assigned to lifeMax at the end of SetDefaults. |
| [realLife](#reallife)<a name="reallife"></a>| int | -1 |  |
| [HitSound](#hitsound)<a name="hitsound"></a>| LegacySoundStyle | null |  |
| [DeathSound](#deathsound)<a name="deathsound"></a>| LegacySoundStyle | null |  |
| [alpha](#alpha)<a name="alpha"></a>| int | 0 |  |
| [color](#color)<a name="color"></a>| Color |  |  |
| [value](#value)<a name="value"></a>| float | 0f |  |
| [buffImmune](#buffimmune)<a name="buffimmune"></a>| bool[] |  |  |
| [knockBackResist](#knockbackresist)<a name="knockbackresist"></a>| float | 1f |  |
| [scale](#scale)<a name="scale"></a>| float | 1f |  |
| [townNPC](#townnpc)<a name="townnpc"></a>| bool | false |  |
| [noGravity](#nogravity)<a name="nogravity"></a>| bool | false |  |
| [noTileCollide](#notilecollide)<a name="notilecollide"></a>| bool |  |  |
| [npcSlots](#npcslots)<a name="npcslots"></a>| float |  |  |
| [boss](#boss)<a name="boss"></a>| bool |  |  |
| [netAlways](#netalways)<a name="netalways"></a>| bool |  |  |
| [](#)<a name=""></a>|  |  |  |

## Static Fields

Static fields are accessed by the classname, not the instance. For example, we write `NPC.downedPlantBoss` to check if Plantera has been defeated yet.

| Field    | Type | Default Value | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|----------|------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [killCount](#killcount)<a name="killcount"></a>| int[] | 0 | Indexed by BannerID. Returns how many kills of the specified BannerID have been killed. Many NPC can share the same BannerID and many NPC actually don't have a Banner ID at all. This code will retrieve the bannerID of an NPC, or -1 if it does not exist: `int bannerID = Item.NPCtoBanner(npc.BannerID());` |
| [savedTaxCollector](#savedtaxcollector)<a name="savedtaxcollector"></a><br>[savedGoblin](#savedgoblin)<a name="savedgoblin"></a><br>[savedWizard](#savedwizard)<a name="savedwizard"></a><br>[savedMech](#savedmech)<a name="savedmech"></a><br>[savedAngler](#savedangler)<a name="savedangler"></a><br>[savedStylist](#savedstylist)<a name="savedstylist"></a><br>[savedBartender](#savedbartender)<a name="savedbartender"></a>| bool | false | These values indicate which NPC have been found in the wild. You can use these in various conditions in your mod. |
| [downedBoss1](#downedboss1)<a name="downedboss1"></a><br>[downedBoss2](#downedboss2)<a name="downedboss2"></a><br>[downedBoss3](#downedboss3)<a name="downedboss3"></a><br>[downedQueenBee](#downedqueenbee)<a name="downedqueenbee"></a><br>[downedSlimeKing](#downedslimeking)<a name="downedslimeking"></a><br>[downedGoblins](#downedgoblins)<a name="downedgoblins"></a><br>[downedFrost](#downedfrost)<a name="downedfrost"></a><br>[downedPirates](#downedpirates)<a name="downedpirates"></a><br>[downedClown](#downedclown)<a name="downedclown"></a><br>[downedPlantBoss](#downedplantboss)<a name="downedplantboss"></a><br>[downedGolemBoss](#downedgolemboss)<a name="downedgolemboss"></a><br>[downedMartians](#downedmartians)<a name="downedmartians"></a><br>[downedFishron](#downedfishron)<a name="downedfishron"></a><br>[downedHalloweenTree](#downedhalloweentree)<a name="downedhalloweentree"></a><br>[downedHalloweenKing](#downedhalloweenking)<a name="downedhalloweenking"></a><br>[downedChristmasIceQueen](#downedchristmasicequeen)<a name="downedchristmasicequeen"></a><br>[downedChristmasTree](#downedchristmastree)<a name="downedchristmastree"></a><br>[downedChristmasSantank](#downedchristmassantank)<a name="downedchristmassantank"></a><br>[downedAncientCultist](#downedancientcultist)<a name="downedancientcultist"></a><br>[downedMoonlord](#downedmoonlord)<a name="downedmoonlord"></a><br>[downedTowerSolar](#downedtowersolar)<a name="downedtowersolar"></a><br>[downedTowerVortex](#downedtowervortex)<a name="downedtowervortex"></a><br>[downedTowerNebula](#downedtowernebula)<a name="downedtowernebula"></a><br>[downedTowerStardust](#downedtowerstardust)<a name="downedtowerstardust"></a><br>[downedMechBossAny](#downedmechbossany)<a name="downedmechbossany"></a><br>[downedMechBoss1](#downedmechboss1)<a name="downedmechboss1"></a><br>[downedMechBoss2](#downedmechboss2)<a name="downedmechboss2"></a><br>[downedMechBoss3](#downedmechboss3)<a name="downedmechboss3"></a><br>| sbyte| -1 | These values indicate which bosses have been defeated in the world. Most are self explanitory. `downedBoss1` refers to Eye of Cthulu, `downedBoss2` refers to either Brain of Cthulhu or Eater of Worlds, and `downedBoss3` referes to Skeletron. `downedMechBoss` 1, 2, and 3 refer to The Destroyer, The Twins, and Skeletron Prime respectively. |

## tModLoader Only

| Field    | Type | Default Value | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|----------|------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [modNPC](#modnpc)<a name="modnpc"></a>| ModNPC | null | The ModNPC instance that controls the behavior of this NPC. This property is null if this is not a modded NPC. |
| [globalNPCs](#globalnpcs)<a name="globalnpcs"></a>| internal GlobalNPC[] | new GlobalNPC[0] | Do not touch. Use `NPC.GetGlobalNPC` |
| [](#)<a name=""></a>| | |  |

# Methods
Remember that static methods are called by writing the classname and non-static methods use the instance name. `NPC.NewNPC(...)` vs `npc.CloneDefaults(...)`
### public static int NewNPC(int X, int Y, int Type, int Start = 0, float ai0 = 0f, float ai1 = 0f, float ai2 = 0f, float ai3 = 0f, int Target = 255)
Spawns an NPC in the world. Use this to spawn minions from bosses. 
Example: `NPC.NewNPC((int)npc.position.X, (int)npc.position.Y, mod.NPCType("ExampleBossMinion");` The `ai` parameters can initialize an NPC with particular data if needed for special AI behavior.

### public static bool AnyNPCs(int Type)
Returns true if there are any NPC of the supplied type alive in the world. Useful for boss spawning items or anytime you need to check if an NPC is alive. 
Example: `if (NPC.AnyNPCs(mod.NPCType("CaptiveElement2"))) {`

### public static int CountNPCS(int Type)
Same as `AnyNPCs` except returns the number of active NPC of the supplied type.

### public static int FindFirstNPC(int Type)
Returns the index of the first NPC of the supplied type. Returns -1 if not found. Use to index into `Main.npc` to get the NPC instance.

### public static void SpawnOnPlayer(int plr, int Type)
Spawns a boss somewhere off screen of the specified player. This is the most common way of spawning bosses.

## tModLoader Only
### public GlobalNPC GetGlobalNPC(Mod mod, string name)

Gets the GlobalNPC instance (with the given name and added by the given mod) associated with this npc instance.

### public T GetGlobalNPC\<T\>(Mod mod) where T : GlobalNPC

Same as the other GetGlobalNPC, but assumes that the class name and internal name are the same.

### public T GetGlobalNPC\<T\>() where T : GlobalNPC

Same as the other GetGlobalNPC, but assumes that the class name and internal name are the same, as well as the Mod. This is the one you should use 99% of the time.

### public void CloneDefaults(int type)

Allows you to copy the defaults of a different type of NPC.