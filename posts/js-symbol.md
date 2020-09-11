---
title: "[JS] Symbol"
description: "이번 포스트의 주제는 Symbol이다. ES6를 배울 때 보통은 소외당하는 개념이지만 자바스크립트가 최초로 표준화된 1997년 이래 처음으로 추가된 새로운 타입인 만큼 자세히 알아보자."
date: 2020-03-29
tags: 
    - JS
    - ES6
    - Symbol
layout: layouts/post.njk
---

이번 포스트의 주제는 Symbol이다. ES6를 배울 때 보통은 소외당하는 개념이지만 자바스크립트가 최초로 표준화된 1997년 이래 처음으로 추가된 새로운 타입인 만큼 자세히 알아보자.

## 심벌즈 말고 Symbol
먼저 symbol을 사용한 코드를 보자.

```js
var sym1 = Symbol();
 
typeof sym1 // "symbol"
 
var sym2 = Symbol('password');
var sym3 = Symbol('password');
 
sym2 === sym3; // false
 
var sym4 = new Symbol(); // TypeError
 
var symObj = Object(sym)
typeof symObj // "object"
```

오! 몇 가지 신기한 결과가 나왔다. 하나씩 살펴보자.

우선 symbol은 literal하게 선언할 수 없다. string의 경우 `var str = "Hello, world!"`와 `var str = String("Hello, world!")` 두 가지 방법으로 변수를 선언할 수 있지만 symbol은 반드시 `Symbol()` 함수를 이용해야 한다. 

또한, 앞서 말했듯이 symbol은 ES6에서 새로 추가된 타입이다. 가끔 object에 속한다고 생각하는 사람들도 있는데 결코 아니다. 위 코드에서 `typeof`의 결과로 `"symbol"`이 나옴을 알 수 있다.

`Symbol()` 함수의 매개변수는 선택사항이다. `sym1`처럼 비워둬도 되고 `sym2`나 `sym3`처럼 값을 넣어줘도 된다. 여기서 질문! 그렇다면 symbol에서 매개변수의 역할은 무엇일까? 

매개변수는 그 symbol이 무엇인지 **설명**해준다. 여기서 많은 사람이 헷갈린다. 많은 사람이 매개변수가 그 symbol의 **값**을 의미한다고 생각한다. 결코 아니다. 다시 한번 강조하지만, 그 symbol에 대한 **설명**일 뿐이다. 다른 개발자가 `sym2`를 보고 "아, 패스워드와 관련된 symbol이구나" 정도만 알게 하면 되는 그 이상 이하의 역할도 아니다. 그래서 `sym2`와 `sym3`를 비교하면 `false`가 나온다. 단순히 symbol에 붙은 **설명**만 같을 뿐 완전히 서로 다른 symbol이기 때문이다. symbol은 선언된 순간 세상에서 유일한 존재가 된다. `sym2`와 `sym3`가 다르듯이 매개변수가 같은 symbol을 포함한 그 어떤 것과도 같지 않게 된다. 이에 대해서는 뒤에서 좀 더 다루겠다.

마지막으로 symbol을 선언하는 데 `new`를 쓰면 `TypeError`가 뜬다. 즉, `new`를 이용해 object로 'boxing'이 불가능하다. 다른 primitive 타입의 경우 `var year = new Number(2017)`처럼 `new`를 사용하면 타입이 object로 변환(coercion)된다. 하지만 symbol의 경우에는 object로 변환이 허용되지 않는다. 하지만 하늘이 무너져도 솟아날 구멍은 있는 법! `Object()` 함수를 이용하면 강제로 object로 변환시킬 수 있다. 위 코드의 가장 아랫줄을 보면 `symObj`의 타입이 "object"로 변했다.

## 그래서 어디다 쓰는 건데

결론부터 말하자면 symbol은 object의 property를 숨기는 데 쓰일 **예정**이었다. 아래의 코드를 보자.

```js
var obj = {
    id: 1234,
    name: "juhojuho",
    [ Symbol("password") ]: "E9HC21"
};
 
for (var key in obj) {
    console.log(key); // logs "id" and "name"
}
 
Object.keys( obj ); // ["id", "name"]
 
Object.getOwnPropertySymbols( obj ) // [Symbol(password)]
```

이번에도 역시 신기한 결과가 나왔다.

`obj`를 보면 `password`의 타입이 symbol이다. 근데 `id`와 `name`과는 다르게 `Symbol("password")]` 좌우로 괄호가 있다. 둘러싸고 있는 `[ ]`는 ES6에 새로 추가된 'Computed Property' 기능인데 이는 추후 포스트에서 자세히 다루겠다. 지금은 걱정하지 말고 일단 넘어가자.

