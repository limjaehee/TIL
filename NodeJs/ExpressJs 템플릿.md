# ExpressJs 템플릿

## 데이터 저장

### ✅ 정적 파일 제공

퍼블리싱 된 html, style, script 파일을 익스프레스로 제공할 때 아래와 같이 한다.

views, public 폴더를 생성한다. 이름은 관례를 따른다.

html 파일은 views 폴더에, script, style 파일은 public 폴더에 모두 넣는다.

```
views
	ㄴconfirm.html
  ㄴindex.html
	ㄴrecommend.html
	ㄴrestaurants.html

public
	ㄴscripts
	ㄴstyles
```

다음과 같이 작성한다.

```jsx
//app.js
const express = require("express");
const path = require("path");

const app = express();
/**미들웨어
 * 모든 수신 요청에 대해 이 공용 폴더에서 찾을 수 있는지 확인
 */
app.use(express.static("public"));

function getFilePath(file) {
  return path.join(__dirname, "views", `${file}.html`);
}

app.get("/confirm", (req, res) => {
  const htmlFilePath = getFilePath("confirm");
  res.sendFile(htmlFilePath);
});

app.get("/", (req, res) => {
  const htmlFilePath = getFilePath("index");
  res.sendFile(htmlFilePath);
});

app.get("/recommend", (req, res) => {
  const htmlFilePath = getFilePath("recommend");
  res.sendFile(htmlFilePath);
});

app.get("/restaurants", (req, res) => {
  const htmlFilePath = getFilePath("restaurants");
  res.sendFile(htmlFilePath);
});

app.listen(3000);
```

기존의 send()가 아닌 sendFile()을 사용하여 파일을 전달한다.

<br>

### ✅ 양식 데이터 구문 분석 & 요청 리다이렉트

데이터를 저장하기 위해 data 폴더를 생성하고 restaurants.json 파일을 생성한다.

```
data
    ㄴrestaurants.json
```

전과 마찬가지로 restaurants.json 안에는 빈 배열을 넣는다.

```json
//restaurants.json
[]
```

다음과 같이 작성한다.

```jsx
//app.js
const express = require("express");
const fs = require("fs");
...
app.use(express.urlencoded({ extended: false }));

function getFilePath(file) {
    return path.join(__dirname, "views", `${file}.html`);
}

...

app.get("/recommend", (req, res) => {
    const htmlFilePath = getFilePath("recommend");
    res.sendFile(htmlFilePath);
});

//다른 HTTP 메서든는 동일한 경로로 사용할 수 있다.
app.post("/recommend", (req, res) => {
    const restaurant = req.body;
    const filePath = path.join(__dirname, "data", "restaurants.json");

    const fileData = fs.readFileSync(filePath);
    const storedRestaurants = JSON.parse(fileData);

    storedRestaurants.push(restaurant);

    fs.writeFileSync(filePath, JSON.stringify(storedRestaurants));

    res.redirect("/confirm");
});

...
```

같은 /recommend 경로로 두가지의 HTTP 메서드를 사용하는데 이는 하나의 메서드를 사용할 때마다 다른 메서드는 가려지게 되므로 가능한 것이다.

res.redirect("")를 사용하면 다른 경로로 리다이렉트 시킬 수 있다.

<br>

## 데이터 출력

### ✅ EJS 템플릿 엔진 추가

npm 패키지를 통해 ejs를 설치한다.

```
$ npm install ejs
```

기존 html확장자를 ejs로 바꿔준다.

```
views
	ㄴconfirm.ejs
  ㄴindex.ejs
	ㄴrecommend.ejs
	ㄴrestaurants.ejs
```

다음과 같이 작성한다.

```jsx
//app.js
...

/**익스프레스 옵션 설정
 * views: 뷰 설정, 템플릿 파일을 찾을 위치를 익스프레스에 알림
 * path.join 안의 'views'는 html이 들어있는 폴더명을 뜻한다.
 * view engine: 뷰 파일을 html로 보내기 전에 처리하는 템플릿 엔진
 */
app.set("views", path.join(__dirname, "views"));
app.set("view engine", "ejs");

app.get("/confirm", (req, res) => {
		//템플릿 렌더링 작업
    res.render("confirm");
});

app.get("/", (req, res) => {
    res.render("index");
});

app.get("/recommend", (req, res) => {
    res.render("recommend");
});

app.get("/restaurants", (req, res) => {
    res.render("restaurants");
});

app.listen(3000);
```

