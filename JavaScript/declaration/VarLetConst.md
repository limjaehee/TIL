# Var & Let & Const
- var, let, const 모두 함수 스코프를 가지고 있다.
- var은 **전역 스코프**를 가지고 있다. (다른 의미로 블록 스코프가 없다)

    ```jsx
    let name = "Max";
    if (name === "Max") {  
        var hobbies = ["Sports", "Cooking"];
    }
    console.log(name, hobbies); //Max ['Sports', 'Cooking']
    
    ```

- let, const는 **블록 스코프**를 가지고 있다.

    ```jsx
    let name = "Max";
    if (name === "Max") {   
        let hobbies = ["Sports", "Cooking"];
    }
    console.log(name, hobbies); //hobbies is not defined
    ```