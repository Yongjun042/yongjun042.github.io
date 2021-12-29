---
title: Error listen EACCES permission denied 해결법
author: Yongjun042
date: 2021-08-09 21:09:00 +0900
categories: [Blog, Develope]
tags: [Error, Windows]
---


해당 포트가 사용중이라는데 문제는 윈도우에 `netstat`을 이용해서 찾아봐도 해당 포트를 사용중인 프로그램은 없었다.

사실 전에도 포트 사용중 오류가 있었는데 이건 포트 고정이라 다른 오류 메세지가 떴고 이 오류로 검색해 [해결책](https://stackoverflow.com/questions/59428844/listen-eacces-permission-denied-in-windows)을 찾았다.

 ```shell

net stop winnat

net start winnat

```

cmd창이나 PowerShell에 다음 명령어를 쳐 winnat 서비스를 껐다 키면 된다.

Hyper-V의 가상 컴퓨터의 포트 연결에 쓰이는데 뭔가 버그가 있는 것 같다.

윈도우 업데이트를 할때도 일부 포트가 막힌다고도 한다.
