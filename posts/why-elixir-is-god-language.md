---
title: "[번역] 큰 잠재력을 가진 언어, Elixir"
description: "함수형 언어 Elixir와 웹 프레임워크 Phoenix는 최근해외에서 크게 인기를 끌고 있는 핫한 놈들입니다. Elixir는 Ruby, Clojure, Erlang의 영향을 받았으며 Erlang의 가상 머신에서 돌아갑니다. Erlang이 가진 함수형 언어의 이점을 가져 오면서 프로그래머가 코딩하는 데 불편한 여러 포인트를 보완한 신생 언어입니다."
date: 2018-06-07
tags:
  - translation
  - Elixir
  - programming-language
layout: layouts/post.njk
---

함수형 언어 Elixir와 웹 프레임워크 Phoenix는 최근해외에서 크게 인기를 끌고 있는 핫한 놈들입니다. Elixir는 Ruby, Clojure, Erlang의 영향을 받았으며 Erlang의 가상 머신에서 돌아갑니다. Erlang이 가진 함수형 언어의 이점을 가져 오면서 프로그래머가 코딩하는 데 불편한 여러 포인트를 보완한 신생 언어입니다. 2011년에 처음 선보였지만 함수형 언어로 프로그래밍 언어 패러다임이 넘어가는 트렌드에 맞춰 빠르게 성장하고 있습니다. Elixir가 이처럼 재빨리 유명해진 이유에는 Phoenix의 역할도 큽니다. Phoenix는 Ruby On Rails의 코어 개발자가 만든 서버 사이드 웹 프레임워크입니다. ROR의 저조한 성능에 실망한 개발자들의 강력 대안(*Phoenix 1.3 버젼 들어와서 ROR의 패턴과 점점 멀어지고 있긴 합니다.*)으로 떠오르며 최근 유명세를 얻고 있습니다. Reddit에 올라온 2017년에 새로 배우고 싶은 기술 스택이 무엇이냐는 질문에 절반 이상이 Elixr와 Phoenix를 선택하기도 했습니다. 그만큼 엄청난 포텐셜을 가진 조합입니다. 저도 아직 초보 Elixir 개발자지만 배울수록 신기하고 배울 맛 나는 감질나는 언어입니다. 이 글은 Elixir에 대한 overview로 적합하다 생각해 번역했습니다. 원문은 [Adrian Philipp](http://adrian-philipp.com/about/)의 [Why the Elixir language has great potential](http://adrian-philipp.com/post/why-elixir-has-great-potential)입니다.

<hr>

지난 몇 주간 저를 바쁘게 한 기며술이 있습니다. 바로 Elixir 언어입니다. 제 주력 언어는 PHP와 Javascript입니다. 동적이고 함수적이고 불변하고 동시성과 패턴 매칭을 지원하는 프로그래밍 언어를 배우는 일은 mind bending이었습니다. Elixir는 모던 언어일 뿐만 아니라..

### 1. Erlang으로 컴파일된다.
치열한 검증을 거쳐온 Erlang VM에서 돌아가는 선택은 탁월했습니다. 이미 동시성, 분산, falut tolerance을 이미 지원하기 떄문입니다. Elixir을 배우기 전까지 Erlang VM이 얼마나 다재다능하고 기술 스택을 간단화시키는지 알지 못했습니다.

### 2. 라이브러리의 질
For all my needs so far, I found some library. 라이브러리의 질과 성숙함은 절 놀라게 했습니다. 웹 프레임워크 Phoenix, 데이터베이스 추상화 라이브러리 Ecto, GraphQL 라이브러리 Absinthe. 더 많은 라이브러리 패키지 매니져 hex에서 찾을 수 있습니다.

### 3. 확상을 위한 Path
premature 최적화는 비쌉니다. 빠르고 더러운 작업에 위한 재작성도 마찬가지입니다. Message passing과 process 떄문에 큰 어플로의 확장은 쉽습니다.

### 4. Reliability and resilience
다른 언어의 process와는 다르게 Erlang의 process는 죽어도 모든 프로세스를 스탑시키지는 않습니다. 만약 프로세스가 죽으면 supervisor가 살려냅니다. supervision 트리에서 어플을 관리하면 프로세스가 죽어도 잘 돌아가는 reliable 시스템을 만들 수 있습니다. Erlang은 [Let It Crash](http://verraes.net/2014/12/erlang-let-it-crash/) 철학을 표방합니다.


### 5. 성능
Elixir/Erlang의 성능에 만족하고 있습니다. 
[수백만분의 일초의 리스펀스 타임](https://medium.com/@Pinterest_Engineering/introducing-new-open-source-tools-for-the-elixir-community-2f7bb0bb7d8c)과 [2백만개의 웹소켓 하나의 서버에 연결하기](http://www.phoenixframework.org/blog/the-road-to-2-million-websocket-connections)을 읽다보면 성능 관련해서는 거정할 필요가 전혀 없습니다. 
특히 PHP와 비교하면 Elixir의 성능은 더욱 두드러집니다. 엄청 많은 웹소켓 커넥션도 무난히 견디는 짱짱은 웹과 모바일 유저에게 색다른 UX를 제공합니다. Elixir와 GraphQL의 콜라보가 기다려집니다.

### 6. 기업에서 적용
비록 Elixir가 신생 언어지만 Pinterest, Bleacher Report, Brightcove 등등의 곳에서 사용하고 있습니다.


## Elixr을 배우는 팁
그동안 객체 지향 언어만 배워왔다면 그동안 익힌 배턴을 버리기 쉽지 않았습니다. 하지만 충분히 가치있는 일입니다. 어떻게 시작해야 하나요?

전 screencasts의 골수팬입니다. 코드를 보는 동시에 설명을 듣는 콜라보를 굉장히 좋아합니다. 운 좋게도 Elixir와 Phoenix에 대한 좋은 screencast 시리즈가 있습니다.

## The Elixir Language
> Q. 왜 함수형 언어를 쓰는 개발자는 자녀를 홈스쿨링 시킬까?
> A. 왜냐하면 class를 싫어하므로 ㅋㅋ;;

Elixir의 창시자는 잘 알려진 루비 덕후 Jose Vlim입니다. Elixir는 Ruby에 영향을 받아기 때문에 가독성이 높습니다. 이 포스트에선 언어 스펙을 자세히 다루지는 않겠습니다. 이미 많은 사람들이 저보다 훨씬 훌륭하게 다뤘습니다.

제가 강조하고픈 Elixir의 유용성은 pipe 연산자입니다. |>는 unix에서 처럼 작동하며 함수 A의 리턴 값을 함수 B의 매개변수로 넣어줍니다. `b(a())` 요렇게 쓰는 대신 `a() |> b()` 요렇게 쓸 수 있습니다.

이외에도 마음에 드는 점이 있습니다. 패턴 매칭이라던가.. Stream 모듈이라던가.. 매크로나 프로토콜 같은 extensibility라던가.. 마크다운 포맷을 위한 문서화라던가..(진짜 신세계..) 

## Elixir의 미래?
누가 알겠어요. 제가 판단하기로 Elixir는 신생 언어지만 주류가 될 수 있는 잠재성이 있습니다. Elixir를 쓰면 빠르게 믿을 수 있는 높은 생산성의 동시성 어플리케이션을 만들 수 있습니다. 전 2017년에 적어도 몇 가지 소규모 프로젝트에 Elixir를 사용할 계획입니다.
