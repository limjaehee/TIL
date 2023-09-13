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
<br>

### 읽기전용 속성 readonly

readonly는 특정 속성이 초기화되고 나면 이후에는 변경되어선 안된다는 특징이 있다

```tsx
class Department {
  // private readonly id: number;

  constructor(private readonly id: number, public name: string) {}
}
```
<br>

### 클래스 확장과 protected

private는 클래스 외부와 확장된 클래스 내에서도 접근할 수 없는 반면

protected는 클래스 외부에서는 접근 할 수 없지만 확장된 클래스 내에서는 접근이 가능하다.

```tsx
class Department {
  protected employees: string[] = [];

  addEmployee(employee: string) {
    this.employees.push(employee);
  }

  printEmployeeInformation() {
    console.log(this.employees);
  }
}

class AccountingDepartment extends Department {
  constructor(id: number) {
    super(id, "Accounting");
  }

  addEmployee(name: string): void {
    if (name === "Max") return;

    this.employees.push(name);
  }
}

const accounting = new AccountingDepartment(6, []);

accounting.addEmployee("Tom");
accounting.printEmployeeInformation();
```
<br>

### Get & Set

게터와 세터는 속성을 읽거나 설정하려 할 때 유용한 방법이다.

```tsx
class AccountingDepartment extends Department {
  private lastReport: string;

  get mostRecentReport() {
    if (!this.lastReport) throw new Error("no content");
    return this.lastReport;
  }

  set mostRecentReport(value: string) {
    if (!value) throw new Error("not valid value!");
    this.addReport(value);
  }

  constructor(id: number, private reports: string[]) {
    super(id, "Accounting");
    this.lastReport = reports[0];
  }

  addReport(text: string) {
    this.reports.push(text);
    this.lastReport = text;
  }
}

const accounting = new AccountingDepartment(6, []);

accounting.mostRecentReport = "Year End report";
accounting.addReport("Rosa");

console.log(accounting.mostRecentReport); //Rosa

accounting.printReports(); //['Year End report', 'Rosa']
```
<br>

### 정적 메서드 & 속성

정적 메서드는 새 키워드 없이 직접 클래스에서 호출하는 것이다.

```tsx
Math.pow(7, -2);
Math.random();
```

정적 메서드는 메소드 앞에 `static`을 붙여 사용한다.

예) 클래스 내에서의 정적 메서드

```tsx
class Department {
  static createEmployee(name: string) {
    return { name: name };
  }
}

const employee1 = Department.createEmployee("Max");
console.log(employee1); //{name: 'Max'}
```

예) 클래스 내에서의 정적 속성

```tsx
class Department {
  static fiscalYear = 2023;
}

console.log(Department.fiscalYear); //2023
```

여기서 중요한 점은 클래스 내부에서 this로 정적 속성과 메소드에 접근할 수 없다는 것이다.

클래스 내에서 접근하기 위해서는 Department.fiscalYear 처럼 클래스 이름을 적어주어야 한다.

```tsx
class Department {
  static fiscalYear = 2023;

  constructor() {
    console.log(this.fiscalYear); //X
    console.log(Department.fiscalYear); //O
  }

	static printYear() {
    console.log(this.fiscalYear); //O
    console.log(Department2.fiscalYear); //O
  }
}
```
<br>

### 추상 클래스

추상 클래스는 일종의 껍데기로서 기능을 제외한 부분을 구현하는 것이며

class 앞에 `abstract`를 표기하며 추상 메서드에도 abstract를 이름 앞에 표기한다.

추상 클래스는 일반 클래스와 달리 인스턴스를 생성하지 않으며 생성하려는 경우 오류 메시지를 띄운다.

```tsx
abstract class Department {
  constructor(protected readonly id: number, public name: string) {}

  abstract describe(this: Department): void;
}

// error
let Department = new Department();
```

추상 메소드는 정의만 있을 뿐 몸체가 구현되어 있지는 않다.

따라서 abstract 키워드가 붙으면 상속받은 클래스에서 메소드를 필수로 구현해야 한다.

```tsx
class AccountingDepartment extends Department {
  constructor(id: number) {
    super(id, "Accounting");
  }

  describe() {
    console.log("accounting id:" + this.id);
  }
}

const accounting = new AccountingDepartment(6);
accounting.describe(); //accounting id:6
```
<br>

### 싱글턴

싱글턴 패턴은 인스턴스가 오직 하나만 생성되어야 하는 케이스에 사용하는 패턴이다.

`private` 접근 제어자를 통해 constructor() 앞에 붙이면 new 키워드를 통해 인스턴스를 생성하지 못하도록 제한할 수 있다.

```tsx
class SingleInstance {
  private static instance: SingleInstance;

  private constructor(protected readonly id: number, public name: string) {}

  static getInstance() {
    if (!SingleInstance.instance) {
      SingleInstance.instance = new SingleInstance(1, "single");
    }

    return SingleInstance.instance;
  }
}

const singleInstance = SingleInstance.getInstance();

console.log(singleInstance); //SingleInstance {id: 1, name: 'single'}
```