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

























