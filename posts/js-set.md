---
title: "[JS] Set"
description: "Set은 Map과 마찬가지로 ES6에 들어 새로 추가된 built-in object 중 하나이다. Set이란 이름답게 우리가 고등학교 1학년 수학시간 첫 단원에서 배웠던 집합과 기능이 유사하다. 집합의 가장 큰 특징이 무엇인가. 집합의 원소들은 서로 다르며, 같은 원소가 여러 개 있을 수 없다."
date: 2020-07-24
tags: 
    - JS
    - ES6
    - Set
layout: layouts/post.njk
---

`Set`은 `Map`과 마찬가지로 ES6에 들어 새로 추가된 built-in object 중 하나이다. "Set"이란 이름답게 우리가 고등학교 1학년 수학시간 첫 단원에서 배웠던 집합과 기능이 유사하다. 집합의 가장 큰 특징이 무엇인가. **집합의 원소들은 서로 다르며, 같은 원소가 여러 개 있을 수 없다.** JS의 `Set`의 특징 역시 여기서 크게 벗어나지 않는다.

## 10-가 1단원 집합
`Set`은 수학의 집합과 같이 중복된 값을 포함할 수 없다. 대신 `Set` 속에 존재하는 값이 아니라면 어떠한 값도 그 안에 새로 포함될 수 있다. `String`, `Boolean` 같은 primitive value는 물론이고 `Array` 등의 ojbect 기반의 값도 가능하다. `Set`에 새 값을 더할 때는 `.add()`를 사용하자.

```js
const newSet = new Set();
newSet.add(123)
newSet.size // 1
newSet.add(123)
newSet.size // 1
newSet.add({id: 4})
newSet.size // 2
newSet.add({id: 4})
newSet.size // 3
```

앗! 혹시 위 결과가 이상하다고 느꼈는가. 아까 분명히 `Set` 안에 중복된 값이 못 들어간다고 했는데 ojbect `{id: 4}`가 두번 들어갔다. 이는 `Set` 안에 해당 object를 가리키는 주소값(reference)가 저장되기 때문이다. 비록 내용은 같을 지라도 각각 다른 주소값을 가지기 때문에 `Set`에 문제 없이 추가되었다. `{a: 1} == {a: 1}`의 결과가 `false`인 것과 같은 이치다.

## Set 제대로 활용하기
`Set`을 다루기 위한 method는 다음과 같다.
- `new Set`: 빈 `Set`을 생성한다.
- `new Set(iterable)`: 빈 `Set`을 생성하고 파라미터로 받은 iterable ojbect 속 값을 중복되지 않게 채워넣는다. 만약 array에 있는 중복된 값을 제거하고 싶을 때, 이 방법을 사용하면 단 한 줄로 원하는 결과를 얻을 수 있다.
- `set.size`: `Set`에 속한 값의 수를 반환한다.
- `set.has(value)`: 만약 파라미터로 받은 value가 `Set`에 속하면 `true`를 반환한다.
- `set.add(value)`: 파라미터로 받은 value을 `Set`에 추가한다. 만약 이미 존재하면 추가하지 않는다.
- `set.delete(value)`: 파라미터로 받은 value가 `Set`에 포함되어 있으면 제거한다.
- `set.keys(), set.values(), set.entries()`: `Map`의 그것과 동일하다. [Map 포스트](https://juhojuho.github.io/posts/js-map/) 참고

## 한 가지만 더 알아보자!
한 가지 의문점이 든다. `Set`에서 말하는 "동일한 값"의 판단 기준은 무엇일까? Boolean type `true`와 String type `"true"`는 같을까? 정답은.. 아니요! 기본적으로 `===` operation과 비슷하다고 생각하면 된다. `===`는 우선 값을 비교하고, type도 같은지 비교한다. 따라서 위 경우는 값은 같으나 type이 다르기 때문에 한 `Set` 안에 들어갈 수 있다. 사소한 팁으로 primitive type 중 하나인 NaN을 서로 비교하면 `===` 결과 서로 다른 값이라 판단되지만 `Set` 안에서는 같은 값이라 판단되어 단 하나만 추가된다. *(진짜 사소하다..)*

```js
const numSet = new Set("1") // {"1"}
numSet.add(1) // {"1", 1}
numSet.add(true) // {"1", 1, true}
numSet.add("true") // {"1", 1', true, "true"}
numSet.add(NaN) // {"1", 1', true, "true", NaN}
numSet.size // 5
numSet.add(NaN) // {"1", 1', true, "true", NaN}
numSet.size // 5
```
  
**Reference**
[Set - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)
[ES6 In Depth: Collections](https://hacks.mozilla.org/2015/06/es6-in-depth-collections/)