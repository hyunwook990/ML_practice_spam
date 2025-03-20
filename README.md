# SKN011-ML-6Team
- 이현민
- 정현욱
- 홍성욱

  <br>
  
📅기간: 2025.03.14 ~ 2025.03.21
---
# 📧스팸메일 구별을 위한 탐색적 데이터 분석 (EDA)
<br>

## 목차
1. 프로젝트 목표 및 기대효과
2. 머신 러닝 절차(파이프라인) 설명
    1) EDA
    2) 데이터 분류
    3) 모델 선정
    4) 학습 및 평가
    5) 피드백
2. 결론
   
<br>

---
## 1. 🎯 프로젝트 목표 및 기대효과
 * 스팸 메일의 패턴 분석
 * 분석을 바탕으로 스팸 의심 메일 분류


<p align="center">
  <img src="./readme_image/spam_filter.jpg" height="300" width="450">
</p>

<div align="center">
  그림 1.1 스팸 메일 분류 이해를 도울 그림
</div>
<br>

---
## 2. 📜 머신 러닝 절차(파이프라인) 설명
 * 절차를 따라가며 프로젝트 진행상황 설명

### 1) 🚫 EDA(Exploratory Data Analysis)
 * 데이터 전처리,인코딩 등
 * 이전 프로젝트의 진행내역 사용
<p align="center">
  <img src="./readme_image/word_heatmap_2.png" height="350" width="350">
</p>

<div align="center">
  그림 2.1.1 EDA 프로젝트 개요 
</div>
<br>

### 2) 💡 데이터 분류
 * 학습 데이터, 테스트 데이터를 분류 (보통 7:3)
 * 주요 매개변수
  * stratify
### 3) ⁉️ 데이터의 주요 문제

---
## 3. 💻 탐색적 데이터 분석 (EDA) 수행

### 1) 🫥 결측치 탐색  

* 각 데이터 속성의 결측치 확인 (결측치가 없을 경우 4601 non-null)

<p align="center">
  <img src="./readme_image/결측치 확인 인포.png" height="550" width="400">
</p>

<div align="center">
  그림 3.1 데이터 결측치 확인을 위한 Pandas의 <code>DataFrame.info()</code> 활용
</div>
<br>

* 결측치는 단어가 존재하지 않았을 가능성이 높아 값을 0으로 대체

<p align="center">
  <img src="./readme_image/결측치 제거 후 확인 인포.png" height="550" width="400">
</p>

<div align="center">
  그림 3.2 결측치가 제거된 데이터프레임
</div>
<br>

### 2) 👨‍👩‍👧‍👦 데이터 군집화

* 스팸 구별을 위한 데이터 속성이 과다함
* 같은 메일에서 자주 함께 사용되는 단어들을 군집화하여 데이터 속성 축소

<p align="center">
  <img src="./readme_image/word_heatmap_1.png" height="650" width="700">
</p>

<div align="center">
  그림 3.3 단어 빈도 속성들 간의 피어슨 상관 계수 heatmap 시각화 이미지
</div>
<br>

<p align="center">
  <img src="./readme_image/word_heatmap_2.png" height="500" width="600">
</p>

<div align="center">
  그림 3.4 피어슨 상관 계수가 높은 단어 그룹 예시
</div>
<br>
  
* 단어 빈도 속성: K-means 클러스터링<sup>*</sup>을 활용하여 48개의 속성을 5개의 그룹으로 구분
* 군집화된 단어들을 시각화하기 위해 주성분 분석 (PCA)<sup>**</sup>으로 데이터 속성을 2개로 축소 후 산점도(scatter) 그래프 작성
  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<sup>*</sup>K-means 클러스터링: 데이터를 K개의 군집으로 나누어 중심점을 기준으로 반복적으로 최적화  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<sup>**</sup>주성분 분석 (PCA): 고차원 데이터를 저차원으로 변환하여 주요 정보만 보존

