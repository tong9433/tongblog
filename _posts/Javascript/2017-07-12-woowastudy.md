---
layout: default
title:  "[JS] Event, Ajax"
date:   2017-07-12 12:00:00
categories: "Javascript"
---


## 과제리뷰
* 한 줄로 쓰면 디버깅이 어려운 점이 있음.
* `<script src ></script>` 를 head 에 넣는 것과 body에 넣는 것의 차이가 있음. body 맨 끝에 둘 것.
	1. Head 에 넣는 경우
	2. body에 넣는 경우

## 수업시작
* 파일로 안하고  `<script>` 태그 안에 소스를 둘 경우 캐시가 되지 않는다.
* `<script src=“파일위치“ ><script> `  이런식으로 코드를 구성할 것이다.


## Event
* 클릭, 키보드, 마우스, 스크롤, 화면 resize 등등
* element.addEventListener( 이벤트 타입, 이벤트 리스너, false);
* [Event reference MDN](https://developer.mozilla.org/en-US/docs/Web/Events) - Event 종류
* 이벤트 리스너의 매개변수로 이벤트 객체가 들어온다.
* DOM을 모두 파싱했다고 브라우저가 알려준다.(DOMContentLoaded)

```javascript
<script>
  document.addEventListener("DOMContentLoaded", function(event) {
    console.log("DOM fully loaded and parsed");
	
		//이안에 이벤트 함수를 둔다. 


  });
</script>
```

* google tab network 부분 : DOMContentLoaded 를 확인 할 수 있음. 리소스를 불러오는 그래프를 볼 수 있음.
* selectedTab를 추가

```javascript
selectedTab.classList.add('selectedTab');
```

* 실습) navigation 을 이용한 Tab 만들기 - EventDelegation

```javascript
  const navigation = document.getElementById('navigation')
    navigation.addEventListener('click', function (e) {
        const selectedTab = document.getElementsByClassName('selectedTab')[0]
        selectedTab.classList.remove('selectedTab')
        e.target.classList.add('selectedTab')
        const selectedSection = document.getElementsByClassName('eleDisplayShow')[0]
        selectedSection.classList.remove('eleDisplayShow')
        const tabId = e.target.getAttribute('id')
        const newSelection = document.getElementById('my_'+tabId)
        newSelection.classList.add('eleDisplayShow')
    })
```

* Ajax ( XMLHTTPRequest 통신 )
페이지 새로고침 없이 서버로 부터 데이트 통신을 하면 좋겠다는 생각에서 출발, 더 좋은 UX를 제공할 수 있는 기술 ( 캐시를 잘해두면 더 좋음), 주로 JSON 포맷을 사용
* XMLHttpRequest () 함수를 생성
* ajax() 함수의 이해 - 의외로 소스는 간단하다.

```javascript
function ajax() { 
var oReq = new XMLHttpRequest(); oReq.addEventListener("load", function() { 
var result = this.responseText;  
//ajax() 함수가 **먼저 반환** 되고 이 리스너는 이벤트 공간에 있다. 여기 callback 함수 내부에 결과값을 이용하는 코드를 넣어야 함. 계속 이 callback이 계속 비동기적으로 계속될 때 계속 생김 => 콜백지옥
});
oReq.open("GET", "http://www.example.org/example.txt"); oReq.send(); 
return result; 
}
```
this.responseText 는 json 파일이 string 값으로 온다. 그리고 `JSON.parse(this.responseText);` 함수를 이용해서 객체형태로 변환.


![사진]({{tong9433.github.io}}/image/woowastudy/woowa0712.png)


* Cross Domain 문제를 해결 할 수 있는 방법은 JSON 방식
* underscores 라이브러리를 활용해서 template 작업하기(Ajax 활용) - 외부라이브 경험하기

```javascript
document.addEventListener("DOMContentLoaded", function(event) {
	console.log("DOM fully loaded and parsed");
    const navi = document.getElementById('navi');

    titleAjax();
    navi.addEventListener("click",function(e){
    	const selectTab = document.getElementsByClassName('selectedTab')[0];
    	selectTab.classList.remove('selectedTab');
    	e.target.classList.add('selectedTab');
    	titleAjax();
    },false);


});


function titleAjax() {
	var oReq = new XMLHttpRequest(); 
		oReq.addEventListener("load", function(e) {
		var htData = JSON.parse(oReq.responseText);
		goExec(htData); 
	});
	const num =document.getElementsByClassName('selectedTab')[0].getAttribute('name');
	oReq.open("GET", "http://jsonplaceholder.typicode.com/posts/"+num);
	oReq.send(); 
}

function goExec(data){
 	var body = _.template("<h1> <%= title %> </h1> </br> <span> <%= body %> </span>");
	var sec= document.getElementById('sec');
    sec.innerHTML=body(data);
} 
```


* **코드 개선 작업 하기** 
1. 조건, 반복문에 해당하는 작업을 최대한 줄이기 
2. Event delegation 을 잘 써서 for문으로 돌렸던 이벤트를 지우세용. 뭔지도 알아오기
3. Ajax 재요청 쓰지 않고 하기.(캐쉬를 사용)

* handlebar 라이브러리를 자주씀.
 [JavaScript 예제로 확인하는 handlebars.js 사용 방법 ](http://programmingsummaries.tistory.com/381)


## Event Delegation 이란

```javascript
<ul id="parent-list">
	<li id="post-1">Item 1</li>
	<li id="post-2" >Item 2</li>
	<li id="post-3">Item 3</li>
	<li id="post-4">Item 4</li>
	<li id="post-5">Item 5</li>
	<li id="post-6">Item 6</li>
</ul>
```

* 다음과 같이 부모리스트와 자식 리스트들이 있을 경우에 자식리스트들은 종종 추가되고 삭제되는 경우가 있다. 이때 각각의 자식 element 들의 경우에 따라 다른 listener 혹은 동일한 listener 을 적용할때 사용한다.

```javascript
document.getElementById("parent-list").addEventListener("click", function(e) {
	if(e.target && e.target.id == "post-3") {
			//event 
	}
});
```

* 이벤트 딜리게이션은 이벤트가 발생되어야 하는 객체에 직접적으로 이벤트를 바인딩 시키는 것이 아닌 **객체 상위 요소에 이벤트를 할당하고 인자를 객체를 넘겨줌**으로서 실제 이벤트 타겟에 간접적으로 이벤트 바인드 효과를 주는 것을 말한다. 이는 javascript의 이벤트 할당이 메모리에 직접적으로 올라가게 됨으로 **반복적이고 과다한 이벤트 할당은 프로그램 성능적으로나 반복적인 코딩등에 문제**로 많이 사용되는 이벤트 바인딩 패턴.



[front-end 개발자 인터뷰 문제 - javascript 영역 ](http://insanehong.kr/post/front-end-developer-interview-javascript/)













