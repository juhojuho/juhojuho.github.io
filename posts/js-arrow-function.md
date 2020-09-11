---
title: "[JS] Arrow Function"
description: "ES6를 한 번이라도 접해본 사람이라면 Arrow Function을 알고 있을 것이다. Arrow Function은 코드의 수를 획기적으로 줄이고 가독성을 높인다. 단순히 문법적 편리성 뿐만 아니라 this의 용법도 달라진다."
date: 2019-04-01
tags: 
    - JS
    - ES6
    - arrow-function
layout: layouts/post.njk
---

ES6를 한 번이라도 접해본 사람이라면 Arrow Function을 알고 있을 것이다. Arrow Function은 코드의 수를 획기적으로 줄이고 가독성을 높인다. 단순히 문법적 편리성 뿐만 아니라 `this`의 용법도 달라진다. 이번 포스트에서 자세히 알아보자.

본격적으로 들어가기 전에 간단한 퀴즈 2개를 풀어보자.

``` js
// Quiz1
var foo = {
    a: function() {
        console.log(this);
    }
}
foo.a() // What's the output?
 
// Quiz2
var bar = {
    b: () => {
        console.log(this);
    }
}
bar.b() // What's the output?

```
`foo.a()`와 `bar.b()`의 출력값은 무엇일까? 만약 첫 번째 문제의 답을 `a`로 구했다면 이 포스트를 읽기 전에 `this`의 용법을 다시 공부하길 추천한다. `foo.a()`는 `foo`를 출력한다. 두 번째 문제는 다소 어렵다. 만약 답을 맞힌다면 이 포스트를 읽지 않고 건너뛰어도 무방하다. 당신은 이미 Arrow Function을 마스터했다! 아마 `bar.b()`의 출력값을 `bar`로 예상했겠지만, 답은 `window`이다. 굉장히 놀랍지 않나! 이제부터 왜 그런지 Arrow Function에 하나씩 대해 알아보자.

## 넘나 편리한 것
가끔 자바스크립트를 쓰다 보면 간단한 로직을 위해 생각보다 많은 코드를 쓸 때가 있다.
```js
var arr = [1, 2, 3, 4, 5];
 
// Before ES6
arr.map(function(x) {
    return x * 2;
});

// ES6
arr.map((x) => x * 2)
```
배열의 모든 값을 2배 하는 간단한 코드다. 위와 아래 중 어떤 코드가 더 읽기 편한가. 물론 워낙 짧은 코드라서 둘 다 가독성이 좋다. 하지만 코드의 양이 많아질수록 syntatic sugar는 큰 도움이 된다. Arrow Function도 그런 면에서 함수를 정의하는 기존의 `function` 키워드의 대체재로써 ES6에 새로 추가되었다. Arrow Function을 쓰면 훨씬 간단하게 함수를 표현할 수 있다.

## 본격적으로 Arrow Function 활용하기

```js
var names = () => 'Harry';
var square = x => x * x;
var nothing = (x, y) => {x + y}
var printAll = (x, y, z) => {
    console.log(x);
    console.log(y);
    console.log(z);
}
 
names() // "Harry"
square(5) // 25
nothing(1, 2) // undefined
printAll('a', 'b', 'c') // logs a, b, c
```
Arrow Function은 `function` 키워드가 필요 없고 특정 조건에서 `return`이나 `{ }` 중괄호까지 생략할 수 있다. 단지 `=>` 키워드만 필요하다. `=>` 앞에는 매개변수가 있고 뒤에 함수의 본문이 위치한다.

매개변수는 기존의 방식처럼 `( )` 소괄호로 감싸면 된다. 단, 매개변수가 하나면 괄호를 생략해도 된다. 매개변수가 없거나 하나 이상이면 괄호가 필요하다.

body는 이전처럼 `{ }` 중괄호로 감싸면 된다. 단, body가 단 한 줄이면 생략할 수 있다. 여기서 조심해야 할 점이 있다. 중괄호를 생략하면 자동으로 `return`이 body에 삽입된다. 위 코드에서 `square` 함수가 그 예다. `return`이 없는 데도 `25`가 반환되었다. 따라서 어떤 값도 함수에서 반환하고 싶지 않으면 body가 한 줄이어도 중괄호를 생략하면 안 된다. 그 예로 위에서 `nothing` 함수는 중괄호를 삽입해 `25`가 아닌 `undefined`를 반환한다.

