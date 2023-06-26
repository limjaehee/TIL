# Express.js

Express는 미들웨어 기반 프레임워크로  여러 다른 함수들을 통해 들어오는 요청을 전달하는 역할을 한다. 쉽게 말해서 node.js를 더 쉽게 사용하기 위한 프레임워크이다.

Express.js를 사용하기 위해 npm 프로젝트로 바꾼 후 설치해준다.

```
$ npm init
$ npm install express --save
```

그런 다음 아래와 같이 입력해준다.

```jsx
const express = require("express");

const app = express();

//미들웨어 등록
app.use((req, res, next) => {
  res.setHeader("Content-Type", "text/html");
  //모든 next는 express.js에 아직 작업이 완료되지 않았다고 알림
  next();
});

app.use((req, res, next) => {
  res.send("<h1>Hello World!</h1>");
});

app.listen(3000);
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

<br>

### ✅ EJS & 템플릿

ejs를 사용해 html구문을 쉽게 렌더링 할 수 있다.

```html
<!-- view/index.ejs -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>NodeJs Demo</title>
  </head>
  <body>
    <h1>Hello <%= user %></h1>
    <form method="POST" action="/">
      <input name="username" type="text" />
      <button type="submit">Send</button>
    </form>
  </body>
</html>
```

<%=와  %>사이에는 뷰에 제공할 변수의 이름을 참조할 수 있다.

이는 ejs 패키지가 이해하는 구문이다.

```jsx
...
app.set("view engine", "ejs"); //확장자 설정
app.set("views", "views"); //폴더명과 같아야 함

...

app.use((req, res, next) => {
  const userName = req.body.username || "Unknown User";
  /**render
   * 첫번째 인자 : 렌더링하고자 하는 view의 이름
   * 두번째 인자 : 템플릿에 전달하고자 하는 전체 프로퍼티 객체
   */
  res.render("index", {
    user: userName,
  });
});

app.listen(3000);
```
