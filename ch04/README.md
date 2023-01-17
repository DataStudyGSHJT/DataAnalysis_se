# CHAPTER 04. 데이터 요약하기
## 04-1. 통계로 요약하기
* 기술통계(descriptive statistics)  
 : 자료의 내용을 압축하여 설명하는 방법 == 요약 통계(summary statistics)
### 기술통계 구하기
* describe()  
 : 수치형 열에 대한 요약 통계를 보여줌
  - count: 누락된 값을 제외한 데이터 개수  
  - mean: 평균  
  - std: 표준편차
  - min: 최솟값
  - 50%: 중앙값
  - 25%, 75%: 순서대로 늘어 놓았을 떄 25% 지점과 75% 지점에 놓인 값
  - max: 최댓값
* describe(percentiles=[0.a, 0.b, 0.c])
  - a, b, c에 위치한 값을 보여줌
* descirbe(include='데이터 타입')  
 : 데이터 타입에 해당하는 열의 기술통계를 보여줌
  - count: 누락된 값을 제외한 데이터 개수  
  - unique: 고유한 값의 개수
  - top: 가장 많이 등장하는 값
  - freq: top 행에 등장하는 항목의 빈도수
### 평균 구하기
* 평균
 : 데이터의 모든 값의 총합을 개수로 나눈 값
* mean() 메서드  
 `df[col_name].mean()`
### 중앙값 구하기
* 중앙값
 : 전체 데이터를 순서대로 늘어 놓았을 때 중앙에 위치한 값
* median() 메서드  
 `df[col_name].median()`
  - 데이터 개수가 홀수: 데이터의 가운데 값 반환
  - 데이터 개수가 짝수: 데이터의 가운데 2개의 값의 평균 반환
* 중복값 제거하고 중앙값 구하기  
 `df[col_name].drop_duplicates().median()`
### 최솟값, 최댓값 구하기
 - min() 메서드 `df[col_name].min()`
 - max() 메서드 `df[col_name].max()`
### 분위수 구하기
* 분위수(quantile)  
 : 데이터를 순서대로 늘어 놓았을 떄 이를 균등한 가격으로 나누는 기준점  
  -> 사분위수: 25%(제 1사분위수), 50%(제 2사분위수, 중앙값), 75%(제 3사분위수)
  - quantile() 메서드 `df[col_name].quantile(0.a)` or `df[col_name].quantile([0.a, 0.b, 0.c])`  
  -> interpolation(보간) : 두 지점 사이에 놓인 특정 위치 값을 구하는 방법
  - `df[col_name].quantile(0.a, interpolation='linear/midpoint/nearest/higher/lower')`  
   ㄴ linear: 두 수의 분위수에 비례한 값 사용  
   ㄴ midpoint: 분위수 상관 없이 두 수 사이의 중앙 값 사용  
   ㄴ nearest: 두 수 중에 가까운 값 사용  
   ㄴ lower/higher: 두 수 중에 더 작은/큰 값 사용
* 백분위 구하기  
 : boolean 배열을 사용한 뒤 mean()을 호출하여 평균을 구하면 비율 계산 가능
### 분산 구하기
* 분산(variance)  
 : 평균으로부터 데이터가 얼마나 퍼져있는지를 나타내는 통계량
  - var() 메서드: `df[col_name].var()`
 ### 표준편차 구하기
 * 표준편차(standard deviation)  
  : 분산의 제곱근, **평균을 중심으로 데이터가 대략 얼만큼 떨어져 분포해 있는지 표현하는 값**
   - std() 메서드: `df[col_name].std()`
 ### 최빈값 구하기
 * 최빈값(mode)  
  : 데이터에서 가장 많이 등장하는 값
  - mode() 메서드: `df[col_name].mode()`
  
## 04-2. 분포 요약하기
**matplotlib 패키지 사용** `import matplotlib.pyplot as plt`
### 산점도 그리기
* 산점도  
 : 두 변수 혹은 두 가지 특성값을 직교 좌표계에 점으로 나타내는 그래프
 - scatter() 함수: `plt.scatter([x축_좌표], [y축_좌표])`
* 투명도 조절하기
  - `plt.scatter([x축_좌표], [y축_좌표], alpha=0 ~ 1)`
    : 0에 가까울수록 투명하고 1에 가까울수록 불투명
  - 양의 상관관계: x축이 증가함에 따라 y축이 증가함
  - 음의 상관관계: x축이 증가함에 따라 y축이 감소함
### 히스토그램 그리기
* 히스토그램  **하나의 특성에 대한 분포 확인용**
 : 수치형 특성의 값을 일정 구간(bin)으로 나누어 구간 안에 포함된 데이터 개수(도수)를 막대 그래프로 그린 것
  - hist() 함수: `plt.hist([1차원_데이터], bins=n)`
  - histogram_bin_edges() 함수: `np.histogram_bin_edges([1차원 데이터], bins=n)`
* 구간 조정하기
  - y축 구간 조정
    - 로그 스케일(log scale) `plt.yscale('log')
     : 한 구간의 도수가 너무 커서 다른 구간의 도수가 표시되지 않는 경우 **y축**을 로그 스케일로 바꿔 해결  
      -> **로그 스케일로 변환된 그래프를 볼 때 실제 데이터는 훨씬 더 격차가 크다는 점을 기억할 것**
  - x축 구간 조정
    - bins 매개변수
    - `plt.xscale('log')`
### 상자 수염 그림 그리기
* 상자 수염 그림 **여러 특성에 대한 분포 확인용**
 : 최솟값, 제 1/2/3 사분위수, 최댓값을 사용해 데이터를 요약한 그래프
  - boxplot() 함수: `plt.boxplot(df[[col1, col2, ...]])`
* 상자 수염 그림 수평으로 그리기
  - vert 매개변수: `plt.boxplot(df[[col1, col2, ...]], vert=False)`  
    ㄴ 기본값은 True(수직) -> False(수평)로 변경 가능
* 수염 길이 조정하기
  - whis 매개변수: `plt.boxplot(df[[col1, col2, ...]], whis=n)`  
    ㄴ 기본값은 1.5배 -> n배로 변경 가능
