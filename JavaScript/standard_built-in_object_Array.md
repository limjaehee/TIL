# Array

## 배열의 특성

### 유사 배열 객체

실제 배열은 아니지만 배열과 비슷한 특성이 있고 몇가지 사용한 메소드가 있다. 노드 리스트, 문자열 등이 있다

```jsx
function genPriceList() {
    console.log(arguments); //[1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
    console.log(Array.isArray(arguments)) //false
}

genPriceList(1,2,3)
```

arguments는 유사 배열 객체로 map과 같은 기능을 사용 할 수 없다. 해당 기능을 사용하고 싶으면 배열로 변경 후 사용하면 된다.

```jsx
function genPriceList() {
    return Array.from(arguments).map((v)=> v+'원');
}
```

### 배열은 객체다.

자바스크립트 배열에 다음과 같이 입력하면 마치 객체처럼 키와 값이 추가되는 것을 볼 수 있다

```jsx
let arr = [1,2,3];
arr['p'] = 'property';

arr; //[1, 2, 3, p: 'property']
```

사실 배열은 내부적으로 의미없는 인덱스가 있는 객체일 수 있다.

```jsx
let arr = [1,2,3];

let arr = {
    0: 1, 
    1: 2, 
    2: 3,
}
```

### array.length 특성

array.length는 길이를 보장하지 않고 마지막 인덱스를 반환한다.

```jsx
let arr = [1,2,3];

//array 9 인덱스에 임의 숫자를 넣는 경우
arr[9] = 10;
arr; //[1, 2, 3, empty × 6, 10]

//array 길이를 임의로 조작하는 경우
arr.length = 0;
arr; //[]
```

<br>

## 배열 생성 방법

```jsx
[1, 2, 3];
new Array('Hi!', 'Welcome!'); //['Hi!', 'Welcome!']
new Array(5); // [empty x 5] 길이만큼의 빈 배열이 생성된다.
Array(5, 2); // [5, 2]
Array.of(1,2); //[1, 2]
```

Array.from()은 이터러블이나 유새 배열 객체를 변환하는데 진짜 배열은 아니다.

```jsx
let object = {
    0: 1,
    1: 2,
    length: 2,
}

let arr = Array.from(object);
arr; //[1,2]
Array.isArray(arr) //true
```

<br>

## 배열 메서드의 종류

### ✅ shift(), unshift()

```jsx
const hobbies = ['Sports', 'Cooking'];
hobbies.push('Reading'); //['Sports', 'Cooking','Reading']
hobbies.unshift('Coding');//['Coding','Sports', 'Cooking','Reading']
//hobbies.pop(); //['Coding','Sports', 'Cooking'] 마지막 요소 삭제
const poppedValue = hobbies.pop();
hobbies.shift() //['Sports', 'Cooking']

hibbies[1] = 'Coding'; //['Sports', 'Cooking']
//hobbies[5] = 'Reading'; //['Sports', 'Cooking', empty x 3, 'Reading']
```

- shift()는 배열을 왼쪽으로 한칸씩 이동하게 한다.
- unshift()는 배열을 오른쪽으로 한칸씩 이동하게 한다.
- shift()과 unshift()는 push()와 pop()보다 실행이 느리다. 배열의 마지막 요소를 수정하는 것보다 배열의 모든 요소를 건드리는 것은 다르기 때문이다.

### ✅ splice()

```jsx
const hobbies = ['Sports', 'Cooking'];
hobbies.splice(1, 0, 'Good Food'); //['Sports', 'Good Food', 'Cooking']
//hobbies.splice(0, 1); //['Good Food', 'Cooking']
const removedElements = hobbies.splice(-1, 1) 
//삭제한 요소를 반환 ['Sports', 'Good Food']
```

### ✅ slice()

```jsx
const testResults = [1, 5.3, 1.5, 10.99, -5, 10];
const storedResults = testResults.slice();
//testResults.slice(0, 2) // [1, 5.3]
//testResults.slice(-3, -1) // [10.99, -5]
//testResults.slice(2) // [1.5, 10.99, -5, 10]

testResults.push(5, 9,1)
```

