# 빅오
여러가지 코드를 서로 비교하고 성능을 평가하는 방법
<hr>

## 빅오를 사용하는 이유

- 코드의 성능을 얘기할 때 정확한 전문용어를 사용하는 것이 중요하기 때문
- 여러 접근법의 장단점을 얘기할 때도 유용함
- 코드를 디버그 할 때 어떤 코드가 느린지 확인할 수 있음
<hr>

## 빠른 코드 계산 방법

- 시간으로 계산할 경우 디바이스나 환경에 따라 다를 수 있다
- 따라서 이런 방식은 정확하지 않고 일일이 시간을 재야 하기 때문에 함수의 연산을 더하는 방식을 사용할 수 있다.

```jsx
//예제 1
function addUpTo(n) {
    let total = 0;
    for (let i = 1; i <= n; i++) {
        total += i;
    }
    return total;
}
```

```jsx
//예제 2
function addUpTo(n) {
    return n * (n + 1) / 2;
}
```

- 위와 같은 경우 예제1에서는 n 숫자에 따라 n이 5라면 연산자가 5개이고, n이 20이라면 연산자가 20개가 된다
- 반대로 예제 2는 연산자가 항상 3개이다.
- 계산 할 때 연산자의 정확한 갯수는 별로 중요하지 않고 **전체적인 추세**를 봐야 한다.
<hr>

## 시간복잡도

```
- f(n) colude be liear (f(n) = n)
- f(n) colude be quadratic (f(n) = n²)
- f(n) colude be liear (f(n) = 1)
- f(n) colude be something entirely different!
```

```jsx
//O(n²) 방식
function rintAllPairs(n) {
    for (let i = 0; i <= n; i++) {
        for (let j = 0; j <= n; j++) {
            console.log(i,j);
        }
    }
}

//O(n) 방식
function addUpTo(n) {
    let total = 0;
    for (let i = 1; i <= n; i++) {
        total += i;
    }
    return total;
}

function logAtLeast5(n) {
    for (let i = 1; i <= Math.max(5, n); i++) {
        console.log(i)
    }
}

//O(1) 방식
function addUpTo(n) {
    return n * (n + 1) / 2;
}

function logAtLeast5(n) {
    for (let i = 1; i <= Math.min(5, n); i++) {
        console.log(i)
    }
}
```

![Untitled](../../assets/JavaScript/comparingBigO.png)
<hr>

## 공간복잡도

```jsx
function sum(arr) {
    let total = 0;
    for (let i = 0; i <= arr.length; i++) {
        total += arr[i];
    }
    return total;
}
```

- total이라는 변수가 하나 있고 i라는 두개의 변수가 있다.
- 새로운 변수를 만들지 않으므로 O(1) 공간이다.

```jsx
function double(arr) {
    let newArr = [];
    for (let i = 0; i <= arr.length; i++) {
        newArr.push(2 * arr[i]);
    }
    return newArr;
}
```

- newArr이란 새로운 배열에 arr 배열이 크기가 커질 수록 newArr에 들어가는 배열도 커지므로 O(n) 공간이라고 할 수 있다.
<hr>

## 객체의 빅오

```
- Object.keys - O(N)
- Object.values - O(N)
- Object.entries - O(N)
- hasOwnProperty - O(1)
```

```jsx
let instructor = {
    firstName: "kelly",
    isInstructor: true,
    favoriteNumbers: [1,2,3,4],
}

Object.keys(instructor)
// ['firstName', 'isInstructor', 'favoriteNumbers']

Object.values(instructor)
// ['kelly', true, Array(4)]

Object.entries(instructor)
// [Array(2), Array(2), Array(2)]

instructor.hasOwnProperty("isInstructor")
// true
```
<hr>

## 배열의 빅오

```
- Insertion - It depends...
- Removal   - It depends...
- Searching - O(N)
- Access    - O(1)
```

```jsx
let names = ["Michael", 'Melissa', "Andrea"];

//           Michael     Melissa    Andrea
//              0            1         2
```

- 예를 들어 name 배열 뒤에 삽입(Insertion)하는 경우 인덱스 순서가 달라지지 않으므로 `O(1)`이다 하지만 앞에 삽입하는 경우 원래 있던 요소의 인덱스를 하나씩 밀어줘야 하기 때문에 `O(N)`이다
- 지우는 것도 똑같다

```
- push - O(1)
- pop - O(1)
- shift - O(N)
- unshift - O(N)
- concat - O(N)
- slice - O(N)
- splice - O(N)
- sort - O(N * log N)
- forEqach/map/filter/reduce/etc. - O(N)

```