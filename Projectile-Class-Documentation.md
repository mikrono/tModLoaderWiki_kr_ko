# Projectile Class Documentation
This page lists methods and fields pertaining to the Projectile class. This page is useful for understanding what to use in `ModProjectile.SetDefaults` and `ModProjectile.AI`. Only common fields and methods are listed. Feel free to add to this list if you find a field that is missing.

Index|
-----|
[Fields](https://github.com/blushiemagic/tModLoader/wiki/Projectile-Class-Documentation#fields-and-properties)|
[Methods](https://github.com/blushiemagic/tModLoader/wiki/Projectile-Class-Documentation#methods)|

# Fields and Properties
You can assign these fields to give your ModProjectile various values. Typically you'll want to refer to this page when writing code for `ModProjectile.SetDefaults`. Be sure to visit [Vanilla Projectile Field Values](https://github.com/blushiemagic/tModLoader/wiki/Vanilla-Projectile-Field-Values) to see what values vanilla projectiles use for these fields. All fields listed are public unless otherwise noted. 

## Vanilla
| Field    | Type | Default Value | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|----------|------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [type](#type)<a name="type"></a>|int |0 | This is the ProjectileID, automatically set.  |
| [aiStyle](#aistyle)<a name="aistyle"></a>| int | 0 | Selects which vanilla code to use for the AI method. Modders can use vanilla aiStyle to mimic AI code already in the game. See [ModProjectile.aiType](http://blushiemagic.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod_projectile.html#aa348d2c84755d8da68bb470cf35ab28d) and [ExampleCloneProjectile](https://github.com/blushiemagic/tModLoader/blob/5a29e2c6608a1cf804d5af7fa09ce4db3b7010d8/ExampleMod/Projectiles/ExampleCloneProjectile.cs) to see how to use vanilla ai. |
| [width](#width)<a name="width"></a>| int |0 | The width of the projectile's hitbox in pixels. |
| [height](#height)<a name="height"></a>|int  | 0| The height of the projectile's hitbox in pixels. |
| [friendly](#friendly)<a name="friendly"></a>| bool| false | If True, this projectile will hurt enemies (!npc.friendly) |
| [hostile](#hostile)<a name="hostile"></a>| bool|false  | If True, this projectile will hurt players and friendly npcs (npc.friendly) |
| [maxPenetrate](#maxpenetrate)<a name="maxpenetrate"></a>| int|1 | How many npc can it hit before dying. (Or tile bounces) Set at the end of SetDefaults automatically to the current value of penetrate.  |
| [penetrate](#penetrate)<a name="penetrate"></a>| int|1 | Current penetrate value. |
| [alpha](#alpha)<a name="alpha"></a>| int| 0 | How transparent to draw this projectile. 0 to 255. 255 is completely transparent. [ExampleBullet](https://github.com/blushiemagic/tModLoader/blob/master/ExampleMod/Projectiles/ExampleBullet.cs) sets this to 255, and the projectile aiStyle of 1 automatically decreases alpha each tick, letting the projectile fade in quickly after being spawned. Useful for projectiles that look odd when initially spawned on the weapon because of texture overlap. |
| [timeLeft](#timeleft)<a name="timeleft"></a>|int | 3600|  |
| [tileCollide](#tilecollide)<a name="tilecollide"></a>|bool |true | The projectile will collide with tiles, usually bouncing or killing the tile depending on `ModProjectile.OnTileCollide`. [ExampleBullet](https://github.com/blushiemagic/tModLoader/blob/master/ExampleMod/Projectiles/ExampleBullet.cs#L36) shows how to bounce. |
| [ignoreWater](#ignorewater)<a name="ignorewater"></a>| bool | false | The projectile will not be affected by water. |
| [extraUpdates](#extraupdates)<a name="extraupdates"></a>|int |0 | Additional update steps per tick. Useful for really fast projectiles such as [Shadowbeam Staff](https://github.com/blushiemagic/tModLoader/wiki/Advanced-Vanilla-Code-Adaption#example-item-and-projectile-shadowbeam-staff-clone). |
| [scale](#scale)<a name="scale"></a>| float | 1f |  |
| [melee](#melee)<a name="melee"><br>[ranged](#ranged)<a name="ranged"><br>[magic](#magic)<a name="magic"><br>[minion](#minion)<a name="minion"><br>[thrown](#thrown)<a name="thrown"><br></a>| bool | false | Determines which crit chance will influence the damage of this projectile |
| [frame](#)<a name=""></a>| | |  |
| [frameCounter](#)<a name=""></a>| | |  |
| [rotation](#)<a name=""></a>| | | Rotation of the projectile. Radians not Degrees. Use MathHelper if you want to convert degrees to radians. 0 is facing right, pi/2 is facing down, and so on. |
| [oldPos](#)<a name=""></a>| | |  |
| [oldRot](#)<a name=""></a>| | |  |
| [oldSpriteDirection](#)<a name=""></a>| | |  |
| [ai](#)<a name=""></a>| float[] | 0,0 |  |
| [localAI](#)<a name=""></a>| float[] | 0,0 |  |
| [noDropItem](#nodropitem)<a name="nodropitem"></a>| bool | false | Set to true if you don't want this item to have a chance to recover the ammo item that shot this. For example, if you shoot the wooden arrow projectile, it will sometimes drop the wooded arrow item. If your weapon shoots multiple arrows for 1 ammo, you might want to consider setting this field to prevent infinite ammo glitches. |
| [minion](#)<a name=""></a>| | |  |
| [minionSlots](#)<a name=""></a>| | |  |
| [spriteDirection](#)<a name=""></a>| | |  |
| [hide](#hide)<a name="hide"></a>| bool | | Projectile is not drawn normally. See [ExampleBehindTilesProjectile](https://github.com/blushiemagic/tModLoader/blob/master/ExampleMod/Projectiles/ExampleBehindTilesProjectile.cs#L30) |
| [lavaWet](#)<a name=""></a>| | |  |
| [wetCount](#)<a name=""></a>| | |  |
| [wet](#)<a name=""></a>| | |  |
| [netUpdate](#)<a name=""></a>| | |  |
| [netUpdate2](#)<a name=""></a>| | |  |
| [numUpdates](#)<a name=""></a>| | |  |
| [identity](#)<a name=""></a>| int | | The projectile's universal unique identifier, which is the same on all clients and the server. Usually used to find the same projectile on multiple clients and/or the server, e.g. Projectile match = Main.projectile.FirstOrDefault(x => x.identity == identity); |
| [light](#)<a name=""></a>| | |  |
| [position](#position)<a name="position"></a>| Vector2 | | |
| [velocity](#velocity)<a name="velocity"></a>| Vector2 | | |
| [active](#active)<a name="active"></a>| bool | | True if this Projectile actually exists. `Main.projectile` will hold a lot of junk data in it. If you are iterating over `Main.projectile`, be sure to check `active` to make sure the projectile is still alive. For example, old projectiles that die aren't removed from the array, they simply have active set to false. |
| [owner](#owner)<a name="owner"></a>| | | The index of the player who owns this projectile. In Multiplayer, Clients "own" projectiles that they shoot, while the Server "owns" projectiles spawned by NPCs and the World. It is very important to check `if (projectile.owner == Main.myPlayer)` for things like dropping items or spawning projectiles in `ModProjectile.AI` and some other methods because `AI` runs simultaneously on all Clients and the Server. This check gates some of the code that should only run on the owners computer. [ExampleJavelinProjectile](https://github.com/blushiemagic/tModLoader/blob/master/ExampleMod/Projectiles/ExampleJavelinProjectile.cs#L88) checks owner for spawning the recovered ammo item. If you don't do this, you will run into desync bugs in your mod. |
| [damage](#damage)<a name="damage"></a>| | | This will always be set in NewProjectile based on the weapons damage. Don't assume that setting it to something in SetDefaults does anything. |
| [knockBack](#)<a name=""></a>| | |  |
| [trap](#)<a name=""></a>| | | If true, this projectile was spawned by a trap tile. |
| [npcProj](#)<a name=""></a>| | | If true, this projectile was spawned by a friendly Town NPC. |
| [projUUID](#)<a name=""></a>| | |  |
| [](#)<a name=""></a>| | |  |
| [](#)<a name=""></a>| | |  |

## tModLoader

| Field    | Type | Default Value | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|----------|------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [modProjectile](#modprojectile)<a name="modprojectile"></a>| ModProjectile | null | The ModProjectile instance that controls the behavior of this projectile. This property is null if this is not a modded projectile. |
| [globalProjectiles](#globalprojectiles)<a name="globalprojectiles"></a>| internal GlobalProjectile[] | new GlobalProjectile[0] | Do not touch. Use `Item.GetGlobalProjectile` |
| [](#)<a name=""></a>| | |  |

# Methods

## Vanilla
Remember that static methods are called by writing the classname and non-static methods use the instance name. `Projectile.NewProjectile(...)` vs `projectile.Kill(...)`

### public static int NewProjectile(float X, float Y, float SpeedX, float SpeedY, int Type, int Damage, float KnockBack, int Owner = 255, float ai0 = 0f, float ai1 = 0f)
Spawns a projectile in the world. The owner variable should pretty much always be set to Main.myPlayer.

### public static int GetByUUID(int owner, int uuid)
Do not use this method to get a projectile on a client based on its projectile.identity. Instead, loop through the Main.projectile[] on the client you wish to find the projectile on and check if there is a projectile that has the same projectile.identity as the projectile you want to find.

## tModLoader
### public GlobalProjectile GetGlobalProjectile(Mod mod, string name)

Gets the GlobalProjectileinstance (with the given name and added by the given mod) associated with this item instance.

### public T GetGlobalProjectile\<T\>(Mod mod) where T : GlobalProjectile

Same as the other GetGlobalProjectile, but assumes that the class name and internal name are the same.

### public T GetGlobalProjectile\<T\>() where T : GlobalProjectile

Same as the other GetGlobalProjectile, but assumes that the class name and internal name are the same, as well as source mod. Use this 99% of the time.

### public void CloneDefaults(int type)

Allows you to copy the defaults of a different type of projectile.