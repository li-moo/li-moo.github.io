---
title: "[JS] Node Express server "
excerpt: "Node.js express로 서버 구동해보기"

categories:
  - BE
tags:
  - [tag1, tag2]

permalink: /BE/learnNodeServer/

toc: true
toc_sticky: true

date: 2024-03-05
last_modified_at: 2024-03-05
---

## node.js란?
- Node.js는 오픈소스이자 크로스플랫폼 JavaScript 런타임 환경이다.   
- JavaScript를 프론트엔드와 서버사이드 모두에서 사용할 수 있어 개발자에게 익숙한 환경 제공한다. [node.js 공식문서](https://nodejs.org/ko/learn/getting-started/introduction-to-nodejs)

<br>

## node.js 설치

> 먼저 터미널에서 `node -v`로 node가 설치되어있는지 확인해주세요.  
> 터미널에서 아래 명령어를 적으시면 설치가 됩니다.

1.`npm init`  
2.`npm i express`  
3.`npm install mysql2 `            
&nbsp;&nbsp;`//MySQL과의 연결을 위해 사용됩니다. `              
&nbsp;&nbsp;`//다른 데이터베이스 드라이버를 사용하셔도 괜찮습니다. `      

설치 됐으면
package.json 파일의 dependencies 항목에 express 모듈이 있는지 확인해주세여

```json
   "devDependencies": {
    "@babel/cli": "^7.23.4",
    "@babel/core": "^7.23.5",
    "@babel/node": "^7.22.19",
    "express": "^4.19.2",
    "mysql2": "^3.9.1"
  },
```
<br>

## express 서버 구동해보기

### 1. index.js
```javascript
const express = require('express') //express 모듈을 가져온다.
const app = express() //가져온 express 모듈의 function을 이용해서 새로운 express 앱을 만든다.
const port = 4000
const db = require('./lib/db');
const bodyParser = require('body-parser');
const cors = require('cors');
const corsOptions = { 
    origin: [
        'http://localhost:3000',
        'http://클라이언트ip주소',
        'http://클라이언트ip주소:3000'
    ],
    credentials: true
} //cors 

app.use(bodyParser.json());
app.use(cors(corsOptions));


app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
}) //포트 4000번에서 이 앱을 실행한다.

// .get 데이터를 DB로 부터 받음
app.get("/member", (req, res) => {
    const memberNumber = req.query.member_number;

    const query = `
			쿼
			리
			문
`;

    db.query(query, [매게변수], (err, result) => {
        if (err) {
            console.error('member data error:', err);
            res.status(500).send('내부 서버 오류');
        } else {
            if (result.length === 0) {
                res.status(404).send('해당하는 회원이 없습니다.');
            } else {
                res.json(result[0]);
                console.log('member data:', result[0]);
            }
        }
    });
});

// DB에 데이터 삽입
app.post('/attendance', (req, res) => {
   const { member_number } = req.body;
		// const ?, ?, ?, ?, ?

    const query = `
        INSERT INTO attendance (매 개 변 수  배 열)
				VALUES (?, ?, ?, ?, ?)
    `;
    db.query(query, [매 개 변 수  배 열], (err, result) => {
        if (err) {
            console.error('데이터 삽입 중 오류 발생:', err);
            res.status(500).json({ error: '데이터 삽입 중 에러' });
        } else {
            res.status(201).json({ id: result.insertId, message: '데이터 삽입 성공' });
        }
    });
});
// 실행 node.index.js -> 서버 실행
// npm nodemon 업데이트 될때마다 node index.js를 실
```
<br>

### 2. lib/db.js
node와 mySql연결

```javascript
require('dotenv').config();
const mysql = require('mysql2'); //mysql 모듈 로드


const db_connection = mysql.createConnection({ //handshake connection
  host: 'localhost',
  port: '3306',
  user: 'root',
  password: '',
  database: '데이터베이스 이름'
});

// db접속
db_connection.connect((err) => {
  if (err) {
    console.error('Error connecting to database:', err);
  } else {
    console.log('Connected to database'); // 터미널에 뜬다면 성공

  }
});

module.exports = db_connection;
```