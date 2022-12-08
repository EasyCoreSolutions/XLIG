
# 장바구니분석으로 마케팅 대상 찾기


&nbsp;2월과 3월의 고객수 대시보드에서 성별이나 연령, 고객등급보다는 옷의 종류에서 변화가 보였기 때문에 고객수를 분석해보기 위해 연관분석 대시보드를 만들어보겠습니다.

---

많이 팔린 10개의 옷의 종류, 함께 구매한 옷에 대한 히트맵, 그리고 장바구니분석을 통해 지지도, 신뢰도, 향상도를 구해 마케팅 대상자들을 찾아보겠습니다.

<h3>1) 데이터 가져오기</h3>
<h5>(1) 히트맵 데이터 가져오기</h5>
많이 팔린 10개의 옷의 종류는 기존의 데이터에서 구할 수 있기때문에 히트맵에 필요한 쿼리만 새로 작성하여 pivotmaster시트에 가져오도록 하겠습니다.
<img src="https://user-images.githubusercontent.com/57983744/204942655-42309db2-3782-4dc8-a2a4-61d3d2820d31.png">
<br>
<details>
<summary> Sample 코드 접기 / 펼치기 </summary>

```

  SELECT ITEM
      , COUNT(DISTINCT CASE WHEN CUST_ID IN (SELECT CUST_ID FROM crm_mart_hj.sample WHERE ITEM = 'ACC' GROUP BY CUST_ID) THEN CUST_ID ELSE NULL END) AS ACC
      , COUNT(DISTINCT CASE WHEN CUST_ID IN (SELECT CUST_ID FROM crm_mart_hj.sample WHERE ITEM = '기타' GROUP BY CUST_ID) THEN CUST_ID ELSE NULL END) AS 기타
      , COUNT(DISTINCT CASE WHEN CUST_ID IN (SELECT CUST_ID FROM crm_mart_hj.sample WHERE ITEM = '다운' GROUP BY CUST_ID) THEN CUST_ID ELSE NULL END) AS 다운
      , COUNT(DISTINCT CASE WHEN CUST_ID IN (SELECT CUST_ID FROM crm_mart_hj.sample WHERE ITEM = '데님' GROUP BY CUST_ID) THEN CUST_ID ELSE NULL END) AS 데님
      , COUNT(DISTINCT CASE WHEN CUST_ID IN (SELECT CUST_ID FROM crm_mart_hj.sample WHERE ITEM = '바지' GROUP BY CUST_ID) THEN CUST_ID ELSE NULL END) AS 바지
      , COUNT(DISTINCT CASE WHEN CUST_ID IN (SELECT CUST_ID FROM crm_mart_hj.sample WHERE ITEM = '반바지' GROUP BY CUST_ID) THEN CUST_ID ELSE NULL END) AS 반바지
      , COUNT(DISTINCT CASE WHEN CUST_ID IN (SELECT CUST_ID FROM crm_mart_hj.sample WHERE ITEM = '블라우스' GROUP BY CUST_ID) THEN CUST_ID ELSE NULL END) AS 블라우스
      , COUNT(DISTINCT CASE WHEN CUST_ID IN (SELECT CUST_ID FROM crm_mart_hj.sample WHERE ITEM = '스웨터' GROUP BY CUST_ID) THEN CUST_ID ELSE NULL END) AS 스웨터
      , COUNT(DISTINCT CASE WHEN CUST_ID IN (SELECT CUST_ID FROM crm_mart_hj.sample WHERE ITEM = '우븐셔츠' GROUP BY CUST_ID) THEN CUST_ID ELSE NULL END) AS 우븐셔츠
      , COUNT(DISTINCT CASE WHEN CUST_ID IN (SELECT CUST_ID FROM crm_mart_hj.sample WHERE ITEM = '우븐조끼' GROUP BY CUST_ID) THEN CUST_ID ELSE NULL END) AS 우븐조끼
      , COUNT(DISTINCT CASE WHEN CUST_ID IN (SELECT CUST_ID FROM crm_mart_hj.sample WHERE ITEM = '원피스' GROUP BY CUST_ID) THEN CUST_ID ELSE NULL END) AS 원피스
      , COUNT(DISTINCT CASE WHEN CUST_ID IN (SELECT CUST_ID FROM crm_mart_hj.sample WHERE ITEM = '자켓' GROUP BY CUST_ID) THEN CUST_ID ELSE NULL END) AS 자켓
      , COUNT(DISTINCT CASE WHEN CUST_ID IN (SELECT CUST_ID FROM crm_mart_hj.sample WHERE ITEM = '점퍼' GROUP BY CUST_ID) THEN CUST_ID ELSE NULL END) AS 점퍼
      , COUNT(DISTINCT CASE WHEN CUST_ID IN (SELECT CUST_ID FROM crm_mart_hj.sample WHERE ITEM = '코트' GROUP BY CUST_ID) THEN CUST_ID ELSE NULL END) AS 코트
      , COUNT(DISTINCT CASE WHEN CUST_ID IN (SELECT CUST_ID FROM crm_mart_hj.sample WHERE ITEM = '특종' GROUP BY CUST_ID) THEN CUST_ID ELSE NULL END) AS 특종
      , COUNT(DISTINCT CASE WHEN CUST_ID IN (SELECT CUST_ID FROM crm_mart_hj.sample WHERE ITEM = '티셔츠' GROUP BY CUST_ID) THEN CUST_ID ELSE NULL END) AS 티셔츠
   FROM crm_mart_hj.sample
  where 1=1
 [and GENDER in ($$pivotmaster!B1$$)]
 [and AGE in ($$pivotmaster!C1$$)]
 [and GRADE in ($$pivotmaster!D1$$)]
 [and ITEM in ($$pivotmaster!E1$$)]
 [and SALE_DT in ($$pivotmaster!F1$$)]
  GROUP BY ITEM

```

