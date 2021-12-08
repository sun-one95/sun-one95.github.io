---
layout: single
title:  "useEffect"
tag: [react]
toc: true
---



# useEffect

> 마운트(처음 나타났을 때),언마운트(사라질 때),업데이트시(특정 props가 바뀔때) 쓰는 hook



```js
import React, { useEffect } from 'react';

function User({ user, onRemove, onToggle }) {
  useEffect(() => {
    console.log('컴포넌트가 화면에 나타남');
    return () => {
      console.log('컴포넌트가 화면에서 사라짐');
    };
  }, []);
  return (
    ...생략
  );
}

```

- useEffect를 사용할 때, 첫번째 파라미터는 함수, 두번째 파라미터는 의존값이 들어있는 배열 (deps)을 넣는다.
- useEffect에서는 함수를 반환 할 수 있는데 이를 ```cleanup``` 함수라고 한다. ```cleanup``` 함수는 ```useEffect``` 에 대한 뒷정리를 해준다고 생각하면 된다.
- ```deps``` 가 비어있는 경우, 컴포넌트가 사라질 때 ```cleanup``` 함수가 호출된다.
- 마운트 시에 하는 작업
  - ```props``` 로 받은 값을 컴포넌트의 로컬 상태로 설정
  - 외부 API 요청 (REST API 등)
  - 라이브러리 사용(D3, Video.js 등...)
  - setInterval 을 통한 반복잡업 혹은 setTimeout 을 통한 작업 예약
- 언마운트 시에 하는 작업
  - setInterval, setTimeout을 사용하여 등록한 작업들 clear 하기(clearInterval, clearTimeout)
  - 라이브러리 인스턴스 제거



## deps 에 특정 값 넣기

```js
import React, { useEffect } from 'react';

function User({ user, onRemove, onToggle }) {
  useEffect(() => {
    console.log('user 값이 설정됨');
    console.log(user);
    return () => {
      console.log('user 가 바뀌기 전..');
      console.log(user);
    };
  }, [user]);
  return (
  	...생략
  )
```

- 컴포넌트가 처음 마운트 될 때에도 호출이 되고, 지정한 값이 바뀔 때에도 호출이 된다.

- 언마운트시에도 호출이 되고, 값이 바뀌기 직전에도 호출이 된다.

- useEffect 안에서 사용하는 상태나, props 가 있다면, useEffect 의 deps에 넣어주어야 한다. 규칙이다.

- 넣지 않는다면 useEffect 에 등록한 함수가 실행 될 때 최신 props / 상태를 가리키지 않게 된다.

- deps 파라미터 생략시 컴포넌트가 리렌더링 될 때마다 호출이 된다.

  

<img src="../images/21-12-02-useEffect/스크린샷 2021-12-08 오후 3.01.40.png" alt="스크린샷 2021-12-08 오후 3.01.40" style="zoom:50%;" />





