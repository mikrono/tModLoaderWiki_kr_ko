# What is Ammo?
Ammo is a system that links Weapon items, ammo items, and projectiles together. The basic idea is this: Weapon items have `item.useAmmo` set to an AmmoID, Ammo items that that weapon should use have `item.ammo` set to that same AmmoID, and that Ammo item has `item.shoot` set to the particular projectile that will be shot by the weapon when the ammo is used.

As an example, let's consider `Wooden Bow` and `Flaming Arrow`. The `Wooden Bow` has `item.useAmmo = AmmoID.Arrow;` and `Flaming Arrow` has `item.shoot = ProjectileID.FireArrow;` and `item.ammo = AmmoID.Arrow;`. When the player is shooting their `Wooden Bow`, Terraria looks in the player's inventory and searches for items with `item.ammo` matching `Wooden Bow`'s `item.useAmmo`. If an ammo item is found, the ammo item's `item.shoot` projectile is spawned and the ammo is consumed.

As a convention, AmmoIDs match the ItemID of the first ammo item. For example, AmmoID.Arrow and ItemID.WoodenArrow are both equal to 40. All other arrows have `item.ammo = AmmoID.Arrow;`.

# How do I use vanilla ammo for my weapon?
To use vanilla ammo, simply set item.useAmmo to the correct AmmoID. For example, `Example Gun` in ExampleMod has `item.useAmmo = AmmoID.Bullet;`.

# How do I make a vanilla ammo?
To make an ammo that belongs to a vanilla ammo class, simply set item.ammo to the correct AmmoID. For example, `Example Bullet` has `item.ammo = AmmoID.Bullet;` set in it's SetDefaults method. In addition, the ammo item must also define the projectile that the ammo shoots. `Example Bullet` shows this example: `item.shoot = mod.ProjectileType("ExampleBullet");`

# How do I make a new ammo class?
A new ammo class can be made by designating one of your ammo items as the AmmoID. For example, Example Mod shows us a new "Wisp" ammo class. The `Wisp` item has `item.ammo = item.type;` to designate it as a the defining ammo of the ammo class. `Spectre Gun` has `item.useAmmo = mod.ItemType("Wisp");` to match the pattern we have established with vanilla ammo items.
Any further ammo would have `item.useAmmo = mod.ItemType("Wisp")` and any other weapons using that ammo would have `item.useAmmo = mod.ItemType("Wisp")`.

# How can I make a new ammo class out of vanilla items?
Use a GlobalItem class to set `item.ammo` and item.shoot to a new projectile that you've made.

```
public class CopperShortsword : GlobalItem
{
	public override void SetDefaults(Item item)
	{
		if (item.type == ItemID.Rope)
		{
			item.ammo = ItemID.Rope;
			item.shoot = mod.ProjectileType("RopeShot")
		}
		if (item.type == ItemID.VineRope)
		{
			item.ammo = ItemID.Rope;
			item.shoot = mod.ProjectileType("VineRopeShot")
		}
		// and so on, for SilkRope and WebRope
	}
}
```

Note that using vanilla items to make new items runs the risk of conflict with other mods.