- slice를 사용하면 기존 배열 기반으로 새 배열의 복사본을 저장한다.
- 두번째 인수의 인덱스 부분까지 자르지만 두번째 인수의 벨류는 포함하지 않는다.
- 인수를 하나만 작성할 경우 인수의 시점부터 배열 끝을 반환한다.
- 인수가 음수면 인덱스를 뒷 부분부터 계산한다

### ✅ concat()

```jsx
const testResults = [1, 5.3, 1.5, 10.99, -5, 10];
const storedResult = testResult.concat([3.99, 2]); //[1, 5.3, 1.5, 10.99, -5, 10, 3.99, 2]
```

- 기존 배열에 새로운 요소를 추가해 복사된 새로운 배열을 반환한다.

### ✅ indexOf()

```jsx
const testResults = [1, 5.3, 1.5, 10.99, -5, 10];
testResults.indexOf(1.5) //
```

- 인수와 일치하는 첫번째 요소가 있으면 종료한다.

```jsx
const personData= [{name: 'Max'}, {name: 'Manuel'}];
personData.indexOf({name: 'Manuel'}) //-1
```

- 또한 원시값만 찾을 수 있고 참조값은 찾을 수 없다.
- 두 객체는 엄밀히 다르다.

### ✅ find()

```jsx
const personData= [{name: 'Max'}, {name: 'Manuel'}];
const manuel = personData.find((person, idx, persons) => {
    return person.name === 'Manuel'
}) //{name: 'Manuel'}

manuel.name = 'Anna';
console.log(manuel, personData);
//'Anna', [{name: 'Max'}, {name: 'Anna'}]
```

- `find는 복사본을 생성하지 않는다.

### ✅ findIndex()

```jsx
const personData= [{name: 'Max'}, {name: 'Manuel'}];
const maxIndex = personData.find((person, idx, persons) => {
    return person.name === 'Max'
}) //0
```

### ✅ includes()

```jsx
const testResults = [1, 5.3, 1.5, 10.99, -5, 10];
testResults.includes(10.99) //true
```

### ✅ forEach()

```jsx
const prices = [10.99, 5.99, 3.99, 6.59];
const tax = 0.19;
const taxAdjustedPrices = [];

prices.forEach((price, idx, prices)=> {
    const priceObj = {index: idx, taxAdjPrice: price *(1 * tax) };
    taxAdjustedPrices.push(priceObj);
})
```

- for 반복문 대신 사용할 수 있다.
- map과 다른 점은 결과값이 반환되지 않고 배열을 순회한다.
- forEach는 for문처럼 중간에 흐름을 제어할 수 없다. try-catch 문을 사용하여 만들 수는 있으나 추천하지 않는 방법이다.

    ```jsx
    let arr = [1,2,3];
    
    arr.forEach((v)=> {
        if(v === 'false') {
            break; //Illegal break statement
        }
    });
    ```


### ✅ map()

```jsx
const prices = [10.99, 5.99, 3.99, 6.59];
const tax = 0.19;

const taxAdjustedPrices = prices.map((price, idx, prices)=> {
    const priceObj = {index: idx, taxAdjPrice: price *(1 * tax) };
    return priceObj 
})
```

- 배열의 각 요소에 관해 전환한 새로운 배열을 반환한다.

### ✅ sort()

```jsx
const prices = [5.99, 10.99, 3.99, 6.59];
const sortedPrices = price.sort((a, b) => {
    if(a > b) return 1; 
    else if(a === b) return 0; 
    else return -1;
})
```

- 기본값으로 sort는 문자열로 해석해 정렬한다.
- 아래를 조건 없이 정렬하면 10.99가 맨 앞에 오게 되는데 문자 앞 1이 있기 때문이다
- 따라서 숫자를 정렬할 때에는 다음과 같은 조건이 필요하다.

### ✅ reverse()

- 반대로 정렬하는 메소드

### ✅ filter()

- 조건에 맞는 새로운 배열을 생성한다.

```jsx
const prices = [10.99, 5.99, 3.99, 6.59];
//const filteredArray = prices.filter((price, idx, prices)=> {
//	return price > 6;
//})
const filteredArray = prices.filter(p => p > 6);
```

### ✅ reduce()

```jsx
const prices = [10.99, 5.99, 3.99, 6.59];
//const sum = prices.reduce((pre, cur, curIndex, prieces) => {
//	return pre + cur;
//}, 0)