이제 앞서 말했듯이 symbol로 된 property가 잘 숨겨졌는지 알아보자. 우선 `for..in`를 사용하면 보이지 않는다. `Object.keys()` 함수로도 나오지 않는다. 실제로 웬만한 방법으로는 symbol로 된 property를 찾기 힘들다. 하지만 안타깝게도 특별한 함수로 숨겨진 property를 찾을 수 있다. `Object.getOwnPropertySymbols()` 함수를 사용하면 숨겨진 property가 보인다. 위 코드의 맨 아랫줄에서 `[Symbol(password)]`가 나옴을 확인할 수 있다. 결국 '예정'이었을 뿐 ES6에서 property를 숨길 수는 없다.

그럼 뭐지, 결국 symbol은 쓰잘데기 없는 것인가? 대답은 '아니요'다. symbol을 굳이 ES6에 추가한 데는 다 이유가 있다! symbol을 사용하면 property 간에 충돌을 피할 수 있다. 앞서 말했듯이 symbol은 그 자체로 유일하다. 심지어 `Symbol('password')`와 `Symbol('password')`조차도 다른 값임을 기억하자. 따라서 symbol을 이용하면 object에 중복된 property를 추가할 때 생기는 충돌을 피할 수 있다. 아래의 경우처럼 말이다. 매개변수가 같은 세 symbol 모두 충돌 없이 `obj`에 저장된다.

```js
var obj = {};
 
obj[Symbol('name')] = "Moon"
obj[Symbol('name')] = "Ahn"
obj[Symbol('name')] = "Lee"
 
obj // Object {Symbol(name): "Moon", Symbol(name): "Ahn", Symbol(name): "Lee"}
```

그런 경우가 실제로 있냐고 반문할 수 있다. 하지만 은근히 많이 일어나는 일이다. 예를 들어 철수가 웹 프로그래밍을 하면서 DOM element의 상태를 추적하고 싶다고 상상해 보자. 철수는 element가 유효한지 체크하기 위해 `isValid`라는 property를 모든 element에 추가했다. 흠, 보기에는 문제없다. 하지만 철수의 코드만이 DOM element를 다루는 게 아니다. 철수가 참조한 다른 라이브러리의 코드도 똑같이 DOM element를 건드린다. 철수가 추가한 `isValid` property 때문에 외부 코드가 `for..in`이나 `Object.keys()` 함수를 쓰면서 예상치 못한 결과를 맞을 수 있다. 또한, 외부 코드가 철수와 똑같이 `isValid` property를 추가할 수도 있다. 최악의 상황에 철수가 사용하는 브라우저가 추후 업데이트로 `isValid`를 모든 DOM element에 추가하면 property 간에 충돌이 일어나 문제가 발생할 수 있다. 따라서 내 코드 뿐만 아니라 다른 코드도 한 object를 다루는 상황에서 symbol이 유용하게 사용될 수 있다. 물론 이 뿐만 아니라 다른 값과 중복되지 말아야 할 고유값이 필요한 모든 경우에 symbol을 쓸 수 있다.

## 한 발자국 더

간혹 한 symbol을 여러 번 사용해야 하는 경우가 있다. 혹은 여러 자바스크립트 파일이나 모듈에서 symbol을 공유해야 하는 경우도 있다. 다행히 symbol을 **symbol registry**에 등록하면 이 문제가 해결된다. `Symbol.for()` 함수를 이용하면 symbol registry에 등록할 수 있고, 나중에 여러 번 불러 쓰는 것도 가능하다. 불러올 때도 똑같이 `Symbol.for()` 함수를 쓰면 이전에 등록했던 symbol이 반환된다. 아래 코드를 참조하자.

```js
var sym1 = Symbol.for('foo');
var sym2 = Symbol.for('foo');
 
sym1 === sym2 //true
```

초반에 `Symbol()` 함수의 매개변수가 단지 *설명*일 뿐이라 했는데 사실 한 가지 기능이 더 있다. 바로 symbol registry에서 매개변수는 일종의 key로 작용한다. `Symbol.for('foo')`을 여러 번 호출해도 매번 같은 symbol이 나오는 이유다.

마지막으로 자바스크립트에는 여러 개의 built-in(이미 구현된) symbol이 있다. 모두 `Symbol()` 함수의 property에 달려 있다. 언젠가 다룰 `Symbol.iterator`를 포함해 각자 ES6에서 중요한 역할을 맡고 있다. 지금 당장 하나씩 설명하기보단 나올 때마다 차근차근 설명하겠다.

진짜 마지막으로 자바스크립트 명세(specification)를 보면 `@@`가 보이는데, 이게 바로 built-in symbol을 나타내는 표시다. 골뱅이가 두 개라고 놀라지 말자.

이 정도면 symbol에 대해 충분히 다뤘다고 생각한다. 혹시 더 궁금한 게 있다면 아래 레퍼런스를 참조하길 바란다. 

**Reference**
[Symbol - JavaScript | MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Symbol)
[ES6 In Depth: Symbols](https://hacks.mozilla.org/2015/06/es6-in-depth-symbols/)
[You Don't Know JS: ES6 & Beyond](https://github.com/getify/You-Dont-Know-JS/blob/master/es6%20%26%20beyond/ch2.md)
