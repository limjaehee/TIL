# [Class] 믹스인

## 믹스인

다른 클래스를 상속받을 필요 없이 이들 클래스에 구현되어 있는 메서드를 답고 있는 클래스이다.

<br>

### 예시

메서드 여러 개가 담긴 객체를 하나 생성하여 클래스 프로토타입에 병합한다.

```jsx
// 믹스인
let sayHiMixin = {
  sayHi() {
    alert(`Hello ${this.name}`);
  },
  sayBye() {
    alert(`Bye ${this.name}`);
  }
};

// 사용법:
class User {
  constructor(name) {
    this.name = name;
  }
}

// 메서드 복사
Object.assign(User.prototype, sayHiMixin);

// 이제 User가 인사를 할 수 있습니다.
new User("Dude").sayHi(); // Hello Dude!
```

믹스인 안에서 믹스인 상속을 사용하는 것도 가능하다.

```jsx
let sayMixin = {
  say(phrase) {
    alert(phrase);
  }
};

let sayHiMixin = {
  __proto__: sayMixin, // (Object.create를 사용해 프로토타입을 설정할 수도 있습니다.)

  sayHi() {
    // 부모 메서드 호출
    super.say(`Hello ${this.name}`); // (*)
  },
  sayBye() {
    super.say(`Bye ${this.name}`); // (*)
  }
};

class User {
  constructor(name) {
    this.name = name;
  }
}

// 메서드 복사
Object.assign(User.prototype, sayHiMixin);

// 이제 User가 인사를 할 수 있습니다.
new User("Dude").sayHi(); // Hello Dude!
```