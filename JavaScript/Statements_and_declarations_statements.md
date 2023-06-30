# 명령문

## ✅ 변수

| var | let | const  |
| --- | --- |--------|
| 재선언 o | 재선언 x | 재선언 x  |
| 재할당 o | 재할당 o | 재할당 x  |
| 함수 스코프 | 함수 스코프 | 함수 스코프 |
| 전역 스코프 | 블록 스코프 | 블록 스코프 |

<br>

### 차이점

var 변수는 **재선언되고**, 재할당이 가능하다.

```jsx
var name = 'bathingape'
console.log(name) // bathingape

var name = 'javascript'
console.log(name) // javascript
```

let 변수는 **재선언이 되지 않고** 재할당이 가능하다.

```jsx
let name = 'bathingape'
console.log(name) // bathingape

let name = 'javascript'
console.log(name) // Uncaught SyntaxError: Identifier 'name' has already been declared
```

const 변수는 **재선언, 재할당 모두 불가능**하다.

```jsx
const name = 'bathingape'
console.log(name) // bathingape

name = 'react'
console.log(name) //Uncaught TypeError: Assignment to constant variable.
```

var, let, const 모두 함수 스코프를 가지고 있다.

var은 **전역 스코프**를 가지고 있다. (다른 의미로 블록 스코프가 없다)

```jsx
let name = "Max";
{
	let name = 'Lisa';
	let hobbies = 'Cooking'
}
console.log(name); //Lisa
console.log(hobbies); //Cooking

```

let, const는 **블록 스코프**를 가지고 있다.

```jsx
let name = "Max";
{
	let name = 'Lisa';
	let hobbies = 'Cooking'
}
console.log(name); //Max
console.log(hobbies); //hobbies is not defined
```

<br>

### **변수 명명 규칙**

변수명에는 오직 문자, 숫자, `$`, `_`만 들어갈 수 있다.

<br>

### 전역 공간 사용 최소화하는 방법

- 전역변수 대신에 지역변수 사용한다.
- window, global을 조작하지 않는다.
- const와 let을 사용한다.
- IIFE, Module, Closure 스코프를 나눈다.

<br>

### 임시변수지양

명령형으로 가득한 로직이기 때문에 어디서 어떻게 동작하는지 디버깅이 힘들다.

추가적인 코드를 작성할 수 있는 유혹이 있기 때문에 코드 유지보수가 어렵다.

임시변수를 없애기 위한 해결책

- 함수 나누기 (추상화를 사용한다)

    ```jsx
    function date() {
      return new Date();
    }
    
    function today() {
      return "오늘 날짜 : " + date();
    }
    
    console.log(today());
    ```

- 바로 반환

    ```jsx
    // X
    function user() {
      let name = "Max";
      return name;
    }
    
    // O
    function user() {
      return "Max";
    }
    ```

- 고차함수 사용 (map, filter, reduce)
- 선언형 코드로 바꿔보는 연습