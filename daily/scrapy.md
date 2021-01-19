### scrapy

> web crawling



- scrapy shell 사용

  - `scrapy shell`

    

- url을 메모리에 저장

  - `fetch('url')`

  

- url 창 보이기

  - `view(response)`

    

- html 코드 보이기 (페이지 소스)

  - `print(response.text)`

  

- xpath를 통해 경로 데이터 가져오기
  - `response.xpath('<xpath>/text()').extract()`   
    - li[1]...li[10] 반복이면 li로 입력



- class="" 스타일시트를 통해 데이터 추출
  -  `response.css('.writing::text').extract()`  
    - <span class="writing"의 text를 추출
  - `response.css('.lede::text').extract()`
    - <span class="lede"의 text를 추출



---



#### scrapy project

> scrapy를 이용한 project



- scrapy 프로젝트 생성

  - `scrapy startproject <프로젝트명>`

    

- 프로젝트 디렉토리로 이동

  - `cd <프로젝트명>`

    

- visual studio code 시작

  - `code .`

    

- spider 생성

  - `scrapy genspider <spider name> <bot을 만들 domain 주소>`

    - https:// 도메인 빼고 작성 

    

- /mybot.py 

  ```python
  # domain 정의 (네이버 뉴스)
  allowed_domains = ['naver.com']
  start_urls = ['http://news.naver.com/main/list.nhn?mode=LS2D&mid=shm&sid1=105&sid2=226/']
  ```

  

- /items.py  

  ```python
  # news title, writer, preview 추출
  class MyscraperItem(scrapy.Item):
      title = scrapy.Field()
      writer = scrapy.Field()
      preview = scrapy.Field() # 필드 정의
  ```

  

- /mybot.py

  ```python
  from myscraper.items import MyscraperItem
  
  def parse():        
      titles = response.xpath('//*[@id="main_content"]/div[2]/ul[1]/li/dl/dt[2]/a/text()').extract()
      writers = response.css('.writing::text').extract()
      previews = response.css('.lede::text').extract()
      
      items = []
      # items에 XPATH, CSS를 통해 추출한 데이터를 저장
      for idx in range(len(titles)):
          item = MyscraperItem()
          item['title'] = titles[idx]
          item['writer'] = writers[idx]
          item['preview'] = previews[idx]
          
          items.append(item)
  
      return items
  
  ```

  

- /settins.py

  ```python
  # 웹스크래핑 시 로봇의 수정 및 동작을 막음
  ROBOTSTXT_OBEY = False
  
  # csv 파일 생성
  # FEED_FORMAT = "csv"
  # FEED_URI = "my_news.csv"
  FEED_FORMAT = "json"
  FEED_URI = "my_news.json"
  FEED_EXPORT_ENCODING = "utf-8 sig"
  ```

  

---



- paging

> 여러 페이지 목록 한번에 스크래핑

  ```python
from scrapy.http import Request

URL = 'http://movie.naver.com/movie/point/af/list.nhn&page=%s'
start_page = 1

class MybotSpider(scrapy.Spider):
    name = 'mybot'
    allowed_domains = ['naver.com']
    start_urls = [URL % start_page]

    def start_requests(self):
        for i in range(6):  # 0 ~ 5
            yield Request(url=URL & (i + start_page), callback=self.parse)
  ```




---



#### scrapy 실행



- 스크래핑 실행
  - `scrapy crawl <bot 이름>`



