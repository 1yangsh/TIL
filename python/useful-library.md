### 유용한 표준 라이브러리



- itertools

  - 반복되는 형태의 데이터를 처리하기 위한 기능들을 제공

  - 순열과 조합 라이브러리는 코딩테스트에서 자주 사용됨

    

- heapq

  - 힙(Heap) 자료구조를 제공

  - 일반적으로 우선순위 큐 기능을 구현하기 위해 사용

    

- bisect

  - 이진 탐색 기능(Binary Search) 제공

    

- collections 

  - 덱(deque), 카운터(Counter) 등의 유용한 자료구조를 포함

    

- math

  - 필수적인 수학적 기능을 제공
  - 팩토리얼, 제곱근, 최대공약수(GCD), 삼각함수 관련 함수부터 파이(pi)와 같은 상수를 포함





---





#### 자주 사용되는 내장 함수



- sum()

- min(), max()

- eval()   #

  ```python
  result = eval("(3+5)*7")
  print(result)
  > 56  
  ```

  

- sorted()

  - 각 원소를 정렬한 결과를 낸다(오름차순)
  - key로 정렬 기준을 정렬하게 한다

  ```python
  array = [('홍길동', 35), ('이순신', 75), ('아무개', 50)]
  result = sorted(array, key=lambda x: x[1], reverse=True)
  print(result)
  > [('이순신', 75), ('아무개', 50), ('홍길동', 35)] # value값 기준으로 내림차순 정렬
  ```



---



#### itertools



- 순열
  - 서로 다른 n개에서 서로 다른 r개를 선택하여 일렬로 나열하는 것

```python
from itertools import permutations
data = ['A', 'B', 'C']
result = list(permutations(data, 3)) # 모든 순열 구하기
print(result)
> [('A','B','c'), ('A', 'C', 'B'), ... , ('C', 'B', 'A')]
```





- 조합
  - 서로 다른 n개에서 순서에 상관 없이 서로 다른 r개를 선택하는 것

```python
form itertools import combinations
data = ['A', 'B', 'C']
result = list(permutations(data, 2)) # 2개를 뽑는 모든 조합 구하기
print(result)
> [('A', 'B'), ('A', 'C'), ('B', 'C')]
```



- 중복 순열

```python
from itertools import product  # 중복을 허용하여 모든 순열 구하기
```

- 중복 조합

```python
from itertools import combinations_with_replacement  # n개를 뽑는 모든 조합 구하기
```





---



#### Collections



- Counter
  - collections 라이브러리
  - 내부의 원소가 몇번씩 등장했는지 알려줌

```python
from collections import Counter
counter = Counter(['red', 'blue', 'blue', 'red', 'blue', 'red', 'green'])
print(counter['blue'])  # 'blue'가 등장한 횟수 출력
```







---



#### math



- gcd()
  - 최대 공약수

```python
import math

# 최소 공배수(LCM)를 구하는 함수
def lcm(a, b):
    return a * b // math.gcd(a, b)

print(math.gcd(21, 14)) # 최대 공약수(GCD) 계산
print(lcm(21, 14))  # 최소 공배수(LCM) 계산
```



