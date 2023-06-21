# 함수

## 함수 개념

```jsx
function startGame() { 
    console.log("Game is starting...");
}

console.dir(startGame);
```

함수는 객체이다.<br>위와 같이 콘솔에 함수를 찍으면 아래처럼 키와 값이 쌍으로 되어있는 객체가 나온다. 이는 해당 함수에 포함되어있는 프로퍼티를 나타낸 것이다.

 <img src = "../../assets/JavaScript/functionIsObject.jpg" width="300px">

<br>
<br>
함수 네이밍

| get  | 값을 반환함 |
| --- | --- |
| show  | 무언가를 보여줌 |
| calc  | 무언가를 계산함 |
| create  | 무언가를 생성함 |
| check  | 무언가를 확인하고 블린값을 반환함 |

<br>


### ✅ 매게변수&인자

매개변수는 함수를 정의할 때 **괄호** 안에 지정하는 변수이다.

이 예시에서 `name`은 매개변수이다.

```jsx
function sayHi(name) { ... }
```

인자는 함수를 호출할 때 함수에 전달하는 **구체적인 값**이다.

```jsx
sayHi('Max');
```

매개변수 `name` 에 대해 `'Max'` 는 함수의 인자이다..

인수는 복사된 값이다. 따라서 **원본을 바꿔주지 않는다.**

<br>

### ✅ Default Value

인자가 넘어오지 않았을 때를 대비해 기본값을 지정할 수 있다.

```jsx
function createSquare({ width = 5, length = 2, height = 10 } = {}) {
  return {
    width,
    length,
    height,
  };
}

createSquare(); //{ width: 5, length: 2, height: 10 }
```

필수로 넘겨야 하는 인자에 대한 에러를 표출할 수도 있다.

```jsx
const required = (argName) => {
    throw new Error('required is ' + argName);
}

function createSquare({ width = required('width'), length = 2, height = 10 } = {}) {
    return {
        width,
        length,
        height,
    };
}

createSquare(); //required is width
```

<br>

### ✅ Rest Parameters

Rest 파라미터를 사용하면 배열로 사용할 수 있다.

```jsx
const sumTotal = (...num) => {
	return num.reduce((acc,cur) => acc + cur);
}

sumTotal(1,23,4); //28
```

<br>

### ✅ 함수 표현식 vs 함수 선언문

```jsx
sum(1, 2) //3

// 함수 선언문
function sum(a, b) {
  return a + b;
}
```

자바스크립트는 함수를 모두 문서의 위쪽에 초기화 한다.

때문에 함수 선언문은 함수가 어디에 있던지 사용이 가능하다.

```jsx
sum(1, 2) //Cannot access 'sum' before initialization

// 함수 표현식
let sum = function(a, b) {
  return a + b;
};
```

함수 표현식에서 정의 이전에 sum()을 호출하면 초기화 전에는 접근할 수 없다고 에러가 나온다.

<br>

### ✅ 익명함수에 이름을 적어주는 이유

이름이 없으면 함수의 오류를 찾기 어렵기 때문이다.

```jsx
startGameBtn.addEventListener('click', function() {
    conosle.log(age);
});

//age is not defined
//at HTMLButtonElement.<anonymous>

startGameBtn.addEventListener('click',  function start() {
    conosle.log(age);
});

//age is not defined
//at HTMLButtonElement.start
```

<br>

### ✅ 메서드

객체에 함수가 저장된 것을 메서드라고 한다.

  ```jsx
  const person = {
      name: "Max",
      greet: function greet() {
          console.log("Hello there!");
      },
  };
  
  person.greet();
  ```

person객체 안에 `greet`라는 메서드가 있다.

  ```jsx
  startGameBtn.addEventListener("click", startGame);
  ```

startGameBtn 객체 안에 `addEventListener`라는 메서드가 있다.

- bind()

    ```jsx
    function sum(num) {
        return num + this.num1 + this.num2; // 5 + 20 + 3
    }
    var myObj = {num1:20, num2: 3};
    var customSum = sum.bind(myObj);
    console.log(customSum(5)); //28
    ```

  함수의 인자를  ‘사전 구성’하려는 상황에서 함수를 직접 호출하지 않을 때 유용하다.

  bind함수를 사용하면 this는 내가 정한 object로 고정된다.

- call
- apply

<br>

## 함수의 종류

### ✅ 화살표 함수

화살표 함수를 맹목적으로 사용 할 경우 다음과 같은 오류가 생긴다.

```jsx
const user = {
    name: "Kiki",
    getName: ()=> {
        return this.name;
    }
}

user.getName(); //undefined
```

화살표 함수를 매서드에 사용할 경우 해당 함수 상위의 스코프를 따르기 때문에 this는 user의 상위 즉 window를 바라보게 된다.

또한 화살표 함수 내에서는 call, apply, bind, arguments 등을 사용할 수 없다.

대신 rest를 사용하여 파라미터를 받을 수 있다.

