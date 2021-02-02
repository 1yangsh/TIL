## heroku 시작하기

> 웹 애플리케이션 배치 모델로 사용되는 여러 프로그래밍 언어를 지원하는 클라우드 PaaS 플랫폼

<br/>

- https://heroku.com

<br/>

1. heroku에서 repository 생성

2. github에서 repository 생성

3. 내 local에 git clone

4. node.js 설치 (설치가 안된 상태라면)

5. heroku에서 git repository 연동

6. `npm init`

7.  웹 서버 설치

   1. `npm install express --save`

      - express는 웹 서버를 실행시켜주는 모듈

        <br/>

예시 파일 생성

```json
// package.json
"scripts": {
    "start": "node app.js", // 추가
    "test": "echo \"Error: no test specified\" && exit 1"
  },
```

App.js 생성

```javascript
const express = require('express')
const app = express();
const port = process.env.PORT || 3000;

app.get('/', (req, res) => res.send('Hello, Nodejs!'))
app.listen(port, () => console.log('Example app listening on port 3000'));
```

<br/>

8. `npm start`

- 서버 정상 실행 확인



---

<br/>

9. heroku CLI 설치 (설치가 안되어 있다면)

- https://devcenter.heroku.com/articles/heroku-cli



10. heroku version 확인

- `heroku --version`

  

12. heroku login

    - `heroku login`

      

13. heroku list

    - list 출력
    - `heroku list`
    - `heroku apps`



11. heroku 반영

- `heroku git:remote -a <연동할 앱>` 



12. heroku에 반영

- `git push heroku master`  // heroku로 바로 반영  (Local -> heroku)
- `git push` // github에 반영



13. github -> heroku로 deploy 방법

- 수동 방법 (manual)
  - github에서 수정된 파일이 push가 된 상태에서 heroku `Deploy Branch` 적용
- 자동 방법 (automatic)
  - `Enable Automatic Deploys`



14. heroku log 확인

- `heroku logs`

  

---

<br/>

```
요약

1. heroku app을 생성
2. github.com 에 repository 생성
3. heroku app과 연동
   - deploy
   	 - manual (수동)
   	 - automatic (자동)
```

