---
layout: default
title:  "[JS] 애니메이션"
date:   2017-07-18 12:00:00
categories: "Javascript"
---


## 복습
* var myFunc = Object.create(obj) 이런식으로도 가능
* 숙제 makeObject 만들어 보기
* obj.isProtoType(fun)

## 웹 UI 개발과 에니메이션
* jQuery 기반 플러그인도 있음.
* 우리나라는 network이 빠르므로 스와이핑을 많이씀. 외국은 주로 정적
* 60fps 가 이상적임. (유투브)
* 웹은 애니메이션을 하기 전에 이것이 좋은 UX 인가를 잘 판단하고 설계해야 함.
* CSS의 transition 속성으로 CSS 속성을 변경하거나 JS로 CSS 속성을 변경할 수 있음 (대체로 CSS로 변경하는 것이 빠름) 

## JavaScript 애니메이션 조작 

### setInterval() : 주기적으로 반복

```javascript
const interval = window.setInterval(()=>{
	console.log('현재시각은',new Date());
},1000); // 1000이 1초, 1초에 한번씩
```

* 제때 일어나야할 이벤트 callback 함수가 손실 될 수 있음 ( 많음 ).
* 정확한 시간에 잘 실행되지 않음.

### setTimeout() : 딜레이를 줄 때, 비 동기의 순서를 조정 가능(안 좋은 방법), 크게 좋은 방법은 아니지만 명시적으로 실행을 부르므로 interval 과는 다르게 재귀적으로 호출하면 같은효

```javascript
const showTime = () => {

	setTimeout(() => {
		console.log('현재시각은',new Date());
		const box=document.getElementById('box');
		showTime();
	},1000);	
}
showTime();
```

### requestAnimationFrame

```javascript
let count = 0;

const showTime = () => {

	if(count>=20) return;
	console.log('현재시각은',new Date());
	console.log(count);
	const box=document.getElementById('box');
	count++;
	requestAnimationFrame(showTime);
}
requestAnimationFrame(showTime);
```

```javascript
const button = document.getElementById('button');

button.addEventListener('click',function(e){
	let count = 0;
	let laf;

	const moveBox = () => {
		
		if(count===401) return;
		
		const box = document.getElementById("box");
		
		box.style.marginLeft=`${count}px`;
		box.style.transform = `translateX(${count}px) scale(${count/400}) rotate(${(count/400)*3600*5}deg)`;
		// if(count%2===1)box.style.backgroundColor="yellow"
		// 	else box.style.backgroundColor="skyblue"

		count++;
		laf=requestAnimationFrame(moveBox);
	}
	requestAnimationFrame(moveBox);

},false);
```

### Animations API
* getComputedStyle 로 하면 inline 말고 css 에 선언된 속성도 뽑아올 수 있음.
* 프론트 엔드 라이브러리가 너무 많음.
* 오래갈 수 있는 코드가 좋음
* 순수 자바스크립트 (low level) 에 대한 학습이 잘 되있어야 함.
* jQuery 는 5년뒤에 사라질 수도 있음.
* 프레임워크 장점 : 빠른 속도로 저수준의 개발자와 함께 프로젝트 진행가능
* GPU 가속을 이용하는 속성을 사용하면 애니메이션 처리가 빠르다.
 [NAVER D2](http://d2.naver.com/helloworld/2061385)


## makeObject 구현하기

```javascript
var healthObj = { showHealth : function() {
		console.log(this.name + "-" + this.lastTime)  
 	}
}

function makeObject(obj,proto){

	Object.keys(obj).forEach(function(key){ 
		obj[key] = {
			value : obj[key]
		}
    });

	return Object.create(proto, obj);
}

var myHealth = makeObject({
	'name' : "달리기",
	'lastTime' : "23:10"
}, healthObj);
```
