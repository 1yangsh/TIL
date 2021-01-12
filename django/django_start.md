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

    ```
    LANGUAGE_CODE = 'ko'
    TIME_ZOME = 'Asia/Seoul'
    ```

    ```
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



