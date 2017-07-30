---
layout: default
title:  "[2017-07-17] JavaScript 객체지향 개념, Airbnb Style"
date:   2017-07-17 12:00:00
categories: "WoowaStudy-Web"
---

## 객체지향 개념 살펴보기
* 가장 처음 DOM content load 하는 부분은 최소화 시키기
* 이름이 없는 함수 : 익명함수
* 콜백함수는 이름이 필요없다. 내가 실행하는 함수가 아니라 비 동기적으로 실행되는 함수 이므로
* 로직안에 `< h1> ~~~~<h2> <li>` 이런거를 두지말자. Handlebars 를 써서 html 내에 숨겨 두는 것이 좋음,
* function 안에 this 를 쓸 경우 bind 를 쓰자 , 함수도 객체이다.

[FoolyCooly :: JavaScript call, apply, bind 메서드](http://codepitcher.tistory.com/4)

* 지정된 this 값 및 초기 인수가 있는 주어진 함수의 복제본 ( 즉 this 가 바뀐 )
* ES6의 ()=> { } 를 사용하면 this 가 이미 바운딩이 되버린다….ㅋㅋㅋㅋ  외우는 건 좋지만 

## 에어비앤비의 스타일 가이드를 따라서 javascript 를 배우자
1. https://firejune.com/1794/Airbnb의+ES6+자바스크립트+스타일+가이드
2. https://github.com/airbnb/javascript

* 주석은 적당하게 쓰는 것이 좋다. 너무 많이 늘리려고 하는 것 보단 , 먼저 코드를 의도가 명확하게 드러낼 수 있도록 짠 후에 간단한 주석만 함수위에 적어 놓는 것이 좋다. 
* 자바스크립트는 mvc 가 구지 필요하지 않다. 지금은 조작만 하자

## Airbnb ES6 자바스크립트 가이드
1. 원시형은 그값을 직접 조작하자 ( string, number, boolean, null, undefined)
2. 참조형(complex type)은 참조를 통해 값을 조작한다.

```javascirpt
const foo = [ 1, 2]
const bar = foo;

bar[0] = 9;
```

