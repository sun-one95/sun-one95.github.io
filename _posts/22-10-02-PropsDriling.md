---
layout: single
title: "PropsDriling 이란"
tag: [PropsDriling]
toc: true
---



## PropsDriling 이란?



Props Driling은 **props를 오로지 하위 컴포넌트로 전달하는 용도로만 쓰이는 컴포넌트들을 거치면서 React Component 트리의 한 부분에서 다른 부분으로 데이터를 전달하는 과정이다.**

여기서, props는 properties의 줄임말인데, 우리가 어떠한 값을 컴포넌트에게 전달해줘야 할 때, props를 사용한다.

```js
App.js

import React from 'react';

function App() {
  return (
    <Hello name="react" />
  )
}

function FirstComponent({name}) {
  return (
		<>
    	<h1>first component</h1>
    	<SecondComponent name={name}/>
    </>  
  )
}

function SecondComponent({name}) {
  return (
  	<>
    	<h1>second component</h1>
  		<Hello naem={name}/>
    </>
  )
}

function Hello({name}) {
  return <h1>{name}</h1>
}

export default App;

```



위의 예시처럼, Hello 컴포넌트에서 해당 name props를 사용하기 위해 name을 전달하는 과정으로 ``App -> FirstComponent -> SecondComponent -> Hello``순으로 props driling 이 된다.



### 문제점 ###

Props 전달이 컴포넌트 하나하나 전달되는 구조이기 때문에, 컴포넌트가 점점 많아질 수록 props를 추적하기 어려워 진다.



### 해결 방안

1. redux와 같은 전역 상태관리 라이브러리를 사용하면, props를 각 컴포넌트에 전달하지 않고, 쉽게 한번에 전달하여 관리할 수 있다.
2. children 사용

​	

















