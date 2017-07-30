---
layout: default
title:  "[2017-07-25] JavaScript 모바일 웹"
date:   2017-07-25 12:00:00
categories: "WoowaStudy-Web"
---

## 모바일 웹 
* 사실 수요가 많이 없음.
* 웹 서비스를 하는 경우 모바일 웹을 하는 것이 좋음.
* 모바일 웹을 먼저 만들어야 하나 PC 웹을 먼저 만들어야 하나 선택사항 복잡한 부분을 먼저해야 하나 간단한 걸 구현하고 하나씩 개발해야하나.. 주어진 자원을 가지고 잘 선택해야 하는 부분.
* 크롱님: 개인적으로는 서비스의 핵심과 본질을 찾는 것이 중요.  모바일을 먼저 개발하는 것도 나쁘지 않음. 현실적으로는 PC를 먼저 하는 경우가 많긴함.
* 모바일 웹 개발을 앞두고 고민 
1. 디바이스
2. 이벤트 방식

* viewport 사용
`<meta name=“viewport” content=“width=device-width,initial-scale=1.0">`
=> 이 설정을 하면 모바일에 맞게 나옴.  HTML head 태그안에 추가

![사진]({{tong9433.github.io}}/image/woowastudy/woowa0725.png)

* 원격으로 모바일폰으로 디버깅하기
 [Android 기기 원격 디버깅 시작하기  |  Web       |  Google Developers](https://developers.google.com/web/tools/chrome-devtools/remote-debugging/?hl=ko)
* Touch Event : 마우스 이벤트가 없지만, 대신 tocuh 이벤트가 있음.
* 클릭보다 touch를 쓰는게 빠르므로 touch를 써야함.
* 이벤트 순서:touchstart -> mousedown -> click

```javascript

const element = document.getElementById('test');

element.addEventListener('touchmove',function(e){
    console.log("touchmove");
    console.log(e.changedTouches)
})

element.addEventListener('touchstart',function (e) {
    console.log("touchstart")
})

element.addEventListener('click',function () {
    console.log("click")
})

element.addEventListener('mousedown',function(){
    console.log("mousedown")

})
```

* 안드로이드 2개 ios 2개 정도는 구비하고 모바일 웹을 테스트 해야함.
* 네이버에서 제공하는 컴포넌트들은 모드 기기들에서 이미 테스트를 거친 컴포넌트들이다. egjs 같은
* [GitHub - naver/egjs: Set of UI interactions, effects and utility components library using jQuery.](https://github.com/naver/egjs)
* feature detection 으로 호환성 이슈를 해결하는 것이 좋음.