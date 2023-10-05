# 스프라이트가 무엇인가요?

"스프라이트"는 일반적으로 2D 게임에서 볼 수 있는 2D 그래픽을 말합니다. 예를 들어, Gold Bar 아이템 스프라이트인 `Item_19.png`는 다음과 같습니다: ![Item_19](https://user-images.githubusercontent.com/4522492/159594962-1a594c3c-cd6c-4741-b60e-f8409461d845.png) .  모드를 만들 때는 일반적으로 모드에 추가하는 각 시각적 콘텐츠에 대해 스프라이트를 만들어야 합니다. 

이 가이드에서는 스프라이트 제작의 기본 사항과 스프라이트가 tModLoader와 관련하여 어떻게 작동하는지에 대해 설명합니다.

## 스프라이트 프레임
많은 스프라이트는 보통 스프라이트시트라고 불리는 단일 이미지 내에 여러 개의 "프레임"을 갖고 있습니다. 이러한 프레임은 일반적으로 애니메이션이나 파생형(variation)에 사용됩니다. 예를 들어 GiantBee 투사체는 다음과 같습니다:     
![](https://i.imgur.com/jRhab1A.png)    
이 텍스처는 추출한 바닐라 텍스처 사본에서 `Projectile_566.png`를 찾으면 찾을 수 있습니다. 프로그래머가 이 스프라이트에 4개의 프레임이 있다고 알려주었기 때문에 게임은 96픽셀 높이의 텍스처를 동일한 크기의 24픽셀 높이 섹션 4개로 분할하는 것을 알고 있습니다. 게임이 이 투사체를 그릴 때 이 4개의 프레임 사이를 순환합니다.

일부 스프라이트 프레임은 파생형을 위해 사용됩니다. 다음은 금속 바(Metal Bars) 스프라이트인 `Tiles_239.png`이며, 이 스프라이트는 모든 금속 바 타일 간에 공유됩니다:    
![](https://camo.githubusercontent.com/9781684c4d69c2de30fc3495148a112bf50207246f4dbf1da36bb5a7afa28620/68747470733a2f2f692e696d6775722e636f6d2f62716f794c71542e706e67)    
타일 스프라이팅은 그 자체로 또 하나의 주제이므로 타일 스프라이트를 만들기 전에 [기본 ModTile 가이드](Basic-Tile-kokr#framed-vs-frameimportant-tiles)를 참조하고 프로그래머에게 세부 사항을 확인하십시오.




## 파일 포맷
모든 tModLoader 스프라이트는 .png 파일이어야 합니다.여러분이 스트라이팅 프로그램에서 특별히 설정을 변경하지 않는 한 .png로 저장하면 충분합니다. 


## 스프라이트 제작 도구
여러분은 기능이 충분한 그림/스프라이팅 프로그램을 다운로드해야 합니다. 그림판은 사용할 수 없습니다. 적합한 프로그램 목록은 [기본 가이드 필요 조건 문서](Basic-Prerequisites-kokr#드로잉-프로그램)를 참조하세요. 이 가이드의 나머지 부분에서는 예제 이미지에서 Aseprite를 사용하겠지만, 목록의 어떤 프로그램이라도 비슷한 기능을 가지고 있을 것입니다.

정식 버전은 유료지만 Aseprite는 100% 안전하고 합법적으로 사용할 수 있는 컴파일 가능한 무료 버전을 제공합니다. 이 [가이드](https://github.com/aseprite/aseprite/blob/main/INSTALL.md)를 이용하여 컴파일할 수 있습니다.

권장되는 다른 스프라이팅 프로그램에는 다음이 포함되지만 이에 국한되지는 않습니다:
* [Piskel](https://www.piskelapp.com/)
* [GIMP](https://www.gimp.org/)
* [LibreSprite](https://libresprite.github.io/#!/)
* [paint.NET](https://www.getpaint.net/)

## 바닐라 스프라이트 리소스
시작하기 전에 바닐라 스프라이트를 쉽게 접근할 수 있는 폴더에 추출해 두는 것이 좋습니다. 이 가이드에서는 `C:\Documents\My Games\Terraria\ModLoader\VanillaTextures` 폴더를 사용하겠습니다. 이렇게 하려면 [이 가이드](Intermediate-Prerequisites-kokr#vanilla-texture-file-reference)를 따르세요. 이 텍스처는 모드에서 만들 유사한 콘텐츠의 크기와 방향에 대한 가이드로 사용할 것입니다. 

바닐라 텍스처의 크기에 대한 정보만 필요한 경우 이 [텍스트 파일](https://forums.terraria.org/index.php?attachments/sprite-information-spreadsheet-tsv.384515/)을 참고할 수 있습니다. 

## 실시간 스프라이트 편집
타일이나 방어구 텍스처를 약간 조정해야 하는 경우, 스프라이트를 변경하고 모드를 다시 빌드하고 다시 로드한 다음 게임 내에서 테스트하여 변경 사항이 올바른지 확인하는 것은 많은 시간이 소요되는 일입니다. "Modders Toolkit(모더 툴킷)"이라는 모드를 사용하여 게임 내에서 텍스처를 실시간으로 편집할 수 있습니다. 아래 동영상을 통해 그 방법을 확인하세요. 변경한 후에는 변경된 PNG를 모드 소스 폴더에 다시 복사해야 합니다.

![](https://thumbs.gfycat.com/KaleidoscopicClosedBeardeddragon-max-1mb.gif)
[HQ 버전](https://gfycat.com/KaleidoscopicClosedBeardeddragon)

## 스프라이트 만들기

### 2x2 픽셀
테라리아는 각 픽셀이 실제로 2x2 픽셀 블록으로 구성된 픽셀 아트 스타일을 사용합니다. 이 아트 스타일을 따를 필요는 없지만 이 개념은 알고 있어야 합니다. 이 스타일을 따르고 싶다면 일반적인 픽셀로 그리고 결과 스프라이트의 크기를 200%까지 조정하면서 크기를 조정할 때 "Nearest Neighbor" 옵션을 사용하면 작업을 단순화할 수 있습니다. 이렇게 하면 패딩도 2픽셀에서 1픽셀로 변경된다는 점에 유의하세요.

스프라이트의 업스케일링이 잘못되었을 때 흔히 볼 수 있는 용어는 믹셀(Mixel)입니다. '믹셀'은 스프라이트 내에서 크기가 다른 픽셀이 섞여 있어 일관성이 떨어지고 스프라이트가 지저분하게 보이는 것을 말합니다. 항상 1x1 픽셀로 스프라이트를 만든 다음 2x2로 업스케일링하는 것이 좋으며, 그 반대의 경우나 두 가지를 혼합하여 사용하는 것은 권장하지 않습니다. 

또 다른 우려 사항은 플레이어가 스프라이트가 너무 작아 보이면 `Item.scale`을 사용하여 직접 크기를 조정하려는 유혹을 받을 수 있다는 것입니다. 이 방법은 1x1 스프라이트를 2x2로 변환하는 데는 유용하지만, 이미 2x2인 스프라이트를 4x4처럼 더 큰 크기로 변환하는 경우와 같은 경우에는 유용하지 않습니다. 이런 식으로 스프라이트를 업스케일링하면 앞서 설명한 대로 믹셀이 발생할 뿐만 아니라 이미 게임에서 2x2 스프라이트와 일치하지 않기 때문입니다.

이 예제의 왼쪽은 픽셀 크기가 일정하지 않은 반면 오른쪽은 일관성을 유지합니다. 오른쪽은 그 자체로 일관성이 있기 때문에 더 깔끔하게 보입니다.    
![](https://i.imgur.com/06pNr1r.png)    

### 크기(Dimensions)
스프라이트의 크기는 스프라이트가 구성되는 높이와 너비를 나타냅니다. 예를 들어 너비가 20픽셀, 높이가 20픽셀인 스프라이트는 20x20 스프라이트가 됩니다.

스프라이트의 크기를 조정하기 전 최대 크기는 2048x2048이므로, 2x2로 크기를 조정하기 전에는 1024x1024가 됩니다.

## 그림 도구

## 패딩
스프라이트에 여러 프레임이 있는 경우 패딩을 사용하여 개별 프레임을 구분합니다. 기본적으로 테라리아는 각 프레임 사이에 2픽셀의 패딩을 예상합니다. 가로로 배치된 프레임의 경우 마지막 프레임을 포함하여 각 프레임 아래에 2픽셀의 패딩이 필요합니다. 다음은 자홍색 패딩을 그려 넣어 패딩이 어디로 가는지 보여주는 GiantBee 발사체 스프라이트입니다.    
![](https://i.imgur.com/CYsKgX3.png)    

타일의 경우 패딩은 오른쪽과 각 프레임 아래에 있습니다.

## 애니메이션
애니메이션은 스프라이트 내에 여러 프레임의 애니메이션을 포함하는 방식으로 이루어집니다. 프레임의 간격이 균일한지 확인하세요.



애니메이션은 일반적으로 반복되지만 사용자 지정 애니메이션 주기도 가능합니다. 애니메이션 속도와 패턴은 프로그래머에게 달려 있으므로 이에 대한 자세한 내용은 별도의 가이드에서 확인할 수 있습니다. 
* [투사체 애니메이션 코드 가이드](Basic-Projectile-kokr#animationmultiple-frames)

## 방향
테라리아의 많은 사물은 스프라이트의 예상되는 방향이 정해져 있습니다. 예를 들어 NPC 스프라이트는 보통 왼쪽을 향하고 ![](https://i.imgur.com/PhcRvIa.png), 화살표 발사체는 위쪽을 향하고 ![](https://i.imgur.com/ogDFqEa.png), 지팡이는 오른쪽 위쪽을 향합니다 ![](https://i.imgur.com/ax451LJ.png). 이러한 규칙은 이러한 스프라이트를 그리는 코드에서 스프라이트가 특정 방향으로 향할 것으로 예상하기 때문에 따르는 것이 유용합니다. 이러한 규칙을 따르지 않으면 히트박스와 드로잉이 불안정해집니다. 가장 유사한 바닐라 스프라이트를 참조하여 스프라이트 방향에 대한 좋은 아이디어를 얻으세요.

### 투사체

### 글로우마스크(Glowmasks)
글로우마스크는 조명에 관계없이 최대 밝기로 그려지는 별도의 스프라이트를 의미합니다. 일반적으로 무기의 빛나는 라이트에 사용됩니다. 예를 들어 일반 VortexDrill 아이템 스프라이트인 ![](https://i.imgur.com/Oa54l03.png)는 주변에 빛이 있을 때만 볼 수 있지만, 해당 글로우마스크 스프라이트인 ![](https://i.imgur.com/dNTFMTM.png)는 주변의 빛과 관계없이 최대 밝기로 그려집니다:    
![](https://i.imgur.com/jjV8Txq.png) ![](https://i.imgur.com/hGEHjCu.png)

글로우마스크 스프라이트를 만들려면 원본 스프라이트의 복사본을 만들고 "빛나지" 않아야 하는 모든 픽셀을 제거합니다. 그런 다음 프로그래머가 올바르게 그릴 수 있도록 코드를 작성해야 합니다.

# 예제
여기서는 칼 스프라이트를 만들어 보겠습니다. 먼저 비슷한 바닐라 검 스프라이트를 살펴보고 얼마나 크게 만들 것인지에 대한 아이디어를 얻겠습니다. 먼저 위키에서 원하는 크기의 무기를 찾아보겠습니다. [Muramasa](https://terraria.wiki.gg/wiki/Muramasa)를 사용합시다. 위키에 있는 아이템 스프라이트는 실제 스프라이트와 완전히 동일하지 않으므로 해당 스프라이트를 사용하지 마세요. 대신 인포박스에서 "Internal Item ID: ###"을(를) 찾아보세요. 여기서 숫자는 155이므로 추출한 바닐라 스프라이트의 사본을 열고 `Item_155.png`를 찾습니다. 스프라이트 편집기에서 이 파일을 열고 다른 이름으로 저장을 사용하여 스프라이트를 저장하는 폴더에 이 파일의 사본을 저장합니다. 이제 동일한 방향과 일반적인 크기를 유지하면서 자유롭게 검을 그립니다. 2x2 픽셀도 주목하세요. 이 방법을 사용하면 바닐라 무기와 같은 크기로 표시되는 스프라이트를 만들 수 있습니다. 이 경우 스프라이트는 58x58픽셀입니다. 시간이 지나면 다양한 엔티티 크기의 규칙을 익히고 바닐라 텍스처 가이드 없이도 스프라이트를 만들 수 있습니다.

# 좋은 픽셀 아트 만들기

이 가이드는 스프라이트의 구조와 모드에 통합하는 방법에 대한 기술적 세부 사항에 중점을 두었습니다. 스프라이트 제작이 어렵다면 함께 작업할 아티스트 친구를 찾는 것이 좋습니다. 하지만 예술적 기술을 더 발전시키고 싶다면 인터넷에 많은 학습 자료가 있으니 참고하세요. [tModLoader 디스코드 서버](https://tmodloader.net/discord)에 가입하고 `#spriting` 채널을 방문하면 다른 스프라이터로부터 조언과 피드백을 얻을 수 있습니다. 다음은 다른 학습 리소스에 대한 몇 가지 링크입니다:
* [궁극의 픽셀 아트 튜토리얼(동영상)](https://www.youtube.com/watch?v=lfR7Qj04-UA)