# 오류 처리

## ✅ try / catch

try / catch 문은 실행할 코드블럭을 표시하고 예외(exception)가 발생(throw)할 경우의 응답을 지정한다.

```jsx
try {
  //예외가 예상되는 코드 혹은 발생시킬 코드
} catch (error) {
  //예외를 처리하는 코드
}

```

util코드 같이 작은 코드도 try 안에 넣는 것이 좋다. (작은 실수를 미연에 방지할 수 있기 때문)

```jsx
function util() {
  return;
}

function handleSubmit(input) {
  try {
    util();
  } catch (error) {}
}
```

catch를 작성할 때 다음을 고려해야 한다.

1. 개발자를 위한 예외처리
2. 사용자를 위한 예외처리
3. 사용자에게 사용을 제안
4. 에러 로그 수집
5. 재귀 호출

```jsx
function handleSubmit(input) {
  try {
    //
  } catch (error) {
    //1. 개발자를 위한 예외처리 => 동료 개발자, 자기 자신
    console.warn(error);
    console.error(error);

    //2. 사용자를 위한 예외처리 => 사용자가 볼 수 있다
    alert('404, not found') // X
    alert('잠시만 기다려주세요, 어떤 문제가 있습니다. 다시 시도해주세요.'); // O

    //3. 사용자에게 사용을 제안
    history.back();
    history.go('안전한 어딘가로..');
    clear();
    Element.focus(); // => 어딘가로 이동을 시켜서 다시 한번 사용자에게 알려주기

    //4. 에러 로그 수집
    sentry.전송();

    //5. 비추천하지만 필요에 따라 사용되는 경우 => 재귀 호출
    handleSubmit();
  }
}
```

finally는 try 선언이 완료된 이후에 실행된다. 이는 예외 발생 여부와 상관없이 실행된다. 주로 데이터 분석을 위한 로그를 심는다.

```jsx
try {
  //
} catch (error) {
  //
} finally {
  //데이터 분석을 위한 로그
}

```

<br>

## ✅ throw

throw문은 사용자 정의 예외를 발생(throw)할 수 있다. 예외가 발생하면 현재 함수의 실행이 중지되고 (throw 이후의 명령문은 실행되지 않는다.), 제어 흐름은 콜스택의 첫 번째 [catch](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/try...catch) 블록으로 전달되며 호출자 함수 사이에 catch 블록이 없으면 프로그램이 종료된다.

```jsx
function getRectArea(width, height) {
  if (isNaN(width) || isNaN(height)) {
    throw new Error('Parameter is not a number!');
  }
}

try {
  getRectArea(3, 'A');
} catch (e) {
  console.error(e); // Expected output: Error: Parameter is not a number!
}

```

throw는 이런 방식으로도 사용할 수 있다.

```jsx
throw { message: "Invalid user input, not a number!" };`
```

새로운 에러를 발생시킬 때 사용자 정의 에러를 사용 할 수 있다.

```jsx
function React() {
  if (!new.target) {
    throw new ReferenceError("생성자입니다!!");
  }
}

React(); //생성자입니다!!
```