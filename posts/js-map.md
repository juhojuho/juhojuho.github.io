---
title: 내맘대로ES6 > Map
date: 2017-03-30 13:25:24
tags: 
    - Javascript
    - ES6
    - Map
layout: layouts/post.njk
---

데이터 구조 수업을 듣다 보면 Map은 반드시 나온다. 그만큼 많이 쓰이고 유명하다. 옆 동네 Python에서 Map을 쓰는 빈도를 봐도 알 수 있다. 하지만 안타깝게도 Map은 ES6 전까지 자바스크립트에 존재하지 않았다. 대신 Object에 몇 가지 꼼수[^1]를 부리며 그 빈자리를 달랠 뿐이었다. 이번 포스트에서는 ES6에 새롭게 추가된 데이터 구조 Map에 대해 알아보자.

## Google Map 할 때 그 Map?
Map은 Object와 비슷하게 key와 value를 쌍으로 저장한다. 이때, key는 중복되지 않는다. 만약 이미 저장된 key가 또 들어오면 기존 value를 새 값으로 덮어쓴다. `set(key, value)` 함수로 key와 value를 저장하고 `get(key)`로 그 key와 연결된 value를 반환한다. 그리고 `new Map()`으로 Map을 생성한다. `new`를 쓰는 거로 보아 Map의 타입이 Object임을 알 수 있다. 마지막으로 삭제는 `delete(key)`로 하며 `clear()`로 모든 key, value 쌍을 지운다. 아래 코드에 기본적인 Map 사용법이 나와 있다.

```javascript
var myMap = new Map();
 
typeof myMap // "object"
 
myMap.set('name', 'Tom');
myMap.set('age', 15);
myMap.set('name', 'Sam');
 
myMap.get('name'); // "Sam"
myMap.get('age'); // 15

myMap.delete('name') // true
myMap.get('name') // undefined

myMap.clear()
myMap.get('age') // undefined
```

## Map과 Object 
여기까지 보면 Map은 Object와 상당히 유사하다. 그렇다면 왜 Map이 필요한 걸까? Map이 Object보다 나은 몇 가지 이유를 살펴보자.

Object의 key는 반드시 String이거나 Symbol이다. [앞선 포스트](https://juhojuho.github.io/2017/03/29/js-symbol/)에서 봤듯이 ES6에서 새로 추가된 타입인 Symbol은 Object의 key로 사용될 수 있다. 반면에 Map의 key는 모든 타입을 허용한다. Number, Boolean은 물론이고 Object까지 가능하다. 즉, Object에 포함되는 function이나 array도 key로 사용할 수 있다.

```javascript
var myMap = new Map();
 
var keyString = 'string key';
var keyObject = { key: 'object key' };
var keyFunction = function() { return 'function key' };
var keySymbol = Symbol('symbol key');
 
myMap.set(keyString, 'I am a string key');
myMap.set(keyObject, 'I am a object key');
myMap.set(keyFunction , 'I am a function key');
myMap.set(keySymbol, 'I am a symbol key');
 
myMap.get(keyString) // "I am a string key"
myMap.get(keyObject) // "I am a object key"
myMap.get(keyFunction); // "I am a function key"
myMap.get(keySymbol); // "I am a symbol key"
```
위 코드처럼 모든 타입이 Map의 key로 사용될 수 있다.

사소하지만 꽤 강력한 차이점이 하나 더 있다. Object는 크기를 구하는 built-in(이미 구현된) method나 property가 없다. `Object.keys(object).length`가 그나마 쉬운 방법이다. 하지만 time complexity가 linear하므로 Object가 커질수록 더 많은 시간이 소요된다. 반면에 Map은 `size` property로 크기를 단번에 구할 수 있다. 이 경우 `size` property가 수시로 갱신되므로 Map의 크기가 커져도 걸리는 시간에 영향을 주지 않는다.

```javascript
myMap.size // 4
```

## Map 가지고 지지고 볶기
Map에 저장된 key와 value를 쭉 늘여놓고 싶으면 `values()`와 `keys()` 함수를 사용하면 된다. 단, 이 두 함수는 iterator를 반환한다. 따라서 추가적인 작업이 들어가야 예쁘게 표현될 수 있다. 지금 단계에서 iterator를 몰라도 괜찮다. 다음 포스트에서 다룰 예정이다. 이제 아래 코드를 보자.

```javascript
var myMap = new Map();
myMap.set('a', 1);
myMap.set('b', 2);
myMap.set('c', 3);
 
myMap.values(); // MapIterator {1, 2, 3}
 
[ ...myMap.values() ]; // [1, 2, 3]
Array.from ( myMap.values() ); // [1, 2, 3]
```

`values()` 함수를 사용하면 Map에 저장된 모든 value가 iterator로 반환된다. iterator를 푸는 가장 원초적인 방법은 `for`문을 쓰는 것이다. 하지만 훨씬 더 손쉬운 방법이 ES6에 존재한다. Spread operator `...` 와 새로운 Array API `from()`을 쓰면 iterator를 array로 변환시킬 수 있다. 하지만 이 부분은 이번 포스트의 범위를 넘어가므로 다음에 다루겠다. 지금은 `values()`가 value의 iterator를 반환하고 이를 푸는 새로운 방법이 ES6에 등장했다는 정도만 알고 있자. `keys()`는 key의 iterator를 반환하며 나머지는 `values()`와 비슷하게 작용한다.

**Reference**
[Map - JavaScript | MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Map)
[ES6 In Depth: Collections](https://hacks.mozilla.org/2015/06/es6-in-depth-collections/)
[You Don't Know JS: ES6 & Beyond](https://github.com/getify/You-Dont-Know-JS/blob/master/es6%20%26%20beyond/ch5.md)

[^1]: 보통 Object에 연결된 prototype을 제거하기 위해 `Object.create(null)`을 이용한다. 만약 평범한 Object를 사용했다면 주렁주렁 달려 있는 property 때문에 Map에 key 삽입시 충돌이 일어날 수도 있다.
[^2]: 쉽게 말해서 C처럼 일일이 `free()` 안 해줘도 된다.