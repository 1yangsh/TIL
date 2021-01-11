### 파이썬 환경설정

- 개발환경 설정
  - 파이썬 SDK 개발 kit
    - 파이썬 기본적으로 제공하는 내부모듈만 포함하고 있음
  - 데이터분석, 시각화, 인공지능 , 웹스크래핑
    - 외부모듈을 사용해야 함 (third party) 
  - Anaconda 사용
    - 기본모듈 + 외부모듈이 포함된  개발  kit 



- PyPI(python package index)
  - python 기반 open source repository (저장소)
    - https://pypi.org/
    - `pip install tensorflow`
- Javascript
  - https://www.npmjs.com/
    - `npm i jquery`
- java
  - https://mvnrepository.com/



- Editor (IDE - integrated development environment) 통합개발환경

  - pycharm 

  - 설치 후 확인 명령어
    - `python --version`
    - `pip --version`

  - pycharm에 anaconda의 python을 연결

    - file->settings -> project -> project interpreter 

    - -> 설정아이콘 -> Add -> 왼쪽메뉴의 System Interpreter 
    - C:\ProgramData\Anaconda3\python.exe ->ok



- 외부 모듈
  - `pip install 외부모듈명`  // 외부 모듈 설치
  - `pip show 외부모듈명`  // 외부 모듈 설치 확인
  - `pip list`  // 외부 모듈 설치 확인
  - `pip uninstall 외부모듈명`  // 외부 모듈 제거