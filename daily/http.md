### http

> HTTP 프로토콜의 대표 메소드(GET, POST, PUT, DELETE)





#### RESTful API

> 서버의 리소스 상태를 표현 / http 프로토콜을 사용

- GET
  - 사용자가 서버에 요청
  - db의 SELECT 와 매핑
- POST
  - 사용자가 서버에게 데이터를 전달할때 사용하는 매서드
  - 전달, 생성, login
  - db에서는 INSERT
- PUT
  - 서버가 가지고 있는 자원(리소스)를 변경할 때
  - db에서는 UPDATE
- DELETE
  - 서버가 가지고 있는 자원(리소스)를 삭제
  - db의 DELETE와 메핑



#### CSRF

> Cross-Site Request Forgery

- 비정상적으로 사용자의 의도와 무관하게 다른 사이트에 HTTP 요청을 보내는 것
- 웹 브라우저는 기본적으로 Same-Origin-Policy에 위반되지 않는 모든 요청에 쿠키를 포함해 전송하여야 한다