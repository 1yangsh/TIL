### django

> Python 기반으로 만들어진 무료 오픈소스 웹 어플리케이션 프레임워크



#### 정의

- 백엔드를 담당하는 파이썬 풀스택 웹프레임워크
- MTV 개발 방식
  - Model : 테이블을 정의한다. (DB와 연동)
  
  - Template : 사용자가 보게 될 화면의 모습을 정의한다
  
  - View : 애플리케이션의 제어 흐름 및 처리 로직을 정의한다.
  
- 대표적 웹 서버
  - Apache
  - Ngin X
- 데이터베이스
  - 먼저 Sqlite
  - 어느정도 구현이 되면 MariaDB
  
  



#### 설치

- Pypi에서 Django 확인
  - https://pypi.python.org/pypi/Django
- `pip install django`
- `django-admin --version`





---

#### 시작

- Django 프로젝트 생성
  - `django-admin starproject <mydjango> .`



- Django 프로젝트 설정 변경

  - mydjango/settings.py

    ```python
    LANGUAGE_CODE = 'ko'
    TIME_ZOME = 'Asia/Seoul'
    ```

    ```python
    import os
    STATIC_URL = '/static/'
    STATIC_ROOT = os.path.join(BASE_DIR, STATIC_URL)
    ```

    

- Django 프로젝트 DB 생성과 Server 시작
  - 데이터베이스 생성
    - `python manage.py migrate`
    - `db.sqlite3` 파일이 생성됨
  - Server 시작
    - `python manage.py runserver`
      - http://localhost:8000   // 서버 실행
  - Superuser 계정 생성
    - `python manage.py createsuperuser`
      - http://localhost:8000/admin // 접속

  


- App 디렉토리 생성


  - `python manage.py startapp <App명>`
  - mydjango/settings.py

      ```python
    INSTALLED_APPS = [
      ...,
      '<App명>'  # 추가
    ]
      ```



---



#### blog 만들기



- Model class 만들기

  - 예) blog post 

  ```
  from django.db import models
  from django.utils import timezone
  
  class Post(models.Model):
      # 작성자
      author = models.ForeignKey('auth.User', on_delete=models.CASCADE)
      # 글 제목
      title = models.CharField(max_length=200)
      # 글 내용
      text = models.TextField()
      # 작성일
      created_date = models.DateTimeField(default=timezone.now)
      # 수정일
      published_date = models.DateTimeField(blank=True, null=True)
  ```

  - 마이그레이션 파일(migration file) 생성하기  (db와 클래스 연동)
    - `python manage.py makemigrations <app이름>`   ## 작업지시 파일 생성
  - 실제 데이터베이스에 Model 추가를 반영하기
    - `python manage.py migrate blog`



---



#### Migrations

- 모델 변경내역 히스토리 관리
- 모델의 변경내역을 Database Schema (db data 구조) 로 반영시키는 효율적인 방법을 제공
- 관련 명령
  - `python manage.py makemigrations <app-name>`  # 마이그레이션 파일 생성
  - `python manage.py migrations <app-name>`  # 마이그레이션 적용
  - `python manage.py showmigrations <app-name>`  # 마이그레이션 적용 현황
  - `python manage.py sqlmigrations <app-name> <migration-name>`  # 지정 마이크레이션의 SQL 내용