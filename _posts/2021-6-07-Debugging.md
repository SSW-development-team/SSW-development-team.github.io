---
title: "Debugging for Korean"
date: 2021-06-07 -0400
categories: Debug
---

디버깅
==

# 목차

* 개요
* 테스트
* 기본 로그 파일
* 크래시 로그
* 에러로그 분석
    1. 일반적 오류
    2. 커먼 폴더
    3. 히스토리 폴더
    4. GFX폴더
    5. 인터페이스 폴더
    6. 로컬라이제이션 폴더
    7. 맵 폴더
* 기록되지 않는 일반적 오류들
    1. 일반적 오류
    2. 이벤트 폴더
    3. 히스토리 폴더
    4. GFX 폴더
    5. 로컬라이제이션 폴더
    6. 맵 폴더
---------------------------------------
# 개요
일반적인 에러는 2가지가 존재합니다. 치명적이거나 치명적이지 않거나.

치명적 에러는 게임의 중요한 데이터를 불러올수 없거나, 데이터가 없어서 작동할수 없을때 발생합니다.
이로 인해서 데스크톱 (CTD)에서 충돌이 발생합니다. 치명적 에러는 **exceptions.log**에 저장됩니다. 일반적으로 이러한 오류는 로딩 과정에서 발생하거나 특정 행동을 할때 발생합니다.

치명적이지 않은 오류는 게임이 유효하지 않은 데이터나 코드를 받았을때 발생합니다. 이러한 에러들은 **error.log**에 저장됩니다.

---------------------------------------
# 테스트
테스트를 하는 것은 모드 오류 수정에 필수적입니다. 일반적으로 테스트는 게임 내의 콘솔을 사용합니다.

콘솔 커맨드 | 메모
---|:---:
reloadfx all|전장의 안개, HDR, 국경등 게임에 사용되는 효과의 대부분을 다시 불러옵니다.
reload texture|지도자 초상화, 연구 아이콘 등 게임에 사용되는 텍스처의 대부분을 다시 불러옵니다.
reload localization|이벤트 제목, 설명등 게임 내에서 사용되는 텍스트의 대부분을 다시 불러옵니다.
reload defines|common/defines에 존재하는 게임 핵심정의파일을 다시 불러옵니다.
reload focus|common/national_focuses에 존재하는 중점계통도를 다시 불러옵니다.
reloadoob|지정된 태그의 OOB 파일을 다시 불러옵니다.
reloadtechnologies|연구 파일을 다시 불러옵니다. 오류가 존재할경우 게임이 강제종료됩니다.
reloadinterface|인터페이스 파일 (.gui)을 다시 불러옵니다.
tdebug|주ID, 프로빈스 ID등의 정보를 표시하는 디버그 툴팁을 활성화합니다.
event|현재 플레이어가 조종하는 국가에 지정된 이벤트를 즉시 실행합니다.
nocb|외교에 대한 제한을 없앱니다.
observe|플레이어를 관찰자 모드로 전환하여 AI가 조종하도록 전환합니다.
aiview|기술등의 버튼위에 마우스를 올릴때, AI의 우선순의를 표시합니다.
tag|플레이어를 특정태그의 국가로 이동시킵니다.
update_loc|특정한 로컬라이제이션 키를 다시 불러옵니다.
updateequipments|common/units/equipment/의 장비 파일을 다시 불러옵니다.
updatesubunits|common/units의 유닛 파일을 다시 불러옵니다.
research_on_icon_click|연구 아이콘을 클릭시 즉시 완료됩니다.
Focus.NoChecks|중점에 대한 조건 제거
Focus.AutoComplete|중점을 즉시 완료합니다.
set_country_flag|현재 국가에 대한 플래그를 설정합니다.
add_ideas|지정한 국민정신을 현재 국가에 바로 추가합니다.

---------------------------------------
# 로그 파일
게임에서는 다양한 로그 파일을 **//Documents/Paradox Interactive/Hearts of Iron IV/logs/**(윈도우) 에 저장합니다. 또한, 이 로그파일들은 게임이 시작할때마다 덮어씌워집니다.

전체 오류를 확인하려면, 스팀의 시작 옵션에 **-debug**를 추가하세요

