---
title: 윈도우 썸네일 만들기
author: Yongjun042
date: 2021-05-23 22:00:00 +0900
categories: [Blog, Develope]
tags: [C#, Windows]
---

윈도우 자체에서 지원하지 않는 파일들의 썸네일 이미지들을 만들어보자.

블루레이 리핑해서 가지고 있을 때 전부 iso기본이미지라 구분이 너무 힘들어서 만들었다.

윈도우에서 썸네일 프로그램을 만들려면 IThumbnailProvider 인터페이스를 이용해서 코드를 C++로 짠 다음에 COM서버로 올려야되는데 다행히 해당 기능을 C#으로 짤 수 있게 하는 C# 프로그램이 있다.

일단 커스텀 썸네일 프로그램을 만들 수 있는 기존의 프로그램이 2개 있다. 둘 다 파일을 읽어들여서 크기에 맞게 썸네일을 Bitmap으로 리턴하면 된다.

## PowerToys

[링크](https://github.com/microsoft/PowerToys)

마이크로소프트에서 만든 윈도우 편의성 툴.
previewpane에 썸네일 확장 프로그램 코드가 있고 IInitializeWithFile, IThumbnailProvider를 상속해서 XYZThumbnailHandler를 생성해서 모듈을 추가할 수 있다.

문제는 해당 문서가 오래된 거라 실제 코드와 다른 점이 많다. 기존에 있는 svg확장프로그램을 참고해서 dll까지 만들 수 있었지만 해당 dll을 해당 프로그램에 연결하는것에 실패해서 여러 방법을 찾다가 포기. 설치하려면 전체 프로젝트를 컴파일해야되서 용량도 커지고 번거롭다.

## SharpShell

[링크](https://github.com/dwmkerr/sharpshell)

.NET프로그램을 이용한 쉘 스크립트 확장 프로그램이다.

SharpThumbnailHandler를 상속해서 만들 수 있다. 튜토리얼도 존재한다. 샘플이 깃허브에 존재하는데 AssociationType.FileExtension대신 .ClassOfExtension을 잘못 사용하고 있다. 다른 문서에서는 ClassOfExtension을 지원 안한다고 써져있는데 뭐가 잘못되는건지 모르겠다.

문제는 자잘한 오류가 꽤 많은 것 같다. 실행이 안되서 엄청 헤맸는데 어느순간부터 오류를 뿜던 코드가 갑자기 된다. [ServerManager](https://github.com/dwmkerr/sharpshell)를 이용해서 디버깅과 설치/삭제가 가능하다.