## 이제부터 더 중요해

Arrow Function의 역할은 단순히 코드 길이를 줄이는 게 아니다. Arrow Function을 사용하면 `this`의 용법이 달라진다.

```js
var foo = {
    bar: function() {
        setTimeout(function() {
            console.log(this);
        }, 100);
    }
}

foo.bar() // logs window
```

보통 `this`는 동적으로 정해진다. run-time에 결정되므로 코드만 봐서는 `this`가 어떤 값일지 모른다. 어떤 문맥(context)에서 `this`가 호출되는지 잘 살펴봐야 한다. 위 예처럼 callback function은 window가 호출하므로 `this`는 window를 가리킨다.

하지만 Arrow function을 사용하면 `this`가 정적으로 정해진다. 그래서 코드만 잘 살펴봐도 `this`에 어떤 값이 들어갈지 쉽게 유추할 수 있다. Arrow function에서 `this`는 한 단계 밖 scope의 `this`와 일치한다. 쉽게 말하면 `this`를 평범한 변수 취급하라는 얘기다. 즉, `this`를 Arrow Function 내부에는 존재하지 않는 변수여서 한 단계 위의 `this`를 가져온다고 생각하면 편하다. 마치 아래 코드에서 변수 `a`가 하듯이 말이다.

```js
function foo() {
    var a = "Hello World!";
    function bar() {
        console.log(a)
    }
    bar()
}
 
foo() // logs "Hello World!"
```

함수 `bar` 내부에 변수 `a`가 존재하지 않아 자바스크립트 엔진은 한 단계 위 scope(이 경우 함수 `foo`)에서 `a`가 있는지 찾는다. 따라서 함수 `foo` 내부의 `a`가 출력된다. Arrow Function 속에 있는 `this`는 변수 `a`와 비슷하게 작동한다. 이제 실제 예를 보며 `this` 찾기를 연습해보자.

```js
var foo = {
    bar: function() {
        setTimeout(() => {
            console.log(this);
        }, 100);
    }
}
 
foo.bar() // logs foo
```

위의 코드에서 `console.log(this)`의 한 단계 밖 scope는 `bar`이다. 즉, Arrow function의 내부에서 `this`는 `bar`에서의 `this`와 일치한다. `bar`에서 `this`는 `foo`를 가리키므로 Arrow function 내부에서 `this`도 `foo`를 가리킨다.

혹시 이해가 가지 않는다면 아래의 코드를 참고하자. 위의 코드를 그대로 풀어쓰면 아래의 코드가 나온다.

```js
var foo = {
    bar: function() {
        var self = this;
        setTimeout(function() {
            console.log(self);
        }, 100);
    }
}
 
foo.bar() //logs foo
```

위 코드는 우리가 흔히 callback function에서 `this`를 정의할 때 주로 쓰는 패턴이다. callback function을 누가 호출할지 예측할 수 없으므로 우린 `this`에 직접 값을 대입해준다. 이 경우 `bar`의 `this`를 변수 `self`에 담아 callback function으로 전해줬다. Arrow Function가 `var self = this;`를 대체한다고 생각하면 편한다. 

이제 맨 위의 퀴즈도 자연스레 풀 수 있을 것이다.
```javascript
// Quiz2
var bar = {
    b: () => {
        console.log(this);
    }
} 
 
bar.b() // logs window
```
`console.log(this)`가 속한 Arrow function의 한 단계 밖 함수는 `b`이다. `b`에서 `this`는 window(global object)를 가리킨다. 따라서 `console.log(this)`의 `this`도 window를 가리키게 된다. 

이런 `this`의 의미를 변화를 이해하지 않으면 우리가 의도했던 바와 다르게 프로그램이 동작할 수 있다. Arrow Function은 결코 문법적 편리성만 제공하지 않는다! 바뀌는 `this`의 용법 반드시 이해하자.

매번 느끼는 거지만 아는 것을 말로 풀어내기는 참 힘들다. 특히 코딩 관련 지식을 글로 풀어내긴 더 힘든 것 같다. 이번 포스트는 내가 봐도 독자가 이해하기 어려울 것 같다(ㅠ_ㅠ). 혹시 이해 가지 않은 부분이 있으면 꼭 댓글로 달아주길 바란다. 그 부분에 대해 더 자세히 설명해 주겠다.