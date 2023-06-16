# Symbol

## 심볼

자바스크립트는 객체 프로퍼티 키로 오직 문자형과 심볼형만을 허용한다.

심볼(symbol)은 유일한 식별자(unique identifier)를 만들고 싶을 때 사용한다.

Symbol()을 이용하면 심볼값을 만들 수 있다.

```jsx
const id = Symbol('id'); //심볼 id에는 ‘id’라는 설명이 붙는다.
```

심볼은 유일성이 보장되는 자료형이기 때문에 동일한 심볼을 여러개 만들어도 각 심볼값은 다르다.

```jsx
const id1 = Symbol('id');
const id2 = Symbol('id');

console.log(id1 === id2) //false
```

### 숨김 프로퍼티

심볼을 이용하면 숨김(hidden) 프로퍼티를 만들 수 있다. 숨김 프로퍼티는 외부 코드에서 접근이 불가능하고 값도 덮어 쓸 수 없는 프로퍼티이다.

```jsx
const user = {
    name: "John",
};

const id = Symbol("id");

user[id] = 1;

console.log(user[id]) //1
console.log(user.id) //undefined
```

### Symbols in a literal

객체 리터럴 {…}을 사용해 객체를 만들 경우, 대괄호를 이용해 심볼형 키를 만들어야 한다.

```jsx
const id = Symbol("id");

const user = {
  name: "John",
  [id]: 123 // "id": 123은 안됨
};
```

### for…in에서 배제되는 심볼

키가 심볼인 프로퍼티는 for..in 반복문에서 제외된다.

```jsx
const id = Symbol("id");
const user = {
  name: "John",
  age: 30,
  [id]: 123
};

for (let key in user) console.log(key); // name과 age만 출력되고, 심볼은 출력되지 않습니다.

// 심볼로 직접 접근하면 잘 작동합니다.
console.log( "직접 접근한 값: " + user[id] ); //직접 접근한 값: 123
```

Object.keys(user)에서도 마찬가지로 키가 심볼인 프로퍼티는 배제된다. ‘심볼형 프로퍼티 숨기기’라 불리는 이런 원칙 덕분에 외부 스크립트나 라이브러리는 심볼형 키를 가진 프로퍼티에 접근하지 못한다.

하지만 Object.assign은 키가 심볼인 프로퍼티를 배제하지 않고 객체 내 모든 프로퍼티를 복사한다.

```jsx
const id = Symbol("id");
const user = {
  [id]: 123
};

const clone = Object.assign({}, user);

console.log( clone[id] ); // 123
```

<br>

## 전역 심볼

심볼은 이름이 같더라도 모두 별개로 취급되지만 이름이 같고 심볼이 같은 개체를 가리키길 원할 때 전역 심볼 레지스토리를 사용할 수 있다. 전역 심볼 레지스트리 안에 심볼을 만들고 해당 심볼에 접근하면, 이름이 같을 경우 동일한 심볼을 반환해준다.

```jsx
// 전역 레지스트리에서 심볼을 읽습니다.
const id = Symbol.for("id"); // 심볼이 존재하지 않으면 새로운 심볼을 만듭니다.

// 동일한 이름을 이용해 심볼을 다시 읽습니다(좀 더 멀리 떨어진 코드에서도 가능합니다).
const idAgain = Symbol.for("id");

// 두 심볼은 같습니다.
console.log( id === idAgain ); // true
```

### Symbol.keyFor

Symbol.keyFor(sym)을 사용하면 심볼 이름을 얻을 수 있다.

```jsx
// 이름을 이용해 심볼을 찾음
const sym = Symbol.for("name");
const sym2 = Symbol.for("id");

// 심볼을 이용해 이름을 얻음
console.log( Symbol.keyFor(sym) ); // name
console.log( Symbol.keyFor(sym2) ); // id
```

### description

전역 심볼이 아닌 일반 심볼에서 이름을 얻고 싶으면 description 프로퍼티를 사용하면 된다.

```jsx
const globalSymbol = Symbol.for("name");
const localSymbol = Symbol("name");

console.log( Symbol.keyFor(globalSymbol) ); // name, 전역 심볼
console.log( Symbol.keyFor(localSymbol) ); // undefined, 전역 심볼이 아님

console.log( localSymbol.description ); // name
```