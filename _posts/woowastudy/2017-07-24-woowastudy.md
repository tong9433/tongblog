---
layout: default
title:  "[2017-07-24] JavaScript 주말간 과제 데모 + 리뷰"
date:   2017-07-24 12:00:00
categories: "WoowaStudy-Web"
---


## 배민프레시 API 사용 실습 : 만들어야 할 것 : TAB UI + SLIDE UI

* 화면이 로딩되면 Ajax를 호출해서 데이터를 보여주자
* DOM LOAD -> AJAX -> DATA binding template 작업
* 실제로는 서버에서 HTML 로 만들어서 보내줄 수도 있음.
* 템플릿 할 것의 타이밍과 개수를 잘 선정해서 실습할 것
* laze loading : 당장 필요 없는 것은 나중에 받아온다. vs 한번에 받아놓고 사용할 때 쓰기
* cache : 한번 받아온 것은 저장해서 사용
* 재사용 ( 중복 코드와 중복컴포넌트 )


## 주말간 과제  데모 발표 및 코드 리뷰

* 핸들바 헬퍼를 더 익혀야 할 것 같음.
* UI가 잘 만들어진듯 하고 호버효과도 섬세하게 잘 됨.
* domcontents loaded 는 한 곳에서만 하게 하자.
* Util 함수들은 object 하나로 만들어서 쓰는 것이 좋음
* 크롱님 : 우리가 짜는 법은 서버를 이용하지 않고 웹에서 작동하므로 좀 더 서버에 부담을 줄이고 속도를 빠르게 할 수 있음.
* 템플릿을 쓸 경우에만 Ajax로 가져오는 방식으로 함.
* 지금 우리가 하는 것이 싱글 웹 어플리케이션.
* `fetch('템플릿소스').then(resp=>{){  return resp.txt });` 이런식으로 템플릿을 가져옴.
* 위에 것이 promise pattern 을 씀 ( 동기적, 체이닝이 됨)
* fetch api는 ajax와 promise pattern 을 섞은 것.
* Mustache 도 핸들바와 비슷한 템플릿 랜더링 도구
* matches method 잘 사용하기 
` target.matches('li')` 이런 코드는 약간 위험한 코드
* constructor 가 너무 의미 없는 것만 있어도 안됨. 의미있는 변수를 매개변수로 받아야함.
* fetch 를 두 번 나열할 경우 어떤 fetch 가 먼저 사용되야 하는지 순서가 헷갈릴 수 있음.  그러므로 then 내부에 fetch를 하는 것이 좋음. then 안에 겹겹히 로직이 길어지는 경우도 자연스러운 경우임.
* 매번 가져오는 경우에는 항상 캐쉬를 할 것.
* precompile를 해놓으면 템플릿을 빨리 할 수 있음
* 업계에서는 html 템플릿화가 완료된 결과를 ajax로 받는 것이 빠르다고 판단했기에 많이 사용함.
* 실제로는 클래스 보다 prototype 을 이용한 방법을 많이씀.
* 즉시실행 함수 (function(){})(); 한번밖에 안쓰고 밖에서 접근 못하는 함수를 쓸 때 
* template 는 html 에 숨기는 것보다 여러 html에서 쓰일 수 있으므로 template을 따른 파일로 보관한 후에 확장성을 높일 것.
* 다른 해결방법을 찾는 것보다 왜 그게 안되는지를 알아가는 자세를 가질 것.
* `width:cal(90%-20px); `이런식으로도 가능 css 3 추가기능