---
title: React 파해치기(1)
author: Yongjun042
date: 2022-03-08 19:41:00 +0900
categories: [Blog, Develope]
tags: [React]

---

## 리액트

### 컴포넌트

출력값이 element tree 인 class 또는 function

``` javascript
import React from 'react';
import './App.css';

function App1() {
  const name = 'react';
  return <div className = "react">{name}</div>
}

class App2 extends Component {
  render() {
    const name = 'react';
    return <div className="react">{name}</div>
  }
}
```

[babel](https://babeljs.io/repl)을 통해서 확인해 보면

``` javascript
"use strict";

var _react = _interopRequireDefault(require("react"));

require("./App.css");

function _interopRequireDefault(obj) { return obj && obj.__esModule ? obj : { default: obj }; }

function App1() {
  const name = 'react';
  return /*#__PURE__*/_react.default.createElement("div", {
    className: "react"
  }, name);
}

class App2 extends Component {
  render() {
    const name = 'react';
    return /*#__PURE__*/_react.default.createElement("div", {
      className: "react"
    }, name);
  }

}
```

jsx로 작성된 코드들이 `React.createElement()`를 통해 Object로 변환되는 것을 볼 수 있다.
함수로 작성된 코드는 바로 호출되고 클래스는 새 인스턴스를 생성한 후 `render()`를 통해서 호출된다.
함수는 그 자체로는 다른 컴포넌트와 상호작용하거나 state와 같은 라이프사이클에 접근할 수 없다.

그리고 생성된 App1을 `console.log(App1())`으로 출력해보면 다음과 같은 리엑트 오브젝트로 변환이 된 것을 알 수 있다.

``` javascript
Object { "$$typeof": Symbol("react.element"), type: "div", key: null, ref: null, props: {…}, _owner: null, _store: {…}, … }
"$$typeof": Symbol("react.element")
...
key: null
props: Object { className: "react", children: "react" }
ref: null
type: "div"
...
```

`console.log(<App1/>)`으로 찍어보면 type 부분이 `type: function App1()`으로 변한 것을 알 수 있다. 이 부분이 실제 컴포넌트 부분이라고 할 수 있다. 참고로 클래스의 경우 function 대신 class App2() 이런 식으로 표시된다. 그리고 이 오브젝트가 바로 가상 DOM이다.

React는 각각의 컴포넌트를 인스턴스를 생성하는 것으로 추적하고 각각의 인스턴스는 라이프사이클과 내부 state를 가지고 있다.

### 재조정(Reconciliation)

리액트에서 변경점이 생기게 되면 바꿔야 되는 부분만 알고리즘을 통해서 비교를 하게 된다.
일반적으로 트리를 비교하게 되면  O(n3)의 복잡도를 가지게 되기 때문에 두 가지 가정을 통해 O(n)의 휴리스틱 알고리즘을 사용한다.

1. 서로 다른 타입의 두 엘리먼트는 서로 다른 트리를 만들어낸다.
2. 개발자가 key prop을 통해, 여러 렌더링 사이에서 어떤 자식 엘리먼트가 변경되지 않아야 할지 표시해 줄 수 있다.

### 엘리먼트 타입이 다를 경우

`<div>` 에서 `<img>`와 같이 타입이 바뀔 경우 해당 자식 노드까지 포함해서 전부 새로 재구축합니다.


### 타입이 같을 경우
같은 엘리먼트일 경우 변경된 속성만 바꿉니다.

### 같은 타입의 컴포넌트 엘리먼트

컴포넌트 인스턴스의 props를 갱신합니다.

### 자식에 대한 처리

자식을 비교하게 될 경우 재귀적으로 첫 자식부터 비교를 하게 됩니다. 그리고 변경점이 생기면 해당 자식 이후 노드를 새로 재구축합니다. 이 경우 맨 앞에 자식이 추가될 경우 전부 재구축해야되는 비효율적인 방식으로 작동합니다. 이럴 경우 Key값을 부여하면 해당 값을 기준으로 비교를 하기 때문에 필요한 부분만 재구축할 수 있습니다. Key값은 index와 같이 상태에 영향을 받는 값을 사용하면 소용이 없습니다.

``` javascript
<ul>
  <li>first</li>
  <li>second</li>
</ul>

//앞에 zero가 추가된게 아니라 li가 전부 삭제되고 새로 생성된다.
<ul>
  <li>zero</li> 
  <li>first</li>
  <li>second</li>
</ul>
```

## 렌더링

이렇게 만들어진 리엑트 노드들은 ReactDOM 이나 React Native 같은 렌더러에 의해서 화면에 표시되게 됩니다. 리액트의 state와 같은 것들이 변경되게 되면 해당 변경점들을 비교한 다음 수정해야 될 노드들을 dispatcher를 통해 렌더러에 요청하게 됩니다.

[참고 How Does React Actually Work? React.js Deep Dive #1](https://www.youtube.com/watch?v=7YhdqIR2Yzo)