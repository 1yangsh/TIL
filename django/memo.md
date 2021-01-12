### 웹 프로그래밍 관련 메모

> web programming information

  

- Static Web Application (정적인 웹)

  - 컴퓨터에 저장된 파일을 그대로 사용자에게 전달하는 웹 페이지

  - 모든 상황에서 모든 사용자에게 동일한 정보를 표시   ex) HTML 페이지

    

- Dynamic Web Application (동적인 웹)

  - 저장된 내용을 다른 변수로 가공 처리하여 사용자에게 전달하는 웹 페이지

  - 상황에 따라 다른 정보를 표시

  - 서버사이드 동적 페이지(server-side dynamic web page)라고 하며 애플리케이션 서버에

    의해 통제     ex) CGI, PHP, ASP, JSP/Servlet, node.js
    





---

####  Web application 기본 구조



- 웹 어플리케이션 기본 구조

  ```
  웹브라우저 <->  웹서버  <-> 데이터베이스 서버
  ```

- Django 기본 구조 

  ```
  웹브라우저  <-> | URLCONF <->  뷰  <->  모델  |  <->  데이터베이스 서버
  		   |                            |
                  |           템플릿            |
  ```





---



#### WAS 패턴



- Seperation of concerns (responsibility)

  - 관심사(책임)의 분리

    

- `MVC 패턴`

  - `Model`         - DB 연동

  - `View`            - 화면(UI)

  - `Controller`  - model과 view의 중재자, 로직 포함

    - ex) Sping MVC 프레임워크 (Java에서)

    - ex) Ruby on Rails

      

- `MTV 패턴`

  - `Model`        - DB 연동
  - `Template`  -  화면(UI)
  - `View`          - 중재자, 로직 포함
    - Django 프레임워크 (python)



---



#### ORM(Object Relational Mapping)

- 객체를 RDB의 Table로 매핑
- Mapping Rule
  -  Model Class <-> Table
  - Object <-> Row(Record), 행
  - Variable <-> Column, 열