</details><br>
새로운 탭에 히트맵 데이터를 가져오기 위한 쿼리를 작성하고 
<img src="https://user-images.githubusercontent.com/57983744/204942658-122d1cbf-eb1c-4edc-b34d-66d0b59e4a59.png">
히트맵 데이터를 pivotmaster 시트에 가져옵니다.
<h5>(2) R데이터프레임 만들기</h5>

<img src="https://user-images.githubusercontent.com/57983744/204942659-9b2f796b-859f-4235-aae9-3aec537498e0.png"><br>
<details>
<summary> Sample 코드 접기 / 펼치기 </summary>

```

  SELECT CUST_ID
     , MAX(CASE WHEN ITEM = '다운' THEN 1 ELSE 0 END) AS '다운' 
     , MAX(CASE WHEN ITEM = '데님' THEN 1 ELSE 0 END) AS '데님' 
     , MAX(CASE WHEN ITEM = '바지' THEN 1 ELSE 0 END) AS '바지' 
     , MAX(CASE WHEN ITEM = '반바지' THEN 1 ELSE 0 END) AS '반바지' 
     , MAX(CASE WHEN ITEM = '블라우스' THEN 1 ELSE 0 END) AS '블라우스' 
     , MAX(CASE WHEN ITEM = '스웨터' THEN 1 ELSE 0 END) AS '스웨터' 
     , MAX(CASE WHEN ITEM = '우븐셔츠' THEN 1 ELSE 0 END) AS '우븐셔츠' 
     , MAX(CASE WHEN ITEM = '우븐조끼' THEN 1 ELSE 0 END) AS '우븐조끼' 
     , MAX(CASE WHEN ITEM = '원피스' THEN 1 ELSE 0 END) AS '원피스' 
     , MAX(CASE WHEN ITEM = '자켓' THEN 1 ELSE 0 END) AS '자켓' 
     , MAX(CASE WHEN ITEM = '점퍼' THEN 1 ELSE 0 END) AS '점퍼' 
     , MAX(CASE WHEN ITEM = '코트' THEN 1 ELSE 0 END) AS '코트' 
     , MAX(CASE WHEN ITEM = '특종' THEN 1 ELSE 0 END) AS '특종' 
     , MAX(CASE WHEN ITEM = '티셔츠' THEN 1 ELSE 0 END) AS '티셔츠' 
     , MAX(CASE WHEN ITEM = 'ACC' THEN 1 ELSE 0 END) AS 'ACC' 
     , MAX(CASE WHEN ITEM = '기타' THEN 1 ELSE 0 END) AS '기타' 
  FROM crm_mart_hj.sample
WHERE 1=1
 [and GENDER in ($$pivotmaster!B1$$)]
 [and AGE in ($$pivotmaster!C1$$)]
 [and GRADE in ($$pivotmaster!D1$$)]
 [and ITEM in ($$pivotmaster!E1$$)]
 [and SALE_DT in ($$pivotmaster!F1$$)]
 GROUP BY CUST_ID

```

