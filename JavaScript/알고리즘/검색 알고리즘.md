# 검색 알고리즘

### 선형 검색

- O(n)
- 정렬되지 않은 배열에 사용한다.
- 모든 배열을 확인한다.
- 선형 검색을 하는 메소드

    ```jsx
    indexOf()
    find()
    findIndex()
    includes()
    ```

<hr>

### 이진 검색

- O(log n) or O(1)
- 정렬되어 있는 배열에 사용한다.
- 배열을 두 부분으로 나누어 중간점을 선택하고 찾는 값이 중간점을 기준으로 좌측 절반과 우측 절반 중 어느 쪽에 있는지 확인한다.
- 방법

    ```jsx
    [1, 3, 4, 6, 8, 9, 11, 12, 15, 16, 17, 18, 19]
    
    /**
        *1 = Left
        *11 = Too Small
        *19 = Right
    */
    ```

  15의 숫자를 찾는다 가정할 때 중간 지점인 11과 비교해 숫자가 크다면 좌측 전부를 제거한다.

    ```jsx
    [12, 15, 16, 17, 18, 19]
    
    /**
        *12 = Left
        *17 = Too Big
        *19 = Right
    */ 
    ```

  그 다음 17을 중간점으로 정하고 15가 17보다 큰지 작은지 확인한다.

    ```jsx
    [12, 15, 16]
    
    /**
        *12 = Left
        *15 = Middle
        *16 = Right
    */ 
    ```

  중간 지점인 15와 비교한다. 숫자가 같으니 해당 값을 반환한다.

- 코드 예시

    ```jsx
    function binarySearch(arr, elem) {
        let start = 0;
        let end = arr.length - 1;
        let middle = Math.floor((start + end) / 2);
        while (arr[middle] !== elem && start <= end) {
            if (elem < arr[middle]) end = middle - 1;
            else start = middle + 1;
            middle = Math.floor((start + end) / 2);
        }
        return arr[middle] == elem ? middle : -1;
    }
    
    result(binarySearch([1, 2, 3, 4, 5], 2)); // 1
    result(binarySearch([1, 2, 3, 4, 5], 6)); // -1
    ```
<hr>

### 나이브 문자열 검색

- 코드 예시

    ```jsx
    function naiveSearch(long, short) {
        let count = 0;
        for (let i = 0; i < long.length; i++) {
            for (let j = 0; j < short.length; j++) {
                if (short[j] !== long[i + j]) {
                    break;
                }
                if (j === short.length - 1) {
                    count++;
                }
            }
        }
        return count;
    }
    
    result(naiveSearch("lorie loled lol", "lol")); //2
    ```