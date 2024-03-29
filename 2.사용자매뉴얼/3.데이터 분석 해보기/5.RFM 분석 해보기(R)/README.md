
# RFM 분석 (R)


앞서 SQL로 작업한 RFM 분석을 이번에는 R을 이용해 실행하고, 동적으로 대시보드를 생성하는 과정을 소개하겠습니다.

R을 사용하면 RFM 등급에 가중치를 부여해 단순히 RFM 등급을 부여하고 이를 결합해 고객군을 형성하는 것보다 더욱 세밀하게 고객군을 나누는 작업을 쉽게 수행할 수 있습니다. 

이 장에서는 데이터를 전처리하고, R로 가져와 RFM 분석을 실행한 다음, 등급 별 가중치를 추가해 신뢰도를 높힌 분석 결과와 시각화 차트를 엑셀 화면으로 가져오는 단계를 XLIG 환경에서 실행할 수 있도록 소개합니다.


---



<h3>1) 데이터 준비</h3>

<h5>(1) 샘플 데이터 가져와 전처리하기</h5>
RFM 분석을 실행하기 위해서는 이전에 사용한 것과 동일하게, 최근성(R) 구매빈도(F) 구매액(M)이 모두 포함된 적절한 데이터가 필요합니다.<br>
<br>

앞서 <a href="/XLIG/2.사용자매뉴얼/3.데이터 분석 해보기/5.RFM 분석 해보기(SQL)/"> 4.RFM 분석 해보기(SQL) </a>를 수행했다면, 동일한 데이터를 R로 보내는 것 만으로 분석을 시작할 수 있기 때문에 전처리 과정을 다시 수행할 필요는 없습니다.



<h5>(2) R데이터프레임 만들기</h5>

다음 단계로, 전처리 단계를 거친 데이터는 RFM 분석을 위해 데이터 프레임 형태로 변환해 R로 이동시킵니다.


<img src="https://user-images.githubusercontent.com/86198387/204687519-30d0d6b5-db5a-473c-aed5-1b7249a3fbee.png"><br>

핸들링한 SQL 코드를 <b> 코드 실행 → 결과유형 → R데이터프레임</b>을 선택해 R의 데이터프레임형태로 변환해 전송합니다. RFM 분석을 위한 데이터 준비가 끝났습니다.<br>
<br>

<h3>2) RFM 분석하기</h3>

이제 R의 기본 기능과 dplyr 라이브러리를 활용해 분석을 실행합니다.

먼저 RFM 등급을 데이터프레임에 추가해야 하므로, 등급을 나눌 기준을 정하고 이에 따라 전체 데이터를 세분화할 것입니다.<br>

<img src = "https://user-images.githubusercontent.com/86198387/204731606-779341ca-b3eb-4072-868e-4a1b062731d5.png" /><br>

RFM 등급을 나누는 기준은 여러가지가 있으나, 이 문서에서는 누적 백분위를 기준으로 실행합니다.<br>

등급은 위의 사진과 같이 총 5단계로 나누어집니다. R 코드를 이용해 R, F, M 등급을 결정합니다.<br>

이 과정에서, 최근성(R)은 고객이 가장 최근에 구매했을수록 높은 등급(5)을 받아야 하므로 빈도(F)와 금액(M)과는 상이한 코드를 이용해 등급을 부여해야 하는 점을 유의해야 합니다.<br>

 기준이 정해졌으니 데이터에 RFM 등급을 추가할 차례입니다.


<img src = "https://user-images.githubusercontent.com/86198387/204732010-51f59724-dbf3-4093-a204-3b508e3140a9.png" /><br>

```
# 작업을 위한 라이브러리 로딩

library(dplyr) 

# 데이터 인풋 

tested <- xligsample

# RFM 등급을 누적 백분위로 나누기 위한 함수 생성 작업

RFM_result <- c() 

j <- 0
i <- i

for(i in 1:5) 
{
  j = j+i
  RFM_result[i] = 1-(j/(1+2+3+4+5))
}

# RFM 등급 부여하기

R_level <- quantile(tested$days, prob = RFM_result)
F_level <- quantile(tested$Frequency, prob = RFM_result)
M_level <- quantile(tested$Money, prob = RFM_result)

# 구매 경과일로 R, 구매 빈도로 F, 총 구매액으로 M 등급 고객에게 부여하기

tested <- tested %>% 
  mutate(R_score = case_when(.$days <= R_level[1] ~ 1,
                             .$days <= R_level[2] ~ 2,
                             .$days <= R_level[3] ~ 3,
                             .$days <= R_level[4] ~ 4,
                             TRUE ~ 5),
         F_score = case_when(.$Frequency >= F_level[1] ~ 1,
                             .$Frequency >= F_level[2] ~ 2,
                             .$Frequency >= F_level[3] ~ 3,
                             .$Frequency >= F_level[4] ~ 4,
                             TRUE ~ 5),
         M_score = case_when(.$Money >= M_level[1] ~ 1,
                             .$Money >= M_level[2] ~ 2,
                             .$Money >= M_level[3] ~ 3,
                             .$Money >= M_level[4] ~ 4,
                             TRUE ~ 5))

# 데이터 확인 

print(tested)

```


