# REST API

## DOM Event

event 인터페이스는 DOM에서 발생하는 이벤트를 나타낸다.

### ✅ target

event 인터페이스의 target 속성은 이벤트가 발생한 대상 객체를 가리킨다.

```jsx
const buttonClickHandler = event => {
    event.target.disabled = true;
};

buttons.forEach(btn => {
    btn.addEventListener('click', buttonClickHandler);
});
```

### ✅ preventDefault()

브라우저의 기본 동작을 차단하고 사용자의 로직을 구현할 수 있게 해준다.

```jsx
form.addEventListener('submit', event => {
    event.preventDefault(); //새로 고침 기본 동작 중단
});
```

### ✅ **stopPropagation()**

Bubbling & Capturing 단계에서 조상 요소에 있는 같은 유형의 이벤트에 대한 다른 리스너는 트리거 하지 않는다.

하지만 preventDefault와는 다르기 때문에 기본 동작이 발생하는 것을 막지는 않는다.

```html
<div>
    <button>click</button>
</div>
```

```jsx
div.addEventListener('click', event => {
    console.log('CLICKED DIV');
});

button.addEventListener('click', event => {
    event.stopPropagation(); //버블링을 막아줌 (div 이벤트가 실행 되지 않음)
    console.log('CLICKED BUTTON');
});
```

<br>

## UI Events

UI 이벤트는 마우스 및 키보드 입력과 같은 사용자 상호 작용을 처리하기 위한 시스템을 의미한다.

### ✅ mouse event

| event | explanation                                              |
| --- |----------------------------------------------------------|
| click | 해당 element를 클릭했을 때(버튼을 눌렀다가 떼었을 때) 발생 합니다.               |
| mousedown | 해당 element에서 마우스 버튼을 눌렀을 때 발생합니다.                        |
| mouseup | 해당 element에서 눌렀던 마우스 버튼을 떼었을 때 발생합니다.                    |
| dblclick | 해당 element에서 마우스 버튼을 더블 클릭했을 때 발생합니다.                    |
| mousemove | 해당 element에서 마우스를 움직였을 때 발생합니다.                          |
| mouseover | 마우스를 해당 element 바깥에서 안으로 옮겼을 때 발생합니다.                    |
| mouseout | 마우스를 해당 element 안에서 바깥으로 옮겼을 때 발생합니다.                    |
| mouseenter | 마우스를 해당 element 바깥에서 안으로 옮겼을 때 발생합니다.<br>버블링이 발생하지 않습니다. |
| mouseleave | 마우스를 해당 element 안에서 바깥으로 옮겼을 때 발생합니다.<br>버블링이 발생하지 않습니다. |
| contextmenu | 마우스 오른쪽 버튼을 눌렀을 때 발생합니다.                                 |

**mouse Instance properties**

| properties | explanation                                                                                     |
| --- |-------------------------------------------------------------------------------------------------|
| relatedTarget | mouseover,  mouseout 이벤트 대상으로만 속성을 사용할 수 있다.<br>마우스가 이벤트 트리거에 들어가기 전에 커서가 어떤 요소 위에 있었는지 알 수 있다. |

<br>

## HTML Element

### ✅ drag event

| event | explanation |
| --- | --- |
| dragstart | 사용자가 객체(object)를 드래그하려고 시작할 때 발생함. |
| dragenter | 마우스가 대상 객체의 위로 처음 진입할 때 발생함. |
| dragover | 드래그하면서 마우스가 대상 객체의 위에 자리 잡고 있을 때 발생함. |
| drag | 대상 객체를 드래그하면서 마우스를 움직일 때 발생함. |
| drop | 드래그가 끝나서 드래그하던 객체를 놓는 장소에 위치한 객체에서 발생함. |
| dragleave | 드래그가 끝나서 마우스가 대상 객체의 위에서 벗어날 때 발생함. |
| dragend | 대상 객체를 드래그하다가 마우스 버튼을 놓는 순간 발생함. |