# 조건문

## ✅ if…else

조건문을 사용할 때에는 부정 조건문을 최소화하는 것이 좋다. 이우는

1. 생각을 여러번 해야할 수 있다.
2. 프로그래밍 언어 자체로 if문이 처음부터 오고 true부터 실행시킨다.

```jsx
//[X] 부정 조건문을 사용해서 한번 더 생각해야 한다.
if (!isNaN(1)) {
  return true;
}

//[O] 함수를 하나 생성해서 긍정 조건문을 추상화한다.
function isNumber(num) {
  return !Number.isNaN(num) && typeof num === "number";
}

if (isNumber(1)) {
  return true;
}

```

예외로, 부정 조건문을 사용해야 할 때는 다음과 같다.

1. Early Return
2. Form Vaildation
3. 보안 혹은 검사하는 로직인 경우

```jsx
function loginService(user) {
  if (!user) return;

  if (!user.token) return;

  login();
}
```