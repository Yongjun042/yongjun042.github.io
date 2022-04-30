---
title: 정적 페이지 배포 사이트 비교
author: Yongjun042
date: 2022-03-08 19:41:00 +0900
categories: [Blog, Develope]
tags: [Pages, CI]

---

## 지원 기능

|                     | Github Pages | GitLab Pages | Cloudflare Pages |
|---------------------|--------------|--------------|------------------|
|  정적 사이트 생성기 | 지킬만       | O            |    O              |
|     이전버전 롤백      |    X   |       X       |         O     |
|     자동 HTTPS      |      O     |     O        |         O        |
|     빌드 제한       |     10빌드/시간    |      무제한     |     500빌드/달      |
|       대역폭        |     100GB      |     무제한       |      무제한        |
|       통계        |      X      |     X       |               O  |

## 배포 방법

### Github

``` yml
name: Build and deploy Jekyll site to GitHub Pages

on:
  push:
    branches:
      - main # or master before October 2020

jobs:
  github-pages:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/cache@v2
        with:
          path: vendor/bundle
          key: ${{ runner.os }}-gems-${{ hashFiles('**/Gemfile') }}
          restore-keys: |
            ${{ runner.os }}-gems-
      - uses: helaili/jekyll-action@v2
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
```

깃허브 actions의 마켓플레이스에 있는 툴들을 이용해서 빌드를 한다.

### Gitlab

``` yml
image: ruby:2.7

workflow:
  rules:
    - if: '$CI_COMMIT_BRANCH'

pages:
  stage: deploy
  script:
    - gem install bundler
    - bundle install
    - bundle exec jekyll build -d public
  artifacts:
    paths:
      - public
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
```

개발할떄 쓰는 스크립트를 그대로 사용하는 방식으로 이루어진다.

### Cloudflare

|                     | Framework preset | Build command | Build output directory |
|---------------------|--------------|--------------|------------------|
|  값 | jekyll    | jekyll build    | _site        |

드롭다운 메뉴에서 jekyll을 선택하고 커맨드와 경로를 지정해주면 된다.


전체적인 기능과 편의성 면에서 앞으로 Cloudflare Pages를 이용하게 될 것 같다.
