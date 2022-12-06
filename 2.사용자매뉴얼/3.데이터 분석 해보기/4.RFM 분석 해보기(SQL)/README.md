
# RFM 분석 (SQL)


이 장에서는 CRM 마케팅을 위해 많은 기업이 고객 가치분석 수단으로 사용하는 RFM 분석을 XLIG 환경에서 실행하는 과정을 소개합니다. 

가장 먼저 샘플 데이터를 XLIG 환경으로 가져와 SQL로 전처리하고, 이를 바탕으로 기초적인 RFM 분석을 실행한 결과와 시각화 차트를 엑셀 화면으로 가져와 대시보드를 구현하는 과정을 진행합니다.


---



<h3>1) 데이터 준비</h3>

<h5>(1) 샘플 데이터 가져와 SQL로 전처리하기</h5>
RFM 분석을 실행하기 위해서는 최근성(R) 구매빈도(F) 구매액(M)이 모두 포함된 적절한 데이터가 필요합니다. 분석을 위해 DB에서 샘플 데이터를 가져와 형태를 확인합니다.
<br><br>
<img src = "https://user-images.githubusercontent.com/86198387/204686830-0f8f0a44-eb47-422f-9232-d60fe9786916.png" /><br>

<details>
<summary> Sample 코드 접기 / 펼치기 </summary>

```

select * from crm_mart_shj.xlig_sample2

```


</details><br>

DB에서 불러들인 샘플 데이터는 고객 ID와 구매일, 구매 금액이 포함된 데이터입니다. 이 데이터를 핸들링해 샘플 고객들의 최종 구매일(R), 구매 빈도(F), 총 구매 금액(M)을 산출하고 분석을 진행할 것입니다.<br>

<img src = "https://user-images.githubusercontent.com/86198387/204687395-3f5f1f6e-ef0c-4ef7-9e42-cee226b6e5ae.png" /><br>

<details>
<summary> Sample 코드 접기 / 펼치기 </summary>

```

select customer_id
     , MAX(trans_date) as Recency
     , count(customer_id) as Frequency
     , sum(tran_amount) as Money     
     , datediff('2015-03-17', MAX(trans_date)) as days     
  from crm_mart_shj.xlig_sample2
  group by customer_id

```


</details><br>

엑셀에서 형태를 확인한 샘플 데이터는 SQL을 이용해 집계합니다. 고객별(customer_id)로 최종 구매일(R), 구매 횟수(F), 총 구매 금액(M)을 산출했습니다. <br>

또한, RFM 레벨 부여를 위해 본 샘플 데이터의 가장 마지막 구매일(2015-03-16)보다 하루 늦은 2015년 3월 17일을 기준으로 최종 구매일과의 차이가 얼마인지 체크하는 칼럼을 추가했습니다.<br>

전처리된 데이터 테이블의 형태는 위 사진을 통해 확인할 수 있습니다.<br>

이 데이터를 바탕으로, 다음 장에서 고객들에게 RFM 등급을 부여하고 분석을 실행해 대시보드를 구현할 수 있습니다.<br>

<h3>2) RFM 분석하기</h3>

이제 SQL 쿼리문을 이용해 기본적인 RFM 분석을 실행합니다.

분석을 위해 RFM 등급을 데이터에 추가해야 하므로, 등급을 나눌 기준을 정하고 이에 따라 전체 데이터를 세분화할 것입니다.<br>

RFM 등급을 나누는 기준은 여러가지가 있으나, SQL을 사용하는 쿼리문에서는 전체 데이터 순위를 기준으로 백분위를 이용해 3등분하겠습니다.<br>

<img src = "https://user-images.githubusercontent.com/86198387/205836682-ffd8e43c-1b48-4919-a4de-1bd2736a59b1.png" /><br>

<details>
<summary> Sample 코드 접기 / 펼치기 </summary>


```

select Z.R_Level       
     , Z.F_Level       
     , Z.M_Level       
     , concat(Z.R_Level,Z.F_Level,Z.M_Level) as RFM_Score
     , count(Z.customer_id) as CUS_CNT 
     , sum(Z.Frequency) as SALE_QTY
     , sum(Z.Monetary) as SALE_AMT    
     , round(sum(Z.Monetary)/count(Z.customer_id),2) as AMT_CUSp   /*객단가*/
  from (
           select A.customer_id
                , A.Recency 
                , case when ROUND(PERCENT_RANK() over(order by Recency),2) <=0.33                then 3          
                       when ROUND(PERCENT_RANK() over(order by Recency),2) between 0.34 and 0.67 then 2
                                                                                                 else 1 end as R_Level
                , A.Frequency
                , case when ROUND(PERCENT_RANK() over(order by Frequency),2) <=0.33                then 3
                       when ROUND(PERCENT_RANK() over(order by Frequency),2) between 0.34 and 0.67 then 2
                                                                                                   else 1 end as F_Level
                , A.Monetary
                , case when ROUND(PERCENT_RANK() over(order by Monetary),2) <=0.33                then 3
                       when ROUND(PERCENT_RANK() over(order by Monetary),2) between 0.34 and 0.67 then 2
                                                                                                  else 1 end as M_Level           
             from (
                      select A0.customer_id  
                           , max(A0.trans_date)  as Recency    
                           , count(A0.customer_id) as Frequency
                           , sum(A0.tran_amount) as Monetary
                        from crm_mart_shj.sample_rfm A0 
                       group by A0.customer_id
                  )A   
        ) Z  
 where M_Level = 1 and F_Level in ('2,''3')
 group by RFM_SCORE
 order by SALE_AMT desc

```

