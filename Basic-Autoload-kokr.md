***
이 가이드가 1.4로 업데이트되었습니다. 이 위키 페이지의 이전 1.3 버전을 보려면 [여기](https://github.com/tModLoader/tModLoader/wiki/Basic-Autoload/26fd108fd8ec4948a8134ffa35c3ee6bb90a1e7c)를 클릭하세요.
***

오토로드(AutoLoad)는 모드로더가 모드에서 해야 하는 작업을 화하기 위해 tModLoader가 여러분의 모드에 대해 몇 가지 가정을 하는 메커니즘입니다. 이 기본 가이드에서는 텍스처가 오토로드 되는 방식에 초점을 맞추는데, 이는 초보 모더의 주요 관심사이기 때문입니다.

아이템, 투사체, NPC는 모두 텍스처가 있어야 제대로 작동합니다. 이 중 하나라도 텍스처가 없으면 모드를 로드할 수 없습니다. tModLoader는 어떤 텍스처 파일을 어떤 사물에 연결할지 마술처럼 알 수는 없지만, 알고 있는 패턴을 기반으로 추측할 수 있습니다.

이것이 그 패턴입니다:

모드 소스 경로를 가져옵니다: 
`C:\Documents\My Games\Terraria\tModLoader\ModSources`

클래스의 네임스페이스를 추가하고 '.'를 '\\'로 바꿉니다: 
`namespace ExampleMod.NPCs`
`C:\Documents\My Games\Terraria\tModLoader\ModSources\ExampleMod\NPCs`

클래스의 클래스명을 추가한 다음 '.png'를 추가합니다:
`public class FlutterSlime : ModNPC`
`C:\Documents\My Games\Terraria\tModLoader\ModSources\ExampleMod\NPCs\FlutterSlime.png`

좀 더 간결하게 설명하자면, 클래스의 네임스페이스에 대응되는 폴더에서 해당 클래스의 클래스명과 일치하는 파일명을 가진 .png 파일을 찾으면 됩니다.

기본적으로 .cs 파일과 .png 파일이 같은 디렉터리에 있는지, 그리고 .cs 파일의 클래스가 .png 파일의 파일명과 일치하는지 확인하면 됩니다. 또한 .cs 파일의 파일명이 클래스 이름과 .png 파일 이름과도 일치하는지 확인하는 것이 좋습니다. 또한 Mod Sources 폴더의 폴더 경로가 네임스페이스와 일치하는지 확인해야 합니다( 
'.'는 폴더 구조에서 '\\'가 됩니다).

시각적 설명입니다:  
![](https://i.imgur.com/9z50wHh.png)