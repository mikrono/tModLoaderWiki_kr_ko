# What is an NPC

An NPC is a "non-player character", in Terraria these are enemies, critters, townspeople, and rarely projectiles disguised as NPCs that you can damage (imp fireball, dark caster's water sphere).

**####**

**This page is very incomplete; For other NPC related pages, visit the wiki search bar**

**####**

# Spawn projectiles

If you want your hostile NPC to spawn projectiles, first of all make sure the projectile you spawn is hostile and not friendly, so that it won't damage other hostile NPCs and so that it will damage players and friendly critters.
This code usually goes in the [AI](https://docs.tmodloader.net/html/class_terraria_1_1_mod_loader_1_1_mod_n_p_c.html#a942333fc831a20cfb1f77c1309259040) hook. It shows the most common scenario: shoot one projectile from the NPC directly towards the targeted player (usually the nearest):

```csharp
npc.TargetClosest();
if (npc.HasValidTarget && Main.netMode != NetmodeID.MultiplayerClient)
{
    Vector2 position = npc.Center;
    Vector2 targetPosition = Main.player[npc.target].Center;
    Vector2 direction = targetPosition - position;
    direction.Normalize();
    float speed = 10f;
    int type = ProjectileID.PinkLaser;
    int damage = npc.damage / 2;
    Projectile.NewProjectile(position, direction * speed, type, damage, 0f, Main.myPlayer);
}
```

You can emit the `npc.TargetClosest();` code if you already have it in your `AI`. You want to couple this usually with a [timer](https://github.com/tModLoader/tModLoader/wiki/Time-and-Timers), and if you want to spawn it some other way, [ExampleGun](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/Items/Weapons/ExampleGun.cs?ts=4) showcases a few variants you can transfer over (logically, don't copy its code into the NPC, it won't work). Visit the [NewProjectile](https://github.com/tModLoader/tModLoader/wiki/Projectile-Class-Documentation#public-static-int-newprojectilefloat-x-float-y-float-speedx-float-speedy-int-type-int-damage-float-knockback-int-owner--255-float-ai0--0f-float-ai1--0f-) documentation for more info.