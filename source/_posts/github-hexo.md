---
title: Github + Hexo 웹 페이지 만들기
date: 2018-01-01 22:44:43
tags: [Github, Hexo]
---
Github에서 제공하는 [Github Pages](https://pages.github.com/)라는 호스팅 서비스를 이용하면 개인 웹페이지를 무료로 생성할 수 있다. 생성된 웹페이지에 [Hexo framework](https://hexo.io/)를 이용하여 테마를 입힌 과정을 기술한다.

> **준비사항:**
>
> *   [Node.js](https://nodejs.org/)
> *   [Git](https://git-scm.com/)
> *   [Github Account](https://github.com/)

# Github Pages

<div style="position:relative;height:0;padding-bottom:56.25%"><iframe src="https://www.youtube.com/embed/2MsN8gpT6jY" style="position:absolute;width:100%;height:100%;left:0" width="640" height="360" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe></div>

Github Pages는 **무료**이다. 또한 가입형 블로그의 경우 제한된 프레임 안에서 사용해야 하기 때문에 편집시 제한이 많이 있다. 하지만 Github Pages의 경우 학습이 필요하긴 하지만 자유롭게 변경이 가능하다. Terminal 기반이 익숙하지 않다면 [Github Pages](https://pages.github.com/)에서 다른 버젼의 설명을 선택하여 진행한다.

### New repo

Github 계정 생성후 아래 이미지와 같이 저장소를 생성한다.
![github-hexo-1](/images/github-hexo-1.png)

> Repository name은 username.github.io
> username은 계정명으로 사용
> Public / Private 중 Public 선택 후 저장소 생성

### [](#Clone "Clone")Clone

프로젝트를 저장할 로컬 폴더로 이동하여 새 저장소를 복제한다.

``` bash
$ git clone https://github.com/username/username.github.io
```

### Hello World

``` bash
$ cd username.github.io
$ echo "Hello World" > index.html
```

### Push it

변경 내역 모두 add, commit, push

``` bash
$ git add --all
$ git commit -m "Initial commit"
$ git push -u origin master
```

### [](#…and-you’re-done "…and you’re done!")…and you’re done!

**[https://username.github.io](https://username.github.io)** 웹 브라우져로 접속하여 확인한다.

# Hexo

<div style="position:relative;height:0;padding-bottom:56.25%"><iframe src="https://www.youtube.com/embed/ARted4RniaU" style="position:absolute;width:100%;height:100%;left:0" width="640" height="360" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe></div>

[Hexo](https://hexo.io/)와 [Jekyll](https://jekyllrb.com/)은 Github Pages와 함께 다양한 빌트인 테마와 플러그인을 지원하고 있다. 즉, HTML작성에 신경쓰지 않고, Markdown문법을 기준으로 텍스트 내용을 작성하면 static site generator가 알아서 HTML로 바꿔준다. 본 사이트는 Hexo를 이용하여 테마를 입혀놓았다.(Jekyll 보다 알흠다운 테마가 많은것 같다.)

### Install Hexo cli, Setup

``` bash
$ npm install hexo-cli -g

// Hexo 설치후 다음 명령을 실행하여 대상 <folder>에서 Hexo 초기화.
$ hexo init <folder>
$ cd <folder>
$ npm install
```

일단 초기화되면 프로젝트 폴더 구조는 다음과 같다.

``` plain
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

### _config.yml

**[사이트 구성 파일](https://hexo.io/docs/configuration.html)**. 여기에서 기본적인 설정을 구성 할 수 있다.

``` yml
# Site
title:
subtitle:
description:
author:

# URL
url: https://username.github.io
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:

deploy:
  type: git
  repo: https://github.com/username/username.github.io
```

### Test

기본적인 설정 완료후 로컬 서버를 구동후 웹 브라우져에서 [http://localhost:4000](http://localhost:4000) 에 접속하여 테스트가 가능하다.

``` bash
$ hexo server
```

### Post and deploy

[Hello Hexo](https://enchoyism.github.io/2017/12/31/hello-hexo/) 포스트를 확인하여 배포 한다. 간혹 배포후 페이지가 업데이트가 되지 않는다면 다음과 같이 해결한다.

``` bash
$ hexo clean
$ hexo deploy --generate
```

### Theme

> [https://hexo.io/themes/](https://hexo.io/themes/)

대부분 Theme 페이지는 Github 페이지 링크 및 자세한 설명이 포함되어 있다.

# [](#Reference "Reference")Reference

*   [https://pages.github.com/](https://pages.github.com/)
*   [https://hexo.io/](https://hexo.io/)
*   [https://jekyllrb.com/](https://jekyllrb.com/)

# [](#Related-Posts "Related Posts")Related Posts

*   [Github + Hexo 웹 페이지 만들기](https://enchoyism.github.io/2018/01/01/github-hexo)
*   [Github + Hexo 검색 엔진 최적화(SEO)](https://enchoyism.github.io/2018/01/02/github-hexo-seo)
