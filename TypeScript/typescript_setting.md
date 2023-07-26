# TypeScript 환경 설정

TypeScript는 JavaScript로 컴파일되어 웹 브라우저나 NodeJS를 통해 실행된다.

### 설치

node, npm이 설치되어있는지 확인한다.

```powershell
//cmd
$ node -v
$ npm -v
```

타입스크립트를 설치한다.

```powershell
//cmd
//Window
$ npm install –g typescript //현재 컴퓨터에 설치
$ npm install typescript //로컬로 설치(현재 위치의 폴더에 설치)

//Mac os
$ sudo npm install -g typescript //현재 컴퓨터에 설치
$ sudo npm install typescript //로컬로 설치(현재 위치의 폴더에 설치)

//버전확인
$ tsc -v
```
<br>

### 프로젝트 설정

1. 프로젝트 폴더를 아래 app.ts 파일, index.html 파일을 생성한다.

   index.html에서 app.js를 import한다.

   여기서 defer을 사용하면 페이지 로딩이 끝난 후 js 스크립트를 실행한다.

    ```html
    <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8" />
            <meta name="viewport" content="width=device-width, initial-scale=1.0" />
            <title>Document</title>
            <script src="app.js" defer></script>
        </head>
        <body>
            <button>Test</button>
        </body>
    </html>
    ```


2. 아래를 입력하여 tsconfig.json 프로젝트 구성 파일을 생성한다.

   이 파일이 생성되면 현재 폴더가 TypeScript로 인식된다.

    ```powershell
    $ tsc --init
    ```

3. tsc 명령어를 실행하여 TypeScript 프로젝트를 컴파일한다.

   이때 app.ts가 컴파일되어 app.js가 생성된다.

    ```powershell
    $ tsc
    ```


4. npm init을 사용하여 package.json을 생성한다. (패키지 질문에는 enter를 쳐도 무방하다)

    ```powershell
    $ npm init
    ```

5. Lightweight web server (node server)인 lite-server를 설치한다.

   lite-server는 간단한 웹서버로서 HTML이나 JavaScript가 변경되면 자동으로 웹페이지를 Refresh 하는 기능도 제공한다.

    ```powershell
    $ npm i lite-server -d
    ```

6. 다음을 추가한 뒤 npm start 명령어를 입력해 lite-server를 실행한다.

    ```json
    //package.json
    {
        "scripts": {
            "start": "lite-server"
        }
    }
    ```

    ```powershell
    $ npm start
    ```

7. 자동으로 컴파일을 하기 위해 터미널에 다음을 입력한다.

    ```powershell
    $ tsc --watch
    ```

<br>

## tsconfig.json 설정

### ✅ 컴파일 제외 & 포함

컴파일 제외를 하고 싶으면 tsconfig.json에서 다음을 입력해준다.

node_modules는 원래 자동 제외되는데 exclude를 사용할 경우 따로 같이 적어줘야 한다.

```json
{
   "exclude": [
      "node_modules", //exclude를 사용할 경우 꼭 필요
      "analytics.ts", //제외할 파일명
      "*.dev.ts" //파일명 뒤에 dev가 들어간 모든 파일
   ]
}
```

컴파일 과정에 포함 시킬 파일도 정할 수 있다.

단 여기에 적힌 파일만 컴파일 시키므로 모든 컴파일하고자 하는 항목을 포함 시켜야 한다.

```json
{
   "include": ["app.ts", "analytics.ts"]
}
```

### ✅ 컴파일 폴더 설정

컴파일 된 js파일을 dist 폴더에 모아 놓을 수 있다.

```json
{
   "compilerOptions": {
      "outDir": "./dist"
   }
}
```

rootDir에 폴더 명을 입력해주면 다른 폴더의 ts스크립트가 dist폴더에 컴파일 되어 들어가는 것을 방지할 수 있다.

```json
{
   "compilerOptions": {
      "rootDir": "./src"
   }
}
```

### ✅ 에러 관련 설정

에러가 생겼을 시 JS에 컴파일 되지 않음

```json
{
    "compilerOptions": {
      "noEmitOnError": true
    }
}
```

사용하지 않는 매개변수에 에러를 표시

```json
{
   "compilerOptions": {
     "noUnusedParameters": true
   }
}
```

함수 내 사용하지 않는 변수에 에러를 표시

```json
{
   "compilerOptions": {
      "noUnusedLocals": true
   }
}
```

함수 내 반환하는 경우가 하나라도 있다면 모든 경우에 반환하도록 해야 함

```json
{
   "compilerOptions": {
      "noImplicitReturns": true
   }
}
```
