# 객체
### 객체 : 리터럴 표기법

`콜론(:)`을 기준으로 왼쪽은 키 오른쪽은 값이라고 부른다

키와 값으로 구성된 것을 프로퍼티라고 부른다.

```jsx
let person = { //객체
	name: "John", //키: "name", 값: "John"
	age: 30,
    hobbies: 'sports'
}

person.isAdmin = true;
person.age = 31;

delete person.age;
person.hobbies = null;
```

- 대괄호 프로퍼티 엑세스

    ```jsx
    const userChosenKeyName = 'level'
    
    let person = {
        name: "John"
        [userChosenKeyName]: 10
    }
    
    person['name']; // 'John'
    ```

- [프로퍼티 유형] 숫자형

    ```jsx
    let numbers = {
        5 : 'kiki',
        1: 'hihi'
    }
    
    numbsers;//{1: 'hihi', 5: 'kiki'} 객체의 키 값은 정렬된다.
    ```

- [프로퍼티 유형] 심볼형

<br>

### 옵셔널 체이닝 `?.`

`?.` 은 평가 대상이 undefined 나 null 이면 평가를 멈추고 undefined 를 반환한다.

<br>

### 객체 분산 연산자

```jsx
let person = {name: "john", age: 30};
const person2 = {...person};
person.name = 'joy';

person2; //{name: 'john', age: 30}
```
<br>

### 구조 분해 할당

```jsx
const person = {name: "Max", age: 30, mbti: 'isfj'};
const {name, mbti : mbtiName, ...other} = person;
name; //'Max'
mbtiName; //'isfj'
other; //{age: 30}
```
<br>

### 참조에 의한 객체 복사

- 객체를 그대로 변수에 저장할 때 객체를 복사해서 저장하는게 아니라 객체가 저장되어있는 ‘메모리 주소’인 객체에 대한 ‘참조 값’이 저장된다.
- 객체를 복사하고 싶으면 `Object.assign` (얕은 복사) 을 사용하면 된다.

    ```jsx
    const person = {name: "Max", age: 30};
    const person2 = Object.assign({}, person);
    person.name = 'Tom';
    person2; //{name: 'Max', age: 30}
    ```
<br>

### this

- this는 사용 위치와(함수에서 사용되는 경우) 함수 호출 방법에 따라 다른 것을 참조한다.
- this는 함수 내부에서 사용되는 경우 함수를 호출한 ‘무언가’를 나타낸다. 이는 전역 컨텍스트, 객체 또는 바인딩된 데이터/객체일 수 있다.
- 함수 외부에서의 this

    ```jsx
    console.log(this); //window (엄격 모드 동일)
    ```

- 일반 함수 / 화살표 함수에서의 this

    ```jsx
    //일반 함수
    function something() { 
        console.log(this);
    }
    something(); //window (엄격 모드 동일)
    
    //화살표 함수
    const something2 = () => { 
        console.log(this);
    }
    something(); //window (엄격 모드 동일)
    ```

- 화살표 함수가 아닌 메서드에서의 this

    ```jsx
    const person = { 
        name: 'Max',
        greet: function() { // or use method shorthand: greet() { ... }
            console.log(this.name);
        }
    };
     
    person.greet(); //'Max' (this는 person 객체를 참조함)
    ```

- 화살표 함수인 메서드에서의 this

    ```jsx
    const person = { 
        name: 'Max',
        greet: () => {
            console.log(this.name);
        }
    };
    
    person.greet() // 아무것도 리턴하지 않음 (아니면 창 객체의 일부 전역 이름을 기록)
    ```

- this는 다른 객체에서 호출하는 경우 결과가 다르게 나올 수 있다.

    ```jsx
    const person = { 
        name: 'Max',
        greet() {
            console.log(this.name);
        }
    };
     
    const anotherPerson = { name: 'Manuel' };
    anotherPerson.sayHi = person.greet; // greet은 여기서 호출되는 것이 아니라 "anotherPerson" 객체의 새로운 프로퍼티/메서드에 할당됨.
    anotherPerson.sayHi(); //Manuel "anotherPerson" 객체에서 호출되었기 때문에 ‘Manuel’을 기록 => "this"는 호출한 "무언가"를 나타냄)
    ```

- 객체 내부에서 this를 사용하는 함수를 분해 할당할 경우 해당 함수 내의 this는 window로 바뀐다.

    ```jsx
    const movie = {
        title: "Dragon World",
        getFormattedTitle() {
            return this.title.toUpperCase();
        }
    }
    
    let {getFormattedTitle} = movie;
    let text = getFormattedTitle();
    
    text; //cannot read properties of undefined (reading 'toUpperCase')
    ```

- 이때 bind를 추가하면 해결된다.

    ```jsx
    let {getFormattedTitle} = movie;
    getFormattedTitle = getFormattedTitle.bind(movie);
    let text = getFormattedTitle();
    
    text; //'DRAGON WORLD'
    ```

- bind 대신 this 관리에 유용한 call과 apply도 사용할 수 있다.
  bind는 나중에 실행할 함수를 준비하고 마지막에는 새로운 함수 객체를 반환해서 저장하는 데 반해 call은 함수를 바로 실행한다. 즉 this가 가리키는 내용을 변경할 때 함수를 실행한다.

    ```jsx
    getFormattedTitle = getFormattedTitle.call(movie);
    getFormattedTitle = getFormattedTitle.apply(movie, [])
    ```

- 화살표 함수는 this를 바인딩하지 않고 컨텍스트를 유지, 즉 함수의 외부에서 바인딩 되었을 this에 바인딩한다. (= 전역 객체에 바인딩 된다)
- 화살표 함수는 this의 바인딩을 변경하지 않는다.
  일반 함수는 this의 바인딩을 시도하며 함수가 실행될 때 전역 객체에 바인딩을 하기 때문에 window 객체가 나오게 된다.

    ```jsx
    //화살표 함수 사용
    const members = {
        teamName: 'Blue Rockets', 
        people: ['Max', 'Manuel'],
        getTeamMembers() {
            this.people.forEach(p => {
                console.log(p + '-' + this.teamName)
            })
        }
    }
    
    members.getTeamMembers() //Max-Blue Rockets / Manuel-Blue Rockets
    ```

    ```jsx
    //일반 함수 사용 
    const members = {
        teamName: 'Blue Rockets', 
        people: ['Max', 'Manuel'],
        getTeamMembers() {
            this.people.forEach(function(p) {
                console.log(p + '-' + this.teamName)
            })
        }
    }
    
    members.getTeamMembers() //Max-undefined / Manuel-undefined
    ```
<br>

### 획득자(Getters) &  설정자(Setters)

- 읽기 전용 프로퍼티를 생성하고 폴백 값으로 유효성 검사를 추가하고 데이터에 접근할 때 기본적인 변화를 주는데 사용한다.

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