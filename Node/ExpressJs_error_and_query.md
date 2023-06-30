# ExpressJs 오류 & 쿼리

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