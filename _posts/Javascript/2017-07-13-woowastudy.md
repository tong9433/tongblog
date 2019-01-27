---
layout: default
title:  "[JS] 캡쳐링, 버블링, Ajax 캐시, JS 객체, 핸들바JS, Prototype"
date:   2017-07-13 12:00:00
categories: "Javascript"
---


##  Event 캡쳐링, 버블링
* 일단 겹쳐있을 경우 이벤트가 모두 다 생성된다.
* Listener의 3번째 인자가 true 이면 캡처링, false 이면 버블링
* 캡처링 : 이벤트가 발생한 요소를 포함하는 부모 HTML부터 이벤트의 근원지인 자식 요소까지 이벤트를 검사
* 버블링 : 이벤트도 자식 요소로부터 부모 요소로 올라오며 실행된다고 하여 이벤트 버블링(Event Bubbling)
* `event.stopPropagation();`
* 예제

```javascript
var a=document.getElementById('a');
var b=document.getElementById('b');
var c=document.getElementById('c');

a.addEventListener("click",function(){
  console.log("a");
},false);


b.addEventListener("click",function(){
  console.log("b");
},false);

c.addEventListener("click",function(){
  event.stopPropagation();
  console.log("c");
},false);

var c = document.querySelectorAll(div);
var arr = Array.from(c);
```


## Ajax 캐시 하는 방법
1. Local storage : 문자열만 보관가능, 속도가 빠름, 보통 쿠키 저장, 이것도 두가지 종류인데 날아가는 것과 안날아가는 것이 있다. 브라우저가 보관
2. Object 형식으로 보관
* 섹션을 하나로 두는 것보단 4개로 두어 돔조작을 최소화 하는 것이 좋을 것 같음.
* 배달의 민족 사이트 처럼 식당의 개수가 계속 늘어나는 경우 돔을 늘이는 것보단 사실 android recyclerView 처럼 구현하는 것이 더 좋을 것.

[꿀벌개발일지 :: HTML5 LocalStorage 살펴보기](http://ohgyun.com/417)


* 전역변수가 많으면 문제점
 
  1. 리팩토링 하기가 힘들어짐.
  2. 함수 변수 이름 붙이기가 힘들어지고 이 변수가 어떻게 사용되는지 알기 힘듬.

* 추가와 변경 및 리팩토링 즉 유지보수가 간단하도록 코드를 짜는 것이 좋다. 
* 또 데이터와 로직을 분리해야함. 여기서 데이터는 변할 수 있는 데이터를 의미함.

## 서버 렌더링 <-> 클라이언트 렌더링 
* 둘 중에 UX가 좋은 것을 택해야 함.
* 서버 렌더링: 서버단에서 완성된 html 을 만들어 보내주는 것.
* 클라이언트 렌더링: json 파일 등을 받아 클라이언트에서 템플릿화 시켜서 html에 입력하는 것

## handlebar(백엔드와 클라이언트에서 사용, node.js) 를 이용해서 템플릿 처리
[Nonblock: Handlebars (for Java) 서버, 클라이언트 동시에 사용하기](http://blog.javarouka.me/2014/08/handlebars-for-java_31.html)


```javascript
function blogAjax() {
	var oReq = new XMLHttpRequest(); 
	
	oReq.addEventListener("load", function(e) {
		var blogData = JSON.parse(oReq.responseText);
		setBlogData(blogData);
	});
	oReq.open("GET", "http://localhost:8000/Desktop/lotto/public/data.json");
	oReq.send(); 
}

function setBlogData(data){
	var myTemp = document.getElementById('myTemplate').innerHTML;
	var sec = document.getElementById('sec');
	var context = data;
	var template = Handlebars.compile(myTemp);
	sec.innerHTML=template(context);
}

```

* handlebars.helper 를 이용해서도 사용가능하다.
* 1) script 태그에 보관 type에 엉뚱한 문자열을 쓰면 브라우저는 javascript 를 해석하지 않고 그냥 무시 / 2) 서버에서 미리 html을 만들어서 ajax() 로 가져올 수 있음 ( 서버사이드 렌더링)  / 3) 최악은 : ajax 없이 페이지를 통채로 주는 것.
* 템플릿 관리를 안하는 것은 ajax를 안쓰는 사이트, ajax 를 쓰면 보통 템플릿을 써서 사용한다.
* react, angular 가 client 렌더링에 초점이 맞춰진 유용한 프레임워크: data 받아서 버무리고 화면에 뿌리기 ( 오늘 한것,  첫 화면 로딩이 느리다) = > 첫 화면 로딩을 서버에서 하는 경우도 많음.
* 서버사이드 렌더링을 지원하는 프레임워크는 좋은 것 (angular react view)
* 대부분 홈페이지들을 뜯어보면 템플릿화해서 서버에서 ajax 로 html 묶음을 보내준다.
* 프리컴파일, 템플릿 최적화 알아보기 , 웹팩: 빌드(서비스를 배포하기 전에 자바스크립트 완성된 함수를 소스에 넣고 컴파일)


