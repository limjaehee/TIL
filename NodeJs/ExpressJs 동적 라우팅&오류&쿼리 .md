# ExpressJs 동적 라우팅 & 오류 & 처리

## 동적 라우팅

웹 쇼핑몰 사이트를 보면 상품 리스트가 있고 클릭하면 상품 상세로 가는 페이지가 있다.

이는 해당 고유 아이디를 사용하여 만든 것으로 동적 라우팅의 예시라 할 수 있다.

그 다음 고유 아이디를 넣기 위해 아래 패키지를 설치한다. (안해도 상관x)

```
$ npm install uuid
```

레스토랑 생성 스크립트에 고유 아이디를 추가해준다.

앞으로 데이터를 생성할 때마다 아이디가 추가된다.

```jsx
//app.js
const uuid = require('uuid')

app.post("/recommend", (req, res) => {
    const restaurant = req.body;
    restaurant.id = uuid.v4();
    ...
});
```

app.js는 다음과 같이 작성한다.

```jsx
//app.js

app.get("/restaurants/:id", (req, res) => {
    const restaurantId = req.params.id;
    const filePath = path.join(__dirname, "data", "restaurants.json");

    const fileData = fs.readFileSync(filePath);
    const storedRestaurants = JSON.parse(fileData);

    const restaurant = storedRestaurants.find((v) => v.id === restaurantId)

    res.render("restaurant-detail", {
        restaurant: restaurant,
    });
})
```

콜론(:)은 동적 라우팅을 시작하는 부분이며 뒤의 식별자 단어는 정해져 있지 않다.

위의 예시는 id로 동적 라우팅을 받는다고 적어놨으므로 req.params.id로 해당 id를 받을 수 있다.

그 다음 기존 레스토랑 리스트를 아이디랑 대조하여 찾아낸 후 렌더링한다.

기존 View Website 대신 View Detail로 바꾼 후 경로를 고유 아이디로 이동하게 바꾼다.

```jsx
//restaurant-item.ejs
<a href="/restaurants/<%= restaurant.id %>">View Detail</a>
```

detail 페이지를 개설한다.

```html
<!--restaurant-detail.ejs-->
<main>
    <h1><%= restaurant.name %></h1>
    <p><%= restaurant.cuisine %> | <%= restaurant.address %></p>
    <p><%= restaurant.description %></p>
    <p><a href="<%= restaurant.website %>">View Website</a></p>
</main>
```

받은 레스토랑 데이터를 뿌려준다.

<br>

## 오류 처리

### ✅ 404

404.ejs 페이지를 생성한다.

```html
<!--404.ejs-->
<main>
    <h1>Page not found!</h1>
    <p>Unfortunately, we weren't able to find this page.</p>
</main>
```

app.js 하단에 다음과 같이 작성한다.

```jsx
//app.js
app.use((req, res)=> {
    res.render('404')
})
```

404를 처리하는 미들웨어는 맨 마지막에 와야 한다. (위에서부터 차례대로 읽기 때문)

### ✅ 500

500.ejs 페이지를 생성한다.

```html
<!--500.ejs-->
<main>
    <h1>Something went wrong!</h1>
    <p>
        Unfortunately, Something went wrong on the server - 
        we're doing our best to fix it as quickly as possible!
    </p>
</main>
```

app.js 하단에 다음과 같이 작성한다.

```jsx
//app.js
app.use((error, req, res, next)=> {
    res.render('500')
})
```

4개의 매개변수를 꼭 추가해야 한다.
error 객체는 익스프레스에서 자동으로 생성되고 채워진다.
error는 발생한 오류 정보를 포함한다.

### ✅ 상태 코드 변경

웹에 표시되는 상태 코드를 변경할 수 있다. 기본은 304가 나온다.

```jsx
//app.js
app.use((error, req, res, next)=> {
    res.status(500).render('500')
})
```

<br>

## 라우트 구성 분할

라우트가 많아질 수록 app.js 파일이 길어진다.

라우터를 분할하기 위해 우선 routes 폴더를 생성하고 하위 js 파일을 추가한다.

```
routes
  ㄴdefault.js
  ㄴrestaruants.js
```

익스프레스를 추가하고 라우터를 가져와 모듈로 확장시킨다.

```jsx
//routes.js
const express = require("express");
const router = express.Router();

router.get("/", (req, res) => {
  res.render("index");
});

module.exports = router;
```

app.js에서 모듈을 경로로 추가하고 미들웨어인 use를 사용한다.

```jsx
//파일명이 아닌 경로를 추가해준다.
const defaultRoutes = require("./routes/default");

app.use("/", defaultRoutes);
```

여기서 use에 사용된 '/'는 필터링을 뜻하며 앞에 자동으로 경로를 추가해준다.

```jsx
//예제 1
app.use("/restaurant")
router.get("/information")
//결과 URL : /restaurant/information

//예제 2
app.use("/")
router.get("/index")
//결과 URL : /index
```

<br>

## 쿼리 사용하기

배열 순서를 바꾸는 버튼을 추가하여 클릭 할 때마다 쿼리로 저장해 순서를 바꾸는 예시이다.

```jsx
//restaurants.ejs
<form action="/restaurants" method="GET">
  <input type="hidden" value="<%= nextOrder %>" name="order" />
  <button class="btn">Change Order</button>
</form>
```

폼 안에 버튼을 두고 클릭 시에 라우트 GET HTTP요청을 하게 만든다.

이때 input은 유저가 보이지 않게 하고 value에 쿼리 값을 넣어준다.

```jsx
//routes/restaurants.js
router.get("/restaurants", (req, res) => {
  let order = req.query.order;
  let nextOrder = "desc";

  if (!order) {
    order = "asc";
  }

  if (order === "desc") {
    nextOrder = "asc";
  }

  const storedRestaurants = resData.getStoredRestaurants();

  storedRestaurants.sort((a, b) => {
    if (
      (order === "asc" && a.name > b.name) ||
      (order === "desc" && b.name > a.name)
    )
      return 1;
    return -1;
  });

  res.render("restaurants", {
    restaurants: storedRestaurants,
    nextOrder: nextOrder,
  });
});
```

쿼리로 받은 값이 ‘asc’이거나 없다면 내림차순을 ‘desc’면 오름차순을 해준다.

쿼리 값은 전환하여 nextOrder로 보낸다.