<p align="center">
  <img src="./readme_image/scatter.png" height="450" width="600">
</p>

<div align="center">
  그림 3.5 상관관계가 높은 단어들을 5개의 그룹으로 군집화한 뒤 시각화
</div>
<br>


* 특수문자 빈도 속성: 6개의 속성을 1개의 그룹으로 통합
* 대문자 연속 속성: 각각이 다른 정보를 나타낼 수 있으므로 유지
* 최종적으로 47개의 데이터 속성을 9개로 축소

<p align="center">
  <img src="./readme_image/그루핑 후 데이터 프레임.png" height="150" width="1000">
</p>

<div align="center">
  그림 3.6 단어/특수문자의 빈도 속성 데이터 군집화 후 데이터 프래임 개요
</div>
<br>


### 3) 🥸 데이터 변환 및 피처 엔지니어링

* 단어들의 빈도 속성을 군집화하였음에도 불구하고 속성값이 0인 데이터가 과다
* 값이 0인 데이터가 많을 경우 데이터의 불균형이 심화될 가능성 존재
* 값이 0인 데이터가 30%를 초과할 경우 임계값을 설정하여 데이터 값을 0과 1로 이진화
* 데이터의 임계값은 이상치를 제외한 값들의 평균 사용
* 값이 0인 데이터가 30%를 초과하는 word_group2, group3, group5의 평균값 확인을 위해 박스(box) 플롯 작성

<p align="center">
  <img src="./readme_image/box_group2.png" height="530" width="600">
</p>  
<div align="center">
  그림 3.7 word_group2_frq 속성의 박스 플롯
</div>
<br>

<p align="center">
  <img src="./readme_image/box_group3.png" height="530" width="600">
</p>  
<div align="center">
  그림 3.8 word_group3_frq 속성의 박스 플롯
</div>
<br>

<p align="center">
  <img src="./readme_image/box_group5.png" height="530" width="600">
</p>  
<div align="center">
  그림 3.9 word_group5_frq 속성의 박스 플롯
</div>
<br>

* 박스 플롯을 통해 group2, group3, group5의 데이터의 불균형이 심함을 확인할 수 있음

<p align="center">
  <img src="./readme_image/전처리 후 그루핑 데이터 프레임.png" height="150" width="1000">
</p>

<div align="center">
  그림 3.10 데이터 속성 군집화 후 데이터 프래임 개요
</div>
<br>


### 4) ⚖️ 데이터 스케일링

* 전체적인 데이터 속성들의 스케일을 통일시키기 위해 `StandardScaler()` 수행

<p align="center">
  <img src="./readme_image/정규화 후 그루핑 데이터 프레임.png" height="150" width="1000">
</p>

<div align="center">
  그림 3.11 데이터 정규화 후 데이터 프래임 개요
</div>
<br>

---
## 4. 📜 결론

**- 본 프로젝트에서는 스팸메일 데이터를 탐색적으로 분석(EDA)하고, 효과적인 데이터 전처리 및 피처 엔지니어링을 수행함.**

**- 스팸메일은 특정 단어의 반복적 사용, 비정상적인 문자 조합 등의 특징을 보이며, 이러한 특성이 데이터 분석을 통해 명확히 확인됨.**

**- 데이터 간 상관관계를 분석하여 연관성이 높은 속성들을 군집화하고, 이를 기반으로 보다 효율적인 데이터프레임을 구축함.**

**- 데이터 불균형 문제를 해결하기 위해 평균값을 활용하여 데이터를 이진화하고, 불균형성을 완화하는 전처리 과정을 적용함.**

**- 데이터의 결측치 제거 및 스케일링을 수행하여 향후 모델 학습 시 데이터의 균형을 유지하도록 조정함.**

**- 본 연구는 대규모 언어모델(LLM) 학습을 위한 기초 단계로서, 언어 데이터에 대한 분석 및 이해도를 증진하는 데 기여할 것으로 기대됨.**