</details> <br>

위와 같이 쿼리문을 추가로 입력해 테이블에 RFM 등급을 추가하고, 이를 바탕으로 분석을 위한 값들을 집계합니다. 결과값은 엑셀시트에 출력해 데이터가 올바르게 표시되는지 확인합니다.<br>



<img src = "https://user-images.githubusercontent.com/86198387/205838095-503774fc-92fc-4259-ab6d-48e46c847768.png" /><br>

기존의 고객별 데이터가 R F M 등급을 기준으로 집계되어, 부여받은 등급별로 고객군이 나누어졌고 이에 알맞게 각 지표들이 산출되었음을 한 눈에 확인할 수 있습니다.<br>

이 데이터 테이블을 바탕으로 대시보드를 생성해 분석의 결과를 확인할 것입니다.<br>



<h3>3) 대시보드 구성하기</h3>

<br>

마지막 단계는 분석 결과를 확인하는 시각화 대시보드의 구성입니다.<br>

<img src = "https://user-images.githubusercontent.com/86198387/205840182-336c5782-5809-4bf1-ae69-058f29512a44.png"/><br>

앞서 <a href="/XLIG/2.사용자매뉴얼/3.데이터 분석 해보기/1.매출대시보드 생성하기/"> 1.매출 대시보드 생성하기 </a> 단계에서 대시보드를 생성하기 위해 피벗 테이블을 생성했던 것과 동일하게, 이번에도 시각화하고 싶은 차트의 수와 관점만큼 엑셀 시트에 피벗 테이블을 추가합니다.<br>

예시로 추가한 피벗들은 각각 RFM 등급 별 구매액, 구매수량, 구매고객수, 객단가를 집계한 값입니다. 이 피벗들을 이용해 대시보드가 위치할 시트에 차트를 생성합니다.<br>

<img src = "https://user-images.githubusercontent.com/86198387/205841343-05014b0b-72ca-42b5-b881-ae75787d6b35.png"/><br>

적당한 형태의 차트를 생성하고, 지시선을 추가하는 등의 작업으로 기본적인 대시보드가 생성되었습니다. 이 차트와 데이터를 바탕으로 확인할 수 있는 결과는 다음과 같습니다.<br>

```


◆ 대부분의 경우 RFM 등급이 111(모두 최상위)에 위치하는 고객군이 가장 상위권을 차지합니다.

◆ 하지만 R 등급이 낮더라도 매출액, 구매 수량이 준수한 고객군 역시 확인할 수 있습니다.

◆ 구매 고객수가 가장 많은 고객군은 최상위 고객군이 아닌 일회성 고객(RFM 333등급)임을 확인할 수 있습니다.

```


다음은 차트를 통해 확인한 결과를 바탕으로, 임의의 마케팅 관점에 따라 전체 고객군을 나누어 살펴보는 테이블을 대시보드에 추가하겠습니다. <br>


<img src = "https://user-images.githubusercontent.com/86198387/205844780-a6feef55-7a40-4fa2-b2f7-cf2bde8d34eb.png"/><br>

<b> 코드 실행 → 탭 추가 </b> 기능을 이용해 조회 관점을 달리하는 다중 쿼리문을 적용합니다. 각각의 분석 관점에 따라 엑셀 시트에 고객군을 출력해 결과를 대시보드에 결합합니다.

<img src = "https://user-images.githubusercontent.com/86198387/205842281-b952f073-9944-44e5-8840-250dedc0c8cd.png"/><br>

대시보드에 아래와 같은 집계 결과가 추가되었습니다.

```

◆ 최근 구매(R)이 부진한 고객이지만 구매 빈도(F)와 구매 금액(M)은 나쁘지 않으므로, 구매 이탈을 관리해야 하는 고객군

◆ 구매 빈도(F) 대비 구매액(M)이 낮은 고객들을 확인하기 위한 구매력 향상 필요 고객군

◆ 구매 금액(M) 대비 구매 빈도(F)가 낮은 고객들을 확인하기 위한 구매 빈도 향상 필요 고객군

```

이로써 RFM 분석 결과 나타난 매출 현황과, 산출한 RFM 등급을 바탕으로 세그멘트를 나눈 고객군을 한 눈에 볼 수 있는 대시보드가 완성되었습니다.

예시처럼 현업 사용자는 인사이트를 얻고자 하는 형태에 따라 자유롭게 XLIG 코드 실행 화면에 탭을 추가해 데이터를 핸들링할 수 있습니다.<br>

또한 XLIG 엔터프라이즈 버전을 사용하고 있다면, 위에서 시각화된 고객군에 속하는 고객이 정확히 누구인지 찾아내는 타겟리스트의 확보, 혹은 더욱 심도 깊은 대시보드의 구성등 다양한 활용이 가능합니다.

<br><br><br>
