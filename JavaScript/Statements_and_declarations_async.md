# [비동기] Promise, async, await

## 동기 **(Synchronous : 동시에 일어나는)**

자바스크립트는 **동기식 언어**이다.

때문에 한 번에 하나의 작업을 수행한다.

이는 즉, 한 작업이 실행되는 동안 다른 작업은 멈춘 상태를 유지하고 자신의 차례를 기다리는 것을 말한다.

이러한 동작을 **단일 스레드(싱글 스레드), 동기**라고 부른다.

```jsx
console.log('1');
console.log('2');
console.log('3');

//**결과
// 1
// 2
// 3
```
<br>

## 비동기 **(Asynchronous : 동시에 일어나지 않는)**

비동기는 요청한 결과를 나중에 결과가 주어지면 보여주는 방식을 말한다.

떄문에 동기 방식과는 달리 완료 순서가 다를 수 있다.

```jsx
console.log('1');
setTimeout(()=> {
  console.log('2');
},1000)
console.log('3');

//**결과
// 1
// 3
// 2
```

자바스크립트 엔진은 코드를 **위에서 부터 밑**으로 실행하게 된다.

setTimeout같은 브라우저 api를 만나게되면 **무조건 브라우저에 요청**을 하게된다

“브라우저야 1초 뒤에 실행해줘~”

응답을 기다리지 않고 다음 함수를 실행하며 1초 뒤에 브라우저에 요청을 받게 되면 해당 함수를 실행시킨다.

동기와 비동기를 카페에서 음료를 주문할 때로 예시를 들어보면 다음과 같다.

<img src="../assets/JavaScript/async_cafe1.png" width="500">

1. 카페에 들어간다
2. 앞사람의 음료가 나올 때까지 순서를 기다린다.
3. 음료를 주문하고 나올 때까지 기다린다.
4. 자리를 찾아서  앉는다

<img src="../assets/JavaScript/async_cafe2.png" width="500">

1. 카페에 들어간다
2. 음료를 주문하고 진동벨을 받는다
3. 자리를 찾아서 앉는다
4. 진동벨이 울리면 음료를 가지고 온다

<br>

## CallBack

바로 실행되는 것이 아니라 다른 함수 실행이 끝낸 뒤 실행되는 즉, callback되는 함수를 말한다.

“나중에 실행해줘~ 나중에 다시 불러줘” → CallBack

### ✅ Synchronous callback

즉각적으로 실행되는 함수

```jsx
function printNow(print) {
	print();
}

printNow(() => console.log('hello'));
```

### ✅ Asynchronous callback

언제 실행되는지 예측 불가능한 함수

```jsx
function printDelay(print, timeout) {
	setTimeout(print, timeout);
}

printDelay(() => console.log('hello'), 2000);
```

### ✅ CallBack 지옥

콜백 지옥은 JavaScript를 이용한 비동기 프로그래밍시 발생하는 문제로서,

함수의 매개변수로 넘겨지는 콜백 함수가 반복되어 **코드의 들여쓰기 수준이 감당하기 힘들 정도로**

**깊어지는 현상**을 말한다.

```jsx
step1(function (err, value1) {
    if (err) {
        console.log(err);
        return;
    }
    step2(function (err, value2) {
        if (err) {
            console.log(err);
            return;
        }
        step3(function (err, value3) {
            if (err) {
                console.log(err);
                return;
            }
            step4(function (err, value4) {
                // 정신 건강을 위해 생략
            });
        });
    });
});
```

이를 해결하기 위해 두가지 방법이 있다.

1. 콜백 함수를 분리한다.

    ```jsx
    step1(afterStep1);
    
    function afterStep1(value1) {
        step2(afterStep2);
    }
    
    function afterStep2(value2) {
        step3(afterStep3);
    }
    
    // 생략
    ```

    위와 같이 사용하면 콜백 함수를 사용하더라도 들여쓰기기가 깊어지지 않아 가독성이 향상된다.
    
    하지만 전 단계에서 정의된 변수등을 다음 함수에서 사용할 수 없다는 점과
    
    완전히 바깥쪽에 변수를 선언해 사용해야 한다는 점, 함수 이름 짓기가 힘들어진다는 단점이 있다.

