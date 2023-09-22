# 좌표계(Coordinates)
많은 모더들이 테라리아의 다양한 좌표계를 이해하지 못해 Tiles, Projectiles, Drawing 코드를 작업할 때 혼란스러워합니다. 이 가이드에서 알려드리겠습니다.

## 공통점
모든 좌표계의 공통점은 X축은 왼쪽에서 오른쪽으로, Y축은 위에서 아래로 올라간다는 사실입니다. **아래는 양수 Y, 위는 음수 Y**입니다. 모든 좌표계는 좌표계가 나타내는 대상의 왼쪽 상단에서 시작합니다.

## 월드 좌표계(World Coordinates)
Players, NPC, Projectiles, Dust는 모두 월드 좌표계에 위치합니다. 월드 좌표는 일반적으로 `Vector2`로 표현되는데, 이는 월드 좌표의 `X` 및 `Y` 성분이 실수 값이므로 3943.23 및 2334.213과 같이 정수가 아닌 값도 유효한 값이라는 뜻입니다. 100% screen zoom을 사용하는 경우 모니터의 각 픽셀은 월드 공간의 1단위와 같습니다. 월드 좌표 0, 0은 월드의 맨 위 왼쪽 모서리에 있습니다. 

## 타일 좌표계(Tile Coordinaates)
타일은 타일 좌표계 위에 있습니다. 타일 좌표계는 타일 관련 메서드와 `Main.tile` 배열에서만 사용됩니다. 타일 좌표는 일반적으로 `Point` 또는 `Point16`으로 표시되며, 이는 타일 좌표의 `X` 및 `Y` 구성 요소는 정수(int)라는 의미입니다.

## 화면 좌표계(Screen Coordinates)
화면 좌표계(실제로는 창 좌표(window coordinates))는 화면에 그리거나 user interface elements를 그리는 것과 같은 작업에 사용됩니다. Entity를 수동으로 그릴 때 흔히 실수하는 것은 Entity를 Entity의 위치에 그리는 것입니다. `draw` 코드는 월드 공간 좌표가 아닌 화면 공간 좌표만 받기 때문에 이 방법은 동작하지 않습니다. 올바르게 그리려면 그리려는 좌표에서 `Main.screenPosition`을 빼야 합니다.

### 타일 그리기(Drawing Tiles)
타일이 항상 화면에 그려지는 것은 아닙니다. `ModTile.SpecialDraw` 또는 `PostDraw` 또는 `PreDraw`를 사용하는 경우, `Vector2 zero = Main.drawToScreen ? Vector2.Zero : new Vector2(Main.offScreenRange);`를 코드에 추가하고 `spriteBatch.Draw`에 대한 모든 호출에 `zero`를 더합니다. 그 이유는 다른 조명 모드를 사용할 때 발생하는 드로잉 좌표의 이동을 수용하기 위해서입니다. F9 키를 눌러 조명 모드를 빠르게 변경하여 코드가 모든 조명 모드에서 작동하는지 확인할 수 있습니다.

## 좌표 변환
### 타일 <--> 월드
각 타일은 월드 좌표 공간의 16x16 영역을 차지합니다. 타일 좌표에 16을 곱하면 해당 타일의 왼쪽 위 모서리를 가리키는 월드 좌표가 얻습니다. Vector2.ToWorldCoordinates를 사용하면 코드를 간소화할 수 있습니다. Point.ToWorldCoordinates는 자동으로 X와 Y에 8을 더하여 타일 중앙의 월드 좌표를 반환합니다.

```cs
// 플레이어 바로 뒤에 있는 타일을 찾습니다.
Point tileLocation = Main.LocalPlayer.Center.ToTileCoordinates();
Tile tile = Main.tile[tileLocation.X, tileLocation.Y];
```

### 월드 <--> 화면
타일을 직접 그릴 때는 위에서 언급된 `Main.drawToScreen`을 유의하고 있어야 합니다.

## 관련 필드
### Main.MouseWorld
현재 마우스 위치의 월드 좌표. 아쉽게도 줌을 사용하면 사용할 수 없습니다.

### Main.MouseScreen
마우스의 화면 좌표. `Main.mouseX`와 `Main.mouseY`와 동일합니다.

# 예시
![](https://i.imgur.com/T6La54i.png)

이 이미지를 살펴봅시다. 흰색 선은 타일 공간을 나타내고, 빨간색 점은 월드 좌표로 변환했을 때 타일의 월드 좌표를 나타냅니다. 오른쪽 아래 타일은 타일 좌표 `3548, 256`에 있습니다. 여기에 16을 곱하여 세계 좌표로 변환하면 `56768, 4096`이 됩니다. 달팽이의 위치는 노란색 상자에 표시됩니다. 엔티티의 위치는 엔티티의 왼쪽 상단 모서리와 관련이 있습니다. 앞서 언급한 타일의 월드 좌표에 `0, 16`을 더하면 `56768, 4112`가 되며, 이는 표시된 달팽이 상단의 위치에 매우 가깝습니다. (달팽이는 여전히 그 지점의 오른쪽에 있는 픽셀입니다.) 이 개념을 시각화하는 데 도움이 되었기를 바랍니다.

몇 가지 실용적인 예는 다음과 같습니다:
```cs
//플레이어의 중앙에서 50좌표 위에 스폰
Vector2 spawnPosition = player.Center + new Vector2(0, -50);

//타일 좌표는 월드의 타일을 인덱싱하는 데 사용되며, 여기서는 X와 Y를 나타내는 `i`와 `j`가 주어집니다.
Tile tileLeft = Main.tile[i - 1, j];
```