const sum = prices.reduce((pre, cur) =>  pre + cur, 0)
```

- pre는 초기값이다.

### ✅ split()

```jsx
const data = 'new york;10.99;2000';
const transformedData = data.split(';'); //['new york', '10.99', '2000']
```

- 구분자를 지정하여 배열로 변환한다.

### ✅ join()

```jsx
const nameFragments = ['Max', 'Schwarz'];
const name = nameFragments.join() //'Max,Schwarz'
const name = nameFragments.join(' ') //'Max Schwarz'
```

- 구분자를 지정하여 글자로 합친다.

### ✅ 분산 연산자 (…)

```jsx
const nameFragments = ['Max', 'Schwarz'];
const copiedNameFragments = [...nameFragments];

const prices = [10.99, 5.99, 3.99, 6.59];
Math.min(...prices); //3.99
```

- 배열의 모든 요소를 꺼낸다.
- **전개 구문**이라고도 한다.
- 복사본이면서 새로운 배열을 생성하기 때문에 원본 배열의 변화가 반영되지 않는다.
- 하지만 안에 객체가 있을 경우 해당 주소를 복사한 것이기 때문에 객체의 변화는 반영된다.

### ✅ 배열 구조 분해

```jsx
const nameData = ['Max', 'Schwarz', 'Mr', 30];
const [firstName, lastName, ...other] = nameData; //Max, Schwarz, ['Mr', 30]
```

### ✅ Set

```jsx
const ids = new Set(['Hi','from','set']);

ids.add(2);
if(ids.has('Hi')) ids.delete('Hi');

for (const entry of ids.entries()) {
    console.log(entry[0])
}
```

- Set은 데이터 구조로 **고유한 값을 관리**할 때 유용하다.
- 예시로 같은 아이디가 있는지 확인 하기 위한 방법도 있다.

### ✅ Map

```jsx
const person1 = {name: 'Max'};
const person2 = {name: 'Manuel'};

const personData = new Map([[person1, [{data: 'yesterday', price: 10}]]]);
personData.get(person1)
personData.set(person2, [{date: 'two weeks ago', price: 20}
])

for (const entry of personData.entries()) {
    console.log(entry)
    //[{…}, Array(1)]
    //[{…}, Array(1)]
}

for (const [key, value] of personData.entries()) {
    console.log(key, value)
    //{name: 'Max'} [{…}]
    //{name: 'Manuel'} [{…}]
}

for (const key of personData.keys()) {
    console.log(key)
    //{name: 'Max'}
    //{name: 'Manuel'}
}

for (const value of personData.values()) {
    console.log(value)
    //[{…}] - 0: {data: 'yesterday', price: 10}
    //[{…}] - 0: {date: 'two weeks ago', price: 20}
}

personData.size //2
```

- 객체 자체를 키로 사용해서 키-값 쌍의 값 데이터를 가져올 수 있다.
- 어떤 자료형이여도 키로 사용할 수 있다.
- 데이터 양이 많을 경우 map이 좀 더 유리하다.
- 데이터를 빠르게(초당) 추가하거나 삭제할 때 유리하다

### ✅ WeakSet, WeakMap

```jsx
let person = {name: "Max"};
const persons = new WeakSet();
persons.add(person);

person = null;
persons; //WeakSet {{…}}
```

```jsx
let person = {name: "Max"};
const persons = new WeakSet();
persons.add(person);

const personData = new WeakMap();
personData.set(person, 'Extra info!');
person = null
personData; //WeakMap {{…} => 'Extra info!'}
```

- 객체 데이터를 저장해야 한다.
- 가비지 컬렉션으로 포함되어 있는 해당 항목을 직접 삭제해서 더 이상 코드에 남아있지 않게 한다.