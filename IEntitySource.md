If you are making a 1.4 tModLoader mod, you will need to use `IEntitySource`. This guide will go over the purpose of `IEntitySource` and the most frequent uses of it.

# Purpose of IEntitySource
To further facilitate interesting modding capabilities, many 1.4 tModLoader methods require the use of a `IEntitySource` object. This object encapsulates context information about the source of a particular spawning event of an Item/Projectile/NPC/etc. For example, when a Boss NPC spawns minions, those minions are spawned with the `IEntitySource` value resulting from the helper method `NPC.GetSource_FromAI()`. The result of that helper method is a `EntitySource_Parent` class with an `Entity` field of type `NPC`. This `IEntitySource` could then be detected by a mod that intended to reduce the HP of all boss minions by half. This additional info can help facilitate many more effects that were previously impossible to accomplish with mods.

# Lifetime
Do not store `IEntitySource` in fields. The validity of the information they encapsulate is only relevant at the moment of spawning. Any lasting effects of the source information will need to be registered in fields within your mod at the time of spawning. The details on why it is a bad idea are too advanced for this guide.

# Methods requiring IEntitySource
The following commonly used methods require `IEntitySource`.
* Gore.NewGore
* Gore.NewGoreDirect
* Gore.NewGorePerfect
* Player.QuickSpawnItem
* Player.QuickSpawnClonedItem
* Item.NewItem
* NPC.NewNPC
* Projectile.NewProjectile
* Projectile.NewProjectileDirect

# Common Usage
The most common usages of `IEntitySource` will be listed here. If you absolutely can't figure out a suitable `IEntitySource` for your situation, passing in `null` is acceptable, but be aware that the purpose of `IEntitySource` is to facilitate advanced modding capabilities and your users might be disappointed that their other mods do not work 100% correctly because you didn't use the correct `IEntitySource`.

* NPC spawning projectiles should use `NPC.GetSource_FromAI()`
* NPC spawning item drops should use `NPC.GetSource_Loot()` (if loot) or `NPC.GetSource_DropAsItem()` (if anything else). (Note that 99.9% of NPC item drops should be using the new loot system)
* NPC spawning other NPC, such as boss minions, should use `NPC.GetSource_FromAI()`
* NPC spawning gore on death should use `NPC.GetSource_Death()`
* Projectiles spawning items, such as arrow recovery drops, should use `Projectile.GetSource_DropAsItem()`
* Projectiles spawning other projectiles, like a splitting projectile or a shooting minion, should use `Projectile.GetSource_FromThis()`
* Held projectile weapons spawning other projectiles using ammo should use `player.GetSource_ItemUse_WithPotentialAmmo(player.HeldItem, usedAmmoItemId)`
* Spawning minions or pets in `ModBuff.Update`: use `player.GetSource_Buff(buffIndex)`
* An accessory spawning a projectile: use `player.GetSource_Accessory(itemInstance)`
* An armor set bonus spawning a projectile: Not properly definable for modded ones, use `player.GetSource_Misc("SetBonus_MySetName")`
* Player spawning Projectiles in ModItem.Shoot should use the `source` passed into the method
* Player.QuickSpawnItem usage for a bag type item should use `player.GetSource_OpenItem(itemtype)`
* Tile dropping an item, such as in ModTile.KillMultiTile or ModTile.Drop, should use `new EntitySource_TileBreak(i, j)`
* Player spawning an item due to dropping or being unable to recover an item from a UISlot (player.GetItem overflow) should use `player.GetSource_Misc("PlayerDropItemCheck")`

# Retrieving Information from IEntitySource
Using the information provided by `IEntitySource` is the other half of the purpose of this feature. There are various methods available that provide the source to the modder, all called `OnSpawn`.

For example, in `GlobalProjectile.OnSpawn`, there is a `IEntitySource source` parameter. We can use casting to cast the `IEntitySource` to the source that we wish to check. This code uses the `is` operator and the [property pattern](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/patterns#property-pattern) to quickly match a specific set of conditions.
```cs
// Property pattern approach
if(source is EntitySource_Parent { Entity: NPC { type: NPCID.Paladin } } && projectile.type == ProjectileID.PaladinsHammerHostile) {
	// Do things here to PaladinsHammerHostile spawned by the Paladin enemy
}
// Non property pattern approach
if(source is EntitySource_Parent parent && parent.Entity is NPC npc && npc.type == NPCID.Paladin && projectile.type == ProjectileID.PaladinsHammerHostile) {
	// Do things here to PaladinsHammerHostile spawned by the Paladin enemy
}
```

Note that for most things, you'll have to manually sync changes. OnSpawn happens on the client or server that spawns the entity, any changes that should reflect on other clients need to by synced in some manner. The files in [ExampleMod/Common/EntitySources](https://github.com/tModLoader/tModLoader/tree/1.4/ExampleMod/Common/EntitySources) show examples of this.
