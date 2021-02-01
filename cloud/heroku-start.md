### heroku 시작하기

> https://heroku.com



1. heroku에서 repository 생성
2. github에서 repository 생성
3. 내 local에 git clone
4. node.js 설치 (설치가 안된 상태라면)
5. heroku에서 git repository 연동
6. `npm init`
7.  웹 서버 설치
   1. `npm install express --save`
      - express는 웹 서버를 실행시켜주는 모듈

```javascript
// package.js
"scripts": {
    "start": "node app.js", // 추가
    "test": "echo \"Error: no test specified\" && exit 1"
  },
```

8. App.js 생성

```javascript
const express = require('express')
const app = express();
const port = process.env.PORT || 3000;

app.get('/', (req, res) => res.send('Hello, Nodejs!'))
app.listen(port, () => console.log('Example app listening on port 3000'));
```



9. `npm start`