파일명 | 설명 | 유용성
---|:---:|:---:
ai.log|AI의 선택지 목록을 표시합니다.|중간
ai_trace.log|AI의 숨겨진 움직임을 표시합니다.|낮음
error.log|치명적이지 않은 다양한 오류를 표시합니다. common폴더에 있는 파일과 관련된 모든 오류를 수정해야 하지만, 많은 오류는 무시할수 있습니다.|높음
exceptions.log|게임에 치명적 오류 발생시의 기록을 보여줍니다.|낮음
executedcommands.log|플레이어와 AI가 사용한 내부 명령을 표시합니다.|낮음
game.log|게임 내부의 국가들의 선택지를 표시합니다. 특정 동작으로 인해 충돌이 발생할때 유용합니다.|높음
graphics.log|위치, 강, 나무와 같은 그래픽 오류들을 표시합니다.|낮음
memory.log|로딩시의 메모리를 보여줍니다. 게임이 충돌하여 강제종료될때 유용합니다.|높음
message.log|현재 세션에 대한 정보를 보여줍니다.|낮음
postedcommands.log| |낮음
random.log|게임시간의 변화를 보여줍니다.|낮음
receivedcommands.log|플레이어가 멀티플레이에서 받은 내부 명령어를 표시합니다.|낮음
sentcommands.log|플레이어가 멀티플레이에서 보낸 내부 명령어를 표시합니다.|낮음
setup.log|프로세스의 각 부분에 대한 로딩완료를 표시합니다. 충돌을 유발할수 있는 파일을 발견하는데 유용합니다.|높음
system.log|호이4가 실행된 시스템 정보 표시.|낮음
system_debug.log|인터페이스 오류 표시.|중간
text.log|로컬라이제이션 오류 표시.|중간
time.log|로딩이 완료되는데 걸리는 시간을 표시합니다.|중간

---------------------------------------
# 충돌 데이터 로그
게임이 충돌할때 원인이 무엇인지에 대한 정보를 제공합니다. **Documents/Paradox Interactive/Hearts of Iron IV/crashes** 대부분의 경우에서 이것은 별로 쓸모 없지만 단서가 존재할수 있습니다.

스팀 실행시에 **-crash-data_log**를 추가하여 meta.yml을 저장하면, 오류가 나기전 마지막으로 인식한 코드를 알수 있습니다. 하지만 마지막의 바로 다음 코드에서 게임이 충돌했을 가능성이 높기에 찾기는 어렵습니다.

---------------------------------------
# 기록된 오류 목록
오류 로그에 있는 것들은 자세하지 않으니 설명하겠습니다.

1. 일반적 오류
    * Unexpected token - 코드가 예상치 못한 곳에 있는 경우입니다. 일반적으로 불필요한 괄호나 필요한 괄호의 부재, 혹은 오탈자로 인한 잘못된 구문입니다. 문법이 정확한지 확인해야 합니다. 텍스트 에디터들은 괄호 강조를 제공하므로, 강조가 있는 에디터를 사용하는 것은 상당히 유용합니다. 때로는 인코딩이 원인일수도 있습니다.

    * Malformed token - 지졍된 코드가 아닌 완전히 잘못된 코드입니다. 예를 들어, 존재하지 않는 트레잇이나 국가간 의견을 추가했을수도 있고, 혹은 단순히 오탈자 일수도 있습니다. 인코딩이 원인일수도 있습니다. 이벤트 파일의 경우, **add_namespace**행이 누락되어 생긴 문제일수 있습니다.

    * Invalid trigger/effect/modifier - 이름에 오타가 있을 가능성이 높습니다. 트리거/효과/모디파이어가 아닌 경우 코드를 다시 확인해주세요. 이러한 오류는 예전에 생성하고 변경한 scripted triggers, scripted effects 혹은 커스텀 모디파이어일수 있습니다.

    * Unknown trigger/effect-type - 바로 이전 항목과 유사하지만, 직접적인 효과가 아닌 괄호를 가진 트리거나 효과가 존재하는 경우를 확인해주세요. 괄호와 오타를 확인해주세요.
2. 커먼 폴더
    * invalid focus for ai strategy - **Hearts of Iron IV/common/ai_strategy_plans/*.txt**에 잘못된 국가중점이 입력되었을때 생성됩니다. 문제가 있는 파일을 완전히 비우거나 경로를 바꾸면 해결됩니다.

    * TAG is not in the tag list - **Hearts of Iron IV/common/country_tags**에 국가 태그가 등록되지 않았지만 **Hearts of Iron IV/common/difficulty_settings**이나 **Hearts of Iron IV/history/countries**에서 사욛되고 있을때 생성됩니다.
3. 히스토리 폴더
    * TAG is missing a history file - 태그가 **Hearts of Iron IV/common/country_tags**에는 정의되었지만, **Hearts of Iron IV/history/countries**에는 정의되지 않은 경우입니다.

    * Definition for state id is either missing or invalid - **Hearts of Iron IV/history/states** 주 번호가 순서가 맞지 않아서 생기는 문제입니다.

    * State ID conflict - 두 주의 ID가 동일합니다. replace_path가 존재하지 않는 한은 파일 이름을 변경하지 마세요, 기본 게임 파일과 모드 파일이 모두 로드됩니다.
    
    * Country does not have any equipment variant for type - 맨더건 DLC가 켜져있는 상태에서, **Hearts of Iron IV/history/countries**에 장비 변종이 저장되지 않았을 경우에 생기는 문제입니다.
4. GFX폴더
5. 인터페이스 폴더
6. 로컬라이제이션 폴더
7. 맵 폴더