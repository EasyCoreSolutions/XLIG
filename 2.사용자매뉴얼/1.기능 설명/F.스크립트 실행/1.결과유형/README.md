
# 결과 유형
<style>
img{
image-rendering: -webkit-optimize-contrast;
transform: translateZ(0); 
backface-visibility: hidden;
}
</style>

<h3>1. 리스트(50줄페이징)</h3>

<div align=center> <img src="https://user-images.githubusercontent.com/57983744/207800372-c5afeb21-b00e-40f5-ac54-417a79bc3e49.png"></div>
&nbsp;코드의 결과값을 50줄 당 ( [1] 참고 ) 1페이지로 잘라서 테이블로 표현합니다.<br>
엑셀의 최대 로우수는 1,048,576개 입니다. 데이터의 양이 많아지면서 이보다 많은 데이터가 존재 할 때가 자주 있습니다. 이런 경우 엑셀에서의 작업환경이 제약이 생길 수 있는데 엑셀에서도 표현할 수 있도록 페이징( [2] 참고 ) 처리되어 출력됩니다.
<br><br><br>
<h3>2. 피벗테이블</h3>
<div align=center><img src="https://user-images.githubusercontent.com/57983744/207801674-76eab357-2a61-471e-a79d-6b0ceb616802.png"></div>
&nbsp;피벗테이블은 함수 없이 다양한 변수를 조합하고 계산하며 요약할 수 있는 좋은 기능입니다.<br>
코드의 결과값을 번거로운 과정 없이 원클릭으로 즉시 피벗테이블을 생성 할 수 있습니다.
<br><br><br>
<h3>3. 리스트테이블</h3>
<div align=center><img src="https://user-images.githubusercontent.com/57983744/207802448-4122e04f-279d-4a8d-bb2c-59b56e6200a1.png"></div>
&nbsp;코드의 결과값 전체를 테이블 형태로 시트에 출력합니다.<br>
테이블은 데이터를 쉽게 조작하고 분석할 수 있도록 정렬 및 필터가 생성되어 있고 계산 및 디자인을 쉽게 할 수 있습니다.
<br><br><br>
<h3>4. 리스트</h3>
<div align=center><img src="https://user-images.githubusercontent.com/57983744/207803523-0149253a-659a-481a-a55f-3560127b34d1.png"></div>
&nbsp;코드의 결과값을 변형없이 그대로 시트에 출력합니다.
<br><br><br>
<h3>5. 워크로 저장</h3>
<div align=center><img src="https://user-images.githubusercontent.com/57983744/203675574-688c63e7-2e1c-490b-8f51-598f13b85764.png"></div>
&nbsp;코드의 결과값을 클릭하우스 데이터베이스에 테이블로 추가합니다.<br>
Import를 성공했다는 메시지와 함께 테이블/필드 속성에서 클릭하우스에 워크테이블의 
이름( [1] 참고 ) 으로 테이블이 생성된 것을 확인 할 수 있습니다.<br>
클릭하우스는 컬럼기반의 데이터베이스로서 빠른 처리속도를 자랑합니다.
<br><br><br>
<h3>6. 결과 > 통계정보</h3>
<div align=center><img src="https://user-images.githubusercontent.com/57983744/203675890-344ad626-1d02-4845-b955-76022826ad22.png"></div>
&nbsp;코드의 결과값의 기술 통계정보를 표현합니다.<br>
칼럼 별 타입, 개수, 합계, 최대값, 최소값, 평균값, 범위, NULL값의 개수, BLANK의 개수와 같이 자료의 특성을 나타내는 값들을 보여줍니다.
<br><br><br>
<h3>7. 내려받기</h3>
<img src="https://user-images.githubusercontent.com/57983744/207806695-9b3e37a2-052e-49bf-a76d-f206abbed646.png">
<br>
<img src="https://user-images.githubusercontent.com/57983744/207807114-86476821-a5f1-422b-8612-1c35639a722c.png"><br>
&nbsp;코드의 결과를 CSV 파일로 저장합니다.<br>
저장한 파일은 SSBI-XLIG-> webapps -> dataqueryConfig -> Directories -> unload -> {계정} 폴더 에 저장됩니다.
