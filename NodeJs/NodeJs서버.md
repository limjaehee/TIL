# NodeJs 서버

## 서버 실행하기

우리가 www.naver.com을 실행하면 서버에서 페이지를 전달받는 것처럼 node.js로 서버를 만들어 전달할 수 있다.

```jsx
//app.js
const http = require("http");

function handleRequest(request, response) {
  //브라우저에 요청이 성공했는지 여부 알림
  response.statusCOde = 200;
  //응답준비 끝
  response.end("<h1>Hello World</h1>");
}

const server = http.createServer(handleRequest);

//포트번호 전달
//개발 중에는 3000을 사용, 실제로는 80이나 443 사용
server.listen(3000);
```

모두 작성한 다음 터미널에서 node app.js를 입력하고 브라우저 창을 열어 [localhost:3000](http://localhost:3000) 을 입력한다.

다른 경로에서의 응답은 다음과 같이 처리한다.

```jsx
function handleRequest(request, response) {
  if (request.url === "/currenttime") {
    response.statusCOde = 200;
    response.end(`<h1>${new Date().toISOString()}</h1>`);
    return;
  }

  response.statusCOde = 200;
  response.end("<h1>Hello World</h1>");
}
```

위와 같이 작성 후 브라우저 창에 [localhost:3000/currenttime](http://localhost:3000/currenttime) 을 입력하면 현재 시간이 나온다.