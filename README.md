# **빅데이터분석기사 실기 연습**

### 결측치 채우기
```python
df = df.fillna(df['칼럼명'].mean()) # mean, median, min, max 등
```

### 결측치 제거
```python
df = df.dropna(subset=['칼럼명']) # axis=0 or 1 -> 결측치가 있는 row, col을 drop
```

### Pandas DataFrame 정렬
```python
df = df.sort_values('칼럼명', ascending=True) # 내림차순: ascending=False
```

### 칼럼 삭제
```python
df = df.drop(['칼럼명'], axis=1)
```

note:
T1-19 시계열 데이터3 Expected Question 다시 풀기
