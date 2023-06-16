## 모듈

모듈은 파일 하나를 뜻하며 스크립트 하나는 모듈 하나이다.

모듈에 특수한 지시자 export와 import를 적용하면 다른 모듈을 불러와 불러온 모듈에 있는 함수를 호출하는 것과 같은 기능 공유가 가능하다.

export를 사용해 함수를 외부로 보내기

```jsx
//hello.js
export function hello(user) {
  return `Hello, ${user}!`;
}
```

import를 사용해 외부 함수 사용하기

```jsx
//main.js
import { hello } from "./hello.js";

console.log(hello("Tom")); //  Hello, Tom!
```

```html
<script src="assets/scripts/main.js" type="module"></script>
```

모듈은 특수한 키워드나 기능과 함께 사용되므로 `<script type=”module”>`과 같은 속성을 설정해 해당 스크립트가 모듈이란 걸 브라우저가 알 수 있게 해줘야 한다.

<br>

## 모듈 기능

### 지연 실행

모듈 스크립트는 마치 defer 속성을 붙인 것처럼 항상 지연 실행된다.

```html
<script type="module">
  alert(typeof button); // 페이지가 모두 로드되고 난 다음에 alert 함수가 실행
  // 얼럿창에 object가 정상적으로 출력됨. 모듈 스크립트는 아래쪽의 button 요소를 '볼 수' 있음.
</script>

하단의 일반 스크립트와 비교해 봅시다.

<script>
  alert(typeof button); // 일반 스크립트는 페이지가 완전히 구성되기 전이라도 바로 실행
  // 버튼 요소가 페이지에 만들어지기 전에 접근하였기 때문에 undefined가 출력
</script>

<button id="button">Button</button>
```

### 외부 스크립트

1. src 속성값이 동일한 외부 스크립트는 한 번만 실행된다.

    ```html
    <!-- my.js는 한 번만 로드 및 실행됩니다. -->
    <script type="module" src="my.js"></script>
    <script type="module" src="my.js"></script>
    ```

2. 외부 사이트같이 다른 오리진에서 모듈 스크립트를 불러오려면 CORS 헤더가 필요하다.

    ```html
    <!-- another-site.com이 Access-Control-Allow-Origin을 지원해야만 외부 모듈을 불러올 수 있습니다.-->
    <!-- 그렇지 않으면 스크립트는 실행되지 않습니다.-->
    <script type="module" src="http://another-site.com/their.js"></script>
    ```

<br>

## 모듈 내보내기

변수나 함수, 클래스를 선언할 때 맨 앞에 export를 붙이면 내보내기가 가능하다.

```jsx
// 배열 내보내기
export let months = ['Jan', 'Feb', 'Mar','Apr', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];

// 상수 내보내기
export const MODULES_BECAME_STANDARD_YEAR = 2015;

// 클래스 내보내기
export class User {
  constructor(name) {
    this.name = name;
  }
}
```

선언부와 export가 떨어져 있어도 내보내기를 할 수 있다.

```jsx
function sayHi(user) {
  alert(`Hello, ${user}!`);
}

function sayBye(user) {
  alert(`Bye, ${user}!`);
}

export {sayHi, sayBye}; // 두 함수를 내보냄
```

<br>

## 모듈 가져오기

가져올 때 목록을 만들어 import {…} 안에 적어준다.

```jsx
// 📁 main.js
import {sayHi, sayBye} from './say.js';

sayHi('John'); // Hello, John!
sayBye('John'); // Bye, John!
```

가져올 것이 많으면 import * as <obj>처럼 객체 형태로 원하는 것들을 가지고 올 수 있다.

```jsx
// 📁 main.js
import * as say from './say.js';

say.sayHi('John');
say.sayBye('John');
```

as를 사용하면 이름을 바꿔서 모듈을 가져올 수 있다.

```jsx
// 📁 main.js
import {sayHi as hi, sayBye as bye} from './say.js';

hi('John'); // Hello, John!
bye('John'); // Bye, John!
```

export에도 as를 사용할 수 있으며 다른 모듈에서 이 함수들을 가져올 때 이름은 hi와 bye가 된다.

```jsx
// 📁 say.js
...
export {sayHi as hi, sayBye as bye};
```

<br>

## export default

모듈은 크게 두 종류로 나뉜다.

1. 복수의 함수가 있는 라이브러리 형태의 모듈
2. 개체 하나만 선언되어있는 모듈

두번째 방식으로 모듈을 만드는 게 보통이기 때문에 함수, 클래스, 변수 등의 개체는 전용 모듈 안에 구현한다.

모듈은 export default라는 특별한 문법을 지원하며 이를 사용하면 해당 모듈엔 개체가 하나만 있다라는 사실을 명확하게 나타낼 수 있다.

```jsx
export default class User {
  constructor(name) {
    this.name = name;
  }
}
```

이렇게 default를 붙여서 모듈을 내보내면 중괄호{} 없이 모듈을 가져올 수 있다.

```jsx
import User from './user.js'; // {User}가 아닌 User로 클래스를 가져왔다.

new User('John');
```

export default를 사용할 경우 내보낼 개체에 이름이 없어도 상관없다.

```jsx
export default class { // 클래스 이름이 없음
  constructor() { ... }
}

export default function(user) { // 함수 이름이 없음
  alert(`Hello, ${user}!`);
}

// 이름 없이 배열 형태의 값을 내보냄
export default ['Jan', 'Feb', 'Mar','Apr', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];
```

<br>

## default name

default 키워드는 기본 내보내기를 참조하는 용도로 사용된다.

```jsx
function sayHi(user) {
  alert(`Hello, ${user}!`);
}

// 함수 선언부 앞에 'export default'를 붙여준 것과 동일합니다.
export {sayHi as default};
```

*를 사용해 모든 것을 객체 형태로 가져오는 경우엔 default 프로퍼티는 정확히 default export를 가리킨다.

```jsx
import * as user from './user.js';

let User = user.default; // default export
new User('John');
```

<br>

## 동적으로 모듈 가져오기

import 문이나 export 문은 정적인 방식을 사용한다.

때문에 런타임이나 조건부로 모듈을 불러올 수 없었다.

### import() 표현식

import(module) 표현식은 모듈을 읽고 이 모듈이 내보내는 것들을 모두 포함하는 객체를 담은 이행된 프로미스를 반환한다.

async 함수 안에서 let module = await import(modulePath)와 같이 사용하는 것도 가능하다.

```jsx
// 📁 say.js
export function hi() {
  alert(`안녕하세요.`);
}

export function bye() {
  alert(`안녕히 가세요.`);
}
```

```jsx
let {hi, bye} = await import('./say.js');

hi();
bye();
```