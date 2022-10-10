---
layout: single
title: "useState"
tag: [react]
toc: true
---



# useState

> 동적인 값이 바뀔 떄 관리하는 Hook이다.



간단한 예시로 숫자가 증가, 감소하는 로직으로 설명해보겠다.

#### Counter.js

```js
import React from 'react';

function Counter() {
  return (
    <div>
      <h1>0</h1>
      <button>+1</button>
      <button>-1</button>
    </div>
  );
}

export default Counter;
```



그 다음, App.js에서 이 Counter.js 컴포넌트를 렌더링 하면



<img src="../images/22-10-10-useState/스크린샷 2022-10-10 오전 9.35.30.png" alt="스크린샷 2022-10-10 오전 9.35.30" style="zoom:67%;" />

이제, 버튼을 클릭한 이벤트가 일어나도록 함수를 작성해 준다.



#### Counter.js

```js
import React, { useState } from 'react';

function Counter() {
  const [number, setNumber] = useState(0)
  const onIncrease = () => {
    setNumber(number + 1)
  }
  
  const onDecrease = () => {
    setNumber(number - 1)
  }
  
  return (
    <div>
      <h1>{number}</h1>
      <button onClick={onIncrease}>+1</button>
      <button onClick={onDecrease}>-1</button>
    </div>
  );
}

export default Counter;
```



동적인 값이 ```state``` 인데, 이 값을 관리하는 함수는 ```useState``` 함수이다.

```js
const [number, setNumber] = useState(0)
```

위의 예시처럼 ```useState``` 를 사용할 때에는 상태의 기본값을 파라미터로 넣어서 호출한다. Ex) ```useState(0)```

이 함수를 호출하면 배열이 반환되는데, 첫번째 원소는 현재 상태, 두번째 원소는 갱신함수(Setter) 이다.

```js
const arr = useState(0)
const number = arr[0]
const setNumber = arr[1]
```

원래는 이렇게 표현하지만, 배열 비구조 할당을 통해 각 원소를 추출해준것이다.



## State를 직접 변경하지 않고 setState를 사용하는 이유



- ```state```를 직접 수정하게 되면 리액트가 render 함수를 호출하지 않아 상태 변경이 일어나도 렌더링이 일어나지 않을 수 있다.
- ```setState``` 는 비동기적으로 동작하기 때문에 ```state``` 가 직접 수정되어 여러번 상태를 업데이트 하는 경우, 이전 업데이트 내용이 다음 업데이트  내용에 덮어 쓰여질 수가 있어 예상치 못한 곳에서 버그가 발생 할 수 있다.
- ```state```는 불변성(immutable)을 유지해야 한다.

s













 











