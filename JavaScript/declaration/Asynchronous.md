# [비동기] Promise, async, await

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