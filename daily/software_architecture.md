## Software Architecture

- Antifragile
  - Auto scaling
    - 탄력적이고 유연한 서버의 증감
  - Microservices
    - 애플리케이션의 기능이 독립적으로 존재, 제공
  - Chaos engineering
    - Agile 방법론을 통해 설계->구현->테스트의 주기를 반복
    - 미리 오류를 발견하여 즉각적으로 수정 가능
    - 변동, 예견되거나 예견되지 않은 불확실성을 미리 처리
  - Continuous deployments
    - 지속적 배포
    - 배포 과정 까지의 단계를 자동적으로 수행



- Architecture
  - 이해 관계자들이 시스템을 이해하는 수준은 모두 다름 -> 관점 -> View
  - UML(Unified Modeling Language)



- Monolith Architecture
  - 모든 업무 로직이 하나의 애플리케이션 형태로 패키지되는 서비스
  - 애플리케이션에서 사용하는 데이터가 한곳에 모여 참조되어 서비스되는 형태
  - 현 클라우드 시대에 사용하지 않는 방식



- Microservice

  - Small autonomous services that work together - Sam Newman

  - What is Microservice?
    - RESTful
    - Small Well Chosen Deployalbe Units
    - Cloud Enabled