## 자바스크립트 객체
* 자바스크립트에서는 객체를 쉽게 만들 수 있다.
* 객체 안에서의 this는 그 객체 자신을 가리킴.
* this 는 실행될 때 결정됨. this 가 예상되지 않게 결정되므로 어렵다. ( 디버깅을 해서 그 때 가르키는 this 가 뭔지 파악)
* 의미있는 기능 을 하나의 객체로 만들 수 있음. 그냥 기능 자체를 아래와 같이 짜는 경우도 아주 많음

```javascript
  var healthObj = {
	name : "tongil",
	lastTime : "PM 10 : 12",
	showHealth : function() {
		console.log(this.name+" - " + this.lastTime);
	}

}
healthObj.showHealth();
``` 

* 하나의 지역변수 전략 ? namespace 패턴 알아보기
 [Web Club :: 자바스크립트 네임스페이스 패턴 - namespace pattern](http://webclub.tistory.com/311)

* new  생성자로 함수를 만들어야 객체가 만들어짐

```javascript
function myfunc(){
	return this;
}

var func = new myfunc();
console.log(typeof(func));//object반환
console.log(myfunc);//window 반환
```

## Prototype
* 모든 객체는 prototype 이라는 키 : 같이 share 할 수 있는것은 share 하자는 의미로 가지고 있음
* 객체.prototype = 객체 이런방식으로 함수를 추가할 수 있음.

```javascript
function Health(name, lastTime){
	this.name = name;
	this.lastTime = lastTime;
}

Health.prototype = healthObj = {
	showHealth : function(){
		console.log(this.name + " is name , time is " + this.lastTime);
	}
}; // var healthObj = { } 로 따로 두어도 됨.

var func = new Health("tongil","2pm");
var func1 = new Health("woori", "3pm");
var func2 = new Health("sowon", "2am");
console.log(func.showHealth());
console.log(func1.showHealth());
console.log(func2.showHealth());
```

* prototype 체인 : proto 는 prototype 객체를 표현한 것 , 아래로 내려가는 것처럼 보이지만 최상위 Object로 타고 올라가는 것. 
* 마지막 **Object** 진짜 최상위 Object 이다. (지금 배운 객체와는 약간 다른 object) 이 chain의 모든 함수는 최상위 Object 까지 연결된다.

![사진]({{tong9433.github.io}}/image/woowastudy/woowa0713.png)

* 자바스크립트 상속은 prototype을 이용하여서도 가능.
* 왜 prototype? 생성자를 통해 생성된 객체들이 여러개 있어도, prototype에 연결된 객체들은 동일한 메모리 공간에서 효율적으로 같이 쓸 수 있음. 즉 아래와 같은 결과가 나옴

```javascript
func.__ proto __===myHealth2. __ proto __ // true 를 반환
```

그러므로 이런 prototype을 인터페이스 같은 형식으로 함.

* ES6 에 Class 라는 키워드가 생김. 좀더 쉽게 클래스를 구현. 
* 프로토타입을 따로 구현 하는 것이 좀 더 편해짐. 하지만 방금 전까지 배운 prototype 코드 들이 순수한 본연의 JS 객체의 모습. 
* ES6 Class가 앞으로의 표준이 될 것. 
* 바벨이라는 모듈로 ES6 코드를 ES5로 전환 가능. 그러므로 앞으로는 이와 같은 syntax를 쓰는 것을 권장함.
* 다시 한번 짜기. TabUI 
* 어디에나 쓸 수 있는 재 사용이 가능한 Tab component 만들기 ( 끝판왕 )