
# 📚 시리즈(Series)

시리즈는 1차원 배열로, 각 원소에 인덱스를 부여하여 데이터를 저장합니다.

```python
import pandas as pd

# 시리즈 생성
sr = pd.Series([10000, 20000, 30000], index=['햄버거', '치킨', '피자'])

# 시리즈 출력
print(sr)

# 값과 인덱스 정보 출력
print('Value : {}'.format(sr.values))
print('Index : {}'.format(sr.index))
```

**결과:**
```
햄버거    10000
치킨     20000
피자     30000
dtype: int64

Value : [10000 20000 30000]
Index : Index(['햄버거', '치킨', '피자'], dtype='object')
```

- `sr.values`: 시리즈의 값들을 출력합니다.
- `sr.index`: 시리즈의 인덱스 목록을 출력합니다.

---

# 📚 데이터프레임(DataFrame)

데이터프레임은 2차원 배열로, 행과 열로 구성됩니다. 엑셀의 표와 유사한 구조로 데이터를 다룰 수 있습니다.

```python
import pandas as pd

# 2차원 리스트로 데이터프레임 생성
values = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
index = ['one', 'two', 'three']
columns = ['A', 'B', 'C']

df = pd.DataFrame(values, index=index, columns=columns)

# 데이터프레임 출력
print(df)

# 값, 인덱스, 열 출력
print('Value : {}'.format(df.values))
print('Index : {}'.format(df.index))
print('Columns : {}'.format(df.columns))
```

**결과:**
```
       A  B  C
one    1  2  3
two    4  5  6
three  7  8  9

Value : [[1 2 3]
 [4 5 6]
 [7 8 9]]
Index : Index(['one', 'two', 'three'], dtype='object')
Columns : Index(['A', 'B', 'C'], dtype='object')
```

- `df.values`: 데이터프레임의 값을 출력합니다.
- `df.index`: 데이터프레임의 행 인덱스를 출력합니다.
- `df.columns`: 데이터프레임의 열 인덱스를 출력합니다.

---

# 📚 데이터프레임 생성하기

## 1) 리스트를 사용한 데이터프레임 생성

```python
import pandas as pd

# 리스트로 데이터프레임 생성
data = [
    ['1000', 'A', 10],
    ['2000', 'B', 20],
    ['3000', 'C', 30]
]

df = pd.DataFrame(data)
print(df)
```

**결과:**
```
      0  1   2
0  1000  A  10
1  2000  B  20
2  3000  C  30
```

열 이름을 지정하려면 `columns` 파라미터를 사용합니다.

```python
df = pd.DataFrame(data, columns=['가격', '이름', '나이'])
print(df)
```

**결과:**
```
     가격 이름  나이
0  1000  A  10
1  2000  B  20
2  3000  C  30
```

## 2) 딕셔너리를 사용한 데이터프레임 생성

```python
import pandas as pd

# 딕셔너리로 데이터프레임 생성
data = {
    '가격' : ['1000', '2000', '3000'],
    '이름' : ['A', 'B', 'C'],
    '나이' : [10, 20, 30]
}

df = pd.DataFrame(data)
print(df)
```

**결과:**
```
     가격 이름  나이
0  1000  A  10
1  2000  B  20
2  3000  C  30
```

---

# 📚 데이터프레임 조회하기

```python
# 앞에서부터 n개 확인
print(df.head(2))  # 첫 2개의 행 출력

# 뒤에서부터 n개 확인
print(df.tail(2))  # 마지막 2개의 행 출력

# 특정 열 확인
print(df['가격'])  # '가격' 열 출력
```

---

# 📚 외부 데이터 읽기

Pandas는 외부 파일(예: CSV 파일)을 쉽게 읽어들여 데이터프레임으로 변환할 수 있습니다.

```python
# 외부 CSV 파일 읽기
df = pd.read_csv('example.csv')
print(df)
```
