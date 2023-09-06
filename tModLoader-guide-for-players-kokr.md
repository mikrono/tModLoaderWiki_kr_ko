___

**[모드를 플레이만 하고 싶지 않고, 만들기도 하고 싶어요.](tModLoader-guide-for-developers-kokr)**

___

**[모드를 플레이만 하고 싶지 않고, tModLoader에도 기여하고 싶어요.](tModLoader-guide-for-contributors-kokr)**

___

## 스팀 설치
스팀에서 [모드로더](https://store.steampowered.com/app/1281930/tModLoader)를 설치하려면, 먼저 스팀에서 [테라리아](https://store.steampowered.com/app/105600/Terraria)를 보유하고 있어야 합니다.
테라리아를 보유하고 있다면 스팀에서 '[tModLoader](https://store.steampowered.com/app/1281930/tModLoader/)'를 검색하고 설치하면 됩니다.
모드로더는 바닐라와 별도로 설치되어 바닐라를 재설치하는 번거로움없이 바닐라와 모드를 모두 플레이할 수 있습니다. 
Family Share를 사용해 테라리아를 설치하신 분은 하단의 [스팀 가족 공유 설치](tModLoader-guide-for-players-kokr#스팀-가족-공유-설치)을 참고하여 주시기 바랍니다.

기본적으로 스팀은 현재 모드로더의 stable 버전을 설치합니다. 현재의 stable 버전은 테라리아 1.4.4 버전의 콘텐츠에 기반하고 있습니다.

### 베타 버전
모드로더는 정기적으로 업데이트합니다. 대부분의 유저는 기본 릴리즈를 사용하게 될 것입니다. 필요하다면 유저들은 스팀의 베타 속성을 사용해 레거시나 프리뷰 버전의 모드로더를 사용할 수 있습니다. 레거시 버전의 모드로더를 사용해 최신 stable 버전의 모드로더로 업데이트되지 않은 모드를 플레이할 수 있습니다. 프리뷰 버전의 모드로더는 stable 버전에 추가되지 않은 새 기능이나 테라리아의 콘텐츠를 테스트하는 데 사용할 수 있습니다.

> **주의사항:** 다른 버전의 모드로더를 사용할 때, 여러분이 원하는 모드가 해당 모드로더 버전에서 사용가능하지 않을 수 있습니다. 따라서 여러분이 원하는 모드들을 한번에 플레이할 수 없을 가능성이 큽니다. 프리뷰 버전의 모드로더에서는 그럴 가능성이 더욱 큽니다.

다음은 선택 가능한 옵션입니다.:
* `default/None` - 이것이 일반적인 플레이어들이 사용할 stable 버전입니다. 현재는 테라리아 1.4.4.9 버전에 기반하고 있습니다. 1.4.3버전에서 플레이중이던 플레이어는 `1.4.3-legacy`버전을 선택해야 합니다.
* `1.3-legacy` - 1.3 모드로더의 최신 버전입니다. 많은 사랑을 받던 몇몇 모드들은 1.3 버전에서만 플레이할 수 있습니다.
* `1.4.3-legacy` - 1.4.3 모드로더의 최신 버전입니다. 1.4.4 버전의 모드로더로 업데이트되지 않은 모드를 플레이하던 플레이어들은 이 버전에 남아있어야 합니다.
* `1.4.4-preview` - 이 버전의 모드로더는 곧 업데이트될 변경점들을 담고 있습니다. 이곳은 우리가 새로운 변경점들을 테스트하는 곳이기도 합니다. 유저들은 프리뷰 버전에서 게임을 플레이하는 것이 추천되지 않는다는 것을 명심하여야 합니다.

### 1.3 (레거시 모드로더)와 기타 베타 옵션을 사용하는 방법
아래 지침에 따라 Steam에서 다른 버전의 티모드로더로 전환하세요. GOG 사용자이거나 특정 버전을 수동으로 설치해야 하는 경우, [수동 설치](tModLoader-guide-for-players-kokr#수동-설치) 지침을 따르세요.

**동영상 지침:**

이 [동영상 지침](https://gfycat.com/AcidicDangerousAmericanwirehair)이 명확하지 않은 경우, 아래의 **글 지침**을 읽어보세요.     
![](https://thumbs.gfycat.com/AcidicDangerousAmericanwirehair-size_restricted.gif)     

<details><요약>글 지침:</summary><blockquote>

 라이브러리로 이동합니다: 

![image](https://user-images.githubusercontent.com/59670736/169886058-3eb1b43c-a113-468b-8213-ef0bcccc8e01.png)

 tModLoader를 찾습니다:

![image](https://user-images.githubusercontent.com/59670736/169886128-43f95278-a2a4-4b13-bb7c-1b670c81b657.png)

 tModLoader를 우클릭합니다:

![image](https://user-images.githubusercontent.com/59670736/169886174-dfe0612b-75d9-4469-8f13-be3649fd35fc.png)

 속성을 클릭하여 Steam 게임 제어판을 엽니다.

![image](https://user-images.githubusercontent.com/59670736/169886269-f3a0e854-bbe6-4c2f-ab4c-6980406fea51.png)

 베타를 선택합니다.

![image](https://user-images.githubusercontent.com/59670736/169886307-e60be211-d331-443f-bcde-109f61c23323.png)

 드롭다운 메뉴에서 원하는 베타를 선택합니다.

![image](https://user-images.githubusercontent.com/59670736/169886370-5a340164-ccfe-4520-a3f5-515fd81671cf.png)

 창을 닫습니다(코드 필요 없음).

![image](https://user-images.githubusercontent.com/59670736/169886435-b9cd7a18-eeb9-46f8-a41f-3dc450be8702.png)

</blockquote></details>

### 어떻게 삭제하나요?
tModLoader -> 관리 -> 제거를 마우스 오른쪽 버튼으로 클릭하기만 하면 됩니다.
바닐라 설치는 영향받지 않습니다.

### 스팀 가족 공유 설치
어떤 이유로 테라리아를 소유하지 않고 가족 공유 테라리아를 사용 중인 경우, tModLoader가 실행되지 않고 Steam 상점으로 이동합니다. 이는 Steam 측의 제한이지만 1.4.X 및 1.3 버전 모두에 대한 해결 방법이 있습니다.
이러한 해결 방법으로는 Steam 멀티플레이어를 사용할 수 없으므로 멀티플레이어를 사용하려면 'IP 연결' 옵션을 사용해야 합니다.


1.4.X 버전의 경우, 해결 방법은 수동 설치 지침에 따라 tModLoader를 설치하고 파일에 포함된 `start-tModLoaderFamilyShare.bat/sh`를 사용하여 실행하는 것입니다. 또는 Steam 클라이언트에서 파일을 다운로드할 수 있는 경우, 이론적으로는 작동하지만 사용자마다 실행가능 여부가 다릅니다.
라이브러리에서 이 파일을 '비스팀 게임'으로 설정하여 게임에서 Steam 오버레이를 활성화할 수도 있습니다.

1.3 버전에서의 해결 방법은 정상적으로 Steam을 통해 tModLoader를 설치한 다음, Terraria 설치 폴더의 `steam_appid.txt` 파일을 기존 파일을 대체하여 tModLoader 설치 폴더에 복사하는 것입니다.

## 수동 설치
이 설치는 GOG에서 Terraria를 구매했거나 특정 버전의 tModLoader를 설치하고자 하는 플레이어에게 필요합니다.

tModLoader 설치는 비교적 쉽습니다.

1. **[releases](https://github.com/tModLoader/tModLoader/releases)** 페이지로 이동하여 원하는 tModLoader 릴리즈를 다운로드합니다.
    1. 'Default/None/1.4.4': **[latest](https://github.com/tModLoader/tModLoader/releases/latest)** 페이지로 이동하여 `tModLoader.zip`을 다운로드합니다.
    2. `1.3`: [v0.11.8.9](https://github.com/tModLoader/tModLoader/releases/tag/v0.11.8.9)를 방문하여 운영체제에 맞는 파일을 다운로드합니다.
    3. `1.4.3`: [releases page](https://github.com/tModLoader/tModLoader/releases)를 방문하여 1.4.3이 언급된 최신 항목을 찾습니다. "Assets"라고 표시된 섹션을 확장하고 "tModLoader.zip" 파일을 클릭하여 다운로드합니다.
    4. `1.4.4-preview`: tModLoader의 프리뷰 버전 다운로드는 [releases page](https://github.com/tModLoader/tModLoader/releases)에서 확인할 수 있습니다. 이름에 "preview"가 있는 가장 최근 항목을 찾습니다. "Assets"라고 표시된 섹션을 확장하고 "tModLoader.zip" 파일을 클릭하여 다운로드합니다. 변경 사항을 최신 상태로 유지하려면 정기적으로 1.4.4-프리뷰의 새 버전을 수동으로 설치해야 합니다. 
2. 다운로드한 압축 파일의 **내용물**를 Terraria 설치 폴더 옆 또는 폴더 안에 있는 `tModLoader`라는 이름의 폴더에 압축을 풉니다. 
1.4 버전에서 폴더에 'Build' 폴더가 포함되어 있다면, 이 중간 폴더를 제거하고 내용을 한 단계 위로 올려야 합니다. (GOG는 보통 `C:\GOG Games`에, Steam은 `C:\Program Files (x86)\Steam\steamapps\common\Terraria`에 설치합니다. 사용자 지정한 경우 Steam 설치 위치를 찾으려면 [이 동영상](https://gfycat.com/SelfreliantAssuredIsabellineshrike)을 참조하세요.) (리눅스를 사용 중이고 GOG에서 게임을 소유하고 있는 경우, '테라리아\게임' 안에 중첩된 옵션을 권장합니다.) 압축 파일의 압축을 푸는 방법을 모른다면, 컴퓨터 사용법을 아는 사람에게 도움을 요청하세요.

    * **옵션 1, 나란히 (권장)**:    
![](https://i.imgur.com/gmrBMSO.png)    
    **옵션 2, 중첩**:    
![](https://i.imgur.com/YWaqZPO.png)    
    * **설치하지 마세요** tModLoader 파일을 테라리아 폴더에 직접 설치하지 마세요. 이 옵션은 Mac의 GOG에서는 지원되지 않습니다.
3. [이 단계는 1.3에만 적용됨] 소유한 게임 버전에 따라 Steam 파일을 제거하거나 추가합니다:
    1. 테라리아의 GOG 버전을 사용 중인 경우, 방금 tModLoader를 추출한 폴더에서 Steam 파일을 삭제합니다(다운로드한 압축 파일에서 이미 삭제되었을 수 있음):
        * Windows: `steam_api.dll`
        * Linux: `lib/libsteam_api.so` 및 `lib64/libsteam_api.so`
        * Mac: `tModLoader.app/Contents/MacOS/osx/libsteam_api.dylib`
    2. Steam 버전의 테라리아를 사용하는 경우, 압축 파일에 Steam 파일이 누락된 경우 테라리아 설치 파일에서 tModLoader 설치 파일로 복사하세요:
        * 사용 중인 플랫폼에 따라 `steam_appid.txt`를 복사합니다:
            * Windows: `steam_api.dll` 및 `CSteamworks.dll`
            * Linux: `lib/libsteam_api.so`, `lib/libCSteamworks.so`, `lib64/libsteam_api.so` 및 `lib64/libCSteamworks.so`
            * Mac: `tModLoader.app/Contents/MacOS/osx/libsteam_api.dylib` 및 `tModLoader.app/Contents/MacOS/osx/CSteamworks`
5.  [1.4 전용] GoG 사용자는 아직 설치하지 않은 경우 Steam을 설치해야합니다. 모드 브라우저는 Steam 설치 파일 중 일부를 사용하여 Steam 창작마당에 쉽게 액세스할 수 있습니다. 이 기능을 사용하기 위해 계정이 필요하거나 로그인할 필요는 없습니다.
   1. Linux용 특별 참고 사항: Steam 설치가 인식되지 않는 경우("Steam 창작마당에 액세스할 수 없음" 오류), 터미널을 통해 APT 저장소에서 Steam을 제거한 후 다시 설치해 보세요.
6. 완료되었습니다. 이제 tModLoader에 대한 바탕 화면 바로 가기를 만들고 거기에서 tModLoader를 실행할 수 있습니다. (`start-tModLoader.bat` 파일은 게임을 시작하므로 바로 가기를 만들 수 있습니다.)

팁: 테라리아 파일의 위치를 쉽게 찾는 방법은 다음과 같습니다: ([동영상 예시](https://gfycat.com/SelfreliantAssuredIsabellineshrike))

1. Steam 게임 라이브러리에서 테라리아를 찾아 마우스 오른쪽 버튼으로 클릭하고 '속성'을 클릭합니다.
2. '로컬 파일' 탭을 찾아 '로컬 파일 찾아보기...' 버튼을 클릭합니다.
3. 이제 테라리아 폴더로 이동합니다(이 폴더에 tModLoader를 설치해야 합니다).

### 수동 설치 일반 문제
Windows 1.3만 해당: 게임이 전혀 실행되지 않는다면, .NET 4.5 또는 XNA 4.0이 설치되어 있지 않을 수 있습니다. 두 인스톨러를 모두 다운로드하여 실행하세요:
1. [Microsoft .NET Framework 4.5](https://www.microsoft.com/en-us/download/details.aspx?id=30653)
2. [Microsoft XNA 프레임워크 재배포 가능 4.0](https://www.microsoft.com/en-us/download/details.aspx?id=20914)

### 어떻게 제거하나요?

1. Steam을 열고 게임 라이브러리 섹션으로 이동하여 테라리아를 찾습니다.
2. 2. Steam **[게임 파일 무결성 확인](https://support.steampowered.com/kb_article.php?ref=2037-QEUH-3335)**을 통해 테라리아를 실행하면 게임 파일이 바닐라 버전으로 재구성됩니다.
4. 완료. 평소처럼 테라리아를 실행할 수 있습니다.

GOG를 사용하는 경우, 이전에 만든 tModLoader 폴더를 삭제하면 됩니다. 여러분의 월드와 플레이어가 저장됩니다.

## tModLoader 다운그레이드하기
1.4 tModLoader는 매달 업데이트됩니다. 때때로 사용 중인 모드가 적시에 업데이트되지 않아 최신 tModLoader 1.4 릴리스에서 작동이 중단될 수 있습니다. 이 경우 수동으로 다운그레이드할 수 있습니다. 수동으로 다운그레이드하려면 **[릴리즈](https://github.com/tModLoader/tModLoader/releases)** 페이지에서 사용하던 버전의 최신 릴리즈를 찾아서 다운로드하세요. 다운로드 파일은 `tModLoader.zip` 파일입니다. 예를 들어, 최근 `v2022.06+`로 업데이트된 tModLoader를 실행했는데 중요한 모드에서 작동이 멈췄다면, 최신 `v2022.05+` 릴리스를 찾아 다운로드하면 됩니다. 다운로드가 완료되면 tModLoader 설치 디렉토리를 열고 모든 파일을 삭제합니다. 저장 폴더가 아닌 설치 폴더에 있는지 확인하세요. 설치 디렉토리를 찾으려면 tModLoader를 마우스 오른쪽 버튼으로 클릭하고 `관리`를 클릭한 다음 `로컬 파일 찾아보기`를 클릭합니다. 이 [동영상](https://giant.gfycat.com/SoulfulImpoliteBigmouthbass.mp4)은 그 과정을 보여줍니다. 원본 파일을 삭제한 후 다운로드 한 .zip에서 파일을 가져와 설치 폴더에 넣을 수 있습니다. 이 [비디오](https://giant.gfycat.com/RashHardAmurstarfish.mp4)는이 과정을 보여줍니다. 오래된 모드가 업데이트되었음을 확인했다면, 설치 디렉터리에 있는 모든 파일을 삭제하고 Steam을 사용하여 게임 무결성을 확인하여 현재 tModLoader 릴리스로 다시 업그레이드할 수 있습니다.  

1.3을 사용하려면 tModLoader 베타 메뉴에서 '1.3-legacy'를 선택하기만 하면 됩니다: [동영상](https://giant.gfycat.com/ConsiderateClutteredBorer.mp4)

## 듀얼 인스톨 - 1.3과 1.4 tModLoader를 동시에 설치합니다.
스팀이 아닌 게임을 추가하는 스팀 기능을 활용하면 1.3과 1.4 tModLoader를 동시에 설치할 수 있습니다. 이렇게 하려면 먼저 '1.3-legacy'로 전환하고 다운로드가 완료되었는지 확인합니다. 설치 폴더를 열고 모든 파일을 복사합니다. 한 단계 위로 이동하여 tModLoader13 폴더를 만듭니다. tModLoader13 폴더로 이동하여 파일을 붙여넣습니다. Steam에서 기본 베타 브랜치인 tModLoader로 다시 전환합니다. 다음으로 `게임` 메뉴를 클릭하고 `내 라이브러리에 스팀이 아닌 게임 추가`를 클릭합니다. 찾아보기...`를 클릭하고 tModLoader13 폴더로 이동합니다(대부분 "C:\Program Files (x86)\Steam\steamapps\common\tModLoader13"에 위치할 것입니다). "tModLoader.exe"를 클릭하고 "열기"를 클릭한 다음 "선택한 프로그램 추가"를 클릭합니다. 마지막으로 라이브러리에서 두 번째 tModLoader 항목을 마우스 오른쪽 버튼으로 클릭하고 속성을 클릭한 다음 "tModLoader"를 "tModLoader 1.3"으로 변경하고 창을 닫습니다. 이제 라이브러리에 레거시 tModLoader와 자동 업데이트되는 1.4 tModLoader가 모두 있습니다. 

## 1.3에서 1.4로 모두 마이그레이션하기
대부분의 경우 1.3에서 1.4로의 전환은 깔끔하게 시작해야 합니다. 두 버전은 사실상 다른 게임이며 사용 가능한 모드가 일치하지 않습니다. 하지만 원하는 경우 기존 모드, 월드, 플레이어를 마이그레이션할 수 있습니다.
### 모드
1.3에서 사용했던 대부분의 모드가 1.4에서는 지원되지 않을 수 있습니다. "모드 다운로드" 메뉴에서 수동으로 모드를 검색하거나 모드팩 파일을 사용하여 모든 모드를 한 번에 다운로드할 수 있습니다. 먼저 1.3 저장 폴더를 열고 모드 폴더에서 enabled.json 파일을 찾습니다. 이 파일은 '\\문서\내 게임\테라리아\모드로더\모드\enabled.json'에 있을 수 있습니다. 1.4 tModLoader를 열고, `작업장`, `모드 팩`을 클릭한 다음, `모드 팩 폴더 열기`를 클릭합니다. 앞서 복사한 enabled.json 파일을 이 폴더에 붙여넣습니다. 뒤로`를 클릭한 다음 `모드 팩`을 클릭하여 메뉴를 새로 고칩니다. "활성화됨"이라는 항목이 보일 것입니다. 모드 브라우저에서 모드 보기`를 클릭한 다음 `모두 다운로드`를 클릭합니다. 모드 브라우저에서 모든 모드를 찾을 수 없다는 메시지가 표시될 가능성이 높습니다. 이 경우 목록에 있는 각 모드의 다운로드 버튼을 클릭하기만 하면 됩니다. 

### 플레이어
게임 내 메뉴를 사용해 플레이어를 마이그레이션하세요. 클라우드 플레이어는 표시되지 않으므로 복사하려면 '1.3-legacy'로 전환하고 클라우드에서 플레이어를 제거해야 합니다.

### 월드
게임 내 메뉴를 사용하여 월드를 마이그레이션하세요. 클라우드 월드는 표시되지 않으므로 복사하려면 `1.3-legacy`로 전환한 후 클라우드에서 제거해야 합니다.

## 설치했을 때 월드와 캐릭터가 사라졌어요!
**tModLoader는 바닐라 월드와 플레이어 파일을 사용하지 않습니다 **.
게임 내에서 원본 바닐라 파일을 _복사_할 수 있는 옵션이 제공될 것입니다. 이 옵션은 테라리아 1.4에서 사용했던 플레이어와 월드에서는 작동하지 않습니다.
바닐라 세이브가 수정되는 것에 대해 걱정할 필요는 없습니다. 수정된 게임플레이에 사용할 수 있도록 복사됩니다. 바닐라로 돌아가면 원래 저장한 내용을 볼 수 있습니다.

tModLoader는 플레이어(.plr) 및 월드(.wld) 파일을 저장하기 위해 별도의 폴더를 사용하는데, 이는 주로 추가 데이터를 저장하기 때문입니다. 바닐라 플레이어와 월드는 다음 폴더에 저장됩니다: '%사용자 프로필%\문서\내 게임\테라리아'(Windows의 경우)의 _플레이어_ 및 _월드_ 폴더에 각각 저장됩니다.

자동 복사가 작동하지 않는 경우, "World" 및 "Player" 폴더를 `%사용자 프로필%\문서\내 게임\테라리아`에서 `%사용자 프로필%\문서\내 게임\테라리아\tModLoader`로 복사해 주세요.

## 모드를 다운로드하고 플레이하려면 어떻게 하나요?
tModLoader는 모드 브라우저와 함께 제공됩니다. 모드 브라우저 가이드](모드 브라우저)를 참고하여 모드를 다운로드하고 플레이하는 방법을 알아보세요.

## 메모리가 너무 부족해서 여러 모드를 실행할 수 없어요!
테라리아와 1.3 tModLoader는 32비트 애플리케이션입니다. 즉, 최대 4기가바이트의 RAM까지만 사용할 수 있습니다. 모드가 많으면 메모리가 부족할 수 있습니다. 해결책은 안타깝게도 모드를 적게 사용하는 것입니다. 또는 비공식 64비트 버전의 1.3 tModLoader를 사용해 볼 수도 있습니다.

1.4 tModLoader는 기본적으로 64비트이므로 이 문제가 완화됩니다.

## macOS Catalina를 사용하고 있습니다. 어떻게 해야 하나요?
문제가 발생하면 64비트 버전의 tModLoader를 사용해보거나 Discord에서 문의해 주세요.

## 1.4에서 모드 팩을 어떻게 사용하나요?
1.4 버전에서는 모드 팩 기능을 대폭 개편하여 몇 가지 추가 기능을 제공합니다.

우선, 평소처럼 활성화된 모드를 저장하세요.
이제 UI에 많은 버튼이 표시됩니다. 이제 그 버튼들을 살펴보겠습니다.
![image](https://user-images.githubusercontent.com/59670736/180901414-3ca8f2f4-c053-4851-8de7-fa617f182db1.png)

무엇보다도, 앞으로는 두 가지 다른 용어를 사용하겠습니다: 
모드 팩은 시간이 지나도 업데이트되지 않는 고정된 모드 사본을 의미합니다.
모드 컬렉션은 항상 최신인 모드 목록을 의미합니다.

처음 두 개의 버튼은 '모드 컬렉션' 스타일로 작동합니다. 
이 목록만 활성화 - 모든 모드를 비활성화하고 컬렉션에 정의된 모드만 로드합니다.
이 목록 활성화 - 기존에 로드된 모드 위에 컬렉션에 정의된 모드를 로드합니다. 컬렉션 스택에 유용합니다.

목록 보기 및 모드 브라우저에서 모드 보기를 사용하면 팩/컬렉션의 모드가 무엇인지 확인하고 모드 브라우저에서 직접 다운로드할 수 있습니다.

활성화된 목록 업데이트 - 현재 활성화된 모드 세트로 모드 팩 또는 컬렉션을 업데이트합니다. 필요에 따라 컬렉션/팩을 삭제하거나 추가합니다.

팩 가져오기(로컬) - tModLoader가 고정된 모드 팩에서 로드할 모드 세트를 확인하도록 지시합니다. 팩에서 로드된 모든 모드는 활성화되어 있는 동안 다운로드한 기존 모드보다 우선합니다. 모드 팩이 활성화되면 오른쪽 상단에 해당 팩이 표시됩니다.

팩 제거(로컬) - 팩 가져오기(로컬)로 변경한 내용을 취소합니다.

팩 인스턴스 내보내기 - ModConfigs 및 <모드팩명>/Mods 폴더의 사본을 InstallDirectory/<모드팩명>으로 내보내서 이전 버전으로 tModLoader의 두 번째 인스턴스를 설정하거나 팩이 포함된 서버를 빠르게 설정할 수 있도록 합니다. InstallDirectory/<모드팩이름>/SaveData/Mods에서 모든 창작마당 게시 ID가 나열된 install.txt 파일과 사용할 tml 버전이 표시된 tmlversion.txt를 확인합니다.

인스턴스 삭제 - 팩 인스턴스 내보내기로 생성된 내보낸 인스턴스 파일을 삭제합니다.