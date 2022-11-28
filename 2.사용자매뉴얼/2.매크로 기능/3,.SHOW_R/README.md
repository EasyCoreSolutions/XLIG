
# SHOW_R

SHOW_R 매크로는 엑셀 환경에서 R을 사용할 수 있도록 지원하는 매크로입니다. 사용자가 R에서 가져올 데이터 셋 명이나, 활용할 필드값을 동적으로 선택할 수 있도록 구현됩니다.

---

- [x] 먼저 매크로를 사용하기 위해 삽입 → 도형으로 이동해 원하는 도형을 하나 생성합니다.
- [x] 생성한 도형을 우클릭하여 매크로 지정 창을 팝업시킨 다음, 매크로 명에 show_r을 입력합니다.

<img src = "https://user-images.githubusercontent.com/86198387/203725049-d51639ba-f327-4e08-856e-9e22ca03259b.png"/>

- [x] 대체 텍스트 보기를 클릭해 텍스트 입력 창을 팝업시킵니다.

<img src = "https://user-images.githubusercontent.com/86198387/203720562-7cf0703a-ca6b-472e-b3c9-578b61c68562.png" />

- [x] 명령어 목록을 참조해 매크로를 작성합니다.

```

# 명령어 목록

label = {Title} : 팝업 창의 타이틀을 설정합니다.

show_rdf = TRUE/FALSE : R 데이터셋을 보여줄지 여부를 선택합니다.

한 매크로에서 아래의 show_field와 동시에 TRUE로 활용할 수 없습니다.

show_fields = TRUE/FALSE : R 필드를 보여줄지 여부를 선택합니다.

rdfcell = {cell addr} : show_fields 옵션을 사용하기 위해 참조할 R 데이터셋의 이름이 포함된 셀값을 입력합니다.

targetcell = {cell addr} : 결과값이 표시될 셀을 설정합니다.

multiple = TRUE / FALSE : 다중 선택을 허가할지 여부를 설정합니다. 다중선택시 값은 콤마로 구분됩니다.

clear = {cell1,cell2...} : 해당 셀의 값을 초기화합니다. 콤마로 구분됩니다.

```

대체 텍스트를 작성 완료하면 아래 예시와 같이 이제 매크로를 사용할 수 있습니다.

<img src = "https://user-images.githubusercontent.com/86198387/204198023-b9133b0f-3a52-4271-8dd1-5ee49b9511d5.png"/>

- [x] R에 저장된 데이터셋중 사용자가 엑셀에 사용하기를 원하는 데이터셋을 선택해, 해당 데이터셋의 이름을 셀에 표시하고 코드 작성에 해당 셀값을 참조할 수 있습니다.

- [x] 대체 텍스트 옵션으로 show_fields를 사용하면 직전 사용한 매크로와 연계해 필드명도 불러올 수 있습니다.

<img src = "https://user-images.githubusercontent.com/86198387/204198215-bc881a0a-8bd3-44f7-90e4-621e936dc24b.png"/>

- [x] 위와 같이 데이터셋과 필드명을 모두 불러오면, XLIG의 코드 실행 기능에서 코드를 작성할 때 R의 데이터셋도 사용해 코드를 작성할 수 있습니다.