dplyr 라이브러리가 지원하는 case when 함수를 이용해 SQL과 유사하게 데이터에 RFM 등급을 추가하고 결과값을 엑셀시트에 출력해 데이터가 올바르게 추가되었는지 확인합니다.<br>

기존 데이터에 R F M 등급이 추가되었음을 확인할 수 있습니다. 이를 토대로 가중치를 부여해 RFM 스코어를 산출하겠습니다.<br>

가중치를 부여하는 방법은 여러가지가 존재하지만, 이 장에서는 등급별 고객 구성비 대비 매출 비중의 합계값을 가중치로 적용해 RFM 스코어에 반영할 것입니다.<br>

<img src = "https://user-images.githubusercontent.com/86198387/204734601-f334a8e3-db01-4cfb-b0e5-88d0390f6766.png" /><br><br>



<img src = "https://user-images.githubusercontent.com/86198387/204735501-77df9076-5fb1-464b-a93a-796e233dc84a.png" /><br>

```

# 가중치 생성 준비작업

# R, F, M 점수별로 각각 고객을 집계해 가중치 산출 작업 수행

R_table <- tested %>%
  group_by(R_score) %>%
  summarise(Cust = n_distinct(customer_id),
            tot_Money = sum(Money))

R_table <- R_table %>%
  mutate(Cust_prop = Cust/sum(Cust),
         Money_prop = tot_Money/sum(tot_Money),
         Effect = Money_prop/Cust_prop) %>%
  arrange(desc(R_score))

R_effect <- sum(R_table$Effect)

F_table <- tested %>%
  group_by(F_score) %>%
  summarise(Cust = n_distinct(customer_id),
            tot_Money = sum(Money))

F_table <- F_table %>%
  mutate(Cust_prop = Cust/sum(Cust),
         Money_prop = tot_Money/sum(tot_Money),
         Effect = Money_prop/Cust_prop) %>%
  arrange(desc(F_score))

F_effect <- sum(F_table$Effect)

M_table <- tested %>%
  group_by(M_score) %>%
  summarise(Cust = n_distinct(customer_id),
            tot_Money = sum(Money))

M_table <- M_table %>%
  mutate(Cust_prop = Cust/sum(Cust),
         Money_prop = tot_Money/sum(tot_Money),
         Effect = Money_prop/Cust_prop) %>%
  arrange(desc(M_score))

M_effect <- sum(M_table$Effect)

RFM_effect <- sum(R_effect,F_effect,M_effect)

weight <- c(R_effect/RFM_effect, F_effect/RFM_effect, M_effect/RFM_effect)

# 가중치를 RFM 등급에 합산하기 위한 함수 작업

RFM_function <- function(x, y, z, w){
  RFM_Score  <- x*w[1]+y*w[2]+z*w[3]
  return(RFM_Score)
}

# 데이터 테이블에 가중치가 포함된 RFM 등급 추가하기

tested_set <- tested %>%
  mutate(RFM_Score = RFM_function(tested$R_score, tested$F_score, tested$M_score, w = Weight))

```

위와 같이 R 코드를 이용해 등급별 고객 구성비 대비 매출 비중의 합계값을 가중치로 산출하고, 이 값을 데이터에 부여하기 위해 함수를 생성해 작동시킵니다. 데이터에 RFM 스코어가 정상적으로 부여되었는지 엑셀 시트로 출력해 확인합니다.<br>

<img src = "https://user-images.githubusercontent.com/86198387/204738659-d903b90f-b007-4721-96a1-f1c6fdfd2721.png" /><br>

처음 SQL에서 가져왔던 데이터에 R, F, M등급과 RFM 스코어가 추가되었습니다. 이제 시각화 대시보드를 구성해 분석의 결과를 확인할 수 있습니다.<br><br>

<h3>3) 대시보드 구성하기</h3>

<br>

마지막으로 위에서 실행한 분석의 결과를 확인하기 위해 대시보드를 구성할 차례입니다.<br>

<img src = "https://user-images.githubusercontent.com/86198387/204739661-37ee7c01-96ad-4514-ab3f-00541b1614d2.png"/><br>

R의 시각화 패키지인 ggplot을 이용해 고객군의 분포료를 먼저 추가하겠습니다. R 코드를 이용해 시각화 차트를 구성합니다.<br>

각각 RFM 점수 대비 구매 금액, 구매 주기 대비 구매액, 구매 빈도 대비 구매액, 구매 주기 대비 구매 빈도를 만들기 위한 코드입니다.<br>

<img src = "https://user-images.githubusercontent.com/86198387/204741332-378a20ac-2f6c-4d23-9852-f9569401f9ce.png"/><br>

