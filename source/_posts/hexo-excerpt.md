---
title: Hexo '더 읽어보기' 설정하기
date: 2020-06-25 22:03:44
categories: Blog
tags:
    - Blog
    - Hexo
---
# Hexo Excerpt
Hexo 의 홈 화면에서 포스트의 모든 내용을 보여주지 않고 일부분만 노출되도록 할 수 있다.
`hexo-excerpt` 플러그인을 설치하면 포스트의 일부만 노출되고 '더 읽어보기' 버튼이 나타난다.

## 플러그인 설치하기
```shell
$ npm install hexo-excerpt
```

## _config.yml 설정하기
`blog/_config.yml` 에서 아래 내용을 추가/수정 해준다.

```yml
# hexo-excerpt
excerpt:
    depth: 7  # 7줄 까지만 노출된다는 뜻
    hideWholePostExcerpt: true
```