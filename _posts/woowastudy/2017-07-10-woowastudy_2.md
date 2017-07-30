---
layout: default
title:  "[2017-07-10] JavaScript 데이터 타입, 연산자, 변수, 디버깅, 배열.."
date:   2017-07-10 12:00:00
categories: "WoowaStudy-Web"
---




## 데이터 타입
* typeof  123 => “number” typeof [] =>”object”
* typeof 연산자로 기본타입을 확인 할 수 있음. ( toString.call() 로 더 자세히 확인 가능)
* 데이터 타입은 많다. 외울 필요는 없다.
* 자바 스크립트 object는 쉽다. 괄호로 키와 value로 된 구조
* undefined 와 null 라는 타입이 있다
* undefined: 값을 할당하지 않는 변수
* **false 로 취급될 수 있는 값** (외우는 게 좋음) : false , “”, 0 , NAN , null, undefined

## 연산자
1. or 연산자 활용 : 첫번째가 true이면 뒤에꺼를 판단하지 않음, 첫번째가 false 이면 뒤에 것을 쓰게됨. 보통 default 를 이와 같이 사용
```javascript
const result = “name” || “default” 
```
 

2.  3항 연산자 활용 : 
```javascript
var data = 11;
var result = (data > 10 ) ? "ok" : "fail";
console.log(result);
```
=> ok 출력
3. 비교는 ===를 사용 

![사진]({{ tong9433.gitub.io }}/image/woowastudy/woowa0710_2.png)

* === 을 쓰면 모두 false 가 나온다. 자바스크립트는 암묵적인 형변환을 일으키므로 ===을 쓰는 것이 좋다.


![사진]({{ tong9433.gitub.io }}/image/woowastudy/woowa0710_3.png)

* 외울 필요없다. 이상한 특징
* 자바스크립트 문법은 MDN 사이트를 신뢰하고 볼 수 있다.



## 자바스크립트 switch 문
```javascript
switch (expression) {
  case value1:
    //Statements executed when the result of expression matches value1
    [break;]
  case value2:
    //Statements executed when the result of expression matches value2
    [break;]
  ...
  case valueN:
    //Statements executed when the result of expression matches valueN
    [break;]
  default:
    //Statements executed when none of the values match the value of the expression
    [break;]
}
```



## 변수의 유효 범위
* 일단 헷갈리면 var만 쓰자
* 조금 안다 const를 쓴다.
* const를 쓰는데 헷갈린다. 그러면 let을 쓰면 된다.
* var는 block scope를 가지지 못함. let, const는 가능 ( 예를들면 for 문 안에 쓸 경우, var 같은 경우는 for( var ..) 써도 바깥에서 유효하다.)
* let은 지원되는 브라우저가 아직 별로 없다.
* ECMAScript6 = Es6  => ES5로 바꿔주는 도구를 쓰고 문법은 ES6을 쓰자. ES6은 현재 대부분 웹브라우저에서 지원이 잘 안된다. ES5는 거의 잘됨.
* const는 변수 재할당이 안됨, 그럴때는 let 사용 ( const a= 4 ;    a =  3 ( 안됨) )
* 함수내에 함수가 선언 될 경우 var printName= function(){ } 이런식으로 변수형태로 있다고 생각하면 된다.


## Debugging 하기 (구글 크롬 환경에서)
```javascript
function setName(lastName) {
  function printName() {
    const firstName = "youn";
    console.log('my name is', firstName + lastName);
}
  printName();
  console.log(firstName); //?
}
```
*  중간에  `debugger;`  이라는 코드를 삽입한다
* **call stack** 와 **scope** 가 중요, scope 는 Local, Global, Closer(부모) 에서 보여준다, call stack 는 함수가 어디로 부터 불러왔는지 그 함수를 쌓는 곳이다. 
* scope 와 call stack 를 잘 이용하여 debugging 를 할 수있다.
* [esc] 버튼을 누르면 디버깅 상태에서 console 창이 나옴. 

## 배열
* 배열 안에는 모든 데이터 타입이 들어갈 수 있음.
* MDN 사이트에 상당히 많음.
* foreach : 기존의 배열에 있는 원소를 하나하나씩 출력 하거나 어떤 함수를 부여할때 !
```javascript
var arr = [ '사과', '바나나', '파인애플' ];
arr.forEach(logArrayElement =(element, index, array) => {console.log('a[' + index + '] = '+element);});
```
* map : 기존의 배열 element에 다른 값으로 맵핑 시키는 역할.
```javascript
Array.prototype.newmap = function(func){ 
	var new_arr = [];

	for(let i = 0; i < this.length; i++){
		new_arr.push(func(this[i],i,this));
	}
	return new_arr;
}
```
```javascript
var arr = ["crong",1,"jk","honux"];
var dollor= arr.map(function(element,index,array){
	if(typeof(element)=="string") return element+"$";
	else return element;
});
dollor.forEach(logArrayElement =(element, index, array) => {console.log('dollor[' + index + '] = '+element);});
```
**map 처럼 동작하는 함수를 만들어오기 비슷하게 동작!** / 모든 배열 요소의 공백을 지울때 사용 가능
* fillter: 기존의 배열에서 true 인 요소만 반환하여 배열을 만듬.
```javascript
Array.prototype.newfilter = function(func){
	var new_arr = [];

	for(let i = 0;i < this.length; i++){
		if(func(this[i],i,this)==true) new_arr.push(this[i]);
	}
    return new_arr;
}
```
```javascript
var arr = ["", 0, "true", "jk", undefined, null, false];
var new_array = arr.filter(function(element,index,array){
   if(element) return true; 
});
new_array.forEach(logArrayElement =(element, index, array) => {console.log('new_array[' + index + '] = '+element);});
```
* prototype 을 이용해서 array의 native 메소드를 추가로 구현할 수 있다. 하지만 실제 프로젝트 할 때는 이런식으로 구현 하는 것은 권하지 않음.

## 느낌표 두개 !! 의 의미
* boolean으로 형 변환. 0을 false로, 1을 true로.

```javascript
var a = 1;
var result = "I'm a result";
 
if(a){
    console.log(result); // " I'm a result " 출력
    console.log(!result); // " false " 출력
    console.log(!!result); // " true" 출력
}
```

```javascript
let a = 0;
console.log(a);   // 0
console.log(!a);  // true
console.log(!!a); // false
 
let b = null;
console.log(b);   // null
console.log(!b);  // true
console.log(!!b); // false
 
let c = undefined;
console.log(c);   // undefined
console.log(!c);  // true
console.log(!!c); // false
```

