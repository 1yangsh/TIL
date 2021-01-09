#### python functions

> 유용한 함수 모음



- zip

  - 2개 이상의 분류된 값을 병렬로 묶음

  - `zip((1,2,3), (4,5,6))`  -> `(1,4), (2,5), (3,6)`

    

- enumerate

  - 각 행의 값에 번호를 차례대로 붙여준다

  - 예) `dict = {i:j for i, j in enumerate(name.split(''))}`    

    ​      -> `{0: 'kim', 1: 'park', 2: 'lee'}`

    



- lambda

  - 함수 이름없이 함수처럼 쓸 수 있는 익명의 함수를 생성

  - `f = lambda x, y: x + y`

     -> `f(1,2)  -> 3`

    

- map 
  - 1차원 형태의 여러 값이 있는 sequence형 자료에 벡터 연산을 각각 적용
  - `list(map(func, list))`
  - ex) `list(map(lambda x: x**2 if x%2==0 else x, ex))`



- reduce

  - 하나의 리스트 안에서 x, y값이 차례대로 계산

  - `from functools import reduce`

  - ex) `reduce(lambda x, y: x+y, [1,2,3,4,5])`

    ​     -> `15    #(1+2+3+4+5)`