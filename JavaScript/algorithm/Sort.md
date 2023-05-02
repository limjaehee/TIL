# 정렬 알고리즘

### 정렬 알고리즘 시각화 사이트

[Sorting Algorithms Animations](https://www.toptal.com/developers/sorting-algorithms)

<br>

### 버블 정렬

<div style="display: flex; gap: 10px;">
    <img src="../../assets/JavaScript/bubble_srot_time.gif" width="300">
    <img src="../../assets/JavaScript/bubble_sort_step.png" width="200">
</div>

- 시간 복잡도
    - 짧은 배열인 경우 **O(n)**
    - 긴 배열의 경우 **O(n2)**
- 인접한 두 원소를 검사하여 정렬하는 알고리즘이다.
- 이름은 정렬 과정에서 원소의 이동이 거품이 수면으로 올라오는 듯한 모습을 보이기 때문에 지어졌다.
- 선택 정렬과 개념이 유사하다
- 오름차순으로 정렬할 때 첫번째 반복에 배열의 최대값이 맨 뒤로 이동한다.
- 반복을 거듭함에 따라 정렬해야 하는 항목 수가 감소한다.

```jsx
function bubbleSort(arr) {
    let noSwaps = true;
    for (let i = arr.length; i > 0; i--) {
        noSwaps = true;
        for (let j = 0; j < i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
                noSwaps = false;
            }
        }
        if (noSwaps) break;
    }
    return arr;
}

bubbleSort([85, 1, -3, 8, 45, 10]) //[-3, 1, 8, 10, 45, 85]
```
<br>

### 선택 정렬

- 시간복잡도
    - 항상 **O(n²)**
- 버블 정렬과 비슷하지만 배열의 최소값이나 최대값을 **선택**해 데이터의 위치를 교환하는 차이점이 있다.

```jsx
function selectionSort(arr) {
    for (let i = 0; i < arr.length; i++) {
        let min = i;
        for (let j = i + 1; j < arr.length; j++) {
            if (arr[min] > arr[j]) min = j;
        }
        if (i !== min) {
            [arr[i], arr[min]] = [arr[min], arr[i]];
        }
    }
    return arr;
}
result(selectionSort([85, 1, -3, 8, 45, 10]));//[-3, 1, 8, 10, 45, 85]
```
<br>

### 삽입 정렬

- 시간복잡도
    - 거의 정렬된 경우 = **O(n)**
    - 정렬되지 않은 배열 = **O(n2)**
- 손안의 카드를 정렬하는 방법과 유사하다 (새로운 카드를 기존의 정렬된 카드 사이 올바른 위치에 삽입하는 것)
- 매 순서마다 해당 원소를 삽입할 수 있는 위치를 찾아 해당 위치에 삽입한다.
- 거의 정렬되어 있는 배열에 사용할 수록 빠르다.
- 실시간으로 데이터가 계속 들어오는 경우 유리하다.
- 예시 코드

    ```jsx
    function insertionSort(arr) {
        for (let i = 1; i < arr.length; i++) {
            let currentVal = arr[i];
            for (var j = i - 1; j >= 0 && arr[j] > currentVal; j--) {
                arr[j + 1] = arr[j];
            }
            arr[j + 1] = currentVal;
        }
        return arr;
    }
    
    result(insertionSort([5, 6, 2])); //[2, 5, 6]
    ```

    - 배열 맨 끝 숫자 2(currentVal) 기준 6과 비교하여 6이 크므로 배열을 수정한다 `[5, 6, 6]`
    - 5와 2를 비교하여 5가 크므로 배열을 수정한다 `[5, 5, 6]`
    - j의 인덱스에 currentVal을 넣어준다. `[2, 5, 6]`

<br>

### 버블, 선택, 삽입 정렬 비교

| 알고리즘 | 시간 복잡도<br>(최상) | 시간 복잡도<br>(평균) | 시간 복잡도<br>(최하) | 공간 복잡도 |
| --- |----------------|----------------|----------------| --- |
| 버블 정렬 | O(n)           | O(n²)          | O(n²)          | O(1) |
| 선택 정렬 | O(n²)          | O(n²)          | O(n²)          | O(1) |
| 삽입 정렬 | O(n)           | O(n²)          | O(n²)          | O(1) |


