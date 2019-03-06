---
title: Github + Hexo 검색 엔진 최적화(SEO)
date: 2018-01-02 23:10:58
tags: [Github, Hexo]
---
Github + Hexo 웹 페이지에서 **검색 엔진 최적화(SEO)에 유용한 플러그인**을 소개한다.

# [hexo-autonofollow](https://github.com/liuzc/hexo-autonofollow)

각 포스트의 외부 링크에 대해 자동으로 `nofollow` 속성을 추가 해준다.

### Install & Option

``` bash
$ npm install hexo-autonofollow --save
```

_config.yml

``` yml
nofollow:
  # 플러그인 활성화
  enable: true
  # 제외할 호스트
  exclude:
    - exclude1.com
    - exclude2.com
```

# [hexo-auto-canonical](https://github.com/HyunSeob/hexo-auto-canonical)

각 포스트마다 자동으로 표준 링크를 만들어 준다.

### Install & Option

``` bash
$ npm install hexo-auto-canonical --save
```

HTML `<head>` 태그안에 추가.
with ejs

``` html
<%- autoCanonical(config, page) %>
```

with jade

```
!{ autoCanonical(config, page) }
```

# [hexo-generator-seo-friendly-sitemap](https://github.com/ludoviclefevre/hexo-generator-seo-friendly-sitemap)

크롤러가 페이지를 효율적으로 크롤링할 수 있도록 sitemap.xml을 자동으로 생성해준다.

### Install & Option

``` bash
$ npm install hexo-generator-seo-friendly-sitemap --save

```

_config.yml

``` yml
# sitemap auto generator
sitemap:
  path: sitemap.xml
```

배포후 [https://username.github.io/sitemap.xml](https://username.github.io/sitemap.xml) 에서 확인 가능하다.

생성된 sitemap.xml 파일을 검색엔진 [구글 웹마스터 도구](https://www.google.com/webmasters/tools/home?hl=ko) | [네어버 웹마스터 도구](http://webmastertool.naver.com/) 등에 제출 해본다.

# [hexo-generator-feed](https://github.com/hexojs/hexo-generator-feed)

Atom 1.0 또는 RSS 2.0 피드를 생성한다.

### Install & Option

``` bash
$ npm install hexo-generator-feed --save
```

_config.yml

``` yml
# rss feed auto generator
feed:
  # feed type (atom / rss2)
  type: rss2
  # feed path (Default: atom.xml / rss2.xml)
  path: rss2.xml
  # 최신 포스트의 갯수 설정. (0 또는 false 입력시 전체 포스트)
  limit: 20
```

배포후 [https://username.github.io/rss2.xml](https://username.github.io/rss2.xml) 에서 확인 가능하다.

생성된 rss2.xml 파일을 검색엔진 [구글 웹마스터 도구](https://www.google.com/webmasters/tools/home?hl=ko) | [네어버 웹마스터 도구](http://webmastertool.naver.com/) 등에 제출 해본다.

# robot.txt

``` plain
User-agent: *
Allow:/

Sitemap: https://username.github.io/sitemap.xml
```

파일을 검색엔진 [구글 웹마스터 도구](https://www.google.com/webmasters/tools/home?hl=ko) | [네어버 웹마스터 도구](http://webmastertool.naver.com/) 등에 제출 해본다.

# [](#Related-Posts "Related Posts")Related Posts

*   [Github + Hexo 웹 페이지 만들기](https://enchoyism.github.io/2018/01/01/github-hexo)
*   [Github + Hexo 검색 엔진 최적화(SEO)](https://enchoyism.github.io/2018/01/02/github-hexo-seo)
