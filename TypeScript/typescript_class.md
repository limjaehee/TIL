# TypeScript 클래스

### 생성자 함수 및 ‘this’ 키워드

예) 정의된 name이 describe함수를 통해 console.log에 찍히는 생성자 함수

```tsx
class Department {
  name: string;

  constructor(n: string) {
    this.name = n;
  }

  describe() {
    console.log("Department: " + this.name);
  }
}

const accounting = new Department("Accounting");

accounting.describe(); //Department: Accounting
```

아래처럼 accounting.describe 함수만을 가져와 accountingCopy에 부여하게 되면

this는 accountingCopy를 바라보게 되고 accountingCopy안에는 name이 없으므로 Undefined를 반환한다.

```tsx
const accountingCopy = { describe: accounting.describe};

accountingCopy.describe(); //Department: Undefined
```

Undefined가 나오는 오류를 방지하기 위해 다음과 같이 행할 수 있는데

우선 첫번째, accountingCopy 안에 name을 적어주어 에러를 피한다.

```tsx
const accountingCopy = { describe: accounting.describe, name: "Development" };

accountingCopy.describe();
```

두번째, describe함수 내에서 Department 인스턴스에서 호출된게 아니라면 에러를 띄우게 한다.

즉 **name 객체가 없다면 에러가 표기된다.**

```tsx
describe(this: Department) {
  console.log("Department: " + this.name);
}
```
<br>

### 클래스 내의 제어자 private

코드를 짜다보면 아래와 같이 직접적으로 값을 할당하는 경우가 있다.

이는 오류가 생길 수 있기 때문에 좋지 못한 방법에 속한다.

```tsx
class Department {
  name: string;

  constructor(n: string) {
    this.name = n;
  }

  describe() {
    console.log("Department: " + this.name);
  }
}

const accounting = new Department("Accounting");

accounting.name = "Development";

accounting.describe(); //Department: Development
```

위를 방지하기 위해 클래스 내부 선언 속성에 `private`를 추가한다. (기본값은 `public`이다.)

타입스크립트는 private를 런타임이 아닌 컴파일 과정에 인식하여 코드에 오류를 띄운다.

```tsx
class Department {
  //privat을 추가하면 생성된 객체 내부에서만 접근할 수 있는 속성이 됨
  private name: string;
  //public 속성은 외부에서 접근할 수 있다. (기본값)
  public employees: string[] = [];
}
```

또한 changeName이라는 매소드를 추가하여 선언을 바꾸는 함수를 만들 수 있다.

이런 방법을 추천하는 이유는

1. 함수를 통일시킨다
2. 유효성검사 등의 기능을 추가하기가 편해진다

```tsx
class Department {
  private name: string;

  changeName(newName: string) {
    this.name = newName;
  }
}

accounting.changeName("Development");

accounting.describe();//Department: Development
```
<br>

### 약식 초기화

필드를 작성하고 생성자 함수 내에서 초기화를 하게 되는데

필드가 많아질 경우 생성자 함수 내에서도 초기화를 해야하는 부분이 많아진다.

```tsx
class Department {
  private id: number;
  public name: string;

  constructor(id: number, n: string) {
    this.id = id;
    this.name = n;
  }
  
    describe(this: Department) {
    console.log(`Department: (${this.id}) : ${this.name}`);
  }
}

const accounting = new Department(10, "Accounting"); //Department: (10) : Accounting

accounting.describe();
```

이를 단순한 방법으로 줄일 수 있는데

필드 정의를 제거하고 생성자 함수 인수에 추가해준다.

```tsx
class Department {
  // private id: number;
  // public name: string;

  constructor(private id: number, public name: string) {}
}
```