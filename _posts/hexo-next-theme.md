---
title: Hexo NEXT 테마 설정하기
date: 2020-06-19 19:28:06
categories: Blog
tags:
    - Hexo
    - Blog
---
# Hexo-NEXT 테마 설치하기
Hexo-NEXT 설치는 [Migration From Jekyll To Hexo](http://joyoon729.github.io/Blog/migration-from-jekyll-to-hexo/#NEXT-%ED%85%8C%EB%A7%88-%EC%84%A4%EC%B9%98) 에 설명해 두었다.

# Hexo-NEXT _config.yml 설정하기
`blog/themes/next/_config.yml` 을 수정한다.
```yml
footer:
    since: 2019

copyright: <UserName>

scheme: Mist

sidebar:
    position: right

toc:
    enable: true
    number: true
    wrap: false
    expand_all: false

back2top:
    enable: true
    sidebar: true
    scrollpercent: true

reading_progress:
    color: "#87daff"
    height: 4px

github_banner:
    permalink: https://github.com/<UserName>

disqus:
    enable: true
    shortname: <DisqusUserName>
```

# Categories, About 등의 페이지 생성하기
## NEXT _config.yml 수정하기 
`blog/themes/next_config.yml`에서 활성화할 페이지 메뉴의 주석을 해제한다.
```yml
menu:
    home: / || fa fa-home
    about: /about/ || fa fa-user
    tags: /tags/ || fa fa-tags
    categories: /categories/ || fa fa-th
    archives: /archives/ || fa fa-archive
```

## Page 생성하기
Hexo-CLI 를 사용해 활성화할 페이지를 생성한다.
```shell
$ hexo new page about # {root}/source/about/index.md 생성
$ hexo new page tags
$ hexo new page categories
```

## 생성된 index.md 수정하기
예를 들어 `blog/source/categories/index.md` 를 수정해본다.
```yml
---
type: categories
comments: false  # comments-false 여야 disqus 비활성화 된다.
---
```

<br><br><br>

