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



---



#### Django

> 파이썬으로 만들어진 무료 오픈소스 웹 애플리케이션 프레임워크

- 설치
  - `pip install django`
- Urlresolver
  - 웹 서버에 요청이 오면 장고로 전달되고, 장고가 웹 페이지의 주소를 가져와 무엇을 할지 확인
  - 각 URL 에 대해 일일히 확인하기는 비효율적이므로, 패턴으로 일치여부를 판단
    - 일치하는 패턴의 경우 요청을 연관된 함수(view)에 넘긴다
    - Urlresolver is a POSTMAN
- MVC
  - Model View Controller
  - 애플리케이션을 세가지 역할로 구분한 개발 방법론

- MTV
  - Model Template View
    - Model : 안전하게 데이터를 저장
    - View : 데이터를 유저에게 보여줌 (MVC와는 다름), URL을 Parsing
    - Template : 사용자 입력과 같은 이벤트 처리를 하고, Model, View 업데이트



- MTV 코딩 순서

  - 화면 설계
    - 뷰, 템플릿
  - 테이블 설계
    - 모델

  1. 프로젝트 생성
     - 프로젝트 및 앱 개발에 필요한 디렉토리와 파일 생성
  2. 모델 생성
     - 테이블 관련 내용 개발 (models.py, admin.py)
  3. URL conf 생성
     - URL 및 뷰 매핑 관계를 정의 (urls.py)
  4. View 생성
     - 애플리케이션 로직 개발 (views.py)
  5. Template 생성
     - 화면 UI 개발 (templates / 디렉토리 하위의 *.html 파일)



- settings.py

  - 데이터베이스 설정

    - Default로 Sqlite3 사용

  - 템플릿 항목 설정

    - TEMPLATES 항목 지정

  - 정적 파일 항목 설정

    - STATIC_URL 등 관련 항목 지정

  - 애플리케이션 등록

    - 프로젝트에 포함되는 모든 애플리케이션 등록

  - 타임존 지정

    - UTC 변경 (한국)

      

- models.py

  - 테이블을 정의하는 파일

  - ORM (Object Relation Mapping) 사용

    - 테이블을 클래스로 매핑하여 CRUD 기능을 클래스 객체에 대해 수행 -> DB 반영

  - 테이블을 클래스, 테이블의 컬럼은 클래스 변수로 매핑

    - django.db.models.Model 클래스 상속

  - models.py에서 DB 변경 사항 발생 시, 실제 DB에서도 반영

    - django 1.7부터 마이그레이션 기능 사용

      - ex) makemigrations, migrate 사용

        

- URLconf 주요사항

  - URL과 View (함수 또는 메소드) 를 매핑하는 urls.py 파일

  - 프로젝트 URL과 앱 URL로 구성하는 것을 추천

  - {% url %} 템플릿 태그 사용

    

- views.py

  - 뷰 로직을 생성하는 파일

  - 함수(Function-based view) 또는 클래스(Class-based view)로 생성 가능

    

- templates

  - 웹 화면(페이지) 별로 템플릿 파일 (*html)이 필요
  - TEMPLATES 설정의 DIR 항목에 지정된 디렉토리에 앱 템플릿 파일 저장
    - ex) templates/ 디렉토리



- Admin
  - 테이블 내용을 열람하고 수정하는 기능을 제공하는 사이트
  - User 및 Group 테이블 관리
    - settings.py에 django.contrib.auth 애플리케이션이 등록
  - 사용자가 비즈니스 로직 개발에 필요한 테이블 관리(CRUD)



- 개발용 웹 서버
  - 테스트용 runserver 제공
  - 상용화를 위해서는 Apache or Nginx 등의 상용 서버로 변경



----



