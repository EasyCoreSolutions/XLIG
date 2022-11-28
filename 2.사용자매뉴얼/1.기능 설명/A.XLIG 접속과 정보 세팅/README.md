

# XLIG 접속과 정보 세팅

XLIG에 로그인하는 방법과 버전 정보 확인 및 DB 접속 정보를 세팅하는 기능을 소개합니다.

---

## 접속

XLIG Addin을 사용하기 위해 접속하는 화면입니다. 접속서버 정보와 계정이 요구됩니다.

<img src = "https://user-images.githubusercontent.com/86198387/204178852-4a3aceda-4813-4414-b529-23e84d14c343.png"/>




- SSBI 접속서버 : SSBI에 접속하기 위해 설치된 서버의 URL을 입력합니다.
<br>

- 사용자명 : XLIG를 사용할 계정명을 입력합니다.
<br>

- 비밀번호 : 패스워드를 입력합니다.

---

## 정보 세팅

주요 업데이트 사항 확인 및 XLIG 실행 옵션, DB 접속정보를 이 화면에서 세팅합니다.

<img src = "https://user-images.githubusercontent.com/86198387/204179166-2f4dfef6-4c96-4705-99bc-28912aedfa3e.png" />

- 주요 업데이트 : XLIG의 매 버전마다 변경되는 업데이트 사항들을 확인할 수 있는 탭입니다.


<img src = "https://user-images.githubusercontent.com/86198387/204194848-2c5cf5a6-c7ce-4af8-988c-7796cada1efb.png" />

- 나의 실행옵션 : XLIG의 실행 옵션들을 설정하는 페이지입니다.
  - ssbiCountLimit : 엑셀 시트에 최대 몇 건까지 데이터를 표시할 것인지 설정합니다.
  - ssbiCountLimitWarning : 경고 메세지로 최대 몇 건까지 데이터를 표시할 수 있는지 출력할지를 설정합니다.
  - ssbiDebug : 디버그 옵션을 설정합니다.
  - ssbiLang : XLIG 사용 언어를 설정합니다.


<img src = "https://user-images.githubusercontent.com/86198387/204194992-7ee14328-37aa-4796-8300-185e79388f54.png" />

- 접속설정 : XLIG가 DB에 접속하기 위한 접속정보들을 입력하는 창입니다.
  - CONNNAME : 커넥션 이름을 의미하며 파일명으로 저장해 연결에 사용됩니다.
  - DBMS : 접속 DBMS의 종류를 입력합니다.
  - DRIVER : 해당 DB의 jdbc / odbc 드라이버를 입력합니다. 
  - URL : DB에 접속하기 위한 접속 정보를 입력합니다.
  - ID : DB에 접속할 ID를 입력합니다.
  - PW : DB 접속에 사용할 계정의 비밀번호를 입력합니다.
  - OWNER : DB 소유자 계정을 입력합니다.    