```

# 시각화를 위한 패키지 로딩 

library(ggplot2)

# 관점별로 시각화 차트 구성하기

a_plot <- ggplot(data = tested_set, aes(x = RFM_Score, y = Money, color = RFM_Score)) +
  geom_point(position = "jitter") +
  theme_bw()
a_plot

b_plot <- ggplot(data = tested_set, aes(x = days, y = Money, color = RFM_Score)) +
  geom_point(position = "jitter") +
  theme_bw()
b_plot

c_plot <- ggplot(data = tested_set, aes(x = Frequency, y = Money, color = RFM_Score)) +
  geom_point(position = "jitter") +
  theme_bw()
c_plot

d_plot <- ggplot(data = tested_set, aes(x = days, y = Frequency, color = RFM_Score)) +
  geom_point(position = "jitter") +
  theme_bw()
d_plot

```

<br>

코드를 실행하면 엑셀 화면에 시각화 차트가 출력됩니다.<br>

각 차트의 형태를 통해 RFM 스코어가 상위에 속하는 고객군이 높은 매출을 기록한다는 사실이나, 구매 빈도는 높지만 구매 금액이 낮은 고객군, 구매 빈도가 높고 구매 금액도 높은 고객군등을 확인할 수 있습니다.<br>

이러한 고객군들을 보다 확인하기 편하도록 집계하는 테이블을 대시보드에 추가하겠습니다.<br>

```

# RETENSION 분석

# RFM 스코어는 높지만 최근 구매(R)가 뜸해 구매 이탈 위험이 있는 고객을 위한 집계

risk_cust <- tested_set %>%
  filter(RFM_Score >= 3 & R_score == 2)


risk_cust %>% group_by(R_score,F_score,M_score,RFM_Score) 
        %>% summarise(CUST_CNT = n_distinct(customer_id), tot_Money = sum(Money)) %>% arrange(desc(CUST_CNT, tot_Money))


# Up-Sell 분석

# 구매 빈도는 높지만 구매 금액이 높지 않은 고객에게 구매 유도를 위한 집계

upsell_cust <- tested_set %>%
  filter(F_score == 4  & M_score == 2 )


upsell_cust %>% group_by(R_score,F_score,M_score,RFM_Score) 
          %>% summarise(CUST_CNT = n_distinct(customer_id), tot_Money = sum(Money)) %>% arrange(desc(CUST_CNT, tot_Money))


# Cross-Sell 분석

# 구매 금액은 많지만 구매 빈도는 적은 고객에게 구매 유도를 위한 집계

crosssell_cust <- tested_set %>%
  filter(M_score == 3 & F_score == 2 )

crosssell_cust %>% group_by(R_score,F_score,M_score,RFM_Score) 
           %>% summarise(CUST_CNT = n_distinct(customer_id), tot_Money = sum(Money)) %>% arrange(desc(CUST_CNT, tot_Money))

```

<br>

<img src = "https://user-images.githubusercontent.com/86198387/204742582-d056e92a-5744-4ca5-ab80-7836388797aa.png"/><br>

대시보드의 가독성을 높이기 위한 차트 네이밍 및 수정 작업과, 테이블 추가가 완료된 대시보드입니다.<br>


이 대시보드에는 아래와 같은 고객군 테이블이 추가되었습니다.

```

◆ RFM 스코어 대비 최근 구매(R)이 부진한 고객들을 추적하기 위한 구매 이탈 우려 고객군

◆ 구매빈도(F) 대비 구매액(M)이 낮은 고객들을 추적하기 위한 구매력 향상 필요 고객군

◆ 구매 금액(M) 대비 구매 빈도(F)가 낮은 고객들을 추적하기 위한 구매 빈도 향상 필요 고객군

```

어떤 고객군을 추적할 것인지는 현업 사용자가 인사이트를 얻고자 하는 형태에 따라 자유롭게 R 코드를 이용해 작업할 수 있습니다.<br>

또한, 고객군에 속하는 옵션 변경 등 동적으로 변환되는 대시보드를 구성하고 싶다면 앞서 매출 대시보드를 작업하면서 조건을 추가했던 것처럼 동일하게 이 대시보드에도 옵션 단추와 분석 실행 단추를 추가할 수 있습니다.<br>


<img src = "https://user-images.githubusercontent.com/86198387/204745181-0283e1be-2ce8-4b9e-8fb5-36255a751d70.png" /><br>


옵션 단추를 포함해서, 분포표와 고객군 집계 상황을 한 눈에 볼 수 있는 대시보드가 완성되었습니다. <br>

위와 같은 기초적인 대시보드 구성 외에도, 이미 사용자는 분석과정에서 RFM 스코어가 포함된 데이터프레임을 완성했으므로, 대시보드에 시각화된 고객군에 속하는 고객 타겟리스트의 확보, 혹은 더욱 심도 깊은 대시보드의 구성등 다양한 활용이 가능합니다.

<br><br><br>
