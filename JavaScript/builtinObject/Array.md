# 배열

### 유사 배열 객체

- 실제 배열은 아니지만 배열과 비슷한 특성이 있고 몇가지 사용한 메소드가 있다.
- 노드 리스트, 문자열 등이 있다

<hr>

### 배열 생성 방법

```jsx
const numbers = [1, 2, 3];
const numbers = new Array('Hi!', 'Welcome!'); //['Hi!', 'Welcome!']
const numbers = new Array(5); // [empty x 5] 길이만큼의 빈 배열이 생성된다.
const numbers = Array(5, 2); // [5, 2]
const numbers = Array.of(1,2); //[1, 2]
const numbers = Array.from([1, 2, 3]) //[1, 2, 3]
const numbers = Array.from('Hi!') //['H', 'i', '!']
```

- Array.from()은 이터러블이나 유새 배열 객체를 변환하는데 진짜 배열은 아니다.

<hr>

### 배열 메서드의 종류

#### shift(), unshift()

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

<br>

#### splice()

```jsx
const hobbies = ['Sports', 'Cooking'];
hobbies.splice(1, 0, 'Good Food'); //['Sports', 'Good Food', 'Cooking']
//hobbies.splice(0, 1); //['Good Food', 'Cooking']
const removedElements = hobbies.splice(-1, 1) 
//삭제한 요소를 반환 ['Sports', 'Good Food']
```

<br>

#### slice()

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

<br>

#### concat()

```jsx
const testResults = [1, 5.3, 1.5, 10.99, -5, 10];
const storedResult = testResult.concat([3.99, 2]); //[1, 5.3, 1.5, 10.99, -5, 10, 3.99, 2]
```

- 기존 배열에 새로운 요소를 추가해 복사된 새로운 배열을 반환한다.

<br>

#### indexOf()

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

<br>

#### find()

```jsx
const personData= [{name: 'Max'}, {name: 'Manuel'}];
const manuel = personData.find((person, idx, persons) => {
    return person.name === 'Manuel'
}) //{name: 'Manuel'}

manuel.name = 'Anna';
console.log(manuel, personData);
//'Anna', [{name: 'Max'}, {name: 'Anna'}]
```

- `find`는 복사본을 생성하지 않는다.

<br>

#### findIndex()

```jsx
const personData= [{name: 'Max'}, {name: 'Manuel'}];
const maxIndex = personData.find((person, idx, persons) => {
    return person.name === 'Max'
}) //0
```

<br>

#### includes()

```jsx
const testResults = [1, 5.3, 1.5, 10.99, -5, 10];
testResults.includes(10.99) //true
```

<br>

#### forEach()

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

<br>

#### map()

```jsx
const prices = [10.99, 5.99, 3.99, 6.59];
const tax = 0.19;

const taxAdjustedPrices = prices.map((price, idx, prices)=> {
    const priceObj = {index: idx, taxAdjPrice: price *(1 * tax) };
    return priceObj 
})
```

- 배열의 각 요소에 관해 전환한 새로운 배열을 반환한다.

<br>

#### sort()

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

<br>

#### reverse()
- 반대로 정렬하는 메소드

<br>

#### filter()
- 조건에 맞는 새로운 배열을 생성한다.

```jsx
const prices = [10.99, 5.99, 3.99, 6.59];
//const filteredArray = prices.filter((price, idx, prices)=> {
//	return price > 6;
//})
const filteredArray = prices.filter(p => p > 6);
 ```

<br>

#### reduce()

```jsx
const prices = [10.99, 5.99, 3.99, 6.59];
//const sum = prices.reduce((pre, cur, curIndex, prieces) => {
//	return pre + cur;
//}, 0)

const sum = prices.reduce((pre, cur) =>  pre + cur, 0)
```

- pre는 초기값이다.

<br>

#### split()

```jsx
const data = 'new york;10.99;2000';
const transformedData = data.split(';'); //['new york', '10.99', '2000']
```

- 구분자를 지정하여 배열로 변환한다.

<br>

#### join()

```jsx
const nameFragments = ['Max', 'Schwarz'];
const name = nameFragments.join() //'Max,Schwarz'
const name = nameFragments.join(' ') //'Max Schwarz'
```

- 구분자를 지정하여 글자로 합친다.

<br>

#### 분산 연산자 (…)

```jsx
const nameFragments = ['Max', 'Schwarz'];
const copiedNameFragments = [...nameFragments];
```

- 배열의 모든 요소를 꺼낸다.
- 전개 구문이라고도 한다.