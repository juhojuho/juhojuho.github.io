---
title: hexo 기반 블로그에 꿀기능 추가하기
description: "hexo는 static site generator다. 쉽게 말해서 블로그 만드는 라이브러리라 생각하면 된다. Ruby 기반의 Jekyll과 함께 커스텀 블로그 계의 양대산맥으로 군림하고 있다. 그 자체만으로 굉장히 강력한 기능을 제공하지만 몇 가지 설정과 플러그인을 추가해 주면 편하게 블로그를 운영할 수 있다."
date: 2019-03-27
tags: 
    - hexo
    - blog
    - markdown
layout: layouts/post.njk
---

[hexo](https://hexo.io)는 static site generator다. 쉽게 말해서 블로그 만드는 라이브러리라 생각하면 된다. Ruby 기반의 Jekyll과 함께 커스텀 블로그 계의 양대산맥으로 군림하고 있다. 그 자체만으로 굉장히 강력한 기능을 제공하지만 몇 가지 설정과 플러그인을 추가해 주면 편하게 블로그를 운영할 수 있다. 아래는 아름다운 블로그를 위한 꿀팁들이다.

## Themes
우선 내 블로그를 좀 더 멋지게 꾸며본다. 기본 theme인 [landscape](https://github.com/hexojs/hexo-theme-landscape)도 좋지만 솔직히 말해서 별 기능은 없다. [여기](https://hexo.io/themes/)에 hexo에서 제공하는 모든 theme 목록이 있다. 어떤 theme은 단순히 레이아웃 변화 외에도 disqus 등의 댓글 시스템이나 google analytics 같은 기능을 블로그에 손쉽게 적용할 수 있게 도와준다. 쇼핑하는 마음으로 서너 개 골라서 직접 블로그에 적용시켜 보자.

## Markdown
Markdown를 사용하면 귀찮게 html tag로 열고 닫지 않아도 손쉽게 웹 템플릿을 생성할 수 있다. hexo도 기본으로 Markdown renderer를 제공하지만 그 기능은 매우 제한적이다. 주석도 못 달고 heading에 자동으로 anchor tag도 안 달아준다. 좀 더 풍부한 기능을 사용하기 위해 [hexo-renderer-markdown-it](https://github.com/celsomiranda/hexo-renderer-markdown-it) 플러그인을 사용하자. [설치 과정](https://github.com/celsomiranda/hexo-renderer-markdown-it/wiki/Getting-Started)을 마친 후에 `_config.yml`에 마크다운 설정을 추가해 주자. 내가 사용하고 있는 설정은 다음과 같다.

```yml
markdown:
  render:
    html: true
    xhtmlOut: false
    breaks: true
    linkify: true
    typographer: true
    quotes: '“”‘’'
  plugins:
    - markdown-it-abbr
    - markdown-it-footnote
    - markdown-it-ins
    - markdown-it-sub
    - markdown-it-sup
```

## RSS
RSS는 간단히 말해서 웹 구독 시스템이다. 우리가 매일 아침 신문을 받아보듯 내가 등록한 사이트에 새 글이 올라오면 자동으로 나에게 일러준다. 내가 RSS xml 파일을 블로그에 올려두면 구독자의 RSS reader가 그 파일을 긁어가는 시스템이다. 따라서 내가 새 글을 올려도 xml 파일을 업데이트해주지 않으면 구독자에게 알림이 가지 않는다. 굉장히 귀찮은 작업이다. 다행히도 [hexo-generator-feed](https://github.com/hexojs/hexo-generator-feed) 플러그인을 사용하면 xml 파일을 자동으로 생성해 준다. 이 역시 설치 후 _config.yml에 설정을 추가해야 한다. 다음은 내가 사용하고 있는 설정이다.

```yml
feed:
  type: atom
  path: atom.xml
  limit: false
  hub:
  content: true
```

## Custom Domain
Github pages를 이용하면 손쉽게 포스트를 발행할 수 있다. 하지만 안타깝게도 도메인이 길어진다. 뒤에 github.io가 붙는다. 어우 거추장스러워~ 다행히도 나만의 도메인을 사용할 수도 있다. 아래의 순서를 따라가자. 아래의 과정은 Github pages 사용자에게만 유효하다.

1. 먼저 도메인을 구입하자. 내 도메인의 경우 juhojuho.com이다.
2. record 몇 개를 추가해 줘야 한다. 내가 도메인을 구입한 곳에서 record 관리 창에 들어간다. 다음의 IP 주소로 A record 2개를 생성한다. 
    > 192.30.252.153
    > 192.30.252.154
3. 블로그 github repo의 `Settings`에서 'Custom domain' 칸에 내 도메인을 입력하고 저장 버튼을 누른다.
4. 마지막으로 CNAME 파일을 추가해야 한다. 내 hexo blog의 루트 폴더에 있는 public 폴더에 CNAME 파일을 생성한다. 그리고 내 도메인을 입력하자. www나 http:// 빼고 오직 도메인만 입력하자.

이외에도 도메인 설정을 더 추가할 수 있지만 이 포스트 성격에 맞지 않다 생각해서 이 정도로 줄였다. 더 많이 알고싶다면 [이 포스트](https://help.github.com/articles/using-a-custom-domain-with-github-pages/)를 참조하자.

## More
사실 대부분의 꿀기능은 이미 hexo의 공식 웹사이트에 적혀 있다. 위에 적은 것들은 하나의 가이드라인이자 개인적으로 추천하는 플러그인일 뿐이다. 더 많은 꿀기능을 원한다면 [이곳](https://hexo.io/plugins/)을 방문하자.
