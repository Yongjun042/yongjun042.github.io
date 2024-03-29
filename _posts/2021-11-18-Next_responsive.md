---
title: 컴포넌트 기반 웹 디자인 살펴보기
author: Yongjun042
date: 2021-11-18 21:09:16 +0900
categories: [Blog, Develope]
tags: [CSS, Web, Responsive]
---

구글에서 올린 [새로운 반응형: 컴포넌트 기반 세상의 웹 디자인](https://web.dev/new-responsive/#designing-for-dark-theme)을 읽고 정리해 보았다.

기존의 뷰포인트 기반 미디어 쿼리는 유저의 요구를 전부 충족시키지도 못하고 컴포넌트 스스로 반응형 스타일을 인젝션할 수도 없다고 하면서 해당 문제를 해결할 수 있는 기능들을 소개하였다. 아직 개발중인 기술도 소개가 되었다.

##사용자 반응성

사용자의 요구에 따라 페이지를 보여줄 수 있는 미디어 옵션들을 소개했다.
`prefers-reduced-motion`, `prefers-contrast`, `prefers-reduced-transparency`, `prefers-color-scheme`, `inverted-colors` 등이 있고 아래 2개의 옵션을 제외하고 현재 기준으로 미구현이거나 구현된 브라우저가 거의 없다.

### prefers-reduced-motion
ADHD인 사람들이나 광과민성을 가진 사람들에게 도움을 줄 수 있는 옵션이다. 또한 저사양 기기에서의 퍼포먼스 향상과 배터리 절약을 기대할 수 있다. 다만 모션을 완전히 없애는 것이 아니라 강조를 위한 필수적인 모션은 남겨둬야 하는 것이 핵심이다.

### prefers-color-scheme
다크모드를 적용하거나 페이지의 악센트 컬러를 변경할 수 있다. 이때 다크 모드는 배경을 단순히 어둡게 하면 안된다. 채도를 감소시켜 눈을 편안하게 해주거나 또한 그림자를 이용한 깊이 표현도 다크 모드에서는 눈에 띄지 않기 때문에 튀어나온 느낌을 위해서 요소의 색을 밝게 해야 된다.

## CSS 컨테이너 쿼리
엘리먼트 쿼리라고도 불리는 이 쿼리는 부모 요소의를 기반으로 반응형 디자인을 할 수 있는 쿼리이다. 전체 페이지가 아니라 부모에 따라 반응을 하기 때문에 더 세부적으로 설정을 할 수 있다.

## 스코프 스타일
많은 프레임워크나 css모듈에서 지원하는 스코프 스타일이 네이티브로 지원될 수도 있다. 시작점과 끝점 지정할 수 있다.

## 폼팩터에 반응하기
듀얼스크린에서 웹페이지를 적절하게 표현할 수 있는`env(fold-top)`, `screen-spanning`과 같은 요소들이 있다. 사실상 서피스 듀오 전용 기술이지 않나 싶다.