# **빅데이터분석기사 실기 연습**

모든 문제 출처 : https://www.kaggle.com/datasets/agileteam/bigdatacertificationkr


- ### 결측치 확인
```python
print(df.isnull().sum())
```

- ### 결측치 채우기
```python
df = df.fillna(df['칼럼명'].mean()) # mean, median, min, max 등

# 뒤에 나오는 값으로 채우기
df = df.fillna(method='bfill') # 앞에 값으로 채우기 method=ffill
```

- ### 결측치 제거
```python
df = df.dropna(subset=['칼럼명']) # axis=0 or 1 -> 결측치가 있는 row, col을 drop
```

- ### Pandas DataFrame 정렬
```python
df = df.sort_values('칼럼명', ascending=True) # 내림차순: ascending=False
```

- ### 칼럼 삭제
```python
df = df.drop(['칼럼명'], axis=1)
```

- ### Min-Max Scale
```python
from sklearn.preprocessing import minmax_scale
df['칼럼명'] = minmax_scale(data['칼럼명'])
```

- ### log1p 스케일링
```python
import numpy as np
df['칼럼명'] = np.log1p(df['칼럼명'])
# x가 0이면 y가 -inf로 수렴하기 때문에 모든 값에 +1을 한 후 log 변환을 하는 log1p 사용
```

- ### IQR 구하기 (Q1, Q3)
```python
# 방법1
import pandas as pd
Q1 = df['칼럼명'].quantile(25)
Q3 = df['칼럼명'].quantile(75)

# 방법2
import numpy as np
Q1 = np.percentile(df['칼럼명'], 25)
Q3 = np.percentile(df['칼럼명'], 75)

IQR = Q3 - Q1
# 이상치
x < Q1 - 1.5 * IQR
x > Q3 + 1.5 * IQR
```

- ### 왜도, 첨도 (skewness, kurtosis)
```python
skew = df['칼럼명'].skew() # 왜도
kurt = df['칼럼명'].kurt() # 첨도
```

- ### datetime으로 type 변경 및 연, 월, 일, 요일 추출
```python
import pandas as pd
df['Date'] = pd.to_datetime(df['Date'])

df['year'] = df['Date'].dt.year
df['month'] = df['Date'].dt.month
df['day'] = df['Date'].dt.day
df['dayofweek'] = df['Date'].dt.dayofweek # Monday=0 ~ Sunday=6
```


note:
T1-19 시계열 데이터3 Expected Question 다시 풀기


## **유형 2**
1. EDA (크기, 칼럼, 널값 등 확인)
2. 널값 제거
3. 이상치 제거
4. 라벨 인코딩
5. train, validation split
6. 모델 학습
7. validation을 활용하여 평가
8. 모델 및 하이퍼파라미터 선택
9. 선택된 모델로 전체 train 데이터 학습
10. predict 후 csv로 저장 (index=False)

- ### Label Encoding (문자열 자료를 숫자로 바꿀때 활용 가능)
```python
from sklearn.preprocessing import LabelEncoder

le = LabelEncoder()
cols = ['칼럼명1', '칼럼명2'] # 변경할 문자열 칼럼 명

for col in cols:
  X_train[col] = le.fit_transform(X_train[col])
  X_test[col] = le.fit_transform(X_test[col])
```

- ### 데이터 세트 분리 (train_test_split)
```python
from sklearn.model_selection import train_test_split

x_train, x_val = train_test_split(X_train, test_size=0.2, random_state=42)
```

- ### Random Forest
```python
# 분류(Classifier)
from sklearn.ensemble import RandomForestClassifier

model = RandomForestClassifier(max_depth=4, n_estimators=50, random_state=10)
### Hyperparameter ###
# 1. n_estimators : Tree 개수 (default=100)
# 2. max_depth : 최대 깊이 (default=None)
# 3. randoma_state : 지정해줘야 매번 같은 결과 도출

model.fit(X_train, y_train['칼럼명']) # y_train에 id가 존재하는 경우 해당 칼럼을 학습 데이터에 넣지 않기 위해 칼럼 지정
print(model.score(X_train, y_train['칼럼명']))

pred = model.predict(X_test) # predict_proba : 확률값

# 회귀(Regressor)
from sklearn.ensemble import RandomForestRegressor

model = RandomForestRegressor(random_state=10)
model.fit(X_train, y_train['칼럼명'])
print(model.score(X_train, y_train['칼럼명']))

pred = model.predict_proba(X_test)
```

- ### XGBoost
```python
from xgbooste import XGBClassifier

model = XGBClassifier(n_estimators=50, random_state=10)
### Hyperparameter ###
# 1. n_estimators : Tree 개수 (default=100)
# 2. max_depth : 최대 깊이 (default=None)
# 3. randoma_state : random seed

model.fit(X_train, y_train['칼럼명'])
print(model.score(X_train, y_train['칼럼명']))

pred = model.predict(X_test) # predict_proba : 확률값
```
