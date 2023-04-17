# Throw
- throw문은 사용자 정의 예외를 발생(throw)할 수 있습니다. 예외가 발생하면 현재 함수의 실행이 중지되고 (throw 이후의 명령문은 실행되지 않습니다.), 제어 흐름은 콜스택의 첫 번째 [catch](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/try...catch) 블록으로 전달됩니다. 호출자 함수 사이에 catch 블록이 없으면 프로그램이 종료됩니다.

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

- throw는 이런 방식으로도 사용할 수 있다.

    ```jsx
    throw { message: "Invalid user input, not a number!" };`
    ```