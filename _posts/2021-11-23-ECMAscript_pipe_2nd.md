---
title: ECMAScript 파이프라인 제안서 2단계 진입
author: Yongjun042
date: 2021-11-23 11:54:00 +0900
categories: [Blog, Develope]
tags: [javascript, ecmascript, pipe]
---
[링크](https://github.com/tc39/proposal-pipeline-operator)

파이프라인이 다음과 같은 EcmaScript의 총 5단계(0~4)의 제안 과정 중 2단계에 진입했습니다. 2단계는 공식 언어를 이용해서 문법과 의미를 설명하는 문서를 작성하는 단계로 피드백을 받는 3단계를 거치면 최종적으로 4단계에서 공식 사양이 됩니다. 해당 문서의 일부를 정리해보았습니다.

## 기존 방식

기존에는 값을 가지고 연속적인 연산을 할 경우
값을 연산의 인자로 넘겨주는 중첩 연산(`three(two(one(value)))`)이나 값의 메소드를 이용해 연산(`value.one().two().three()`)을 해야 했습니다.

### 중첩 연산

``` javascript
console.log(
  chalk.dim(
    `$ ${Object.keys(envars)
      .map(envar =>
        `${envar}=${envars[envar]}`)
      .join(' ')
    }`,
    'node',
    args.join(' ')));

```

#### 장점

* 모든 종류의 문법에 대해 사용이 가능합니다.

#### 단점

* 연산을 중첩해서 사용할 경우 읽는 방향이 오른쪽에서 왼쪽으로 일반적인 방식의 반개이기 때문에 읽기가 힘들어집니다.
* 인자값이 여러개일 경우 읽는 방향을 여러번 바꿔야 합니다.
* 수정하기가 어렵습니다. 

### 메소드 체이닝 연산

jQuery가 사용하는 방식입니다.

#### 장점

* 읽는 방식은 왼쪽에서 오른쪽으로 일정합니다.
* 함수의 인자값이 함수의 이름으로 묶여있어 수정이 용이합니다. 
* 자바스크립트의 `await`, `yield`, 배열 리터럴, 등과 같은 다른 문법을 사용할 수 없습니다.

#### 단점

* 메소드 체이닝을 이용할 경우 값이 해당 함수를 메소드 형식으로 갖고있을 경우만 사용이 가능합니다.  


### 임시 변수 방식
``` javascript
const envarString = Object.keys(envars)
  .map(envar => `${envar}=${envars[envar]}`)
  .join(' ');
const consoleText = `$ ${envarString}`;
const coloredConsoleText = chalk.dim(consoleText, 'node', args.join(' '));
console.log(coloredConsoleText);
```
임시 변수를 사용해서 파이프와 비슷한 효과를 낼 수 있습니다. 하지만 코드가 너무 길어지고 많은 변수명을 추가로 지어야 합니다.

단일 변수를 사용할 경우 연산 도중에 값이 바뀔 수 있다는 단점이 있습니다.
``` javascript
// setup
function one () { return 1; }
function double (x) { return x * 2; }

let _;
_ = one(); // _ = 1.
_ = double(_); // _ = 2.
_ = Promise.resolve().then(() =>
  // 2가 출력되지 안습니다!
  // `_`가 아래에서 쟇할당되었기 때문에 1을 출력합니다.
  console.log(_));

// promise 콜백 전에 _가 1이 됩니다.
_ = one(_);
```

## 파이프 방식

``` javascript
Object.keys(envars)
  .map(envar => `${envar}=${envars[envar]}`)
  .join(' ')
  |> `$ ${% raw %}{%}{% endraw %}`
  |> chalk.dim(%, 'node', args.join(' '))
  |> console.log(%);
```
파이프는 `|>`를 이용해 왼쪽에 있는 값을 오른쪽으로 념겨주는 역할을 합니다.

Hack 방식과 F#방식 중 Hack 방식이 선택되었습니다.

### Hack 표현식

`value |> one(%) |> two(%) |> three(%)`와 같은 방식으로 작동합니다. 파이프 기호의 우항에는 표현식이 들어가야 되고 `%`기호를 이용해 좌항의 값이 들어갈 자리를 정합니다.

* 우항에는 어떤 표현식이든 들어갈 수 있다는 장점이 있습니다.

* 단항 연산의 경우에도 `(%)`와 같이 표현을 해줘야 한다는 단점이 있습니다.

### F# 단항 함수
`value |> one |> two |> three`와 같은 방식으로 작동합니다. 우항에는 항상 단항 함수로 평가되어야하고 좌항의 값은 단일 매개변수가 됩니다.

* 단항 함수를 호출할 경우 매우 간단하다는 장점이 있습니다.
* 단항 함수가 아닌 다른 구문의 경우 화살표 함수로 감싸야 된다는 단점이 있습니다. `value |> x=> foo(1, x)`
* `await`와 `yield`의 경우 포함하고 있는 함수로 스코프가 제한되어 있기 때문에 `value |> await`를 awaiting promise로 `value |> yield`를 yielding generatir로 따로 취급을 해 줘야 합니다.

단항 함수가 아닌 다른 구문이 더 자주 쓰이고, 제안된 다른 기능과의 호환이 더 좋다는 점, F#의 경우 특수 케이스가 존재한다는 점, `value |> someFunction + 1`와 같은 식의 경우 Hack 방식은 문법 오류지만 F#방식은 런타임 오류라는 점, TC39에서 F#파이프 방식을 여러번 반려했다는 점에서 Hack 방식이 선택되었습니다.

### 규칙

* topic reference인 `%`(임시)는 영항 연산자로 topic value의 위치를 나타내면서 렉시컬 스코프이고, 불변값입니다.
* 파이프 연산자인 `|>`는 화살표 함수`=>`, 대입 연산자`=`, `+=`, 생성 연산자`yield`와 우선순위 가 같습니다.
* 더 높은 우선순위를 가지고 있는 연산자는`,` 하나뿐입니다.
* `v => v |> % == null |> foo(%, 0)`는 `v => (v |> (% == null) |> foo(%, 0))`와 같이 묶을 수 있습니다. 
* `value |> foo + 1` 는 `%`가 없기 때문에 올바르지 않은 구문입니다. 또한 `%`는 파이프 바깥에서 사용할 수 없습니다.
* 같은 우선순위를 가지고 있는 연산자는 파이프와 사용할 수 없고 사용하게 될 경우 순서를 명시해줘야 합니다.`a |> b ? % : c |> %.d`의 경우 `a |> (b ? % : c) |> %.d`나 `a |> (b ? % : c |> %.d)`로 작성해야 합니다.
* 동적으로 컴파일되는 코드는 해당 코드 바깥에서 사용할 수 없습니다. `v |> eval('% + 1')`는 올바르지 않은 구문입니다.

* 사이드이펙트는 `,`를 이용해 `value |> (sideEffect(), %)`와 같이 사용할 수 있습니다.

