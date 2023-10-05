# 왜 기하학인가?
모더가 모드에서 구현하고자 하는 많은 시각적으로 흥미로운 효과와 흥미로운 동작에는 약간의 기하학 지식이 필요합니다. 이 가이드에서는 기하학의 가장 기본적인 사용법에 대해 설명합니다.

예를 들어, Dust나 투사체를 원호로 스폰하거나, 적이 플레이어를 향해 발사하거나, 귀환 동작을 작성하는 것은 모두 기하학을 활용합니다. 이 가이드의 후반부에서는 이러한 개념에 대한 예제를 통해 기본 사항을 설명합니다.

# 사전 지식
## 기하학 교육
프로그래머, 특히 비디오 게임에 관심이 있는 프로그래머에게 기하학은 필수적인 기술입니다. 테라리아는 2D 게임이기 때문에 필요한 기하학 지식이 방대하지는 않지만 벡터의 기본에 익숙해지는 것은 필수입니다.

## 좌표
테라리아는 다양한 좌표계를 사용하며, 그래픽 작업을 해본 적이 없다면 X와 Y의 방향이 낯설 수 있습니다. [기본 좌표계 가이드](Coordinates-kokr)를 참조하여 월드 좌표와 양의 X와 Y의 방향에 익숙해지도록 하세요.

## 회전
회전은 도가 아닌 라디안으로 표현됩니다. 도를 사용하려면 `MathHelper.ToRadians` 메서드를 호출하면 됩니다. 일반적으로 무기가 30도 호를 그리며 발사한다고 선언하는 것과 같이 일부 동작을 처음 할당할 때만 도를 사용하고자 합니다. 또한 rotation이 0일 때는 오른쪽을 향한다는 점에 유의하세요. 90도 회전하면 Y가 아래를 가리키므로 수직으로 아래를 가리키게 됩니다. 

# Vector2
`Vector2`는 기하학에서 배운 대로 2차원 벡터를 나타내는 구조체입니다. `Vector2`에는 2차원 벡터의 `X`와 `Y`성분의 크기를 나타내는 `X`와 `Y`의 두 필드가 포함됩니다. tModLoader에서 Vector2는 크게 두 가지 용도로 사용됩니다. 첫 번째 목적은 위치입니다. `Player`, `Projectile`, `Dust` 또는 `NPC`와 같은 많은 게임 요소의 위치를 나타내는 데 `Vector2`가 사용됩니다. `Vector2`는 속도를 나타내는 데도 사용됩니다. 속도는 무언가가 X와 Y 방향으로 이동하는 속도입니다. 예를 들어 플레이어의 위치가 `3, 7`이고 플레이어의 속도가 `4, 8`이라면, 플레이어의 위치는 게임에서 위치를 업데이트할 때마다 그 속도만큼 이동합니다. 이 예제에서는 한 번의 업데이트 후 플레이어의 새로운 위치는 `3 + 4 = 7`, `7 + 8 = 15`로 `7, 15`가 됩니다. 보시다시피 벡터를 추가하는 것은 각 개별 구성 요소를 추가하는 방식으로 이루어집니다. 벡터 A가 위치를 나타내고 벡터 B가 속도를 나타낸다면, X의 단위시간이 지난 후 새로운 위치는 `A + X * B`가 됩니다. 다음은 테라리아 좌표계에서 이 개념을 설명하는 몇 가지 다이어그램입니다. 영상에서 각 틱 마다 플레이어의 속도와 동일하게 위치가 어떻게 변경되는지 확인하십시오. 게임은 초당 60회의 업데이트로 실행되므로 속도 값은 초당 월드 스페이스 단위가 아닌 업데이트당 월드 스페이스 단위라는 걸 기억해주세요.     

