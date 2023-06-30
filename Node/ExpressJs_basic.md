# Express.js

Express는 미들웨어 기반 프레임워크로 여러 다른 함수들을 통해 들어오는 요청을 전달하는 역할을 한다. 쉽게 말해서 node.js를 더 쉽게 사용하기 위한 프레임워크이다.

Express.js를 사용하기 위해 npm 프로젝트로 바꾼 후 설치해준다.

```
$ npm init
$ npm install express --save
```

경로를 추가할 경우 아래와 같이 get을 사용한다.

```jsx
//app.js
const express = require("express");

const app = express();

app.get("/currenttime", (req, res) => {
  res.send(`<h1>${new Date().toISOString()}</h1>`);
}); //localhost:3000/currenttime

app.get("/", (req, res) => {
  res.send("<h1>Hello World!</h1>");
}); //localhost:3000

//기본 포트 200
app.listen(3000);
```

[localhost:3000](http://localhost:3000) 과 [localhost:3000/currenttime](http://localhost:3000/currenttime을) 페이지에 따라 다른 결과가 나온다.

<br>

### ✅ 데이터 구문 분석

```jsx
//app.js
...

//use는 종류에 상관없이 모든 요청을 처리한다.
app.use(express.urlencoded({ extended: false }));

app.get("/", (req, res) => {
  res.send(`
  <form action="/store-user" method="POST">
    <label>Your Name</label>
    <input type="text" name="username"/>
    <button>Submit</button>
  </form>`);
}); //localhost:3000

//form action에 있는 경로와 똑같이 입력
app.post("/store-user", (req, res) => {
  const userName = req.body.username;
  res.send(`<h1>${userName} stored!</h1>`);
});

...
```

**express.urlencoded**

express 서버로 POST 요청을 할 때 input 태그의 value를 전달하기 위해 사용한다.

요청의 본문에 있는 데이터가 URL-encoded 형식의 문자열로 넘어오기 때문에 ( = 자바스크립트 구문이 아니기 때문에) 객체로의 변환이 필요하다.

해당 스크립트를 추가하지 않으면 오류가 생긴다.

<br>

### ✅ (서버 측) 파일에 데이터 저장

폴더 내에 data라는 폴더를 생성하고 users.json 파일을 추가한다.

users.json 파일에 배열로 데이터를 담기 위해 대괄호를 작성해준다.

```json
//users.json
[]
```

app.js 파일 store-user 경로 post 함수에 다음과 같이 입력해준다.

```jsx
//app.js

const fs = require("fs");
const path = require("path");

...

function getUsersFilePath() {
  //__dirname : 절대 경로를 보유하고 있는 내장된 변수 또는 상수
  return path.join(__dirname, "data", "users.json");
}

function getUsersFileData() {
  const filePath = getUsersFilePath();
  const fileData = fs.readFileSync(filePath);

  return JSON.parse(fileData);
}

app.post("/store-user", (req, res) => {
  const userName = req.body.username;
  const filePath = getUsersFilePath();
  const existingUsers = getUsersFileData();

  existingUsers.push(userName);

  fs.writeFileSync(filePath, JSON.stringify(existingUsers));

  res.send(`<h1>${userName} stored!</h1>`);
});
```

1. 파일 절대경로를 가져오기 위해 path.join를 사용하여 filePath를 생성하고
2. fs.readFileSync를 통해 해당 파일의 데이터를 불러온다.
3. JSON 형태를 자바스크립트 구문으로 변환하여 데이터를 추가한다.
4. 다시 JSON 형태로 바꿔서 파일에 덮어씌운다.

<br>

### ✅ 파일 데이터 읽기 & 동적 응답 반환

```jsx
app.get("/users", (req, res) => {
  const existingUsers = getUsersFileData();

  let responseData = "<ul>";

  for (const user of existingUsers) {
    responseData += `<li>${user}</li>`;
  }

  responseData += "</ul>";

  res.send(responseData);
});
```

[localhost:3000/users](http://localhost:3000/users) 로 들어가면 다음과 같이 데이터가 html 파일로 변환 된 것을 볼 수 있다.

<br>

### ✅ nodemon

노드몬은 파일을 변경할 때마다 변경 사항을 저장하고 서버를 다시 실행한다.

개발 중에만 사용하므로 노드몬을 devDependencies에 설치해준다.

```
$ npm install nodemon --save-dev
```

패키지 파일에서 실행 명령어를 수정해준다.

```json
//package.json
"scripts": {
  "start": "nodemon app.js"
},
```

터미널에서 npm start를 입력하면 nodemon app.js가 자동으로 실행된다.

```
$ npm start
```

<br>

### ✅ body-parser

이 패키지는 req 본문을 분석하고 req객체에 추가해준다.

```
$ npm install body-parser --save
```

body-parser과 Express.js를 이용한 데이터 파싱&응답 연습

```jsx
const express = require("express");
const bodyParser = require("body-parser");

const app = express();

//들어오는 요청 본문을 분석하고 추출함
//extended로 본문이 분석되는 방식을 제어 (기본 값)
app.use(bodyParser.urlencoded({ extended: false }));

//body-parser가 분석된 본문을 req 객체의 본문 필드에 내보냄
app.use((req, res, next) => {
  res.setHeader("Content-Type", "text/html");
  //모든 next는 express.js에 아직 작업이 완료되지 않았다고 알림
  next();
});

//app.use 안의 req는 모두 동기화 되어 같은 값을 지님
app.use((req, res, next) => {
  const userName = req.body.username || "Unknown User";
  res.send(
    `<h1>Hi ${userName}</h1>
     <form method='POST' action='/'>
         <input name='username' type='text'>
         <button type='submit'>Send</button>
     </form>`
  );
});

app.listen(3000);
```
