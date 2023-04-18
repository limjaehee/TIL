# JavaScript 엔진

### 자바스크립트 엔진이란?

자바스크립트 코드를 실행하는 프로그램 혹은 인터프리터

### 자바스크립트 엔진 코드 해석 순서

![Untitled](../../assets/JavaScript/JavaScript_engine_order.png)

1. 브라우저가 HTML 파일을 읽어 스크립트를 감지하여 실행한다.
2. 인터프리터가 스크립트를 로드하고 읽어들여서 가상 머신이 이해하기 쉽게 만든 바이트 코드로 변환한다.
  - 인터프리터는 한줄 한줄 해석해서 바이트 코드를 만든다.
  - 때문에 가장 빠른 방식은 아니다.
3. 인터프리터에서 전달받은 바이트 코드를 머신코드(최적화 코드)로 컴파일링 하여 컴퓨터로 전달한다.
  - 인터프리터에서 스크립트를 실행할 때 함께 작업이 이루어진다.
  - 스크립트를 실행할 수 있는 가장 빠른 방법이다.

### 자바스크립트 엔진 특징

- 브라우저별로 스크립트를 실행하는 엔진이 다르다.
  - chrome은 v8

    [JavaScript V8 Engine Explained | HackerNoon](https://hackernoon.com/javascript-v8-engine-explained-3f940148d4ef)

  - Firefox는 Spider Monkey

    [SpiderMonkey — Firefox Source Docs  documentation](https://firefox-source-docs.mozilla.org/js/index.html)

- 자바스크립트 엔진은 실행 및 컴파일 시간을 단축할 수 있는 최적화 기술을 적용한다.
  - 이전 실행과 현재 실행 시 달라진 부부닝 없는 코드의 경우 재컴파일링을 거치지 않고 컴파일 된 코드를 다시 사용한다.
  - 이때 재컴파일될 필요가 없어서 코드를 그대로 사용하므로 빠른 방법으로 실행할 수 있다.
  - 하지만 코드 변경으로 인터프리터를 거치게 되면 컴파일러 절차도 필요하다.
- 브라우저 API가 빌트인 되어있다.