![](https://i.imgur.com/GCqPyFc.png) ![](https://thumbs.gfycat.com/BoldFatFantail-size_restricted.gif)     

벡터를 사용하여 두 점 사이의 차이를 나타낼 수도 있습니다. 일반적인 예로 플레이어를 향해 투사체를 쏘는 적을 들 수 있습니다. 적이 플레이어를 향해 발사하도록 프로그래밍하려면 코드에서 발사할 방향을 알아야 합니다. npc.position에서 player.position을 빼면 그 방향을 계산할 수 있습니다. 벡터의 뺄셈은 덧셈과 동일하게 작동하며, 벡터의 X 성분끼리 서로 빼고 벡터의 Y 성분끼리 서로 뺍니다. A에서 B까지의 벡터는 다음과 같이 계산됩니다: `vector = B - A`. `14, 4`에 적이, `3, 12`에 플레이어가 있다고 가정해 봅시다. 플레이어 위치에서 적의 위치를 빼면 `-11, 8`이라는 결과를 얻을 수 있습니다. 이 벡터는 방향이 아닌 단순한 위치의 차이를 나타내므로 코드에서 직접 사용할 수 없습니다. 이 벡터를 방향으로 변환한 후 투사체를 스폰하는 데 사용하려면 아래의 Vector2.Normalize 섹션을 읽어보세요. 아래 다이어그램은 적에서 플레이어까지의 결과 벡터를 보여줍니다. 

![](https://i.imgur.com/TT90bk0.png)    

### Position vs Center
위 다이어그램에서 화살표가 엔티티의 왼쪽 모서리들끼리 가리키는 것을 보셨을 것입니다. 실제로 `player.position`은 게임이 설계된 방식대로 왼쪽 상단 모서리를 가리킵니다. 이것이 의미하는 바는 코드에서 `player.position`을 실제로 사용하는 경우는 거의 없다는 것입니다. 대신, 엔티티의 중앙을 가리키는 값인 `player.Center`를 사용합니다. (모든 엔티티는 사각형의 히트박스를 가지고 있다고 생각할 수 있습니다). `npc.Center`와 `projectile.Center`도 마찬가지입니다. 이제부터는 이 위치를 사용하는 것이 더 합리적이기 때문에 가이드에서는 `.Center`를 사용할 것입니다.

## 가속도
게임에서 투사체와 같은 물체의 위치를 업데이트할 때 현재 속도를 가져와서 현재 위치에 더합니다. 이 작업은 초당 60회 발생하며 월드 좌표에서 작동합니다. 총알과 같은 투사체의 경우 이 작업만으로 충분하지만, 시간에 따라 속도에 영향을 주는 '가속'을 구현하여 투사체에 흥미로운 움직임을 부여할 수도 있습니다. 이 가속은 다양한 `AI` 메서드와 다양한 충돌 메서드에서 발생합니다. 가속도는 중력, 바람 저항, 유도기능을 구현하는 데 사용할 수 있습니다.

### 중력
중력은 업데이트할 때마다 엔티티에 양수 Y 속도를 더하여 시뮬레이션합니다. 게임에서는 플레이어에게 중력을 기본으로 구현합니다. NPC는 `npc.noGravity`가 `false`면 중력의 영향을 받습니다. Dust에도 `noGravity` 필드가 있습니다. 투사체는 중력의 영향을 받지 않으므로 모더가 원하는 경우 `ModProjectile.AI`에서 투사체에 중력을 추가해야 합니다. [기본 ModProjectile 가이드](Basic-Projectile-kokr#gravity)에서 중력 구현에 대한 자세한 내용을 확인할 수 있습니다. 

```cs
projectile.velocity.Y = projectile.velocity.Y + 0.1f;
```

### 항력(Drag) 또는 바람 저항(Wind Resistance)
바람 저항을 시뮬레이션하려면 [기본 ModProjectile 가이드](Basic-Projectile-kokr#wind-resistance)를 참조하십시오. 기본적으로 속도의 성분에는 1보다 약간 작은 숫자가 곱해지므로 천천히 줄이세요.

### 유도(Homing)
가장 간단한 형태의 호밍은 기본적으로 목표물을 향해 가속하는 것입니다. [기본 ModProjectile 가이드](Basic-Projectile-kokr#homing)를 참고하세요.

### 충돌(Collision)과 튕김(Bounce)
투사체가 단단한 타일과 충돌하면 속도가 순간적으로 반전되어 투사체가 튕깁니다. 자세한 내용은 [기본 ModProjectile 가이드](Basic-Projectile-kokr#bounce-and-ontilecollide)를 참고하세요. 

### 가속도 시각화 예시

아래 gif에서는 위에서 설명한 대부분의 내용을 요약한 것을 볼 수 있습니다. 빨간색 화살표는 속도를, 파란색 화살표는 가속도를 나타냅니다. Shuriken 예제에서는 가속력이 약간 왼쪽과 아래쪽을 가리키는 것을 볼 수 있습니다. 왼쪽 힘은 바람 저항에 의해 발생하고 아래쪽 힘은 중력에 의해 발생합니다. 또한 주목할 만한 점은 수리검이 생성된 직후 1/3초 동안은 수리검에 어떤 힘도 작용하지 않는다는 것입니다. 이는 [지연 중력](Basic-Projectile#delayed-gravity) 섹션에 표시된 것과 같은 타이머를 통해 이루어지며 수리검에 흥미로운 움직임 동작을 부여합니다. 수류탄에는 바람의 저항력이 없기 때문에 중력만 보입니다. 수류탄이 타일과 충돌하면 순간적으로 큰 힘이 발생합니다. 이것이 투사체의 속도를 반전시키는 충돌력입니다.    

![](https://thumbs.gfycat.com/LittleLeanHalcyon-size_restricted.gif)     

## Vector2.Normalize
벡터를 다룰 때 대부분의 경우 벡터의 크기는 중요하지 않고 벡터가 나타내는 방향만 중요합니다. 적과 100 유닛 떨어진 플레이어와 적과 같은 방향으로 1000 유닛 떨어진 다른 플레이어가 있다고 가정해 보겠습니다. 적에서 각 플레이어까지의 벡터를 계산하면 이 벡터는 같은 방향을 가리키지만 한 벡터는 10배 더 길어집니다. 플레이어를 향해 투사체를 생성하는 `AI` 메서드에서 이 벡터를 그대로 사용하면 두 번째 투사체는 10배 더 빠르게 이동합니다! 저희는 플레이어가 아무리 멀리 떨어져 있더라도 적의 투사체가 일정한 속도를 갖기를 원합니다. (지금은 총알 스타일의 투사체를 상상하고 있습니다. 적이 수류탄과 같은 것을 던지는 경우 적의 최대 투척 속도까지 거리를 고려하고 싶겠지만, 여기서 말하는 것은 그런 종류의 고급 AI 동작이 아닙니다). 

이 문제에 대한 해결책은 벡터를 '정규화'하는 것입니다. 벡터를 정규화한다는 것은 단순히 벡터의 길이가 1이 되도록 스케일을 조정하여 단위 벡터로 만드는 것을 의미합니다. 학교에서 배운 피타고라스의 정리를 이용해 이 작업을 수행할 수 있지만, 다행히도 `Vector2` 클래스에는 이 기능이 이미 포함되어 있습니다. 하지만 0으로 나누면 게임이 충돌할 가능성이 있기 때문에 일반적인 정규화 메서드를 사용하지 않으려 합니다. 대신 SafeNormalize라는 메서드를 사용합니다:
```cs
// 이 코드는 ModNPC.AI 메서드에 있을 것입니다.
// 먼저, 두 벡터를 올바른 순서로 빼서 npc에서 플레이어까지의 벡터를 계산합니다(A에서 B까지의 벡터는 B - A).
Vector2 vectorFromNpcToPlayer = player.Center - npc.Center;
// 다음으로 SafeNormalize를 호출하여 벡터의 길이를 1(일명 단위 벡터)로 축소하면 방향만 나타내는 벡터가 생성됩니다.
Vector2 directionFromNpcToPlayer = vectorFromNpcToPlayer.SafeNormalize(Vector2.UnitX);
// 다음으로, 우리가 정해둔 발사 속도를 npc가 갖도록 합시다. 여러분은 적절한 속도를 찾기 위해 실험해 볼 수 있습니다.
float shootVelocity = 10;
// 마지막으로, 벡터에 shootVelocity를 곱하여 의도한 정해둔 초기 속도로 의도한 방향으로 투사체를 스폰합니다.
Projectile.NewProjectile(npc.Center, directionFromNpcToPlayer * shootVelocity, ProjectileID.BombSkeletronPrime, 5, 0, Main.myPlayer);
```

## Vector2.ToRotation
정규화되거나 정규화되지 않은 Vector2가 주어지면, 해당 벡터에 대해 `ToRotation` 메서드를 호출하여 rotation 값을 계산할 수 있습니다. 악마의 눈처럼 날아다니는 적들은 대상을 향하도록 회전하는 경우가 많습니다. 적에서 플레이어까지의 벡터를 나타내는 벡터를 사용하여 적의 회전을 설정하거나 현재 적의 속도를 사용하여 npc.rotation을 설정할 수 있습니다. 좀 더 고급 상황에서는 적 NPC가 플레이어를 향해 즉시 회전하지 않고 목표물을 향해 천천히 회전하도록 할 수 있습니다. 
```cs
// 먼저, 보고자 하는 대상을 가리키는 벡터를 계산합니다.
Vector2 vectorFromNpcToPlayer = player.Center - npc.Center;
// 둘째, ToRotation 메서드를 사용하여 해당 Vector2를 라디안 단위의 회전을 나타내는 실수로 변환합니다.
float desiredRotation = vectorFromNpcToPlayer.ToRotation();
// 이제 두 가지 중 하나를 할 수 있습니다. 가장 간단한 방법은 회전 값을 직접 사용하는 것입니다.
npc.rotation = desiredRotation;
// 두 번째 접근법은 해당 회전을 사용하여 최대 회전 속도를 준수하면서 NPC를 돌리는 것입니다. 좋은 값을 얻을 때까지 실험해 보세요.
npc.rotation = npc.rotation.AngleTowards(desiredRotation, 0.02f); 
```

## 벡터 곱하기
벡터에 실수를 곱하면 벡터의 크기를 조절할 수 있습니다. 일반적으로 위의 `Vector2.Normalize` 예제에서와 같이 정규화된 벡터의 크기를 조정합니다. 이 예제에서는 플레이어에게 쏘려는 방향을 나타내는 단위 벡터가 있었습니다. 이 벡터에 의도한 shoot velocity를 곱하여 이전과 같은 방향이지만 훨씬 더 긴 벡터를 만들었습니다. 이 벡터2를 `Projectile.NewProjectile` 메서드의 `velocity` 파라미터에 사용했으므로 결과적으로 원하는 방향으로 원하는 속도로 투사체가 스폰됩니다.

다른 이유로 벡터를 곱할 수도 있습니다. 예를 들어, [ExampleFlailProjectile.cs](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/Projectiles/ExampleFlailProjectile.cs#L139)에서는 투사체 속도 벡터에 `0.2f`를 곱하여 투사체의 속도를 원래 속도의 5분의 1로 효과적으로 줄였습니다. 이렇게 하면 도리깨가 타일을 튕길 때 무게감이 생깁니다.

## Vector2 길이
피타고라스 정리를 사용하지 않고도 벡터의 길이를 쉽게 구할 수 있습니다. 벡터의 길이를 계산하는 것은 많은 상황에서 매우 유용할 수 있습니다. 예를 들어 플레이어가 특정 범위 안에 들어올 때까지 총을 쏘지 않는 적이 있습니다. 적에서 플레이어까지 벡터의 길이를 확인하면 적에게 투사체를 스폰할지 여부를 결정하도록 할 수 있습니다.
```cs
// Vector2의 Length 메서드를 사용하여 기존 Vector2의 길이를 확인할 수 있습니다.
Vector2 vectorFromNpcToPlayer = player.Center - npc.Center;
float distanceBetweenPlayerAndNpc = vectorFromNpcToPlayer.Length();
// 또는 Vector2.Distance 메서드를 사용하여 두 개의 기존 Vector2 사이의 길이를 계산할 수 있습니다.
float distanceBetweenPlayerAndNpc = Vector2.Distance(player.Center, npc.Center);
// 그런 다음 해당 값을 로직에 사용합니다.
if(distanceBetweenPlayerAndNpc < 300) {
    // 플레이어가 범위 내에 있으므로 여기에 투사체를 스폰합니다.
}
```

### Vector2 길이 제곱
가장 가까운 엔티티를 찾기 위해 여러 엔티티를 반복하는 등 컴퓨터 집약적인 작업을 하는 경우, 길이 제곱 메서드를 사용하는 것이 더 효율적이라는 점에 유의해야 합니다. 이 상황에서는 `Vector2.DistanceSquared` 또는 `Vector2.LengthSquared`를 사용하는 것이 더 효율적입니다.

## Vector2 회전
벡터를 회전하는 것은 여러 용도로 유용할 수 있습니다. 일반적인 예로 무기의 부정확도를 부여하는 것이 있습니다. 원본 Vector2를 가지고 `RotatedByRandom` 메서드를 호출하면 주어진 라디안만큼 회전한 새로운 Vector2를 계산할 수 있습니다. Shotgun과 Chain gun 예제에서 실제로 작동하는 것을 보려면 [ExampleGun.cs](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/Items/Weapons/ExampleGun.cs#L61)를 참조하세요. 결과 분포가 균등하게 분포되지 않으므로 이 효과에 적합합니다.

벡터를 무작위가 아닌 양만큼 회전시킬 수도 있습니다. `RotatedBy` 메서드가 이를 수행합니다. 예를 들어, 벡터를 `MathHelper.Pi / 2` 또는 `MathHelper.ToRadians(90)`만큼 회전하여 원래 벡터에 수직인 벡터를 계산할 수 있습니다. 이 벡터를 사용하여 분할 투사체를 만들 수 있습니다. 벡터를 반복적으로 회전하면 호를 나타내는 여러 벡터를 계산할 수 있습니다. Vampire Knives 예제에서 실제로 작동하는 것을 보려면 [ExampleGun.cs](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/Items/Weapons/ExampleGun.cs#L87)를 참조하세요. 

// 기타?

# 랜덤 벡터
랜덤 벡터를 활용하면 시각 및 동작 효과에 다양성을 쉽게 추가할 수 있습니다. 랜덤 벡터를 생성하는 방법에는 여러 가지가 있습니다. 다음 접근 방식과 함께 제공되는 다이어그램은 해당 접근 방식을 사용하여 여러 더스트를 스폰한 게임 내 예시를 통해 그 동작을 설명합니다. 

## 원 내 랜덤 벡터
이 접근법은 모더가 랜덤 벡터를 생성하고자 할 때 가장 일반적인 결과입니다. 결과가 잘 분포되어 있습니다.
```cs
// 일반 접근법
Vector2 speed = Main.rand.NextVector2Circular(1f, 1f);
// 원호 내에서만 벡터를 생성합니다. 
Vector2 speed = Main.rand.NextVector2Unit((float)MathHelper.Pi / 4, (float)MathHelper.Pi / 2) * Main.rand.NextFloat();
// NextVector2Circular를 사용하면 타원형 분포에 대해 너비와 높이 반지름을 제공할 수 있습니다.
Vector2 speed = Main.rand.NextVector2Circular(0.5f, 1f);
```    
![](https://i.imgur.com/RJZuLoA.png) ![](https://i.imgur.com/N6XF5CZ.png) ![](https://i.imgur.com/wGW3hFn.png)    
![](https://thumbs.gfycat.com/MeaslyRecklessFantail-size_restricted.gif) ![](https://thumbs.gfycat.com/FoolhardyQuerulousBluewhale-size_restricted.gif) ![](https://thumbs.gfycat.com/FlakyEvilBrontosaurus-size_restricted.gif)

## 원 가장자리를 따라 랜덤 벡터 만들기
모더는 가장자리에 도달하는 랜덤 벡터를 생성함으로써 길이나 크기가 일정한 랜덤 벡터를 생성할 수 있습니다.    
```cs
// 일반 접근법
Vector2 speed = Main.rand.NextVector2Unit();
// 선택적 매개변수로 회전 범위를 지정할 수 있습니다. 이 예에서 시작 회전은 MathHelper.Pi / 4이며 그보다 더 큰 MathHelper.Pi / 2까지 가능합니다.
Vector2 speed = Main.rand.NextVector2Unit((float)MathHelper.Pi / 4, (float)MathHelper.Pi / 2);
```    
![](https://thumbs.gfycat.com/BlandDismalGnu-size_restricted.gif) ![](https://thumbs.gfycat.com/PlainImpressionableCricket-size_restricted.gif)   

## 정사각형 내 랜덤 벡터
오래된 테라리아 코드 중 상당수는 벡터를 무작위화하는 이상한 접근 방식을 사용합니다. 각 성분 X와 Y는 다음과 같은 방식으로 무작위로 생성됩니다:   
```cs
float xSpeed = Main.rand.NextFloat(-1f, 1f);
float ySpeed = Main.rand.NextFloat(-1f, 1f);
// 다른 접근법
Vector2 speed = Utils.RandomVector2(Main.rand, -1f, 1f);
```
언뜻 보기에는 잘 작동하는 것처럼 보이지만, 이 접근 방식은 실제로 원하지 않을 수 있는 이상한 분포를 가지며, 실제로 가상의 사각형 모서리로 뻗어나가는 의도한 것보다 더 긴 벡터를 생성할 수 있습니다. 아래 이미지와 GIF에서 이상한 모양이 형성되는 것을 확인할 수 있습니다.   
![](https://i.imgur.com/rmPZlwk.png)    
![](https://thumbs.gfycat.com/LightCircularBluefish-size_restricted.gif)    

## 랜덤 벡터 사각형 모서리
별로 유용하지 않습니다.

# 예시
이제 기하학에 대한 기본 지식이 생겼고, 그 지식을 쉽게 활용할 수 있는 다양한 벡터2 메서드를 살펴봤으니, 이제 기하학를 사용하여 모드에 흥미로운 동작을 프로그래밍할 수 있습니다.

이 예제에서는 더스트 또는 프로젝타일을 사용하여 기법을 설명했지만, 서로 바꿔 사용할 수 있습니다. 각 파라미터의 목적을 알기 위해 사용 중인 메서드의 메서드 시그니처를 참조하는 것만 잊지 마세요.

## 무언가의 무작위 버스트/원 생성하기
위의 랜덤 벡터 섹션에 사용된 코드입니다. 이 예제에서는 for 루프를 사용하여 원 가장자리를 따라 각각 무작위 벡터를 가진 50개의 더스트를 스폰합니다. 벡터에 5를 곱하여 스케일을 조정하고 더 크게 만들어 Dust가 좋은 거리로 이동하도록 합니다.
```cs
for (int i = 0; i < 50; i++) {
	Vector2 speed = Main.rand.NextVector2CircularEdge(1f, 1f);
	Dust d = Dust.NewDustPerfect(Main.LocalPlayer.Top, DustID.BlueCrystalShard, speed * 5, Scale: 1.5f);
	d.noGravity = true;
}
```
몇 가지 간단한 기하학를 사용하여 스폰 위치를 같은 지점에서 멀어지게 변경할 수 있습니다. `Main.LocalPlayer.Top`에 `speed * 32`를 추가하면 Dust가 모두 같은 지점에서 시작하지 않고 작은 원에서 시작하여 바깥쪽으로 확장됩니다. 
```cs
Dust d = Dust.NewDustPerfect(Main.LocalPlayer.Top + speed * 32, DustID.BlueCrystalShard, speed * 2, Scale: 1.5f);
```
효과를 더 쉽게 시각화하기 위해 속도를 줄였습니다.    
![](https://thumbs.gfycat.com/ImaginativeMenacingIbex-size_restricted.gif)    

## 목표물에 쏘기
[Example Worm](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/NPCs/Worm.cs#L36)은 플레이어를 향해 투사체를 쏘는 적의 기본 패턴을 보여줍니다.

## 목표물 향하기
[Vector2.ToRotation](Geometry-kokr#vector2torotation)을 사용하여 타겟을 향하게 할 수 있습니다. 엔티티 텍스처의 방향이 오른쪽을 향하지 않는 경우 `MathHelper.Pi / 2`를 추가하여 엔티티를 추가로 90도 회전시켜야 할 수 있습니다. 또한 투사체나 NPC가 왼쪽을 향할 때 스프라이트가 뒤집히는 경우도 있습니다. 이는 `spriteDirection` bool을 통해 수행됩니다. 이 bool이 참이면 `MathHelper.Pi`를 추가하여 보정하기 위해 180도 회전을 추가로 추가해야 할 수 있습니다:
```cs
// ModProjectile.AI 끝 부분에
if (projectile.spriteDirection == -1) { 
	projectile.rotation += MathHelper.Pi;
}
```

// 할 일: 어떤 일이 자동으로 일어나는지, 투사체와 NPC 모두에서 다양한 방향과 spriteDirection 코드를 어디에 넣어야 하는지 설명하기.

# 자세히 알아보기
이 가이드를 마스터한 후 콜리전을 배우면 유용할 수 있습니다. // 할일: 콜리전 가이드 만들기.