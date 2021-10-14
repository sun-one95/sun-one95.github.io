---
layout: single
title:  "비동기 함수"
tag: [JS, Asynchronous]
toc: true
---

# 비동기 함수

> 들어가기에 앞서 비동기 함수에 근간인 콜백함수에 대해 정리한다.

**콜백함수**

- 다른 함수(A)의 전달인자(argument)로 넘겨주는 함수(B)
- 파라미터를 넘겨받는 함수(A)는 콜백함수(B)를 필요에 따라 즉시 실행(synchronously)할수도 있고, 아니면 나중에(asynchronously) 실행할 수도 있다.

```js
function B() {
  console.log('called at the back!')
}
function A(callback) {
  callback() // callback === B
}
A(B);
```

- 콜백함수는 함수 실행을 연결하는 것이 아니라 함수 자체를 연결하는 것이다.



## blocking vs non-blocking

1. blocking

   - 예시) 전화가 오면 하던 일을 멈추고 처리해야 한다.
   - 요청에 대한 결과가 동시에 일어난다.(synchronous)

2. non-blocking

   - 예시) 문자가 오면 확인 후, 나중에 답장할 수 있다.

   - 요청에 대한 결과가 동시에 일어나지 않는다.

## 동기 vs 비동기

> 커피 주문으로 알아보는 동기와 비동기

만일 커피가 **동기적**으로 작동한다면?

1. 손님 1이 아메리카노를 주문한다.
2. 주문을 받은 직원이 아메리카노를 내린다.
3. 직원이 손님 1에게 아메리카노를 전달한다.
4. 손님 2가 카페라떼를 주문한다.
5. 주문을 받은 직원이 카페라떼를 내린다.
6. 직원이 손님 2에게 카페라떼를 전달한다.

=> 손님 2는 손님 1이 아메리카노를 전달받을 때까지 주문도 하지 못하고 기다려야 한다.

**비동기적인 커피주문**

1. 손님 1이 아메리카노를 주문한다.(요청에 blocking이 없다.)
   1. 주문을 받은 직원이 아메리카노를 내린다.
   2. 아메리카노가 완성되면 직원이 손님 1을 부른다.
   3. 직원이 손님 1에게 아메리카노를 전달한다.
2. 손님 2가 카페라떼를 주문한다.(요청에 blocking이 없다.)
   1. 주문을 받은 직원이 카페라떼를 내린다.
   2. 카페라떼가 완성되면 직원이 손님 2을 부른다.
   3. 직원이 손님 2에게 카페라떼를 전달한다.

=> 응답이 비동기적으로 이루어 진다. 여기서 콜백은 ''음료가 완성되면''이다.

``` js
// 동기적 함수
function waitSync(ms) {
  let start = Date.now();
  let now = start;
  while (now - start < ms) {
    now = Date.now();
  } 
} // 현재 시각과 시작 시간을 비교하며 ms 범위 내에서 무한 루프를 도는 blocking 함수입니다.

function drink(person, coffee) {
  console.log(person + '는' + coffee + '를 마십니다');
}
function orderCoffeeSync(coffee) {
  console.log(coffee + '가 접수되었습니다');
  waitSync(2000);
  return coffee;
}
let customers = [{
  name: 'Steve',
  request: '카페라떼'
},
{
	name: 'John',
  request: '아메리카노'
}]
// call synchronously
customers.forEach(function(customer) {
  let coffee = orderCoffeeSync(customer.request);
  drink(customer.name, coffee);
})

// 출력문
// 카페라떼가 접수되었습니다.
// Steve는 카페라떼를 마십니다.
// 아메리카노는 접수되었습니다.
// John는 아메리카노를 마십니다.

```

```js
// 비동기적 함수
function waitSync(callback, ms) {
  setTimeout(callback, ms); // 특정 시간 이후에 callback 함수가 실행되게끔 하는 브라우저 내장 기능입니다.
} 

function drink(person, coffee) {
  console.log(person + '는' + coffee + '를 마십니다');
}
function orderCoffeeSync(menu, callback) {
  console.log(menu + '가 접수되었습니다');
  waitSync(function() {
    callback(menu)
  }, 2000);
}
let customers = [{
  name: 'Steve',
  request: '카페라떼'
},
{
	name: 'John',
  request: '아메리카노'
}]
// call asynchronously
customers.forEach(function(customer) {
  let coffee = orderCoffeeSync(customer.request, function(coffee) {
    drink(customer.name, coffee);
  });
})

// 카레라떼가 접수되었습니다.
// 아메리카노가 접수되었습니다.
// Steve는 카레라떼를 마십니다.
// John는 아멜리카노를 마십니다.
```

