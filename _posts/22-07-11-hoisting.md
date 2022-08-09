---
layout: single
title: "호이스팅과 Temporal Dead Zone"
tag: [hoisting]
toc: true
---



# 호이스팅



``호이스팅`` 이란, 인터프리터가 변수와 함수의 메모리 공간을 선언 전에 미리 할당하는 것을 의미합니다.

쉽게 말해, 자바스크립트로 코드를 작성한 뒤 코드를 실행하면, 자바스크립트는 먼저 이 코드에 선언된 변수들이 무엇인지 조사를 하는데, 그것을 호이스팅이라고 합니다. Var, let, const 모두 호이스팅이 됩니다.

```var```로 선언한 변수의 경우 호이스팅 시 ``undefined`` 로 변수를 초기화합니다. 반면 ``let``과 ``const``로 선언한 변수의 경우 호이스팅시 변수를 초기화하지 않습니다.

```js
// 변수 선언하지 않고 변수를 출력하는 경우
console.log(a)

// ReferenceError: a is not defined ~~~
```

위의 경우는 변수를 선언하지 않았기 때문에 에러가 발생합니다.



```js
// var 변수로 선언했을 경우

console.log(a)
var a = 1
console.log(a)

// undefined
// 1
// a가 선언되기 전에도 a가 에러가 아닌 undefined 로 리턴된다. 
```

이러한 결과값이 발생하는 이유는 호이스팅이라는 개념때문이다.  

코드가 실행되기 전에 호이스팅이 발생하여 var와 같은 변수는 변수의 선언과 초기화를 같이 시켜버리기 때문입니다.

첫번째 a를 출력할 때는 a가 1로 할당되기 전이므로 undefined 라고 출력이 되며, 다음 a가 1로 할당되고 난 후, 다음 출력은 1로 출력이 됩니다.

```js
console.log(a)
a = 1
var a
console.log(a)

// undefined
// 1
```

위 예시는 말이 안 될 것 같지만 이러한 결과도 호이스팅이라는 개념 때문에 위와 같은 값이 출력됩니다.

```js
var a = 2

function foo() {
  var b = 1
}

console.log(b)

// ReferenceError: b is not defined ~~~
```

위 예시는 선언된 변수 b가 함수안에 선언된 지역변수이기 때문에 그 블록 밖에서는 b가 선언되지 않았기 때문에 에러가 뜹니다.

```js
for (var i = 1; i < 5; i++) {
  console.log(i)
}

console.log(i)

// 1
// 2
// 3
// 4

// 5
```

이러한 결과는 자바스크립트는 함수만 지역변수로 호스팅 하고, 나머지는 전역변수로 처리하기 때문에 이와같은 값이 리턴됩니다.



### var보다 let을 사용해야 하는 이유(feat.TDZ)

```js
var a = 1
console.log(a)
var a = 2
console.log(a)

// 1
// 2
```

위의 예시를 보면, 같은 변수명이 두번 선언 될 수 없습니다. 하지만 출력값이 리턴되기 때문에 이러한 점을 극복하고자 let 변수를 사용하게 되었습니다.

```js
let a = 1
console.log(a)
let a = 2
console.log(b)

// SyntaxError: Identifier 'a' has already declared

-----------------------------
for (var i = 1; i < 5; i++) {
  console.log(i)
}

console.log(i)

// 1
// 2
// 3
// 4

// ReferenceError: i is not defined ~~~

-----------------------------
console.log(a)
let a = 2
console.log(a)

// ReferenceError: Cannot access 'a' before initialization
```

let 은 호이스팅이 됩니다. 하지만, var과 다르다면, let은 Temporal Death Zone(TDZ)의 특성이 있습니다.

즉, 변수가 선언되기 전에는 변수를 허용하지 않는다. 일시적으로 죽은 존이다라는 뜻입니다.





















