기존 파일 경로를 받아오던 getFilePath 함수를 제거하고 res.render() 함수 안에 변환된 ejs파일명을 적어준다

템플릿 렌더링은 템플릿 엔진을 사용해서 파일을 생성하고 HTML로 변환하여 브라우저로 전송하는 역할을 한다. index.ejs로 등록하지 않는 이유는 ejs가 자동으로 붙여주기 때문이다.

<br>

### ✅ 템플릿 동적 콘텐츠 렌더링

<%=와 %>사이에는 뷰에 제공할 변수의 이름을 참조할 수 있다.

```html
<!-- restaurants.ejs -->

<p>We found <%= restaurants.length %> restaurants.</p>
```

여기서 코드를 작성할 때 <%= %> 의 시작 부분이 빨갛게 표현될 수 있는데 ejs 코드를 오류라고 해석하기 때문이다. 실제로는 패키지를 설치했기 때문에 작동에는 아무 상관이 없다.

다음과 같이 작성한다.

```jsx
//app.js

app.get("/restaurants", (req, res) => {
  const filePath = path.join(__dirname, "data", "restaurants.json");

  const fileData = fs.readFileSync(filePath);
  const storedRestaurants = JSON.parse(fileData);

  res.render("restaurants", {
    restaurants: storedRestaurants,
  });
});
```

res.render 안에 필요한 부분을 추가한다.

<br>

### ✅ 반복문 콘텐츠 출력

ejs 구문 내에 for문을 작성하여 콘텐츠를 반복 생성해준다.

태그의 속성으로도 사용이 가능하다.

```html
<!-- restaurants.ejs -->

<ul id="restaurants-list">
  <% for (const restaurant of restaurants) { %>
  <li class="restaurant-item">
    <article>
      <h2><%= restaurant.name %></h2>
      <div class="restaurant-meta">
        <p><%= restaurant.cuisine %></p>
        <p><%= restaurant.address %></p>
      </div>
      <p><%= restaurant.description %></p>
      <div class="restaurant-actions">
        <a href="<%= restaurant.website %>">View Website</a>
      </div>
    </article>
  </li>
  <% } %>
</ul>
```

<br>

### ✅ 조건부 콘텐츠 렌더링

조건문도 자바스크립트처럼 똑같이 작성한다.

```html
<!-- restaurants.ejs -->

<% if(restaurants.length) { %>
<p>Find your next favorite restaurants with help of our other users!</p>
<% } else { %>
<p>Unfortunately, we have no restaurants yet.</p>
<% } %>
```

<br>

## 일부 콘텐츠 포함

views 폴더 내에 includes 폴더를 생성하고 header.ejs를 추가한다.

```
views
	ㄴincludes
		ㄴheader.ejs
```

header.ejs에 아래와 같이 작성한다.

<!DOCTYPE html>과 같은 구문은 빼고 작성한다.

```html
<header id="main-header">
  <div id="logo"><a href="/">Eatwell</a></div>
  <nav>
    <ul>
      <li class="nav-item">
        <a href="/restaurants">Browse Restaurants</a>
      </li>
      <li class="nav-item">
        <a href="/recommend">Share a Restaurant</a>
      </li>
      <li class="nav-item">
        <a href="/about">About Eatwell</a>
      </li>
    </ul>
  </nav>
  <button id="drawer-btn">
    <span></span>
    <span></span>
    <span></span>
  </button>
</header>
```

헤더를 포함시킬 페이지로 이동하여 기존의 헤더를 지우고 다음과 같이 작성한다.

```html
<!-- 일부 html 렌더링 -->
<%- include('includes/header', {}) %>
```

여기서 주의할 점은 <%=이 아닌 `<%-` 이라는 것이다.

include 함수를 사용하면 ejs 파일에 대한 경로를 지정할 수 있고 ejs는 해당 파일을 분석해서 html 구문으로 처리해준다. 두 번째 인자로는 동적인 객체를 전달할 수 있다.

동적인 객체는 다음과 같이 key와 value로 전달한다.

```html
<%- include('includes/restaurants/restaurant-item', {restaurant: restaurant}) %>
```
