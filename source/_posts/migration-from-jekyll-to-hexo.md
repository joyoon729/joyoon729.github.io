---
title:  Migration From Jekyll To Hexo
date: 2020-06-19 15:10:08
categories: Blog
tags:
    - Hexo
    - Blog
---
# Hexo 로 변경한 이유
오랫동안 방치해뒀던 블로그를 다시 시작하기로 했다.

이전엔 블로그를 Jekyll 로 구성했었는데, Hexo 에 마음에 드는 테마가 있기도 했고, 블로그를 다시 시작하는 마당에 새로 다시 만들어보고자 Hexo 로 구성을 해 보았다.

# Hexo 설치 및 구성
## 준비물
- github 계정
- Node.js
- npm

## 설치하기
Jekyll 로 블로그 구성을 할 때 `bundler` 를 사용해 Jekyll 설치 및 환경 구성을 했던 것처럼, Hexo 는 `npm` 을 사용해 설치를 한다.

Ruby에 익숙하지 않아서 `bundler` 를 통한 설치는 이해도 잘 안되고 막힘이 많았었는데, 익숙한 Node.js 의 `npm` 으로 Hexo 설치는 쉬웠다.

```shell
$ npm install hexo-cli -g
$ hexo init blog
$ cd blog
$ npm install
```

명령어 네 줄로 아래 구조를 가진 Hexo 기본 블로그 템플릿이 만들어진다.

```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

## Hexo 서버 실행하기
로컬에서 Hexo 서버를 실행해서 배포 전에 미리 확인해볼 수 있다.

```shell
$ hexo server
```
`http://127.0.0.1:4000` 로 접속해 생성된 사이트 확인 가능하다.

## _config.yml 수정하기
루트 폴더의 `_config.yml` 를 수정해서 Hexo 설정을 변경할 수 있다.
나는 `title`, `author`, `language`, `url`, `permalink` 를 변경했다.

```yml
title: joyoon
author: Jo Yoon Gi
language: ko
url: http://joyoon729.github.io
permalink: :category/:title/
```

# NEXT 테마 설치
1. hexo init 한 경로로 이동한다.
`ex) ~/blog/`
```shell
$ git clone https://github.com/theme-next/hexo-theme-next themes/next
```

2. `_config.yml` 의 theme 수정하기
```yml
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next
```

3. Hexo 서버 재시작하기
```shell
$ hexo clean
$ hexo server
```

# 배포하기
1. `_config.yml` 에서 deploy 설정을 한다.
```yml
deploy:
    type: git
    repo: https://github.com/<UserName>/<UserName>.github.io
    branch: master
```

2. `hexo-deployer-git` 모듈을 설치한다.
```bash
$ npm install hexo-deployer-git --save
```

3. deploy 커맨드를 실행한다.
```bash
$ hexo deploy
```
그러면 `deploy.repo` 에 해당하는 레포지토리에 배포된다.

# 참고 사이트
[hexo 블로그 시작하기 | Hexo Porject](https://enesto.github.io/2018/11/18/181118_hexo%20%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0/)

<br><br><br><br>


