
# Numpy를 사용해서 datetime64 날짜 및 시간 함수 다루기

## 📚 datetime64란?

datetime64는 Numpy에서 날짜와 시간을 고해상도로 다루기 위한 데이터 타입입니다.

이를 통해 우리는 날짜와 시간을 다양한 단위(예: 연, 월, 일, 초, 밀리초 등)로 표현하고 계산할 수 있습니다.

기본적으로 datetime64는 아토초(10^-18초)까지 시간을 표현할 수 있지만, Python의 표준 datetime 모듈은 마이크로초(10^-6초)까지만 다루므로, Numpy에서 제공하는 datetime64는 훨씬 더 정밀한 시간 처리가 가능합니다.

---

## 📚 Numpy 객체를 이용해 날짜 생성하기

Numpy에서는 datetime64 객체를 사용하여 날짜나 시간을 생성할 수 있습니다.

이때, 날짜와 시간은 다양한 단위로 지정할 수 있습니다.

```python
import numpy as np

example1 = np.datetime64('2023-11-08')  # '2023-11-08' 날짜 생성
example2 = np.datetime64(100000, 'ns')  # 100000 나노초
example3 = np.datetime64(100000, 's')   # 100000 초
example4 = np.datetime64(100000, 'D')   # 100000 일

print(example1)  # 출력: 2023-11-08
print(example2)  # 출력: 1970-01-01T00:00:00.000100000
print(example3)  # 출력: 1970-01-02T03:46:40
print(example4)  # 출력: 2243-10-17
```

- example1: 날짜 형식으로 2023-11-08을 생성합니다.
- example2: 100,000 나노초를 기준으로 날짜/시간을 생성합니다.
- example3: 100,000초는 1970년 1월 2일 03:46:40에 해당합니다.
- example4: 100,000일은 약 2243년 10월 17일에 해당하는 날짜입니다.

---

## 📚 Numpy의 array() 함수를 사용하여 날짜 생성하기

Numpy에서는 datetime64 타입을 이용해 배열을 만들 수 있습니다.

날짜를 배열로 생성할 때는 날짜 단위를 명시할 수 있으며, 연도, 월, 일 등의 단위로 날짜를 표현할 수 있습니다.

```python
import numpy as np

example1 = np.array('2023-11-08', dtype='datetime64')  # 기본 날짜
example2 = np.array('2023-11-08', dtype='datetime64[D]')  # 일 단위
example3 = np.array('2023-11-08', dtype='datetime64[M]')  # 월 단위
example4 = np.array('2023-11-08', dtype='datetime64[Y]')  # 연 단위

print(example1)  # 출력: 2023-11-08
print(example2)  # 출력: 2023-11-08
print(example3)  # 출력: 2023-11
print(example4)  # 출력: 2023
```

- example1: 기본적인 날짜 형식입니다.
- example2: 'D' 단위로 지정하여, 날짜는 그대로 표현됩니다.
- example3: 'M' 단위로 지정하면, 월까지만 표시됩니다.
- example4: 'Y' 단위로 지정하면, 연도만 표시됩니다.

---

## 📚 arange()를 사용하여 날짜 범위 생성하기

np.arange() 함수를 사용하여 일정한 간격을 두고 날짜 범위를 생성할 수 있습니다.

이때 날짜 단위도 지정할 수 있습니다.

```python
import numpy as np

example1 = np.arange('2023-10', '2023-11', dtype='datetime64[D]')  # 일 단위
example2 = np.arange('2023-10', '2023-11', dtype='datetime64[W]')  # 주 단위
example3 = np.arange('2023-10', '2023-11', dtype='datetime64[M]')  # 월 단위
example4 = np.arange('2022-10', '2023-11', dtype='datetime64[Y]')  # 연 단위
example5 = np.arange('2023-11-07', '2023-11-08', dtype='datetime64[h]')  # 시 단위
example6 = np.arange('2023-11-07', '2023-11-08', dtype='datetime64[m]')  # 분 단위
example7 = np.arange('2023-11-07', '2023-11-08', dtype='datetime64[s]')  # 초 단위

print(example1)
print(example2)
print(example3)
print(example4)
print(example5)
print(example6)
print(example7)
```

- example1: 2023년 10월 1일부터 10월 31일까지 하루 단위로 생성합니다.
- example2: 2023년 9월 28일부터 10월 19일까지 주 단위로 생성합니다.
- example3: 2023년 10월 한 달 동안의 월 단위 범위를 생성합니다.
- example4: 2022년 10월부터 2023년 11월까지 연 단위 범위를 생성합니다.
- example5: 2023년 11월 7일에서 11월 8일 사이, 시 단위로 생성합니다.
- example6: 2023년 11월 7일에서 11월 8일 사이, 분 단위로 생성합니다.
- example7: 2023년 11월 7일에서 11월 8일 사이, 초 단위로 생성합니다.

---

## 📚 날짜 간 차이 구하기

두 날짜 간의 차이를 계산하려면 datetime64 객체를 뺄 수 있습니다.

이때 결과는 timedelta와 유사한 형태로 반환됩니다.

```python
import numpy as np

example = np.datetime64('2023-11') - np.datetime64('2023-01')
print(example)  # 출력: 10 months
```

두 날짜 사이의 차이는 10 months라는 결과로 출력됩니다.

datetime64는 날짜 간의 차이를 연도, 월, 일 등의 단위로 계산할 수 있습니다.
