### 웹 데이터 추출

- web crawling
  - 웹 크롤러(자동화 Bot)
  - 일정한 규칙으로 웹 페이지 브라우징
- web scraping
  - 웹 사이트에서 원하는 정보를 추출



---



URL

- ```
  https://news.naver.com/main/list.nhn?mode=LS2D&sid2=258&sid1=101&mid=shm&date=20210119&page=1
  https://news.naver.com/main/list.nhn?mode=LS2D&mid=shm&sid2=258&sid1=101&date=20210119&page=2
  https://news.naver.com/main/list.nhn?mode=LS2D&sid2=258&sid1=101&mid=shm&date=20210119&page=3
  ```

-     client (web browser)
      -> server(요청)
                  - 주소(리소스의 위치):https://news.naver.com/main/list.nhn
                  - 전달 파라미터 : mode=LS2D&sid2=258&sid1=101&mid=shm&date=20210119&page=1
                      (key)=(value)
                      	mod=LS2D&
                     	    sid1=101&
                      	mid=shm&
                     		date=20210119&
                     		page=1
                      여러개의 파라미터 전달 시 구분 기호: &





