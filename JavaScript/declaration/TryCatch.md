# Try & Catch
- try catch 문은 실행할 코드블럭을 표시하고 예외(exception)가 발생(throw)할 경우의 응답을 지정합니다.
- try는 오류 발생 여부를 제어할 수 없는 경우에 사용한다

    ```jsx
    try {
        nonExistentFunction();
    } catch (error) { //이름 지정 보통 error로 칭함
        console.error(error);
        // Expected output: ReferenceError: nonExistentFunction is not defined
        // (Note: the exact output may be browser-dependent)
    }
    
    ```

- finally는 try 선언이 완료된 이후에 실행된다. 이는 예외 발생 여부와 상관없이 실행된다.

    ```jsx
    try {
        nonExistentFunction();
    } catch (error) { // Typically named "error"
        console.error(error);   
        throw error;
    } finally {
        console.error('error');
    }
    
    ```