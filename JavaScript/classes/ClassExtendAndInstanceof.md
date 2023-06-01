# [Class] 내장 클래스 확장과 instanceof

## 내장 클래스 확장하기

배열, 맵 같은 내장 클래스도 확장이 가능하다.

```jsx
// 메서드 하나를 추가합니다(더 많이 추가하는 것도 가능).
class PowerArray extends Array {
  isEmpty() {
    return this.length === 0;
  }
}

let arr = new PowerArray(1, 2, 5, 10, 50);
alert(arr.isEmpty()); // false

let filteredArr = arr.filter(item => item >= 10);
alert(filteredArr); // 10, 50
alert(filteredArr.isEmpty()); // false
```

filter, map 등의 내장 메서드가 상속받은 클래스인 PowerArray 의 인스턴스(객체)를 반환한다. 이 객체를 구현할 때 내부에서 객체의 constructor 프로퍼티를 사용한다.

따라서 아래와 같은 관계를 갖는다.

```jsx
arr.constructor === PowerArray
```

<br>

## ****instanceof로 클래스 확인하기****

instanceof 연산자는 객체가 특정 클래스에 속하는지 아닌지를 확인할 수 있으며 상속 관계도 확인할 수 있다.

obj가 Class에 속하거나 상속받는 클래스에 속하면 true가 반환된다.

```jsx
class Rabbit {}
let rabbit = new Rabbit();

// rabbit이 클래스 Rabbit의 객체인가요?
alert( rabbit instanceof Rabbit ); // true
```

생성자 함수에서도 사용할 수 있다.

```jsx
// 클래스가 아닌 생성자 함수
function Rabbit() {}

alert( new Rabbit() instanceof Rabbit ); // true
```

위와 같은 결과가 나오는 이유는 instanceof는 평가 시, 함수는 고려하지 않고 평가 대상의 prototype을 고려하기 때문이다. 평가 대상의 prototype이 프로토타입 체인 상에 있는 프로토타입과 일치하는지 여부를 고려한다.

Array와 같은 내장 클래스에서도 사용할 수 있다.

```jsx
let arr = [1, 2, 3];
alert( arr instanceof Array ); // true
alert( arr instanceof Object ); // true
```

위에서 arr이 Object에도 속하는 이유는 Array는 프로토타입 기반으로 Object를 상속받기 때문이다.

<br>

### toString으로 타입 확인하기

일반 객체를 문자열로 변환하면 [Object Object]가 된다.

```jsx
let obj = {};

alert(obj); // [object Object]
alert(obj.toString()); // 같은 결과가 출력됨
```

이는 toString의 구현방식 때문인데 이러한 기능을 활용하면 typeof, instanceof의 대안을 만들 수 있다.

```jsx
let s = Object.prototype.toString;

console.log( s.call(123) ); // [object Number]
console.log( s.call(true) ); // [object Boolean]
console.log( s.call(null) ); // [object Null]
console.log( s.call(undefined) ); // [object Undefined]
console.log( s.call([]) ); // [object Array]
console.log( s.call(alert) ); // [object Function]
```

<br>

### Symbol.toStringTag

Symbol.toStringTag 를 사용하면 toString의 동작을 커스터마이징 할 수 있다.

```jsx
let user = {
  [Symbol.toStringTag]: "User"
};

alert( {}.toString.call(user) ); // [object User]
```