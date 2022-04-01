# 1.서울시 CCTV 데이터 분석



## 가설

인구수가 많은 자치구 일수록 범죄의 위험이 늘어날테니 CCTV의 갯수도 더 많을것이다.



 ## 데이터 불러오기

#### 1.cctv 데이터 불러오고 데이터시트 다듬기

```python
CCTV_FILE = '01. CCTV_in_Seoul.csv'
POP_FILE = '01. population_in_Seoul.xls'

import pandas as pd

df_cctv = pd.read_csv(CCTV_FILE)
df_cctv.head()
```

**일단 cctv 데이터시트를 먼저 확인한다**

<img src="Seoul_CCTV_Analysis_Practice.assets/image-20220401232330101.png" alt="image-20220401232330101" style="zoom: 80%;" />



위 사진처럼 cctv 데이터 시트가 잘 출력되었다. 근데 자치구 컬럼의 컬럼명이 `기관명`으로 되어있다. 컬럼이름을 재설정해준다.

```python
df_cctv = df_cctv.rename(
    columns={'기관명' : '구별'}
)
df_cctv.head()
```



<img src="Seoul_CCTV_Analysis_Practice.assets/image-20220401232530509.png" alt="image-20220401232530509" style="zoom:80%;" />



#### 2.서울시 인구수 데이터 시트를 가져오고 다듬기

이제 서울시의 인구수 데이터 시트를 가져올껀데 엑셀파일형식이라 colab에선 엑셀을 불러오는 버전이 낮아서 오류가 뜨니 버전 업그레이드 후에 엑셀파일을 불러온다.

```python
!pip install xlrd==1.2.0
df_pop_seoul = pd.read_excel(POP_FILE)
df_pop_seoul.head()
```

![image-20220401233522374](Seoul_CCTV_Analysis_Practice.assets/image-20220401233522374.png)



필요한 데이터는 자치구별로 총 인원수. 즉, 합계만 필요하나 남자,여자 성별로 구분된 인구수의 데이터는 필요없으니 다듬어주고 **자치구(B),인구수 총합계(D), 한국인수 총합계(G), 외국인수 총합계(J), 65세이상 고령자의 총합계(N)**의 시리즈들만 가져오고 나머지는 지워준다.



```python
df_pop_seoul=pd.read_excel(
    POP_FILE,
    header=2,
    usecols='B,D,G,J,N'
)
df_pop_seoul.head()
```

![image-20220401234137939](Seoul_CCTV_Analysis_Practice.assets/image-20220401234137939.png)



**보기쉽게 컬럼명을 변경해준다.**

```python
df_pop_seoul.columns = ['구별','인구수','한국인','외국인','고령자']
df_pop_seoul.head()
```

![image-20220401234232751](Seoul_CCTV_Analysis_Practice.assets/image-20220401234232751.png)



0번째 `합계` 행은 필요없는 행이니 지워준다. `drop` 함수를 이용한다.

```python
df_pop_seoul=df_pop_seoul.drop(0,axis=0)
df_pop_seoul.head()
```

![image-20220401234516570](Seoul_CCTV_Analysis_Practice.assets/image-20220401234516570.png)



데이터시트에 `NAN` 값이 없는지 확인해준다.

```python
df_pop_seoul.info()
```

```python
<class 'pandas.core.frame.DataFrame'>
Int64Index: 26 entries, 1 to 26
Data columns (total 5 columns):
 #   Column  Non-Null Count  Dtype  
---  ------  --------------  -----  
 0   구별      25 non-null     object 
 1   인구수     25 non-null     float64
 2   한국인     25 non-null     float64
 3   외국인     25 non-null     float64
 4   고령자     25 non-null     float64
dtypes: float64(4), object(1)
memory usage: 1.2+ KB
```



모든 컬럼마다 `NAN` 값이 한개씩 있는걸 보니 하나의 행 전체가 다 `NAN` 값일 가능성이 높아보인다. 확인해보면

```python
df_pop_seoul[df_pop_seoul['구별'].isnull()]
```

![image-20220401234902625](Seoul_CCTV_Analysis_Practice.assets/image-20220401234902625.png)



26번 행이 `NAN` 값으로만 채워진걸 확인할수있다. `NAN` 값으로 채워진 데이터는 의미없는 데이터이니 지워준다. `dropna` 함수를 이용한다.

```python
df_pop_seoul=df_pop_seoul.dropna(axis=0)
df_pop_seoul.isnull().sum()
```

```python
구별     0
인구수    0
한국인    0
외국인    0
고령자    0
dtype: int64
```



#### 3.의미있을것같은 데이터를 데이터시트에 추가해준다





## 데이터 살펴보기

