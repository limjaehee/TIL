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