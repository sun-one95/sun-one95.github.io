---
layout: single
title: "id와 Selector"
tag: [js]
toc: true
---



# id와 class 셀렉터의 차이점

``class``는 여러명이 공유할 수 있지만, ``id``는 한명당 하나씩만 가져야 하는 유일무이한 값이어야 한다는 것이다. 

``class``는 동일한 값을 갖는 요소(element)들이 여러개 존재할 수 있으며, 

하나의 element가 여러개의 ``class``에 포함될 수도 있다. 따라서 classs는 style을 구분하는 기준으로 많이 사용한다. 

``id``는 동일한 값을 갖는 element가 여러개 존재할 수 없으며, 하나의 element는 하나의 id값만 가질 수 있다.

``'유일무이', '고유한 값'`` 등으로 이해하면 될 것 같다. 

따라서 ``id``는 특정한 element에 이름을 붙이는 데 주로 사용한다. 

style은 ``tag < class < id`` 순서로 우선 적용되어, 이런 특징을 이용해 단계적으로 element 간의 중복을 제거할 수 있다.



나 또한, css를 특정 부분을 달리해야 할 때는 ``id``를 설정하고, 공통적인 css 를 부여하고 싶을 때는 ``class``선택자를 사용한다.