2. Promise, async 패턴 도입

<br>

## Promise

```jsx
const promise = new Promise((resolve, reject) => {
  // 비동기 작업
	reselve() //객체, 배열, 문자 모든게 올 수 있다.
	reject()
});

promise
	.then(response => {
		//성공 콜백 함수
	}, err => {
		//then 처리 과정에서 오류가 날 경우 두번째 인자는 오류를 처리하는 함수를 적는다
		//catch를 더 많이 쓴다.
	})
	.catch(response => {
		//실패 콜백 함수
	})
	.finally(() => {
		//무조건 호출하는 함수
		//인자가 없음
	})
```

then은 새로운 promise가 담길 수 있다.

then은 순차적으로 진행되며 중간에 promise가 오는 경우 반환값을 기다리게 된다.

```jsx
const setTimer = (duration) => {
  let promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(new Date().getSeconds())
    },duration)
  });
  return promise
}

setTimer(2000)
  .then(response => {
    console.log('시작', response)
    return setTimer(5000)
  })
  .then(response => {
    console.log('5초 후', response)
    return setTimer(2000)
  })
  .then(response => {
    console.log('7초 후', response)
  })

//시작 13
//5초 후 18
//7초 후 20
```

오류를 처리할 수 있다.

```jsx
promise
	.then(response => {
		response = response * 1 //1. 실행
	})
	.catch(response => {
		return '💡' //중간 에러시 처리
	})
	.then(response => {
		return new Promise((resorve, reject) => {
			resorve(response - 1) 
		})
	})
```

### ✅ Promise.all : 병렬

```jsx
async function getApple() {
    await delay(2000);
    return "🍎";
}

async function getBanana() {
	  await delay(1000); //1. 딜레이 후
    return "🍌"; //2. 리턴 출력
}

function pickAllFruits() {
    //3. 모두 응답 후 출력
    return Promise.all([getApple(), getBanana()]).then((fruits) =>
        fruits.join("+")
    );
}

pickAllFruits().then(console.log); //🍎+🍌
```

### ✅ Promise.race

```jsx
...
function pickOnlyOne() {
    //빠르게 응답받은 하나만 출력
    return Promise.race([getApple(), getBanana()]);
}

pickOnlyOne().then(console.log); //🍌
```

<br>

## Async / await

함수 앞에 async를 붙이면 함수  내부에서 일어나는 일이 보이지 않게 바뀌며 반드시 promise를 반환한다.

```jsx
//async를 사용한 함수
async function f() {
  return 1;
}

f().then(alert); // 1

//동일한 결과의 promise
async function f() {
  return Promise.resolve(1);
}

f().then(alert); // 1
```

await 은 반드시 async 안에서만 사용할 수 있다.

자바스크립트는 await를 만나면 promise가 처리될 때까지 기다린다.

기다리는 동안에 엔진은 다른 일을 처리할 수 있기 때문에 CPU 리소스가 낭비되지 않는다.

```jsx
const setTimer = (duration) => {
    let promise = new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(new Date().getSeconds())
        },duration)
    });
    return promise
}

async function getSecound() {
    const a = await setTimer(2000);
    const b = await setTimer(5000);
    console.log(a,b)
}
console.log(new Date().getSeconds()); //19
getSecound(); //21, 26
```

async 내에서 await을 만나면 실행이 끝날 때까지 기다리게 된다

따라서 위와 같이 2초 뒤에 21초가 반환되고 5초 뒤에 26초가 반환되며 a와 b의 값이 모두 있으므로 출력 후 함수가 종료된다.



### ✅ try catch 오류 처리

```jsx
async function getUser() {
	  let user; 
	  
	  try {
	    user = await getUserData();
	  } catch {
	    console.log('error');
	  }
}
```