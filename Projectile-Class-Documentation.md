# Projectile Class Documentation
This page lists methods and fields pertaining to the Projectile class. This page is useful for understanding what to use in `ModProjectile.SetDefaults` and `ModProjectile.AI`. Only common fields and methods are listed. Feel free to add to this list if you find a field that is missing.

Index|
-----|
[Fields](https://github.com/blushiemagic/tModLoader/wiki/Projectile-Class-Documentation#fields-and-properties)|
[Methods](https://github.com/blushiemagic/tModLoader/wiki/Projectile-Class-Documentation#methods)|

# Fields and Properties
You can assign these fields to give your ModProjectile various values. Typically you'll want to refer to this page when writing code for `ModProjectile.SetDefaults`. Be sure to visit [Vanilla Projectile Field Values](https://github.com/blushiemagic/tModLoader/wiki/Vanilla-Projectile-Field-Values) to see what values vanilla projectiles use for these fields. All fields listed are public unless otherwise noted. 

| Field    | Type | Default Value | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|----------|------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [type](#type)<a name="type"></a>|int |0 | This is the ProjectileID, automatically set.  |
| [aiStyle](#aistyle)<a name="aistyle"></a>| int | 0 | Selects which vanilla code to use for the AI method. Modders can use vanilla aiStyle to mimic AI code already in the game. See [ModProjectile.aiType](http://blushiemagic.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod_projectile.html#aa348d2c84755d8da68bb470cf35ab28d) and [ExampleCloneProjectile](https://github.com/blushiemagic/tModLoader/blob/5a29e2c6608a1cf804d5af7fa09ce4db3b7010d8/ExampleMod/Projectiles/ExampleCloneProjectile.cs) to see how to use vanilla ai. |
| [width](#width)<a name="width"></a>| int |0 | The width of the projectile's hitbox in pixels. |
| [height](#height)<a name="height"></a>|int  | 0| The height of the projectile's hitbox in pixels. |
| [friendly](#friendly)<a name="friendly"></a>| bool| false | If True, this projectile will hurt enemies (!npc.friendly) |
| [hostile](#hostile)<a name="hostile"></a>| bool|false  | If True, this projectile will hurt players and friendly npcs (npc.friendly) |
| [maxPenetrate](#maxpenetrate)<a name="maxpenetrate"></a>| int|1 | How many npc can it hit before dying. (Or tile bounces) Set in SetDefaults.  |
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
| [rotation](#)<a name=""></a>| | |  |
| [oldPos](#)<a name=""></a>| | |  |
| [oldRot](#)<a name=""></a>| | |  |
| [oldSpriteDirection](#)<a name=""></a>| | |  |
| [ai](#)<a name=""></a>| float[] | 0,0 |  |
| [localAI](#)<a name=""></a>| float[] | 0,0 |  |
| [noDropItem](#)<a name=""></a>| | |  |
| [minion](#)<a name=""></a>| | |  |
| [minionSlots](#)<a name=""></a>| | |  |
| [spriteDirection](#)<a name=""></a>| | |  |
| [hide](#)<a name=""></a>| | |  |
| [lavaWet](#)<a name=""></a>| | |  |
| [wetCount](#)<a name=""></a>| | |  |
| [wet](#)<a name=""></a>| | |  |
| [netUpdate](#)<a name=""></a>| | |  |
| [netUpdate2](#)<a name=""></a>| | |  |
| [numUpdates](#)<a name=""></a>| | |  |
| [identity](#)<a name=""></a>| | |  |
| [light](#)<a name=""></a>| | |  |
| [position](#)<a name=""></a>| | |  |
| [velocity](#)<a name=""></a>| | |  |
| [active](#)<a name=""></a>| | |  |
| [owner](#)<a name=""></a>| | |  |
| [damage](#damage)<a name="damage"></a>| | | This will always be set in NewProjectile based on the weapons damage. Don't assume that setting it to something in SetDefaults does anything. |
| [knockBack](#)<a name=""></a>| | |  |
| [trap](#)<a name=""></a>| | |  |
| [npcProj](#)<a name=""></a>| | |  |
| [projUUID](#)<a name=""></a>| | |  |
| [](#)<a name=""></a>| | |  |
| [](#)<a name=""></a>| | |  |

## tModLoader Only

| Field    | Type | Default Value | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|----------|------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [modProjectile](#modprojectile)<a name="modprojectile"></a>| ModProjectile | null | The ModProjectile instance that controls the behavior of this projectile. This property is null if this is not a modded projectile. |
| [globalProjectiles](#globalprojectiles)<a name="globalprojectiles"></a>| internal GlobalProjectile[] | new GlobalProjectile[0] | Do not touch. Use `Item.GetGlobalProjectile` |
| [](#)<a name=""></a>| | |  |

# Methods
Remember that static methods are called by writing the classname and non-static methods use the instance name. `Projectile.NewProjectile(...)` vs `projectile.Kill(...)`

### public static int NewProjectile(float X, float Y, float SpeedX, float SpeedY, int Type, int Damage, float KnockBack, int Owner = 255, float ai0 = 0f, float ai1 = 0f)
Spawns a projectile in the world. The owner variable should pretty much always be set to Main.myPlayer.

## tModLoader Only
### public GlobalProjectileGetGlobalProjectile(Mod mod, string name)

Gets the GlobalProjectileinstance (with the given name and added by the given mod) associated with this item instance.

### public T GetGlobalProjectile\<T\>(Mod mod) where T : GlobalProjectile

Same as the other GetGlobalProjectile, but assumes that the class name and internal name are the same.

### public T GetGlobalProjectile\<T\>() where T : GlobalProjectile

Same as the other GetGlobalProjectile, but assumes that the class name and internal name are the same, as well as source mod. Use this 99% of the time.

### public void CloneDefaults(int type)

Allows you to copy the defaults of a different type of projectile.