```jsx
const user = {
    name: "Kiki",
    getName: (...rest) => {
      return rest;
    },
};
console.log(user.getName("Tom")); //['Tom']
```

또한 화살표 함수로는 생성자 함수를 사용할 수 없다.

```jsx
const Person = (name, city) => {
    this.name = name;
    this.city = city;
}

const person = new Person('kiki', 'seoul'); //Person is not a constructor
```

<br>

### ✅ 순수 함수

함수의 외부는 바꾸지 않고 동일한 입력값에 대한 출력이 항상 똑같다.

```jsx
function add(num1, num2){
	return num1 + num2;
}

add(1,5) //6
```

<br>

### ✅ 비순수 함수

비순수 함수란 입력값에 대한 출력값을 예측 할 수 없는 것을 말한다.

```jsx
function addRandom(num){
	return num + Math.random();
}

addRandom(5) //5.24589115
```

부수 효과(Sdie Effects)란 함수의 외부를 바꿔놓는 것이며 이것은 HTTP 요청일 수도 있고 데이터베이스에 저장된 데이터(외부 변수)일 수도 있다.

아래는  부수 효과를 만들어내는 부수 함수 예시다.

```jsx
let previousResult = 0;

function add(num1, num2){
	const sum = num1 + num2;
	previousResult = sum; //부수 효과
	return sum;
}

add(5, 1) //6
```

<br>

### ✅ 팩토리 함수

애플리케이션의 다른 부분에서 여러 번 호출되는 어떤 함수가 있을 때 사전 설정하여 쉽게 호출할 수 있다.

```jsx
function createTaxCalculator(tax) {
	function calculateTax(amount) {
		return amount * tax;
	}
	return calculateTax;
}

const calculateVatAmount = createTaxCalculator(0.19);

calculateVatAmount(100); //19
calculateVatAmount(200); //38
```

<br>

### ✅ 클로저(closure)

자신을 포함하고 있는 외부함수보다 내부함수가 더 오래 유지되는 경우, 외부 함수 밖에서 내부 함수가 호출되더라도 외부함수의 지역변수에 접근할 수 있는 이런 함수를 클로저라고 한다.

```jsx
function outerFunc() {
  var x = 10;
  var innerFunc = function () { console.log(x); };
  return innerFunc;
}

/**
 *  함수 outerFunc를 호출하면 내부 함수 innerFunc가 반환된다.
 *  그리고 함수 outerFunc의 실행 컨텍스트는 소멸한다.
 */
var inner = outerFunc();
inner(); // 10
```

간단히 말하면 자신이 생성될 때의 환경을 기억하는 함수이다.

JavaScript의 모든 함수는 클로저이다. 이는 환경에 정의된 변수가 닫혀있기 때문이고 이를 기억하기 떄문이다.

```jsx
let userName = 'Max';

function greetUser() {
	console.log('Hi' + userName);
}

username = 'Lisa';

greetUser()
```

함수는 초기화 될 때 안에 있는 userName이라는 변수를 저장하는데 변수가 바뀌게 되면 반영이 된다.

따라서 함수를 생성하고 로깅 할 때 변수의 값을 복사하지 않고 변수 그 자체에 로깅한다고 할 수 있다.

**클로저의 메모리 관리**

자바스크립트 엔진은 기본적으로 변수 사용량을 추적해서 변수가 어떤 함수나 어떤 곳에서도 사용된 적이 없으면 삭제한다.

<br>

### ✅ 그림자(Shadow)변수

함수 내부와 외부에 같은 이름의 변수를 사용하는 경우 내부 함수 혹은 내부 환경은 외부 환경보다 우선시 된다.

함수는 실행될 때 먼저 내부 환경을 확인하고 변수를 찾을 수 없을 때만 외부 환경을 확인한다.

```jsx
function greetUser() {
	let name = 'Anna';
	console.log('Hi' + Anna);
}

let name = 'Pororo';
greetUser() //Hi Anna
```

<br>

### ✅ 재귀 함수

자체적으로 함수를 호출하는 재귀는 루프보다 간단하게 작성할 수 있다.

```jsx
//일밚 함수
function powerOf(x, n) {
	let result = 1;
	for(let i = 0; i < n; i++) {
		result *= x;
	} 
	return result;
}

//재귀 함수
function powerOf(x, n) {
	if(n === 1) return x;
	return x * powerOf(x, n - 1);
}

powerOf(2, 3);
```

<br>

### ✅ 콜백함수

addEventListener는 사용자가 정의한 콜백 함수를 넘겨주며 실행시킨다.

```jsx
someElement.addEventListener('click', function(e) {
	console.log(someElement + '이 클릭 되었습니다.');
})
```

```jsx
function introduce (lastName, firstName, callback) {
    var fullName = lastName + firstName;
    
    callback(fullName);
}

introduce("홍", "길동", function(name) {
    console.log(name);
});
//홍길동
```