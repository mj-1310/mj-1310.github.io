---
layout: post
title: "머신러닝 19. 지도학습: 비선형회귀 (일반화가법모형)"
author: "MJ"
categories: [science, statistical_method]
tags: [statistics, machinelearning, multicampus, bigdata_analysis_edu, CRT]
image: 
---

---
&nbsp; &nbsp; **목차**<br>
&nbsp; &nbsp; 1. [데이터셋 준비](#1)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (1) [무작위 숫자로 데이터셋 생성: runif()](#1_1)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (2) [예측변수와 반응변수 관계 시각화: plot()](#1_2)<br>
&nbsp; &nbsp; 2. [데이터탐색: 선형성에 대한 의심을 중심으로](#2)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (1) [선형회귀분석 실시: stats::lm() / summary()](#2_1)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (2) [회귀모델 성능(데이터 적합성) 파악: rcompanion::compareLM / anova()](#2_2)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (3) [회귀모델 시각화](#2_3)<br>
&nbsp; &nbsp; 3. [일반화선형분석](#3)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (1) [일반화선형모형 모델 피팅: gam::gam() / summary()](#3_1)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (2) [회귀모델 간 성능 비교: AIC() / BIC() / rcompanion::compareLM](#3_2)<br>
&nbsp; &nbsp;&nbsp;&nbsp; (3) [일반화선형분석 모델 시각화](#3_3)<br>

---

<br>	

024

<br>	

024

### 1. 독립변수와 종속변수간 선형관계가 아닌경우의 대안

- 곡선회귀(Curvilinear regression); 비선형회귀(Nonlinear regression)



● 예측선이 하나의 직선으로 나오지 않으므로 새로운 그래프 모양을 찾은게 비선형회귀.
y = a x x^2 + b 나, 거기서도 예측선이 잔차를 품지 못하면 삼차식, 사차식의 곡선의 예측선을 만든다. 이걸 다항회귀라고 한다. 

● 수학에서 x에 제곱을 씌우니까 그래프의 곡선 모양이 나오던 거에서 착안한 것. 그래서 독립변수에 제곱을 씌워서 원하는 그래프를 만드는 것. 


##### (3) 일반화가법모형(Genelizaed Additive Model)

- 각 독립변수별로 종속변화와의 관계를 나타내는 개별적인 비선형 예측식을 만들고 이들 간의 선형적 결합을 통해 예측성능을 향상시

- 다양한 함수를 사용하면서 다양한 관계를 표시할 수 있으므로 일반화(generalized)라는 이름이 붙음


● 독립변수 별로 변수의 특성이 다를 것이다. 그러므로 개별 변수의 특성에 따라 어떤 건 제곱을 통해 집어넣기도 하고, 어떤 변수는 스플라인 방식으로 설정해서 서로서로 더해서 예측하는 모형. 1번 독립변수는 이방식, 2번 독립변수는 저 방식.

● generalized: 일반화라는 건 lm함수의 조건이 정교하게 맞지 않다보니, 일반화, 조건을 회피할 수 있는 방식을 사용한 것.

<br>

---------------코딩------------------------------------------------------------------------------------------------------

<br>

<a id="1"></a>

## 1. 데이터셋 준비

<br>

<a id="1_1"></a>

### (1) 무작위 숫자로 데이터셋 생성: runif()

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="1_2"></a>

### (2) 예측변수와 반응변수 관계 시각화: plot()

<br>

<br><br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="2"></a>

## 2. 데이터탐색: 선형성에 대한 의심을 중심으로

<br>

<a id="2_1"></a>

### (1) 선형회귀분석 실시: stats::lm() / summary()

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="2_2"></a>

### (2) 회귀모델 성능(데이터 적합성) 파악: rcompanion::compareLM / anova()

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="2_3"></a>

### (3) 회귀모델 시각화

<br>

<br><br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="3"></a>

## 3. 일반화선형분석

<br>

<a id="3_1"></a>

### (1) 일반화선형모형 모델 피팅: gam::gam() / summary()

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="3_2"></a>

### (2) 회귀모델 간 성능 비교: AIC() / BIC() / rcompanion::compareLM

<br>

<br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

<a id="3_3"></a>

### (3) 일반화선형분석 모델 시각화

<br>

<br><br>

<!-- ------------------------------------------------------------------------------------------------------------------------------- -->

###### <참고 문헌> 

<br>

1. 최점기 박사님 강의 <br>