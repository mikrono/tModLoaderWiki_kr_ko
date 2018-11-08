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
| [](#)<a name=""></a>| | |  |
| [](#)<a name=""></a>| | |  |
| [](#)<a name=""></a>| | |  |
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
### public ItemInfo GetModInfo(Mod mod, string name)

Gets the ItemInfo instance (with the given name and added by the given mod) associated with this item instance.

### public T GetModInfo\<T\>(Mod mod) where T : ItemInfo 

Same as the other GetModInfo, but assumes that the class name and internal name are the same.

### public void CloneDefaults(int type)

Allows you to copy the defaults of a different type of item.