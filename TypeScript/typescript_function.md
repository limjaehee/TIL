# TypeScript 함수

## 함수의 반환 타입

### ✅ 함수의 인자

기존 자바스크립트의 함수 선언 방식에서 **매개변수**와 함수의 **반환 값**에 타입을 추가한다.

함수의 인자는 모두 필수 값으로 간주된다. 또한 인자는 추가로 받을 수 없으며 적게 받을 수도 없다.

```tsx
function add(n1: number, n2: number): number {
  return n1 + n2;
}

add(10, 20); // 30

//An argument for 'b' was not provided.
add(10);

// Argument of type 'string' is not assignable to parameter of type 'number'.
add(10, "a");

//Argument of type 'null' is not assignable to parameter of type 'number'.
add(10, null);

/// error, too many parameters
add(10, 20, 30);
```

매개변수의 갯수만큼 인자를 넘기지 않고 싶다면  `?` 를 이용해서 정의한다.

이때 b의 값이 없으면 undefined가 되기 때문에 아래와 같이 조건을 추가한다.

```tsx
function sum(a: number, b?: number): number {
  if (b) return a + b;
  
  return a;
}
```

매개변수 초기화는 ES6 문법과 동일하다.

```tsx
function sum(a: number, b = 100): number {
    return a + b;
}

sum(10) //110
sum(10, undefined) //110

//Argument of type 'null' is not assignable to parameter of type 'number | undefined'
sum(10, null)
```

### ✅ 함수의 반환 값이 없는 경우

함수의 반환 값이 없을 경우 void를 사용해 의도적으로 반환문이 없다는 것을 명시해준다.

```tsx
function printResult(num: number): void {
  console.log("Result: " + num);
}
```

### ✅ REST 문법이 적용된 매개변수

```tsx
function sum(a: number, ...nums: number[]): number {
    let total: number = 0;
    nums.forEach((num) => (total += num));

    return a + total;
}

console.log(sum(10, 30, 40, 50)) //130
```

<br>

## 타입의 기능을 하는 함수

```tsx
function add(n1: number, n2: number): number {
  return n1 + n2;
}

let combineValues; //combineValues는 any 타입으로 지정된다.

combineValues = add;
combineValues = 5; //let combineValues: number
console.log(combineValues(8, 8)); //error
```

위의 예시에서 combineValues는 처음에 any 타입으로 지정되며

숫자 5를 할당할 때 combineValues의 타입은 number로 지정되기 때문에 아래 콘솔에서 에러가 난다.

이런 에러를 방지하기 위해 combineValues가 함수를 지니게 된다고 명시해야 한다.

```tsx
...

let combineValues: Function;

combineValues = add;
combineValues = 5; //Type 'number' is not assignable to type 'Function'.
console.log(combineValues(8, 8));
```

combineValues 변수에 다른 함수를 할당할 경우 동작이 되는 현상이 발생한다.

combineValues는 모든 함수 타입을 허용하기 때문이다.

이로 인해 다른 타입을 반환할 가능성이 있으며 실수를 할 가능성이 있다.

```tsx
...

combineValues = () => {
  return "a";
};

console.log(combineValues(8, 8));
```

이를 방지하기 위해 함수 타입을 생성하여 원하는 함수의 반환 타입을 지정해준다.

```tsx
...

let combineValues: (a: number, b: number) => number;

combineValues = add;
combineValues = () => {
  return "a";
}; //string' is not assignable to type '(a: number, b: number) => number'.
console.log(combineValues(8, 8));
```

<br>

## callback 함수 타입

콜백의 함수 타입은 다음과 같이 작성한다.

여기서는 반환값이 없으므로 void를 추가한다.

```tsx
function addAndHandle(n1: number, n2: number, cb: (num: number) => void) {
  const result = n1 + n2;
  cb(result);
}

addAndHandle(10, 20, (result) => {
  console.log(result); //30
});
```

<br>

## This 함수

```tsx
let deck = {
    suits: ["hearts", "spades", "clubs", "diamonds"],
    cards: Array(52),
    createCardPicker: function () {
        // NOTE: 아랫줄은 화살표 함수로써, 'this'를 이곳에서 캡처할 수 있도록 합니다
        return () => {
            let pickedCard = Math.floor(Math.random() * 52);
            let pickedSuit = Math.floor(pickedCard / 13);

            return { suit: this.suits[pickedSuit], card: pickedCard % 13 };
        };
    },
};

let cardPicker = deck.createCardPicker();
let pickedCard = cardPicker();

console.log("card: " + pickedCard.card + " of " + pickedCard.suit);
```