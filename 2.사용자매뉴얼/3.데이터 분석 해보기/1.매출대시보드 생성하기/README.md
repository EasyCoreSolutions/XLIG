
# 매출대시보드 생성하기


&nbsp;마케터가 되었다고 가정하고 데이터 분석을 진행해보겠습니다.<br>
먼저 효과적이고 효율적인 마케팅을 할 수 있도록 매출현황을 보고 싶습니다. 그리고 매출현황을 보고 새로운 인사이트를 찾아내고 마지막으로 찾아낸 인사이트를 바탕으로 마케팅 대상자들을 추출해내고 싶습니다.<br>
앞서 말한 3가지를 XLIG를 활용하여 구현해보겠습니다.

---

<h3>1) 데이터 가져오기</h3>
<h5>(1) 쿼리 작성</h5>
<div align=center>
<img src="https://user-images.githubusercontent.com/57983744/204941620-8bcccaea-509f-43cc-b8f4-b259e72b4557.png"></div>
&nbsp;분석할 데이터가 있는 데이터베이스의 접속정보를 선택하고 데이터를 불러오는 쿼리를 입력합니다.<br>
차트만들기에 좋은 피벗테이블로 결과유형을 선택하고 피벗테이블을 모두 모아놓을 시트 'pivotmaster'에 데이터를 불러오겠습니다.
<h5>(2) 데이터 선택</h5>
<div align=center>
<img src="https://user-images.githubusercontent.com/57983744/204941686-27ab1ac4-aa86-4331-a0c8-681159d30bb1.png"></div>
&nbsp;대시보드에 생성할 차트의 수만큼 피벗테이블을 복사한뒤 각 피벗테이블들을 만들어질 차트에 알맞은 데이터를 선택해줍니다.
<h3>2) 차트 만들기</h3>
<h5>(1) 차트 삽입하기</h5>
<div align=center>
<img src="https://user-images.githubusercontent.com/57983744/204941728-83f07072-4700-47ad-b325-e50e1ebd2709.png"></div>
&nbsp;피벗테이블을 기반으로 대시보드 시트에 차트를 만들어주겠습니다.<br>
피벗테이블을 선택하여 [삽입]-[피벗차트] 에서 데이터에 따라 적당한 차트를 선택하여 대시보드 시트에 차트를 삽입합니다.
<h5>(2) 대시보드 구성하기</h5>
<div align=center>
<img src="https://user-images.githubusercontent.com/57983744/204941730-4e28feb4-38cb-4562-9c50-6f0b5e2d5db9.png"></div>
대시보드의 틀을 만들고 삽입한 차트들을 보기좋게 수정한 후 위치를 배치하여 대시보드를 구성합니다.
<h3>3) 사이드바 만들기</h3>
&nbsp;하나의 대시보드에서 다양한 관점의 데이터를 보기 위해 데이터의 필터가 필요합니다.<br>앞서 설명했던 show_popup 매크로를 이용하여 데이터 필터 버튼을 만들겠습니다.
<a href="/XLIG/2.사용자매뉴얼/2.매크로 기능/1.SHOW_POPUP/">매크로 사용법 보기</a>
<h5>(1) 팝업선택시트 만들기</h5>
<div align=center>
<img src="https://user-images.githubusercontent.com/57983744/203703963-58d46055-8905-4d26-a43f-53b617dfdb88.png"></div>
&nbsp;팝업시트에 필요한 데이터를 불러오는 쿼리 5개를 작성하여 commcode 시트에 불러오겠습니다.<br>
<div align=center>
<img src="https://user-images.githubusercontent.com/57983744/203704399-854ff033-6d53-421f-8361-b698b0e4a3a0.png"></div>
<h5>(2) 버튼 만들기</h5>
<img src="https://user-images.githubusercontent.com/57983744/203706855-610d244a-0a84-4d7d-9a24-ace64c0089ee.png"><br>
&nbsp;[삽입]->[도형] 에서 버튼에 적합한 도형을 선택하여 삽입하고 [우클릭]->[매크로지정] 을 통해 show_popup 매크로를 선택한 뒤 대체 텍스트를 입력해줍니다.
range에는 commcode 시트에 불러왔던 데이터의 범위를 지정하여주고 targetcell 에는 pivotmaster 시트의 빈 공간을 지정하여 줍니다. 다른 값들도 데이터에 따라 적절하게 설정해준 후 버튼을 대시보드에 배치하여줍니다.
<div align=center>
<img src="https://user-images.githubusercontent.com/57983744/204941734-ef49a6a7-426e-4b51-a36e-b742b15071e6.png"></div>
<h5>(3) 쿼리 수정</h5>
<div align=center>
<img src="https://user-images.githubusercontent.com/57983744/204941735-90c14eef-0e40-4f5d-9ee6-b406cd6da0ba.png"></div>
&nbsp;데이터를 불러오는 쿼리의 WHERE 절에 targetcell의 값들을 불러오는 값들을 입력해주면 버튼을 통해 선택하는 값에 따라 변경되는 쿼리가 완성됩니다.<br>
<a href="/XLIG/2.사용자매뉴얼/2.매크로 기능/4.매크로 활용/">코드에 적용하기</a><br><br>
<img src="https://user-images.githubusercontent.com/57983744/203720623-8734d125-ff07-4541-b5f9-3ecea08d6f1a.png"><br>
GRADE버튼을 클릭하여 휴면등급을 선택에서 제외하고 프로젝트를 실행한다면<br><br>
<div align=center>
<img src="https://user-images.githubusercontent.com/57983744/204941739-fbb4b7d9-ead8-4f29-a07d-7790d113a1fe.png"></div>
대시보드도 휴면상태의 고객이 제외된 대시보드로 변경된 것을 볼 수 있습니다.
<br><br><br>
<a href="/XLIG/2.사용자매뉴얼/3.데이터 분석 해보기/2.대시보드 살펴보기/">(다음) 대시보드 살펴보기</a>