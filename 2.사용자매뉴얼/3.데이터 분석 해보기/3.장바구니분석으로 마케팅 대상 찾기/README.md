
# 장바구니분석으로 마케팅 대상 찾기


&nbsp;2월과 3월의 고객수 대시보드에서 성별이나 연령, 고객등급보다는 옷의 종류에서 변화가 보였기 때문에 고객수를 분석해보기 위해 연관분석 대시보드를 만들어보겠습니다.

---

많이 팔린 10개의 옷의 종류, 함께 구매한 옷에 대한 히트맵, 그리고 장바구니분석을 통해 지지도, 신뢰도, 향상도를 구해 마케팅 대상자들을 찾아보겠습니다.

<h3>1) 데이터 가져오기</h3>
<h5>(1) 히트맵 데이터 가져오기</h5>
많이 팔린 10개의 옷의 종류는 기존의 데이터에서 구할 수 있기때문에 히트맵에 필요한 쿼리만 새로 작성하여 pivotmaster시트에 가져오도록 하겠습니다.
<img src="https://user-images.githubusercontent.com/57983744/203888283-5bb30db7-1042-44dc-9cfd-3a8d19a29ee8.png">
<br>
새로운 탭에 히트맵 데이터를 가져오기 위한 쿼리를 작성하고 
<img src="https://user-images.githubusercontent.com/57983744/204177658-85042eee-1240-4d7e-ba58-2652b894a06d.png">
히트맵 데이터를 pivotmaster 시트에 가져옵니다.
<h5>(2) R데이터프레임 만들기</h5>

<img src="https://user-images.githubusercontent.com/57983744/203899615-f044dc69-362a-49da-aab7-6a0c25f6222d.png"><br>
장바구니 분석을 하기 위해 필요한 데이터셋을 만들기 위한 쿼리를 입력하고 rdf1 이름으로 데이터프레임을 만들겠습니다.<br>
<br>
<h3>2) 장바구니 분석하기</h3>
<img src="https://user-images.githubusercontent.com/57983744/203901007-af4e6d0c-4f02-414c-8e36-ab85406c4009.png"><br>
rdf1데이터셋으로 장바구니 분석을 하는 R코드를 입력하여 rdf1시트에 출력합니다.
데이터셋을 logical형태로 변환하고 apriori 라이브러리를 이용해 지지도가 0.1, 신뢰도가 0.6 이상인 항목들을 향상도 순으로 출력하는 R코드를 입력하였습니다.
<br><br>
<img src="https://user-images.githubusercontent.com/57983744/203901118-21eed28b-d401-4bfc-9c29-90c160d1273f.png">
<br>
코드의 결과가 rdf1시트에 표시됩니다.
<br><br>
<img src="https://user-images.githubusercontent.com/57983744/203902177-a23d4365-9ac6-4450-a2b1-42215fb5ab6e.png">
<br>
해당 값을 엑셀 함수를 활용하여 필요한 데이터(좌측변수, 우측변수, 지지도, 신뢰도, 향상도)를 추출해줍니다.
<br><br>
<h3>3) 대시보드 구성하기</h3>
<br>
<img src="https://user-images.githubusercontent.com/57983744/204194602-197f0c54-cfd1-43d9-b02f-2913e12b5a33.png">
<br>
<img src="https://user-images.githubusercontent.com/57983744/203902387-5dcf9efb-7d8b-4ead-89d9-276c148f493c.png">
<br>
앞에서 pivotmaster 시트에 구해놓은 히트맵 데이터와 장바구니 분석 결과를 활용하여 연관분석 대시보드(Dashboard2)를 구성합니다.<br>
좌측 상단에는 많이 팔린 10개의 품목을 순서대로 보여주는 표, 우측 상단에는 한 고객이 함께 구매한 옷의 종류에 대한 히트맵, <br>하단에는 장바구니분석의 결과인 지지도, 신뢰도, 향상도 및 그에 따른 추천 마케팅을 표시하였습니다.
<br><br>
<img src="https://user-images.githubusercontent.com/57983744/203902546-e138bb59-2a1a-4a69-a1fe-e81b29630553.png">
<br>
장바구니 분석 또한 사용자가 원하는 기준으로 분석 할 수 있게끔 변수를 입력할 수 있는 곳이 필요합니다.<br>대시보드 좌측에 기준이 되는 지지도와 신뢰도를 입력할 수 있는 곳을 만들어주겠습니다. 
<br><br>
<img src="https://user-images.githubusercontent.com/57983744/203910470-27657c68-5f8e-482c-a810-cdbfe3ae0e37.png">
<br>
해당 입력값을 반영할 수 있도록 R코드를 수정합니다.<br><br>
<img src="https://user-images.githubusercontent.com/57983744/203902615-f900dedc-6060-4051-b8da-4d9226e2fcff.png">
<br>
기준 지지도를 0.1에서 0.08로 조금 낮춰 5개의 항목이 나타나게 대시보드가 변경되었습니다.
<br><br>
<img src="https://user-images.githubusercontent.com/57983744/203902734-f9c2cb35-3209-4acc-b3fa-63d43c619fcd.png">
<br>
이전의 대시보드로 이동할 수 있는 화살표 버튼을 만들고 모든 프로젝트를 실행 할 수 있는 버튼을 만들어 매출대시보드와 연관분석 대시보드를 완성하였습니다.
<br><br><br>
<a href="/XLIG/2.사용자매뉴얼/3.데이터 분석 해보기/2.대시보드 살펴보기/">(이전) 대시보드 살펴보기</a>