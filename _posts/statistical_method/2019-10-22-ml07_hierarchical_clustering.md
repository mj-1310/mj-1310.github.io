---
layout: post
title: "머신러닝 07. 비지도학습: 계층적 군집분석(hclust)"
author: "MJ"
categories: [science, statistical_method]
tags: [statistics, machinelearning, multicampus, bigdata_analysis_edu, GNT]
image: 
---


---
&nbsp; &nbsp; **목차**<br>
&nbsp; &nbsp; 1. [데이터셋 로딩](#1)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (1) [데이터셋 로딩](#1_1)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (2) [데이터셋 간단 조회: psych::headTail()](#1_2)<br>
&nbsp; &nbsp; 2. [기술통계분석](#2)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (1) [기술통계: summary() / psych::describe()](#2_1)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (2) [연속변수 탐색: boxplot()](#2_2)<br>
&nbsp; &nbsp; 3. [데이터 전처리](#3)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (1) [유클리드 거리: dist()](#3_1)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (2) [열이름을 소문자로 변경: tolower()](#3_2)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (3) [연속변수 스케일링: scale()](#3_3)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (4) [일부 레코드 간의 유클리드 거리: dist()](#3_4)<br>
&nbsp; &nbsp; 4. [군집분석](#4)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (1) [계층형 군집분석(평균연결법): hclust()](#4_1)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (2) [군집분석 결과 플롯팅: plot()](#4_2)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (3) [군집분석 결과 확인: $order / str()](#4_3)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (4) [계층형 군집분석에 사용되는 다양한 거리: method = single / complete / centroid / ward.D / ward.D2](#4_4)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (5) [예상 군집 개수 계산: NbClust::NbClust()](#4_5)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (6) [군집화가 얼마나 잘됐는지 확인: cutree() / silhouette() / Dumm Index()](#4_6)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (7) [덴드로그램에서 군집 나누기: rect.hclust()](#4_7)<br>
&nbsp; &nbsp; 5. [결과 프로파일링](#5)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (1) [결과 확인 및 요약집계: cutree() / aggregate()](#5_1)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (2) [시각화: lattice::parallelplot() / caret::featurePlot()](#5_2)<br>

---

<br>
017
<br>

---------------코딩------------------------------------------------------------------------------------------------------

<br>

<a id="1"></a>

## 1. 데이터셋 로딩

<br>

<a id="1_1"></a>

### (1) 데이터셋 로딩

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<br>

<a id="1_2"></a>

### (2) 데이터셋 간단 조회: psych::headTail()

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<br><br>

<a id="2"></a>

## 2. 기술통계분석

<br>

<a id="2_1"></a>

### (1) 기술통계: summary() / psych::describe()

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<br>

<a id="2_2"></a>

### (2) 연속변수 탐색: boxplot()

<br>

● 연속변수니까 boxplot에 넣어봐도 좋다. 

● 열량은 데이터의 레인지가 크다.

● 원체 레코드가 작기에 아웃라이어라고 삭제하고 제거하는 건 바람직하지 않다. 

● 연속형 변수가 레인지가 다르거나 분포특성이 다를 때 이런 걸 그대로 넣으면 열량 같은 변수의 영향력이 굉장히 커져서, 군집을 묶을때 제대로 안 묶일 가능성이 크다. 

● 그래서 위처럼 변수의 레인지가 서로 크게 다른 경우, 스케일링이라는 작업을 거쳐서, 흔히 말하는 z변수로 만들어서 레인지를 -1~1 사이(혹은 0~1)로 센터링한다. 표준화시킨다. 그런 목적에서 EDA를 할 수 있어야 한다.

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<br><br>

<a id="3"></a>

## 3. 데이터 전처리

<br>

<a id="3_1"></a>

### (1) 유클리드 거리: dist()

<br>

● Beef Braised ~ Beef Braised 의 거리는 0

● Beef Braised ~ Hamburger의 거리는 95.6 

● 이런 식으로 거리 매트릭스를 구한 것. 

● 그러나 이건 공정하지 않다. 5개 변수 중 어떤 변수는 범위가 너무 넓고 어떤 변수는 범위가 너무 좁다. 

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<br>

<a id="3_2"></a>

### (2) 열이름을 소문자로 변경: tolower()

<br>

● 공교롭게도 행이름 자리에 레코드들이 들어가 있다. 이것도 나중에 한 컬럼명으로 묶어줘야지. 그리고 빼야함. 왜냐하면 군집분석에는 연속변수만 들어가야하기 때문. 

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<br>

<a id="3_3"></a>

### (3) 연속변수 스케일링: scale()

<br>

● scale 함수의 도움말을 펼쳐보면 center라는 옵션이 있다. center = TRUE가 디폴트 값인데,
  값이 자동으로 -1에서 +1사이로 센터링을 해준다. (평균이 0이고 표준편차가 1)

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<br>

<a id="3_4"></a>

### (4) 일부 레코드 간의 유클리드 거리: dist()

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<br><br>

<a id="4"></a>

## 4. 군집분석

<br>

<a id="4_1"></a>

### (1) 계층형 군집분석(평균연결법): hclust()

<br>

● hclust() : hierarchical. 계층형으로 군집을 나눠주는 함수. (혹은 응축형 분석)

거리 차이 제일 작은 걸 묶어서 한 그룹 만들고 그 담으로 작은 걸 묶어서 한 그룹, 계속 만드는 형식.
최종적으로는 하나의 그룹이 되고, 밑으로 내려오면 여러 그룹을 만드는 함수. 

● 옵션에 거리 계산한 데이터를 집어넣도록 되어있다. 

● 방법은 여러가지가 있는데, 평균연결기법을 사용.

● 얘는 이렇게 하고 그림으로 많이 그린다. 

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<br>

<a id="4_2"></a>

### (2) 군집분석 결과 플롯팅: plot()

<br>

● hang 옵션: 그룹 개체들이 묶인 걸 표시할 때, 숫자에 따라 가지가 길어지기도 하고 짧아지기도 하는 것.

● cex 옵션: 레이블의 크기.

● 잘 보면 맨 위는 두 그룹으로 되어 있다. x축에 전체 27개의 레코드가 있는데, 제일 위에 보면 먼저 두 개 그룹으로 나뉜다. 

● 26개 안에서도 거리 차이가 존재하니 또 2개 그룹으로 나누고, 계속 나눌 수 있을만큼 나눠서 최종적으로 다시 2개로 나뉜다. 

● 이렇게 위쪽에서 아래쪽으로 내려오면서 해석해도 되고 아니면 아래에서부터 묶어가며 올라가도 된다.

● pork roat, pork simmered와 먼저 묶고, 하는 식을. 도저히 비슷한 게 없으면 단독으로 존재. 

● y축이 height인데, 이게 거리 차이 계산한 거라고 보면 된다. 거리차가 멀수록 높고, 가까울수록 낮다. 

● 그룹은 좀 높은 레벨에서 나무를 컷팅해서 나눌 수도 있고 좀 낮은 레벨에서 컷팅해 나눌수도 있다. 

● sardines canned만 영양 성분이 굉장히 튀는 레코드인 것으로 예상.

● clams raw, clams canned도 영양 성분이 꽤 튀는 레코드인 것으로 예상.


<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<br>

<a id="4_3"></a>

### (3) 군집분석 결과 확인: $order / str()

<br>

● 구조를 살펴보면 $표시된 부분들은 수학적, 통계적으로 의미있는 항목들. 

● 그림으로는 군집 몇 개 정도 가능할 지 윤곽을 본 것이고, 나중에 대규모 데이터에서 어떤 것이 몇 번째 그룹에 속해 있는지 보려면 $order 가필요하다. 

● $order의 숫자는 인덱스 번호. 25가 sardines canned. 

● 거리 구한 걸 이렇게 저렇게 묶어보면 어떨지 아이디어도 다양하다. 

● 여기까지는 그룹을 임의대로 구성한 다음, 거리 차이의 평균이 어느정도인지 계산한 것.

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<br>

<a id="4_4"></a>

### (4) 계층형 군집분석에 사용되는 다양한 거리: method = single / complete / centroid / ward.D / ward.D2

<br>

● 와드기법. 한국어로 굳이 번역하진 않는다. 그룹이 명확하게 구분이 될 경우, 지들끼리 비교. 거리 차이를 자기네 요소들끼리 세부적으로 구해서 합을 구함. 그 차이가 다른 그룹과 비교해서 명확히 차이가 나면 군집이 비교되는 식. 


<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<br>

<a id="4_5"></a>

### (5) 예상 군집 개수 계산: NbClust::NbClust()

<br>

● 2개부터 15개 군집 중 추천해달라. (27개 레코드이기 때문에 이 정도가 적정)

● 스크리더 표라고 부름. 군집을 나눴을 때, 나눈 갯수의 설명력을 인덱스 값으로 표시한 것. 

● 3개에서 4개로 군집 분리할 때는 차이 얼마 없으므로 분리의 의미 없다, 로 해석.

● 4개에서 5개로 군집 분리할 떄는 차이가 좀 있으므로 분리의 의미 있다, 로 해석. 

● 5개 정도로 군집을 분리하는 게 어떻겠냐, 로 해석. 



● 4개로 나눠보는 건 어때? 가 2개 추천. 13개 군집 나누는 건 어때? 도 2개 추천. 등등 이렇게 읽는 것. 

● 일단 5개로 나눠보자. 
<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<br>

<a id="4_6"></a>

### (6) 군집화가 얼마나 잘됐는지 확인: cutree() / silhouette() / Dumm Index()

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<br>

<a id="4_7"></a>

### (7) 덴드로그램에서 군집 나누기: rect.hclust()

<br>

● rect.hclust: rectangle의 약자.


● 그룹5에 많은 식자재가 들어 있고, 범용적인 식자재를 의미한다. 그룹 1은 묶이지도 못하며, 왼쪽은 거의 아웃라이어. 

● 여기서 이제 각각 프로파일링을 해봐야 한다. 

● 실제로 이 덴드로그램을 잘라보자. 그리고 임의의 그룹 번호를 붙여줘 보자. 

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<br><br>

<a id="5"></a>

## 5. 결과 프로파일링

<br>

<a id="5_1"></a>

### (1) 결과 확인 및 요약집계: cutree() / aggregate()

<br>

특성에 따라 분석하는 걸 프로파일링


● 개별 레코드 별로 얘는 어느 그룹인지 알려줌.

● cutree로 잘라낸 그룹별로 어떤 레코드가 어느 그룹인지 알려줌




● 원본 데이터에 그룹핑 된 내용 추가. 변수명은 명시적으로 group으로 만듦. 

● 다섯 개 변수의 움직임을 통해 그룹을 알아냄.

● beef라고 쓴 친구들은 대체로 1번 그룹이네~ 같은 특성 알아냄. 

● 데이터프레임 만들 때 스케일링 되기 전인 원본 데이터에 묶음.

● 스케일링된 데이터에 그룹 표시를 하면 데이터들이 모두 표준화된 값이 나온다. 그럼 우리가 이제 할 일은 프로파일링 작업인데, 표준화된 것으로 하면 단위도 없어져있고 아무튼 애매함.

● 그룹 변수가 집계 변수이기 때문에, 그룹 별로 변수들의 평균이나 중앙값을 구하고, 각각 그룹의 특성을 알아내면 된다. (aggregation() 함수, dplyr::groupby, summarize 함수 써도 됨.)

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<br>

<a id="5_2"></a>

### (2) 시각화: lattice::parallelplot() / caret::featurePlot()

<br>


● caret 패키지로도 군집분석을 많이 하며 특히 머신러닝에서 많이 이용. 

● featurePlot() 함수: 개별 변수 별로 나눠질 때 그룹 별로 같이 움직이는지 확인 가능. 

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<br><br>


###### <참고 문헌> 

<br>

1. 최점기 박사님 강의 <br>
2. 통계학개론 (영지출판사)