</details><br>
장바구니 분석을 하기 위해 필요한 데이터셋을 만들기 위한 쿼리를 입력하고 rdf1 이름으로 데이터프레임을 만들겠습니다.<br>
<br>
<h3>2) 장바구니 분석하기</h3>
<img src="https://user-images.githubusercontent.com/57983744/204942663-37b81379-9a2f-4172-ad1b-21f46faef1f6.png"><br>
<details>
<summary> Sample 코드 접기 / 펼치기 </summary>

<pre>

library(arules)
rdf1$CUST_ID<-NULL
rdf1$AUTOSEQ<-NULL
dat<-as.data.frame(sapply(rdf1,as.logical))
dat<-as(dat,"transactions")
rule<-apriori(dat,control=list(verbos=F),parameter=list(support  0.05, confidence = 0.5, minlen=2))
rule<-sort(rule,by='lift')
result<-inspect(rule)


</pre>

</details><br>
rdf1데이터셋으로 장바구니 분석을 하는 R코드를 입력하여 rdf1시트에 출력합니다.
데이터셋을 logical형태로 변환하고 apriori 라이브러리를 이용해 지지도가 0.05, 신뢰도가 0.5 이상인 항목들을 향상도 순으로 출력하는 R코드를 입력하였습니다.
<br><br>
<img src="https://user-images.githubusercontent.com/57983744/204942665-484aff6b-675d-4028-b276-96f54371270b.png">
<br>
코드의 결과가 rdf1시트에 표시됩니다.
<br><br>
<img src="https://user-images.githubusercontent.com/57983744/204942667-35419e1a-34a2-4fe7-a39a-8b309ce863ec.png">
<br>
해당 값을 엑셀 함수를 활용하여 필요한 데이터(좌측변수, 우측변수, 지지도, 신뢰도, 향상도)를 추출해줍니다.
<br><br>
<h3>3) 대시보드 구성하기</h3>
<br>
<img src="https://user-images.githubusercontent.com/57983744/204942668-cdc203ff-4168-4d81-b076-685c1c848707.png">
<br>
앞에서 pivotmaster 시트에 구해놓은 히트맵 데이터와 장바구니 분석 결과를 활용하여 연관분석 대시보드(Dashboard2)를 구성합니다.<br>
좌측 상단에는 많이 팔린 10개의 품목을 순서대로 보여주는 표, 우측 상단에는 한 고객이 함께 구매한 옷의 종류에 대한 히트맵, <br>하단에는 장바구니분석의 결과인 지지도, 신뢰도, 향상도 및 그에 따른 추천 마케팅을 표시하였습니다.
<br><br>
<img src="https://user-images.githubusercontent.com/57983744/204942670-bfb991b7-c543-4711-b884-4e78e73c8615.png">
<br>
장바구니 분석 또한 사용자가 원하는 기준으로 분석 할 수 있게끔 변수를 입력할 수 있는 곳이 필요합니다.<br>대시보드 좌측에 기준이 되는 지지도와 신뢰도를 입력할 수 있는 곳을 만들어주겠습니다. 
<br><br>
<img src="https://user-images.githubusercontent.com/57983744/204942673-f30b064d-4dd8-4879-8d27-8aed26f5715d.png">
<br>
<details>
<summary> Sample 코드 접기 / 펼치기 </summary>

```

library(arules)
rdf1$CUST_ID<-NULL
rdf1$AUTOSEQ<-NULL
dat<-as.data.frame(sapply(rdf1,as.logical))
dat<-as(dat,"transactions")
rule<-apriori(dat,control=list(verbos=F),parameter=list(support = [[##Dashboard2!C27##]], confidence = [[##Dashboard2!C29##]],minlen=2))
rule<-sort(rule,by='lift')
result<-inspect(rule)



```

</details><br>

해당 입력값을 반영할 수 있도록 R코드를 수정합니다.<br><br>
<img src="https://user-images.githubusercontent.com/57983744/204942675-9163b5a4-4418-4a10-b267-3f5796c1093b.png">
<br>
기준 지지도를 0.05에서 0.03로 조금 낮춰 5개의 항목이 나타나게 대시보드가 변경되었습니다.
<br><br>
<img src="https://user-images.githubusercontent.com/57983744/204942678-a0bb5f4c-1fb6-4d17-b7fc-2e0e2ffe3407.png">
<br>
이전의 대시보드로 이동할 수 있는 화살표 버튼을 만들고 모든 프로젝트를 실행 할 수 있는 버튼을 만들어 매출대시보드와 연관분석 대시보드를 완성하였습니다.
<br><br><br>
<a href="/XLIG/2.사용자매뉴얼/3.데이터 분석 해보기/2.대시보드 살펴보기/">(이전) 대시보드 살펴보기</a>