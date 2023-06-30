# 객체

## 객체 : 리터럴 표기법

콜론(:)을 기준으로 왼쪽은 키 오른쪽은 값이라고 부른다

키와 값으로 구성된 것을 **프로퍼티**라고 부른다.

```jsx
let person = { //객체
    name: "John", //키: "name", 값: "John"
    age: 30, 
    hobbies: 'sports'
}

person.isAdmin = true;
delete person.age; //키,값 제거
```

### Computed Property Name

```jsx
const userChosenKeyName = 'level'

let person = {
    name: "John",
    [userChosenKeyName]: 10
}

person['name']; // 'John'
```

### Number Property Name

```jsx
let numbers = {
    5 : 'kiki',
    1: 'hihi'
}

numbsers;//{1: 'hihi', 5: 'kiki'} 객체의 키 값은 정렬된다.
```

### 옵셔널 체이닝 ?.

?. 은 평가 대상이 undefined 나 null 이면 평가를 멈추고 undefined 를 반환한다.

```jsx
let person = {
    name: 'Lisa',
    options: {
        favorite: 'gitter'
    }
}

person?.options?.favorite; //'gitter'
person?.auctor?.admin; //undefined
```

<br>

## 구조 분해 할당

구조분해 할당을 통해 객체의 프로퍼티를 변수로 사용할 수 있다.

mbti를 mbtiName이란 이름으로 변경할 수 있고 나머지 객체를 모두 …other로 넣을 수 있다.

```jsx
const person = {
    name: "Max", 
    age: 30, 
    mbti: 'isfj'
};
const {name, mbti : mbtiName, ...other} = person;
name; //'Max'
mbtiName; //'isfj'
other; //{age: 30}
```

인덱스를 통해 할당할 수도 있다.

```jsx
const orders = ["First", "Second", "Third"];

const { 0: fr, 2: th } = orders;
console.log(fr); //First
console.log(th); //Third
```

<br>

## 획득자(Getters) &  설정자(Setters)

읽기 전용 프로퍼티를 생성하고 폴백 값으로 유효성 검사를 추가하고 데이터에 접근할 때 기본적인 변화를 주는데 사용한다.

```jsx
const movie = {
    info: {
        set title(val) {
            if(val.trim() === '') {
                this._title = 'DEFAULT';
                return;
            }
            this._title = val.toUpperCase();
        },
        get title() {
            return this._title;
        }
    }
}

movie.info.title = 'dragon world';
console.log(movie); //info: {_title: 'DRAGON WORLD'}
```

<br>

## new 연산자와 생성자 함수

객체의 인스턴스를 생성하는 함수이다. 동일한 프로퍼티와 메소드를 갖는 객체를 생성할 때 유용하다.

1. 함수 이름의 첫 글자는 대문자로 시작
2. 반드시 new 연산자를 붙여 사용

인자가 3개 이상으로 많아지면 객체로 전달하는 것이 좋다.

```jsx
function Person({ name, age, location }) {
    this.name = name;
    this.age = age ?? 10;
    this.location = location;
}

//[X] 인자의 순서와 값을 지켜야 한다.
const poco = new Person('poco', null, 'korea')

//[O] 인자의 순서 상관없이 갑을 전달할 수 있다.
const poco = new Person({
    name: "poco",
    location: "korea",
});
```

필수 인자와 옵션 인자를 의도하여 나눌 수 있다.

```jsx
function Person(name, { age, location }) {
    this.name = name;
    this.age = age ?? 10;
    this.location = location ?? "korea";
}

const pocoOption = {
  location: "korea",
};

//name은 필수, 나머지는 옵션에 해당한다.
const poco = new Person("poco", pocoOption);
```

<br>

## 객체 메서드의 종류

### ✅ Object.freeze

Object.freeze를 사용하면 객체 안에 있는 내용들은 동결된다.

```jsx
const STATUS = Object.freeze({
    PENDING: 'PENDING',
    SUCCESS :'SUCCESS',
    FAIL: 'FAIL'
})

STATUS.SUCCESS = 'NONE';
STATUS; //{ PENDING: 'PENDING', SUCCESS: 'SUCCESS', FAIL: 'FAIL' }

//동결 여부 확인
Object.isFrozen(STATUS.SUCCESS) //true
```

하지만 중첩되어있는 프로퍼티는 동결 되지 않는다.

```jsx
const STATUS = Object.freeze({
    SUCCESS :'SUCCESS',
    OPTIONS: {
        GREEN: 'GREEN',
        RED: 'RED'
    },
})

//수정, 삭제 가능
STATUS.OPTIONS.GREEN = 'G';
delete STATUS.OPTIONS.RED;
STATUS.OPTIONS; //{ GREEN: 'G' }

//동결 확인
Object.isFrozen(STATUS.OPTIONS); //false
```

deep Freeze를 사용하기 위해서는

1. 대중적인 유틸 라이브러리(lodash) 사용
2. 직접 유틸 함수 생성
3. stackoverflow
4. TypeScript ⇒ readonly

<br>

### ✅ Object.assign

객체를 그대로 변수에 저장할 때 객체를 복사해서 저장하는게 아니라 객체가 저장되어있는 ‘메모리 주소’인 객체에 대한 ‘참조 값’이 저장된다.

객체를 복사하고 싶으면 `Object.assign` (얕은 복사) 을 사용하면 된다.

```jsx
const person = {name: "Max", age: 30};
const person2 = Object.assign({}, person);
person.name = 'Tom';
person2; //{name: 'Max', age: 30}
```

**객체 분산 연산자**

```jsx
let person = {name: "john", age: 30};
const person2 = {...person};
person.name = 'joy';

person2; //{name: 'john', age: 30}
```

<br>

### ✅ Object.prototype.hasownProperty

hasownProperty은 let이나 const처럼 보호가 되지 않는다. 따라서 사용자가 직접 생성할 수 있기 때문에 프로토타입에 접근하여 사용해야 한다.

이는 자바스크립트 오류로 보통은 프로토타입에 접근하지 않는 것을 권장하지만 hasownProperty은 예외로 가능하다.

```jsx
const person = {
    hasOwnProperty: ()=> {
        return 'hasOwnProperty';
    },
    name: 'Tom'
}

person.hasOwnProperty('name') //'hasOwnProperty'
Object.prototype.hasOwnProperty.call(person, 'name') //true
```