# 호이스팅

변수 선언 / 함수 선언 되어진 변수 또는 함수에 대해 코드가 이동되지는 않지만 코드가 최상위로 끌어올려진 것 처럼 동작하는 현상을 말한다

런타임 시기(동작 시기)에 선언과 할당이 분리된 것이다.

### var

```jsx
console.log(userName); // undefined
var userName = "Max";
```

선언이 끌어 올려지는 것처럼 보이는 것이지 할당이 끌어올려지는 것이 아니다.

```jsx
var userName; //보이지 않는 작업

console.log(userName); // undefined
var userName = "Max";
```

### let, const

var과 마찬가지로 호이스팅은 그대로 존재하지만 undefined로 초기화하지 않는다. 즉 초기값을 할당하지 않는다.

때문에 초기화를 할 수 없다는 오류가 나오게 된다.

```jsx
console.log(userName); //nnot access 'userName' before initialization
let userName = "Max";
```

### function

함수도 호이스팅 된다.

```jsx
var sum;

console.log(typeof sum); //function

function sum() {
  return 1 + 1;
}
```