#### Regular expression (Regex) - 정규표현식



| 정규표현식 | 내용                            |
| ---------- | ------------------------------- |
| ^          | line의 시작                     |
| $          | line의 끝                       |
| .          | 모든 문자                       |
| \s         | 빈칸                            |
| \S         | 빈칸을 제외한 문자              |
| *          | 문자가 0번이상 반복             |
| *?         | 문자가 0번이상 반복(non-greedy) |
| +          | 문자가 1번이상 반복             |
| +?         | 문자가 1번이상 반복(non-greedy) |
| [aeiou]    | (영문)모음 중 한 문자           |
| [^XYZ]     | 해당 문자를 제외한 문자         |
| [a-z0-9]   | 범위(range)를 포함한 문자 세트  |
| (          | 문자열 추출 시작                |
| )          | 문자열 추출 끝                  |





---





- 정규표현식 모듈 import

  - `import re`

- 정규표현식을 이용한 문자열 검색 메소드

  

  1. match()

     ```python
     import re
     p = re.compile('[a-z]+')
     m = p.search('python')   
     print(m)
     ```

     ```
     > python
     ```

     

  2. search()

     ```python
     import re
     p = re.compile('[a-z]+')
     m = p.search('3 python')   # 맞는 문구만 찾아옴
     print(m)
     ```

     ```
     > python  
     ```

     

  3. findall()

     ```python
     import re
     p = re.compile('[a-z]+')
     m = p.search('life is too short')  # 리스트 형식으로 반환
     print(m)
     ```

     ```
     > ['life', 'is', 'too', 'short']
     ```

     

  4. finditer()

     ```python
     import re
     p = re.compile('[a-z]+')
     m = p.search('life is too short')  # match를 반복 가능한 형태로 출력
     print(m)
     ```

     





---



- 한글만
```python
 import re
 words = re.compile('[\u3131-\u3163\uac00-\ud7a30-9]+')
 words = re.compile('[가-힣]+')
 result = words.findall(문자열)
```

  

- 영어 + 숫자

```python
 import re
 words = re.compile('[a-zA-Z0-9]+')
 result = words.findall(문자열)      
    
 # result = re.findall('[a-zA-Z0-9]+', 문자열)
```





참고 : https://nachwon.github.io/regular-expressions/