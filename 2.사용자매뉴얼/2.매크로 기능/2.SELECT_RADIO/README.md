
# SELECT_RADIO

SELECT_RADIO 매크로는 기존 엑셀 매크로에서 제공하는 라디오 버튼과 연계해 엑셀 화면에 동적으로 값이 출력되게끔 지원하는 매크로입니다. 이를 이용해 동적으로 옵션을 선택해 대시보드를 구성할 수 있습니다.

---

- [x] 먼저 매크로를 사용하기 위해 엑셀 개발 도구 → 삽입 → 옵션 단추(양식 컨트롤)을 선택해 원하는 위치에 라디오 버튼을 생성합니다.
- [x] 생성한 버튼을 우클릭하여 매크로 지정 창을 팝업시킨 다음, 매크로 명에 select_radio를 입력합니다.

<img src = "https://user-images.githubusercontent.com/86198387/203723053-bc6affdc-b702-4525-88c6-ce5331b431f8.png"/>


- [x] 라디오 버튼을 우클릭해 컨트롤 서식을 선택합니다.

<img src = "https://user-images.githubusercontent.com/86198387/203723783-263d1a3a-7df8-43aa-b64c-4e1a71861c8d.png" />

- [x] 대체 텍스트 탭을 선택한 다음, 명령어 목록을 참조해 매크로를 작성합니다.

```
# 명령어 목록

value = {value} : 라디오 버튼을 선택하면 표시될 값을 입력합니다.

targetcell = {cell addr} : 결과값이 표시될 셀을 설정합니다.
```

대체 텍스트를 작성 완료하면 아래 예시와 같이 이제 매크로를 사용할 수 있습니다.

<img src = "https://user-images.githubusercontent.com/86198387/204197603-1df66dd4-cfa4-4eb9-8bd6-6ac16c9d6ce1.png"/>

- [x] 라디오 버튼을 선택하면 매크로에 입력한 값이 셀에 표시됩니다.

<img src = "https://user-images.githubusercontent.com/86198387/203724365-36e8102c-ec49-433a-ba42-68006395bddb.png" />

- [x] 여러개의 라디오 버튼을 생성해 targetcell을 동일한 위치에 지정하면, 선택에 따라 값이 변하는 옵션 선택이 구현 가능합니다.