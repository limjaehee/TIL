# Variable Environment와 Lexical Environment의 차이

JavaScript의 실행 컨텍스트는 두 가지 중요한 구성 요소로 나뉩니다: **Variable Environment**와 **Lexical Environment**. 둘 다 변수를 다루는 공간이지만, 각자의 역할과 필요성이 구분됩니다. 이 차이를 이해하려면 자바스크립트에서 `var`, `let`, `const`가 만들어진 역사적 배경과 이들의 동작 방식을 살펴봐야 합니다.

---

## 1. Variable Environment와 Lexical Environment란?

- **Variable Environment**: 변수 선언과 관련된 정보를 담는 공간으로, 실행 컨텍스트가 생성될 때 초기화됩니다. 내부적으로 `EnvironmentRecord`라는 데이터 구조를 통해 실행 중 생성된 변수들의 정보를 기록합니다.

- **Lexical Environment**: `Variable Environment`를 확장하여 생성됩니다. 현재 스코프와 상위 스코프를 참조하며, 변수와 함수 선언 등 현재 컨텍스트의 실행에 필요한 정보를 담습니다.

---

## 2. Variable Environment와 Lexical Environment를 나눈 이유

과거 자바스크립트는 `var` 키워드만 사용해 변수를 선언했습니다. 그러나 ES6(ECMAScript 2015) 이후, `let`과 `const`가 추가되면서 새로운 동작 방식을 지원하게 되었고, 기존 `var`와의 차이점을 처리하기 위해 실행 컨텍스트를 두 구조로 나누었습니다.

### `var`의 동작 방식

`var`는 선언된 변수에 대해 **호이스팅(Hoisting)** 이 발생합니다. 즉, 선언이 스코프의 최상단으로 끌어올려지며, 변수는 초기화되지 않았더라도 `undefined`로 미리 할당됩니다.

```javascript
console.log(a); // undefined
var a;
a = 1;
console.log(a); // 1
```

이 코드는 내부적으로 아래처럼 동작합니다:

```javascript
var a; // 선언이 호이스팅됨
console.log(a); // undefined
a = 1;
console.log(a); // 1
```

- **Variable Environment**는 이처럼 `var` 변수의 선언과 초기화를 `undefined`로 처리하며, 이후 값이 할당되면 이를 업데이트합니다.

---

### `let`과 `const`의 등장

ES6에서는 `let`과 `const`를 도입해 기존 `var`의 문제점을 해결했습니다. 이들은 호이스팅이 발생하지 않는 것처럼 보이지만, 사실은 **Temporal Dead Zone(TDZ)** 이라는 개념을 통해 선언 이전에 접근하지 못하도록 설계되었습니다.

```javascript
console.log(a); // ReferenceError
let a = 1;
console.log(a); // 1
```

#### Temporal Dead Zone(TDZ)
TDZ는 변수 선언 이전의 구간으로, 변수에 접근할 경우 **ReferenceError**가 발생합니다. 이는 코드의 예측 가능성을 높이고 선언 전에 변수에 접근하는 실수를 방지합니다.

---

### Variable Environment와 Lexical Environment의 분리

`let`과 `const`의 동작을 지원하기 위해 기존 `Variable Environment`를 수정할 수도 있었지만, 이는 복잡성과 유지보수의 어려움을 증가시켰을 것입니다. 대신 실행 컨텍스트를 두 구조로 나누는 접근 방식을 채택했습니다.

- **Variable Environment**: `var` 변수와 관련된 정보를 처리합니다.
- **Lexical Environment**: `let`, `const`, 함수 선언을 처리하며, 필요 시 `Variable Environment`의 데이터를 확장합니다.

---

## 3. Variable Environment와 Lexical Environment의 차이를 보여주는 예시

```javascript
function example() {
    console.log(a); // undefined
    var a = 10;

    if (true) {
        let b = 20;
        const c = 30;

        console.log(a); // 10
        console.log(b); // 20
        console.log(c); // 30
    }

    console.log(a); // 10
    // console.log(b); // ReferenceError
    // console.log(c); // ReferenceError
}
example();
```

1. **`var` 선언**:
    - `var a`는 `example` 함수 스코프에서 관리됩니다.
    - 변수 선언 시, `Variable Environment`의 `EnvironmentRecord`에 `a: undefined`로 저장됩니다. 이후 값이 할당되면 `10`으로 업데이트됩니다.

2. **`let`, `const` 선언**:
    - `let b`와 `const c`는 블록 스코프(`if`문) 내에서만 유효합니다.
    - 블록을 벗어나면 이 두 변수는 접근할 수 없습니다.
    - 이 변수들은 `if` 블록의 **Lexical Environment**에서 관리됩니다.

---

### Lexical Environment 생성 과정

블록 스코프를 만나면 **Variable Environment를 복사하지 않고 새로운 Lexical Environment가 생성됩니다.** 이는 스코프 체인을 통해 상위 컨텍스트에 접근할 수 있기 때문에, 별도의 복사 과정을 생략할 수 있습니다.

#### 함수와 블록 스코프에서 Lexical Environment

```javascript
function foo() {
    let a = 1;
    var b = 2;

    if (true) {
        let c = 3;
        const d = 4;

        console.log(a); // 1 (foo 함수의 Lexical Environment 참조)
        console.log(b); // 2 (foo 함수의 Lexical Environment 참조)
        console.log(c); // 3 (if 블록의 Lexical Environment에서 관리)
        console.log(d); // 4 (if 블록의 Lexical Environment에서 관리)
    }

    // console.log(c); // ReferenceError
    // console.log(d); // ReferenceError
}
foo();
```

### 스코프 체인과 Lexical Environment

1. **`foo` 함수 호출 시**:
    - `foo` 함수 실행 컨텍스트가 생성됩니다.
    - `foo`의 **Lexical Environment**와 **Variable Environment**가 초기화됩니다.
    - `Lexical Environment`는 `let`, `const` 변수를, `Variable Environment`는 `var` 변수를 관리합니다.

2. **`if` 블록 실행 시**:
    - `if` 블록에 진입하면 새로운 **Lexical Environment**가 생성됩니다.
    - 이 환경은 `c`와 `d`를 관리하며, 상위 Lexical Environment(`foo`)를 참조합니다.
    - `var` 변수 `b`는 상위 Lexical Environment에서 참조됩니다.

---

### 복사가 필요 없는 이유

Lexical Environment는 스코프 체인을 통해 상위 컨텍스트에 접근할 수 있으므로, `Variable Environment`를 복사할 필요가 없습니다. 이를 통해 메모리 사용과 실행 효율을 최적화할 수 있습니다.

```javascript
function bar() {
    let x = 10;
    var y = 20;

    if (true) {
        let z = 30;
        console.log(x); // 10 (상위 Lexical Environment 참조)
        console.log(y); // 20 (상위 Lexical Environment 참조)
        console.log(z); // 30 (현재 Lexical Environment 참조)
    }
}
bar();
```

---

## 결론

- **Variable Environment**는 `var` 변수의 선언 및 초기화를 관리하며, 초기값으로 `undefined`를 설정합니다.
- **Lexical Environment**는 `let`과 `const`를 포함한 블록 스코프 변수를 관리하며, 스코프 체인을 통해 상위 환경에 접근합니다.
- 실행 컨텍스트를 두 구조로 나눔으로써 ES6 이전의 호환성을 유지하면서도 ES6 이후의 새로운 동작 방식을 유연하게 처리할 수 있습니다.

이 설계를 통해 자바스크립트는 코드의 안정성과 유지보수성을 크게 향상시켰습니다.