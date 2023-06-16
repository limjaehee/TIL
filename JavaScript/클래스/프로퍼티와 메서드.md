# [Class] 프로퍼티와 메서드

## 정적 메서드와 정적 프로퍼티

### ✅ 정적 메서드

prototype이 아닌 클래스 함수 자체에 메서드를 설정할 수 있다. 이런 메서드를 정적(static) 메서드라고 부른다.

정적 메서드는 static 키워드를 붙여 만들 수 있다.

```jsx
class User {
  static staticMethod() {
    alert(this === User);
  }
}

User.staticMethod(); // true
// = User.prototype.constructor.staticMethod() 
// static 메서드는 User자체의 프로퍼티가 된다.
```

정적 메서드는 프로퍼티 형대로 직접 할당하는 것과 동일하다.

```jsx
class User { }

User.staticMethod = function() {
  alert(this === User);
};

User.staticMethod(); // true
```

User.staticMethod()가 호출될 때 this의 값은 크래스 생성자인 User 자체가 된다. (점 앞의 객체)

static 키워드를 사용하여 메서드를 정의하면 new ‘클래스명’으로 인스턴스 객체를 생성하지 않아도 해당 메소드를 사용할 수 있다는 장점이 있다.

<br>

### ✅ 정적 프로퍼티

정적 프로퍼티는 앞에 static 키워드를 붙인다.

```jsx
class Article {
  static publisher = "Ilya Kantor";
}

alert( Article.publisher ); // Ilya Kantor
```

위는 Article에 프로퍼티를 직접 할당한 것과 동일하게 작동한다.

```jsx
Article.writer = "Tom lian";
```
<br>

### ✅ 상속

정적 프로퍼티와 메서드는 상속된다.

```jsx
class Animal {
  static planet = "지구";
  constructor(name, speed) {
    this.speed = speed;
    this.name = name;
  }
  static compare(animalA, animalB) {
    return animalA.speed - animalB.speed;
  }
}

class Rabbit extends Animal {
}

let rabbits = [
  new Rabbit("흰 토끼", 10),
  new Rabbit("검은 토끼", 5)
];

rabbits.sort(Rabbit.compare);

alert(Rabbit.planet); // 지구
```

<br>

## private, protected 프로퍼티와 메서드

프로그래밍에서 객체는 커피머신과 같다. 우리가 커피머신의 내부 요소들을 몰라도 사용할 수 있는 이유는 외형이 단순하기 때문이다.

### ✅ 내부 인터페이스와 외부 인터페이스

객체 지향 프로그래밍에서 프로퍼티와 메서드는 두 그룹으로 분류된다.

- 내부 인터페이스 -  동일한 클래스 내의 다른 메서드에선 접근할 수 있지만, 클래스 밖에선 접근할 수 없는 프로퍼티와 메서드
- 외부 인터페이스 - 클래스 밖에서도 접근 가능한 프로퍼티와 메서드

커피머신으로 비유하면 기계 안쪽에 숨어있는 뜨거운 물이 지나가는 관이나 발열 장치 등이다.

하지만 커피머신의 보호 커버를 벗기지 않고는 커피머신 외부에서 내부로 접근할 수 없다. 밖에서는 세부 요소를 알 수 없고, 접근도 불가능하다. 내부 인터페이스의 기능은 외부 인터페이스를 통해야만 사용할 수 있다.

자바스크립트에서는 두 가지 타입의 객체 필드(프로퍼티와 메서드)가 있다.

- public: 어디서든지 접근할 수 있으며 외부 인터페이스를 구성한다.
- private: 클래스 내부에서만 접근할 수 있으며 내부 인터페이스를 구성할 때 쓰인다.

다수 언어에서 자신과 자손 클래스만 접근을 허용하는 ‘protected’ 필드를 지원한다. protected 필드는 private와 비슷하지만, 자손 클래스에서도 접근이 가능하다는 점이 다르다.

<br>

### ✅ 프로퍼티 보호하기

protected 프로퍼티 명 앞엔 밑줄 _이 붙는다.

(밑줄은 프로그래머들 사이에서 외부 접근이 불가능한 프로퍼티나 메서드를 나타낼 때 쓰인다.)

```jsx
class CoffeeMachine {
  _waterAmount = 0;
  set waterAmount(value) {
    if (value < 0) throw new Error("물의 양은 음수가 될 수 없습니다.");
    this._waterAmount = value;
  }
  get waterAmount() {
    return this._waterAmount;
  }
  constructor(power) {
    this._power = power;
  }
}

// 커피 머신 생성
let coffeeMachine = new CoffeeMachine(100);

// 물 추가
coffeeMachine.waterAmount = -10; // Error: 물의 양은 음수가 될 수 없습니다.
```

<br>

### ✅ 읽기 전용 프로퍼티

프로퍼티를 생성할 때만 값을 할당할 수 있고, 그 이후에는 값을 절대 수정하지 말아야 하는 경우가 있는데, 이럴 때 읽거 전용 프로퍼티를 활용할 수 있다.

읽기 전용 프로퍼티를 만들려면 setter(설정자)는 만들지 않고 getter(획득자)만 만들어야 한다.

```jsx
class CoffeeMachine {
  constructor(power) {
    this._power = power;
  }
  get power() {
    return this._power;
  }
}

// 커피 머신 생성
let coffeeMachine = new CoffeeMachine(100);
coffeeMachine.power = 25; // Error (setter 없음)
```

<br>

### ✅ private 프로퍼티

private 프로퍼티와 메서드는 # 으로 시작하며 #이 붙으면 클래스 안에서만 접근할 수 있다.

```jsx
class CoffeeMachine {
  #waterLimit = 200;

  #checkWater(value) {
    if (value < 0) throw new Error("물의 양은 음수가 될 수 없습니다.");
    if (value > this.#waterLimit) throw new Error("물이 용량을 초과합니다.");
  }
}

let coffeeMachine = new CoffeeMachine();

// 클래스 외부에서 private에 접근할 수 없음
coffeeMachine.#checkWater(); // Error
coffeeMachine.#waterLimit = 1000; // Error
```

#은 자바스크립트에서 지원하는 문법으로, private 필드를 의미한다. private 필드는 외부나 자손 클래스에서 접근할 수 없다.

private 필드는 public 필드와 상충하지 않기 때문에 private 프로퍼티 #waterAmount와 public 프로퍼티 waterAmount를 동시에 가질 수 있다.

```jsx
class CoffeeMachine {
  #waterAmount = 0

  get waterAmount() {
    return this.#waterAmount;
  }
  set waterAmount(value) {
    if (value < 0) throw new Error("물의 양은 음수가 될 수 없습니다.");
    this.#waterAmount = value;
  }
}

let machine = new CoffeeMachine();

machine.waterAmount = 100;
alert(machine.#waterAmount); // Error
```

CoffeeMachine을 상속받는 클래스에서는 #waterAmount에 직접 접근할 수 없다. #waterAmount에 접근하려면 waterAmount의 getter와 setter을 통해야 한다.

```jsx
class MegaCoffeeMachine extends CoffeeMachine {
  method() {
    alert( this.#waterAmount ); // Error: CoffeeMachine을 통해서만 접근할 수 있습니다.
  }
}
```