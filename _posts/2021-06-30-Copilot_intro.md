---
title: 깃허브 코파일럿 소개 요약
author: Yongjun042
date: 2021-06-30 14:30:00 +0900
categories: [Blog, Develope]
tags: [GitHub, Ide]
image:
  src: "/assets/img/postimg/copilot/header.PNG"
---

![copilot sample](/assets/img/postimg/copilot/autocomplete.PNG "copilot sample")

[공식 페이지](https://copilot.github.com/)

깃허브에서 공개한 함수 자동 작성 AI 확장 프로그램

주석 -> 코드 변환

반복되는 코드 자동 작성

테스트 코드 작성

JavaScript, Python, Ruby, TypeScript, Go 지원

## 기본 소개

Visual Studio Code Extension으로 제공. 다른 IDE는 현재 계획 없음.

프리뷰 기간에는 사용자 수를 제한하고 있음. 유료버전 가능성 있음.

OpenAI Codex를 이용해 소스코드와 자연어를 인식해서 처리.

첫 시도에 43%의 정확도 10번의 시도 후 57%의 정확도를 보임.

작은 함수들로 나누고, 이름을 명확하게 짓고, 주석을 잘 달았을 경우 더 좋은 성능을 보임.

오직 현재 작업중인 파일 내용만 이해하기 때문에 사용자 정의 타입이 다른 파일에 있을 경우 해당 내용을 현재 파일에 붙여넣는 것이 좋음.

파일의 모든 맥락을 기억할 수 없기 때문에 직전의 맥락만 사용이 됨.

옛날 문법이나 라이브러리를 추천할 수도 있음.

## AI소개

![copilot ai diagram](/assets/img/postimg/copilot/diagram.png "copilot ai diagram")

영어와 공개된 소스코드 -깃허브 포함- 이용

0.1%로 트레이닝 셋의 내용이 나왔고 주로 맥락이 없는 빈 페이지나 널리 쓰이는 해법이 있는 경우 발생.

개인정보를 노출시키지 않기 위해 필터링을 하였지만 아주 낮은 확률로 노출이 될 수 있음.

대부분 개인정보같아 보여도 AI가 만든 가짜 개인정보.

안전하지 않은 코드나 더이상 지원되지 않는 함수를 사용할 수 도 있음.

혐오적이거나 편향된 결과를 필터링했지만 해당 정보가 나올 수 도 있음.

좀 더 생산적이고 흥미로운 작업에 집중할 수 있을 것이고, 진입장벽을 낮출 것으로 예상

## 사용자 자료 수집

파일의 일부를 서버로 보냄. 제안된 코드를 사용했는지 여부를 기록. 추후 선택 여부 결정

수집된 자료는 자동으로 분석되고 모델 개선이나 악용 여부를 위해서만 사람이 읽어봄. 해당 데이터는 다른 사용자에게 공유되지 않음.
