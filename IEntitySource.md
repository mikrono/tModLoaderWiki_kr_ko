If you are making a 1.4 tModLoader mod, you will need to use `IEntitySource`. This guide will go over the purpose of `IEntitySource` and the most frequent uses of it.

# Purpose of IEntitySource
To further facilitate interesting modding capabilities, many 1.4 tModLoader methods require the use of a `IEntitySource` object. This object encapsulates context information about the source of a particular spawning event of an Item/Projectile/NPC/etc. For example, when a Boss NPC spawns minions, those minions are spawned with the `IEntitySource` value resulting from the helper method `NPC.GetSpawnSourceForNPCFromNPCAI()`. The result of that helper method is a `EntitySource_Parent` class with an `Entity` field of type `NPC`. This `IEntitySource` could then be detected by a mod that intended to reduce the HP of all boss minions by half. This additional info can help facilitate many more effects that were previously impossible to accomplish with mods.

# Lifetime
Do not store `IEntitySource` in fields. The validity of the information they encapsulate is only relevant at the moment of spawning. Any lasting effects of the source information will need to be registered in fields within your mod at the time of spawning. The details on why it is a bad idea are too advanced for this guide.

# Methods requiring IEntitySource
The following commonly used methods require `IEntitySource`.
* Player.QuickSpawnItem
* Item.NewItem
* NPC.NewNPC
* Projectile.NewProjectile

# Common Usage
The most common usages of `IEntitySource` will be listed here. If you absolutely can't figure out a suitable `IEntitySource` for your situation, passing in `null` is acceptable, but be aware that the purpose of `IEntitySource` is to facilitate advanced modding capabilities and your users might be disappointed that their other mods do not work 100% correctly because you didn't use the correct `IEntitySource`.

* NPC spawning projectiles should use `NPC.GetSpawnSource_ForProjectile()`
* NPC spawning item drops should use `NPC.GetItemSource_Loot()`. (Note that 99.9% of NPC item drops should be using the new loot system)
  * 2022.4+ use `NPC.GetSource_Loot`
* NPC spawning other NPC, such as boss minions, should use `NPC.GetSpawnSourceForNPCFromNPCAI()`
  * 2022.4+ use `NPC.GetSource_FromAI`
* Projectiles spawning items, such as arrow recovery drops, should use `Projectile.GetItemSource_DropAsItem()`
* Projectiles spawning other projectiles, like a splitting projectile or a shooting minion, should use `Projectile.GetProjectileSource_FromThis()`
* Held projectile weapons spawning other projectiles using ammo should use `player.GetProjectileSource_Item_WithPotentialAmmo(player.HeldItem, usedAmmoItemId)`
* Spawning minions or pets in `ModBuff.Update`: use `player.GetProjectileSource_Buff(buffIndex)`
* An accessory spawning a projectile: use `Player.GetProjectileSource_Accessory(itemInstance)`
* Player spawning Projectiles in ModItem.Shoot should use the `source` passed into the method
* Player.QuickSpawnItem usage for a bag type item should use `player.GetItemSource_OpenItem(Type)`
* Tile dropping an item, such as in ModTile.KillMultiTile or ModTile.Drop, should use `new EntitySource_TileBreak(i, j)`
* Player spawning an item due to dropping or being unable to recover an item from a UISlot (player.GetItem overflow) should use `player.GetItemSource_Misc(ItemSourceID.PlayerDropItemCheck)` 
  * 2022.4+ use `player.GetSource_Misc("PlayerDropItemCheck")`
