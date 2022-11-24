---
sort : 1
---

# SHOW_POPUP

SHOW_POPUP 매크로는 SQL로 생성한 데이터에서 여러 값을 선택해 동적으로 대시보드를 구성할 수 있도록 기능을 제공합니다.

- [x] 먼저 매크로를 사용하기 위해 삽입 → 도형으로 이동해 원하는 도형을 하나 생성합니다.
- [x] 생성한 도형을 우클릭하여 매크로 지정 창을 팝업시킨 다음, 매크로 명에 show_popup을 입력합니다.

<img src = "https://user-images.githubusercontent.com/86198387/203720146-471057e4-4a7f-48d1-ae9b-1c2a41fdcc51.png" />

- [x] 대체 텍스트 보기를 클릭해 텍스트 입력 창을 팝업시킵니다.

<img src = "https://user-images.githubusercontent.com/86198387/203720562-7cf0703a-ca6b-472e-b3c9-578b61c68562.png" />

- [x] 명령어 목록을 참조해 매크로를 작성합니다.
```
# 명령어 목록

label = {Title} : 팝업 창의 타이틀을 설정합니다.

range = {valid range. cf) A1:A10} : 팝업 창에 참조할 셀의 범위를 설정합니다.

targetcell = {cell addr} : 팝업 창에서 선택한 결과값이 표시될 셀을 설정합니다.

multiple = TRUE / FALSE : 다중 선택을 허가할지 여부를 설정합니다. 다중선택시 값은 콤마로 
구분됩니다.

quote = TRUE / FALSE : SQL에서 동적으로 활용하기 위해 선택값에 싱글 쿼테이션 마크를 표시할지 여부를 표시합니다.
```

대체 텍스트를 작성 완료하면 아래 예시와 같이 이제 매크로를 사용할 수 있습니다.

<img src = "https://user-images.githubusercontent.com/86198387/203721779-8f593ca5-b7cc-4d3c-aa7c-5c7f02d95b02.png" />

- [x] 생성한 도형을 클릭하면 매크로 창이 팝업되며, 값을 선택해 엑셀에 동적으로 값이 나타나게 표시할 수 있습니다.