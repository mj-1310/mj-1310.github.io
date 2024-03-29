---
layout: post
title: "머신러닝 23. 지도학습: 비선형모형 (랜덤 포레스트)"
author: "MJ"
categories: [science, statistical_method]
tags: [statistics, machinelearning, multicampus, bigdata_analysis_edu, CRT]
image: 
---

---
&nbsp; &nbsp; **목차**<br>
&nbsp; &nbsp; 1. [가상 데이터셋 만들기](#1)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (1) [가상 데이터셋 만들기1](#1_1)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (2) [변수컬럼 1개를 이용한 예측변수 행렬 생성: stats::model.matrix()](#1_2)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (3) [변수컬럼 1개를 이용한 예측변수 행렬 생성: model.matrix()](#1_3)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (4) [모든 예측변수 투입해 예측변수 행렬 생성: model.matrix() / usefull::build.x()](#1_4)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (5) [가상 데이터셋 만들기2](#1_5)<br>
&nbsp; &nbsp; 2. [데이터 로딩 & 정제 & 탐색](#2)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (1) [데이터 생성 및 변수컬럼명 소문자로 변경](#2_1)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (2) [범주형 변수컬럼에 대한 팩터형 작업](#2_2)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (3) [연속형 변수컬럼에 대한 스케일링 작업](#2_3)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (4) [변수컬럼별 객체타입확인](#2_4)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (5) [데이터 분할](#2_5)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (6) [분류규칙에 가장 영향을 많이 미치는 변수 탐색](#2_6)<br>
&nbsp; &nbsp; 3. [로지스틱 회귀분석을 이용한 분류](#3)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (1) [로지스틱회귀 모델 피팅: stats::glm() / summary()](#3_1)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (2) [로지스틱분석에 사용한 투입변수 중요도 계산: caret::varImp()](#3_2)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (3) [단계별 투입모형: step()](#3_3)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (4) [도출된 로지스틱 분류모델의 훈련데이터에 대한 우량/불량 확률계산](#3_4)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (5) [로지스틱 분류모델의 훈련데이터 분류값과 실제 분류값 교차비교](#3_5)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (6) [로지스틱 분류모델의 훈련데이터에 대한 분류결과 정확성(Accuracy) 평가](#3_6)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (7) [도출된 로지스틱 분류모델의 검증데이터에 대한 우량/불량 확률계산](#3_7)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (8) [로지스틱 분류모델의 학습데이터와 검증데이터 분류정확성 비교](#3_8)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (9) [로지스틱모델의 성능 종합평가: 혼동표](#3_9)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (10) [로지스틱모델의 성능 종합평가: ROC curve 그래프](#3_10)<br>
&nbsp; &nbsp; 4. [고전적 의사결정나무를 이용한 분류](#4)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (1) [의사결정나무 모델 피팅: rpart::rpart()](#4_1)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (2) [의사결정나무의 분류모델 도출결과 확인](#4_2)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (3) [의사결정나무 분석에 사용한 투입변수 중요도 계산](#4_3)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (4) [의사결정나무의 가지치기(prunning): 복잡성 파라미터 확인 -> 나무가지치기](#4_4)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (5) [훈련데이터 성과변수 비교](#4_5)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (6) [의사결정나무 훈련데이터 정확성 평가](#4_6)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (7) [의사결정나무 검증데이터 정확성 평가](#4_7)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (8) [의사결정나무 모델의 훈련데이터와 검증데이터 분류정확성 비교](#4_8)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (9) [의사결정나무 모델의 성능 종합평가: 혼동표](#4_9)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (10) [의사결정나무 모델의 성능 종합평가: ROC curve 그래프](#4_10)<br>
&nbsp; &nbsp; 5. [랜덤포레스트를 이용한 분류](#5)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (1) [랜덤포레스트 분석: randomForest::randomForest()](#5_1)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (2) [랜덤포레스트 모델 분석 결과 확인](#5_2)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (3) [랜덤포레스트 파라미터 튜닝 ](#5_3)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (4) [튜닝 후 랜덤포레스트 피팅](#5_4)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (5) [랜덤포레스트 규칙에 활용된 변수들의 상대적 중요성 분석](#5_5)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (6) [랜덤포레스트 훈련데이터 정확성 평가](#5_6)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (7) [랜덤포레스트 검증데이터 정확성 평가](#5_7)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (8) [랜덤포레스트 모델의 훈련데이터와 검증데이터 분류정확성 비교](#5_8)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (9) [랜덤포레스트 모델의 성능 종합평가: 혼동표](#5_9)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (10) [랜덤포레스트 모델의 성능 종합평가: ROC curve 그래프](#5_10)<br>

---

<br>	

# 문제시나리오: 고객 신용데이터를 활용한 신용카드 발급 분류규칙 도출


● 금융권에서 가급적 고신용자를 선택해서 신용카드를 잘 발급하면 좋기 때문에 그런 걸 위한 분석



### 고객 신용데이터를 활용한 신용카드 발급 분류규칙 도출

### (분석배경)

- 저신용자 신용카드발급: 결제 대금 갚지 못해 손실 가능성이 있음

- 저신용자 신용카드발급억제: 손실 가능성을 사전에 차단할 수 있음

- 고신용자 신용카드 발급: 카드사용수수료를 받아 수익성개선

- 고신용자 신용카드 발급억제: 카드사용수수료 이익을 상실하게 됨




### (기본 의문사항)

- 왜 신용카드 사용자중에 우량고객과 불량고객이 나누어지는 것일까?

- 우량과 불량고객을 구분하는 특성요인은 무엇인가?




### (분석목표)

- 카드사용신청자중 우량과 불량고객을 구분하는 규칙을 도출해 우량고객에게는 카드발급을 불량고객에게는 카드발급제한을 두기 위함




● 민감도 (1을 잘 찾는 것: 고신용자에게 더 발급) / 특이도 (0을 잘 찾는 것: 저신용자에게 덜 발급)



### 1. raw 데이터 준비




● 머신러닝은 변수 설정의 디테일에 대해 고민을 많이 하지 않음. 상관성이 약한 변수는 축소하는 기법으로 해나가면 되기 때문.



1. (qualitative) CHK_ACCT: 당좌예금 계좌상태

- 0: 0 DM미만, 1: 0-200 DM미만, 2: 200 DM 이상, 3: 당좌예금계좌 없음



● 0: 계좌가 있는데 0마르크. 이런 사람들은 불량고객일 거라고 예상.

● 3: 아예 예금 계좌가 없음.



2. (numeric) DURATION: 신용거래 개월수

● 연속 변수. 거기 ㄱ간이 어느정도 있었는지 중요.


3. (qualitative) HISTORY: 신용기록

- 0: 신용거래 없음, 1: 기한 내 변제, 2: 변제 기한 남음, 

- 3: 연체 이력있음, 4: 신용불량자


● 기존에 갚았고 안 갚았고의 입장에서. 충분히 개연성 있는 변수.


4. (qualitative) PURPOSE: 신용목적

- 1: NEW_CAR, 2:	USED_CAR, 3: FURNITURE, 

- 4: RADIO/TV, 5:	EDUCATION, 6:	RETRAINING



● 어떤 목적으로 차를 빌리려 했는지에 따라서도 결과가 달랐다. (retraining: 자기계발)

● 보기 항목을 보면 소비 측면, 재투자 측면, 자기 계발 지향적인 특성이 있다. 

● 소비 지향적인 고객은 리스크가 있어 상환능력을 의심해볼 수 있는 지점이 있다. 



5. (numeric) AMOUNT: 신용금액

6. (qualitative) SAV_ACCT: 보통예금 계좌 평균잔고

- 0: 100DM미만, 1: 100~500DM미만, 2: 500~1000DM미만, 

- 3: 1,000DM이상, 4: 모름/보통예금 계좌 없음


------------------------------위는 금융 관련된 변수라면 아래부터는 개인 정보-----


7. (qualitative) EMPLOYMENT: 현직장 재직기간

- 0: 무직, 1: 1년 미만, 2: 1~4년 미만, 3: 4~7년 미만, 4: 7년 이상

8. (numeric) INSTALL_RATE: 가처분 소득대비 할부금 이자율

9. (qualitative) PERSONAL_STATUS : 결혼상태

- 1: MALE_DIV, 2:	MALE_SINGLE, 3:	MALE_MAR_or_WID


● 가정이 안정적인지 아닌지에 따라 상환 능력 달라질 수 있다는 해석.


10. (qualitative) CO-APPLICANT: 공동 신청자, 0: 아니오, 1: 예

11. (qualitative) GUARANTOR: 보증인 유무, 0: 아니오, 1: 예

12. (qualitative) 현 주소지 거주기간: PRESENT_RESIDENT

- 0: 1년이하, 1: 1~2년 이하, 2: 2~3년 이하, 3: 4년 이상


● 자주 거주지 이전하는게 안정성이 있는지 없는지.


13. (qualitative) REAL_ESTATE: 부동산 보유, 0: 아니오, 1: 예


● 담보성의 측면


14. (qualitative) PROP_UNKN_NONE: 보유재산 유무, 0: 아니오, 1: 예

15. (numeric) AGE: 연령

16. (qualitative) OTHER_INSTALL: 타할부거래 유무, 0: 아니오, 1: 예

17. (qualitative) RENT: 임대주택 유무, 0: 아니오, 1: 예

18. (qualitative) OWN_RES: 주택 유무, 0: 아니오, 1: 예

19. (numeric) NUM_CREDITS: 현재 대출건수

20. (qualitative) JOB: 직종

- 0:무직/비숙련직 비정규직, 1: 비숙련 정규직, 

- 2: 숙련직/작업감독, 3: 관리직/자영업자/전문직, 책임자

21. (numeric) NUM_DEPENDENTS: 부양가족수

22. (qualitative) TELEPHONE: 전화가입 유무, 0: 아니오, 1: 예


● 이 당시에는 전화가입 유무도 개인의 경제력을 평가할 수 있는 지표이기도 했다.

● 요즘에는 회원권 여부, 문화센터나 스포츠 센터를 다니고 있는지로 대체해서 볼 수 있다.


23. (qualitative) FOREIGN: 외국인 근로자 여부, 0: 아니오, 1: 예

24. (qualitative) RESULT: 우량신용도 여부, 0: 아니오, 1: 예



<br>

---------------코딩------------------------------------------------------------------------------------------------------

<br>

## 1. 가상 데이터셋 만들기

<br>

<a id="1_1"></a>

### (1) 가상 데이터셋 만들기1

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="1_2"></a>
### (2) 변수컬럼 1개를 이용한 예측변수 행렬 생성: stats::model.matrix()

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="1_3"></a>
### (3) 변수컬럼 1개를 이용한 예측변수 행렬 생성: model.matrix()

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="1_4"></a>
### (4) 모든 예측변수 투입해 예측변수 행렬 생성: model.matrix() / usefull::build.x()

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="1_5"></a>
### (5) 가상 데이터셋 만들기2

<br>

<br><br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="2"></a>

## 2. 데이터 로딩 & 정제 & 탐색

<br>

<a id="2_1"></a>

### (1) 데이터 생성 및 변수컬럼명 소문자로 변경

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="2_2"></a>

### (2) 범주형 변수컬럼에 대한 팩터형 작업

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="2_3"></a>
### (3) 연속형 변수컬럼에 대한 스케일링 작업

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="2_4"></a>

### (4) 변수컬럼별 객체타입확인

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="2_5"></a>

### (5) 데이터 분할

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="2_6"></a>

### (6) 분류규칙에 가장 영향을 많이 미치는 변수 탐색

<br>

<br><br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="3"></a>

## 3. 로지스틱 회귀분석을 이용한 분류

<br>

<a id="3_1"></a>

### (1) 로지스틱회귀 모델 피팅: stats::glm() / summary()

<br>


<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="3_2"></a>

### (2) 로지스틱분석에 사용한 투입변수 중요도 계산: caret::varImp()

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="3_3"></a>

### (3) 단계별 투입모형: step()

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="3_4"></a>

### (4) 도출된 로지스틱 분류모델의 훈련데이터에 대한 우량/불량 확률계산

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="3_5"></a>

### (5) 로지스틱 분류모델의 훈련데이터 분류값과 실제 분류값 교차비교

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="3_6"></a>

### (6) 로지스틱 분류모델의 훈련데이터에 대한 분류결과 정확성(Accuracy) 평가

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="3_7"></a>

### (7) 도출된 로지스틱 분류모델의 검증데이터에 대한 우량/불량 확률계산

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="3_8"></a>

### (8) 로지스틱 분류모델의 학습데이터와 검증데이터 분류정확성 비교

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="3_9"></a>

### (9) 로지스틱모델의 성능 종합평가: 혼동표

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="3_10"></a>

### (10) 로지스틱모델의 성능 종합평가: ROC curve 그래프

<br>

<br><br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="4"></a>

## 4. 고전적 의사결정나무를 이용한 분류

<br>

<a id="4_1"></a>

### (1) 의사결정나무 모델 피팅: rpart::rpart()

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="4_2"></a>

### (2) 의사결정나무의 분류모델 도출결과 확인

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="4_3"></a>

### (3) 의사결정나무 분석에 사용한 투입변수 중요도 계산

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="4_4"></a>

### (4) 의사결정나무의 가지치기(prunning): 복잡성 파라미터 확인 -> 나무가지치기

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="4_5"></a>

### (5) 훈련데이터 성과변수 비교

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="4_6"></a>

### (6) 의사결정나무 훈련데이터 정확성 평가

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="4_7"></a>

### (7) 의사결정나무 검증데이터 정확성 평가

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="4_8"></a>

### (8) 의사결정나무 모델의 훈련데이터와 검증데이터 분류정확성 비교

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="4_9"></a>

### (9) 의사결정나무 모델의 성능 종합평가: 혼동표

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="4_10"></a>

### (10) 의사결정나무 모델의 성능 종합평가: ROC curve 그래프

<br>

<br><br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="5"></a>

## 5. 랜덤포레스트를 이용한 분류

<br>

<a id="5_1"></a>

### (1) 랜덤포레스트 분석: randomForest::randomForest()

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="5_2"></a>

### (2) 랜덤포레스트 모델 분석 결과 확인

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="5_3"></a>

### (3) 랜덤포레스트 파라미터 튜닝 

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="5_4"></a>

### (4) 튜닝 후 랜덤포레스트 피팅

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="5_5"></a>

### (5) 랜덤포레스트 규칙에 활용된 변수들의 상대적 중요성 분석

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="5_6"></a>

### (6) 랜덤포레스트 훈련데이터 정확성 평가

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="5_7"></a>

### (7) 랜덤포레스트 검증데이터 정확성 평가

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="5_8"></a>

### (8) 랜덤포레스트 모델의 훈련데이터와 검증데이터 분류정확성 비교

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="5_9"></a>

### (9) 랜덤포레스트 모델의 성능 종합평가: 혼동표

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="5_10"></a>

### (10) 랜덤포레스트 모델의 성능 종합평가: ROC curve 그래프

<br>

<br><br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

###### <참고 문헌> 

<br>

1. 최점기 박사님 강의 <br>