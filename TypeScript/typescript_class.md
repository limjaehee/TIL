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