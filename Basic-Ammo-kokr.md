***
이 가이드가 1.4로 업데이트되었습니다. 이 위키 페이지의 이전 1.3 버전을 보려면 [여기](https://github.com/tModLoader/tModLoader/wiki/Basic-Ammo/3ac8255f8a511bc25d15090d52fcd045febccb4f)를 클릭하세요.
***

# 탄약(Ammo)이란 무엇인가요?
탄약은 무기 아이템, 탄약 아이템, 투사체를 서로 연결하는 시스템입니다. 기본 개념은 다음과 같습니다: 무기 아이템에는 `Item.useAmmo`가 AmmoID로 설정되어 있고, 무기가 사용해야 하는 탄약 아이템에는 `Item.ammo`가 동일한 AmmoID로 설정되어 있으며, 해당 탄약 아이템에는 탄약 사용 시 무기가 발사할 특정 투사체에 대한 `Item.shoot`가 설정되어 있습니다.

예를 들어 `Wooden Bow`과 `Flaming Arrow`를 살펴봅시다. `Wooden Bow`에는 `Item.useAmmo = AmmoID.Arrow;`가 있고, `Flaming Arrow`에는 `Item.shoot = ProjectileID.FireArrow;` 및 `Item.ammo = AmmoID.Arrow;`가 있습니다. 플레이어가 `Wooden Bow`을 쏘면 테라리아는 플레이어의 인벤토리에서 `Wooden Bow`의 `Item.useAmmo`와 일치하는 `Item.ammo`가 있는 아이템을 검색합니다. 탄약 아이템이 발견되면 탄약 아이템의 `Item.shoot` 발사체가 생성되고 탄약이 소모됩니다.

일반적으로 AmmoID는 첫 번째 탄약 아이템의 ItemID와 일치합니다. 예를 들어, AmmoID.Arrow와 ItemID.WoodenArrow는 모두 40입니다. 다른 모든 화살표에는 `Item.ammo = AmmoID.Arrow;`가 있습니다.

# 무기에 바닐라 탄약을 사용하려면 어떻게 해야 하나요?
바닐라 탄약을 사용하려면 Item.useAmmo를 올바른 AmmoID로 설정하면 됩니다. 예를 들어, ExampleMod의 `Example Gun`에는 `Item.useAmmo = AmmoID.Bullet;`가 있습니다.

# 바닐라 탄약은 어떻게 만들 수 있나요?
바닐라 탄약분류(Ammo class)에 속하는 탄약을 만들려면 Item.ammo를 올바른 AmmoID로 설정하기만 하면 됩니다. 예를 들어, `Example Bullet`은 SetDefaults 메서드에 `Item.ammo = AmmoID.Bullet;`가 설정되어 있습니다. 또한 탄약 아이템은 탄약이 발사하는 투사체도 정의해야 합니다. `Example Bullet`은 다음과 같습니다: `Item.shoot = ModContent.ProjectileType<Projectiles.ExampleBullet>();`

# 탄약분류를 새로 만들려면 어떻게 하나요?
탄약 아이템 중 하나를 AmmoID로 지정하여 새 탄약분류를 만들 수 있습니다. 예를 들어, Example Mod에서는 새로운 탄약분류 "ExampleCustomAmmo"를 보여줍니다. `ExampleCustomAmmo` 아이템에는 탄약분류의 AmmoID로 지정하기 위해 `Item.ammo = Item.type;`이 있습니다. `ExampleCustomAmmoGun`에는 바닐라 탄약 아이템에 설정한 패턴과 일치하도록 `Item.useAmmo = ModContent.ItemType<ExampleCustomAmmo>();`가 있습니다.
추가 탄약에는 `Item.ammo = ModContent.ItemType<ExampleCustomAmmo>()`가, 해당 탄약을 사용하는 다른 무기에는 `Item.useAmmo = ModContent.ItemType<ExampleCustomAmmo>()`가 있습니다.

# 바닐라 아이템으로 새로운 탄약분류를 만들려면 어떻게 해야 하나요?
GlobalItem 클래스를 사용하여 `Item.ammo` 및 `Item.shoot`을 만든 새 투사체로 설정합니다.

```cs
public class AmmoModificationsGlobalItem : GlobalItem
{
	public override void SetDefaults(Item item)
	{
		// 이 item은 "item"이라고 이름붙은 매개변수이기 때문에 여기에 "Item" 대신 "item"을 입력했습니다.
		if (item.type == ItemID.Rope)
		{
			item.ammo = ItemID.Rope;
			item.shoot = ModContent.ProjectileType<RopeShot>();
		}
		if (item.type == ItemID.VineRope)
		{
			item.ammo = ItemID.Rope;
			item.shoot = ModContent.ProjectileType<VineRopeShot>();
		}
		// 실크 로프와 웹 로프의 경우 등...
	}
}
```

바닐라 아이템을 사용하여 새 아이템을 만들면 다른 모드와 충돌할 위험이 있으므로 호환성을 유지하고 플레이어가 혼란스러워하지 않도록 아껴서 사용하세요.