### 비동기의 주요 사례

- DOM Element의 이벤트 핸들러
  - 마우스, 키보드 입력(click, keydown 등)
  - 페이지 로딩(DOMContentLoaded 등)
- 타이머
  - 타이머 API (setTimeout 등)
  - 애니메이션 API (requestAnimationFrame)
- 서버에 자원 요청 및 응답
  - fetch API
  - AJAX (XHR)

## 01. Promise

프로미스는 비동기 작업을 조금 더 편하게 처리 할 수 있도록 ES6에 도입된 기능이다. 이전에는 비동기 작업을 처리 할 때에는 콜백 함수로 처리를 해야 했었는데, 콜백 함수로 처리를 하게 된다면 비동기 작업이 많아질 경우 코드가 쉽게 난잡해지게 되었습니다.

```js
function increaseAndPrint(n, callback) {
  setTimeout(() => {
    const increased = n + 1;
    console.log(increased);
    if (callback) {
      callback(increased);
    }
  }, 1000);
}

increaseAndPrint(0, n => {
  increaseAndPrint(n, n => {
    increaseAndPrint(n, n => {
      increaseAndPrint(n, n => {
        increaseAndPrint(n, n => {
          console.log('끝!');
        });
      });
    });
  });
});
```

코드 읽기가 복잡해진다. 이런 식의 코드를 Callback Hell(콜백 지옥)이라고 부른다.

### Promise 만들기

Promise는 성공 할 수도 있고, 실패 할 수도 있다. 성공 할 때에는 resolve 를 호출해주면 되고, 실패할 때에는 reject를 호출해주면 된다.

resolve 를 호출 할 때 특정 값을 파라미터로 넣어주면, 이 값을 작업이 끝나고 나서 사용 할 수 있다. 작업이 끝나고 나서 또 다른 작업을 해야 할 때에는 Promise 뒤에 `.then(...)` 을 붙여서 사용하면 된다.

실패하는 상황에서는 `reject` 를 사용하고, `.catch` 를 통하여 실패했을시 수행 할 작업을 설정 할 수 있다.

```js
function increaseAndPrint(n) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const value = n + 1;
      if (value === 5) {
        const error = new Error();
        error.name = 'ValueIsFiveError';
        reject(error);
        return;
      }
      console.log(value);
      resolve(value);
    }, 1000);
  });
}

increaseAndPrint(0)
  .then(n => {
    return increaseAndPrint(n);
  })
  .then(n => {
    return increaseAndPrint(n);
  })
  .then(n => {
    return increaseAndPrint(n);
  })
  .then(n => {
    return increaseAndPrint(n);
  })
  .then(n => {
    return increaseAndPrint(n);
  })
  .catch(e => {
    console.error(e);
  });
```

Promise 를 사용하면, 비동기 작업의 개수가 많아져도 코드의 깊이가 깊어지지 않게 된다.

하지만, 이것도 불편한점이 있긴 하다. 에러를 잡을 때 몇번째에서 발생했는지 알아내기도 어렵고 특정 조건에 따라 분기를 나누는 작업도 어렵고, 특정 값을 공유해가면서 작업을 처리하기도 까다롭다. 다음 섹션에서 배울 async/await 을 사용하면, 이러한 문제점을 깔끔하게 해결 할 수 있다.

## 02. async/await

async/await 문법은 ES8에 해당하는 문법으로서, Promise 를 더욱 쉽게 사용 할 수 있게 해준다.

async/await 문법을 사용할 때에는, 함수를 선언 할 때 함수의 앞부분에 `async` 키워드를 붙여준다. 그리고 Promise 의 앞부분에 `await` 을 넣어주면 해당 프로미스가 끝날때까지 기다렸다가 다음 작업을 수행 할 수 있다.

```js
function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function process() {
  console.log('안녕하세요!');
  await sleep(1000); // 1초쉬고
  console.log('반갑습니다!');
}

process().then(() => {
  console.log('작업이 끝났어요!');
});
```

