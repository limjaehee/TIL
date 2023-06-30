# ExpressJs 동적 라우팅

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
