# Tagged Template Literals

## Template Literal

동적인 문자열을 처리할 때 백틱(`)을 사용하여 출력할 수 있다.

```jsx
const userName = 'Tom';
const age = 20;
const output = 'Hi, ${userName} and I am ${age}';
```

## **Tagged Template Literal**

태그드(Tagged) 탬플릿은 리터럴의 발전된 형태로써, 함수 형태로 사용할 수 있다.

또한 복잡한 논리를 사용할 때 출력값을 생성하고자 하는 경우에 활용할 수 있다.

```jsx
function productDescription(strings, productName, productPrice) {
    console.log(strings); //(3) ['This product (', ') is ', '.', raw: Array(3)]
    console.log(productName); //JavaScript Course
    console.log(productPrice); //29.9
		let priceCategory = 'cheap';
    if(productPrice > 20) {
        priceCategory = 'fair';
    }
    return `${strings[0]}${productName}${strings[1]}${priceCategory}${strings[2]}`
}

const prodName = 'JavaScript Course';
const prodPrice = 29.9;

const productOutput = productDescription`This product (${prodName}) is ${prodPrice}.`;
console.log(productOutput); //This product (JavaScript Course) is fair.
```

productDescription 함수는 일반적인 함수 호출 방식인 productDescription() 이 아닌 productDescription`` 형태이다.

productDescription 함수의 첫번째 인자는 모든 동적이지 않은 부분을 제외하고 정적인 문자열 부분을 반환한다. 나머지 인자는 동적 데이터를 순서대로 저장한다.