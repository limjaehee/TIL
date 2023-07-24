# TypeScript 기본 타입

## 기본 타입

`:`을 이용하는 방식을 타입 표기(Type Annotation)라고 한다.

### ✅ String

```tsx
let str: string = "text";
```

### ✅ Number

```jsx
let num: number = 123;
```

### ✅ Boolean

```jsx
let isUser: boolean = true;
```

### ✅ Array

```tsx
let arr: number[] = [1, 2, 3];

//아래와 같이 제네릭을 사용할 수 있다.
let arrGeneric: Array<number> = [1, 2, 3];
```

### ✅ Object

```tsx
let person: {
  name: string;
  age: number;
} = {
  name: "Max",
  age: 20,
};
```

### ✅ Tuple

Turple은 배열의 길이가 고정되고 각 요소의 타입이 지정되어 있는 배열 형식을 의미한다.

```tsx
let person: {
  role: [number, string]
} = {
  role: [2, 'author']
};
```

### ✅ Enum

이넘은 C,Java와 같은 다른 언어에서 흔하게 쓰이는 타입으로 특정 값(상수)들의 집합을 의미한다.

```tsx
enum Role {
  ADMIN, //0
  READ_ONLY, //1
  AUTHOR, //2
}

let person: {
  role: Role;
} = {
  role: Role.READ_ONLY, //1
};
```

시작 숫자를 0으로 시작하지 않으려는 경우, 식별자에 숫자를 입력할 수 있다.

```tsx
enum Role {
  ADMIN = 5, //5
  READ_ONLY, //6
  AUTHOR, //7
}

let person: {
  role: Role;
} = {
  role: Role.READ_ONLY, //6
};
```

텍스트를 입력하거나 혼합도 가능하다.

```tsx
enum Role {
  ADMIN = "ADMIN", //ADMIN
  READ_ONLY = 100, //100
  AUTHOR = 200, //200
}

let person: {
  role: Role;
} = {
  role: Role.READ_ONLY, //100
};
```

### ✅ Any

기존 자바스크립트로 구현되어있는 웹 서비스 코드에 타입스크립트를 점진적으로 적용할 때 활용하면 좋은 타입이다. 모든 타입에 대해 허용한다는 의미를 품고 있다.

any 타입이 사용된 부분은  타입스크립트 컴파일러가 작동하지 않기 때문에 가능한 쓰지 않는 것이 좋다.

```tsx
let favoriteActivities: any[];
favoriteActivities = ["Sports"];
```

### ✅ Void

변수에는 undefinded만 할당하고, 함수에는 반환 값이 없어도 되는 타입이다.

```tsx
let undefinedType: void = undefined;
```

### ✅ unknown

```tsx
let userInput: unknown;
let userName: String;

userInput = 5;
userInput = "Max";
userName = userInput; //Type 'unknown' is not assignable to type 'String'.
```

any는 타입 확인을 수행하지 않는 대신 unknown은 좀 더 제한적으로 userInput에 어떤 타입을 가지고 있는지 확인해야 userName에 할당이 가능하다.

```tsx
if (typeof userInput === "string") {
    userName = userInput;
}
```

### ✅ Never

함수의 끝에 절대 도달하지 않는다는 의미를 지닌 타입이다.

```tsx
// 이 함수는 절대 함수의 끝까지 실행되지 않는다는 의미
function neverEnd(): never {
    while (true) {}
}
```

<br>

## 타입 지정

### ✅ union 타입 지정

다양한 타입이 들어올 수 있게 하려면 `|(or)` 기호를 이용해 union 타입을 지정하면 된다.

```tsx
function combine(input1: number | string, input2: number | string) {
  /**타입스크립트는 여러 타입이 입력되었을 때
   * 더하기 연산자를 사용할 수 없는 타입도 있을 것이라고 이해하기 때문에
   * 아래 런타임 검사를 추가해준다.*/
  if (typeof input1 === "number" && typeof input2 === "number") {
    return input1 + input2;
  }
  return input1.toString() + input2.toString();
}

const combinedAges = combine(30, 26);
console.log(combinedAges); //56

const combinedNames = combine("Max", "Anna");
console.log(combinedNames); //MaxAnna
```

### ✅ 리터럴 타입

리터럴 타입은 단순한 특정 변수나 매개변수가 아닌 정확한 값을 가지는 타입이다.

```tsx
function combine(
  input1: number | string,
  input2: number | string,
  resultConversion: "as-number" | "as-text", //as-number와 "as-text 두기자 타입 생성, 다른 텍스트가 담길 경우 오류
) {
  if (
    (typeof input1 === "number" && typeof input2 === "number") ||
    resultConversion === "as-number" //as-number일 경우 추가
  ) {
    return +input1 + +input2; //숫자로 변형하여 더함
  }
  return input1.toString() + input2.toString();
}

const combinedAges = combine("30", "26", "as-number");
console.log(combinedAges); //56
```

<br>

## Type alias

타입이 너무 길다면 타입을 변수에 담아서 지정할 수도 있다.

`type 타입이름` 으로 선언해서 사용한다.

주로 대문자로 작명하는 것이 컨벤션이라고 한다.

```tsx
type Combinable= string | number;

let a: Combinable = 'str';
a = 3;
```

### ✅ array tuple 타입

`array` 를 만들 때, 이 자리에는 무조건 이 타입이 들어와야 한다고 지정할 수 있다.

```tsx
type Mytype = [number, boolean];
let hyun:Mytype = [1, true];
```

### ✅ object 타입 지정

```tsx
type Member = {
    name: string,
    age: string,
    email: string,
    address: string,
    field: string,
    sex: string,
    schoo: string,
}
```

object 타입을 지정할 게 너무 많으면 이런 방법을 사용할 수 있다.

```tsx
type Member = {
    [key: string]: string // string 타입의 키 값은 string으로 지정
}

let hyun :Member = {
    "name": "hyun",
    "age": "27",
    "major": "software",
    "address": "suwon",
}
```