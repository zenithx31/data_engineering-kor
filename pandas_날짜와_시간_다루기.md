
# 📚 Pandas에서 날짜와 시간 다루기: Timestamp, DatetimeIndex, Period

Pandas는 날짜와 시간 관련 데이터를 다루기 위한 여러 함수를 제공합니다.

Timestamp, DatetimeIndex, Period와 같은 객체들은 날짜와 시간 데이터를 처리하고 분석하는 데 큰 도움이 됩니다. 이번 글에서는 이들 객체와 관련된 주요 기능들을 알아보겠습니다.

---

## 📚 Timestamp

Timestamp는 Pandas에서 하나의 날짜와 시간을 표현하는 객체입니다. 

datetime64와 유사하지만, 보다 편리한 기능을 제공합니다. Timestamp 객체는 다양한 단위로 날짜와 시간을 생성할 수 있습니다. 기본적으로 나노초(ns) 단위까지 지원합니다.

### 예시 1: 하나의 날짜만 사용할 때

```python
import pandas as pd

example1 = pd.Timestamp(1234.5678)  # 나노초 단위 (디폴트)
example2 = pd.Timestamp(1234.5678, unit='D')  # 일 단위

print(example1)
print(example2)
```

**출력:**

```
1970-01-01 00:00:00.000001234
1973-05-19 13:37:37.920000
```

example1: 1234.5678을 나노초 단위로 해석하여 1970-01-01에서 시작되는 시간을 출력합니다.

example2: 1234.5678을 일 단위로 해석하여 1973-05-19의 시간을 출력합니다.

---

## 📚 DatetimeIndex

DatetimeIndex는 여러 날짜를 다룰 때 유용한 객체입니다. 여러 날짜를 하나의 인덱스에서 관리할 수 있게 도와줍니다.​

### 예시 2: 단일 날짜와 여러 날짜를 DatetimeIndex로 변환하기

```python
import pandas as pd

example1 = pd.to_datetime('2023-11-1 12')  # 단일 날짜
example2 = pd.to_datetime(['2023-1-1', '2023-10-31'])  # 여러 날짜

print(example1)
print(example2)
```

**출력:**

```
2023-11-01 12:00:00
DatetimeIndex(['2023-01-01', '2023-10-31'], dtype='datetime64[ns]', freq=None)
```

example1: 2023-11-1 12는 단일 날짜로 변환됩니다.

example2: 여러 날짜들을 DatetimeIndex 객체로 변환하여 다룰 수 있습니다.

---

### 예시 3: 특정 기간의 날짜를 자동 생성하기

`pd.date_range()`를 사용하여 특정 기간 동안의 날짜를 생성할 수 있습니다.

```python
import pandas as pd

example = pd.date_range('2023-1-1', '2023-10-31')
print(example)
```

**출력:**
304개의 날짜가 2023년 1월 1일부터 10월 31일까지 하루 단위로 생성됩니다.

---

## 📚 Period와 PeriodIndex

Period는 특정 시점이 아닌 기간을 포괄하는 개념입니다.

예를 들어, 한 달 또는 1년의 기간을 표현할 때 사용됩니다. 반면, Timestamp는 하나의 특정 시점을 의미합니다.

### 예시 4: Period 생성하기

```python
import pandas as pd

example1 = pd.Period('2023-11-1')  # 기본 단위는 'D' (일 단위)
example2 = pd.Period('2023-11-1', freq='M')  # 월 단위
example3 = pd.Period('2023-11-1', freq='Y')  # 연 단위

print(example1)
print(example2)
print(example3)
print(type(example1))
```

**출력:**

```
2023-11-01
2023-11
2023
<class 'pandas._libs.tslibs.period.Period'>
```

example1: 2023-11-1은 기본적으로 일 단위(D)로 해석됩니다.

example2: 2023-11-1은 월 단위(M)로 해석되어 2023-11로 출력됩니다.

example3: 2023-11-1은 연 단위(Y)로 해석되어 2023으로 출력됩니다.

---

### 예시 5: PeriodRange 생성하기

Period를 사용하여 여러 기간을 생성할 수도 있습니다.

`pd.period_range()`는 일정한 주기나 범위 내에서 Period 객체들을 생성하는 데 유용합니다.

```python
import pandas as pd

example1 = pd.period_range('2023-1', '2023-11')  # 기본적으로 월 단위
example2 = pd.period_range('2023-1', '2023-11', freq='M')  # 월 단위
example3 = pd.period_range('2022-1', '2023-11', freq='Y')  # 연 단위

print(example1)
print(example2)
print(example3)
print(type(example1))
```

**출력:**

```
PeriodIndex(['2023-01', '2023-02', ..., '2023-11'], dtype='period[M]', freq='M')
PeriodIndex(['2023-01', '2023-02', ..., '2023-11'], dtype='period[M]', freq='M')
PeriodIndex(['2022', '2023'], dtype='period[Y]', freq='Y')
<class 'pandas._libs.tslibs.period.Period'>
```

example1: 2023년 1월부터 11월까지 월 단위로 생성된 기간들입니다.

example2: 월 단위(M)로 2023년 1월부터 11월까지 생성됩니다.

example3: 연 단위(Y)로 2022년과 2023년의 기간이 생성됩니다.
