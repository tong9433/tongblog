---
layout: default
title:  "[2017-07-11] JavaScript DOM"
date:   2017-07-11 12:00:00
categories: "WoowaStudy-Web"
---



## DOM - 웹 프로트엔드 개발의 본질
* DOM node 의 최상위 노드가 doucument 임.
* document, element, text 크게 세가지가 있음.
* DOM API가 제공됨 - 함수들의 묶음
* document.getElementById( ) : document 에서 키값에 해당되는 것을 찾기
* Dom을 여러형태로 가져오지만 그것은 배열이 아니다. 객체 형태
* toString.call( ) 을 통해서 더 상세한 형태를 볼 수있음.
* "[object HTMLCollection]" , “[boject HTMLDivElement]” 이런식으로 형태가 나옴
* DOM APIs

```
document. APIs
: https://www.w3schools.com/jsref/dom_obj_document.asp
element. APIs
https://www.w3schools.com/jsref/dom_obj_all.asp
```

* 요즘에는 Dom 조작을 잘 안함.
* element.innerHTML : set 혹은 return 값을 받는 역할을 함.
* querySelctor 실습

```javascript
document.querySelector("li.giftlst_l:nth-child(3)");
<li class=​"giftlst_l">​…​</li>​ //이런식으로 대상을 찾을 수 있음. 
```

```javascript
toString.call(document.querySelectorAll("li")) 
```


```javascript

var p = document.createTextNode("6.pineapple");
var m = document.createTextNode("7.melon");
var li = document.createElement("li");
var li_2 = document.createElement("li");

li.appendChild(p);

document.querySelector("body>ul").appendChild(li);

var ul=document.querySelector("ul");
//document.querySelector("ul").removeChild(li);

//item = document.querySelector("body>ul");

//item.replaceChild(m,item.childNodes[1]);


li_2.appendChild(m);

// document.querySelector("body>ul").appendChild(li_2);

var sp1 = document.querySelector('ul>li:nth-child(2)').appendChild(li_2);

var ul = document.querySelector('ul');

ul.insertBefore(li_2,sp1);

var ul_1 = document.querySelector('ul');

// var apple = document.querySelector('ul>li:nth-child(1)');
// var gr_st = document.querySelector('ul>li:nth-child(5)');

// ul_1.insertBefore(apple,gr_st);

```

```javascript
var m = document.querySelector("ul>li.red");
var ul = document.querySelector("ul");

while(document.querySelector("ul>li.red")!=null){
  var m = document.querySelector("ul>li.red");
  ul.removeChild(m);
}
```



## function 동적인 파라미터 처리

```javascript
function myFunction(x, y) {
    y = y || 0;
}
// default 설정 
x = findMax(1, 123, 500, 115, 44, 88);

function findMax() {
    var i;
    var max = -Infinity;
    for (i = 0; i < arguments.length; i++) {
        if (arguments[i] > max) {
            max = arguments[i];
        }
    }
    return max;
}
// 추가 argument 다루는 방법
```
* argument 라는 객체가 생성되므로 그 array 를 이용한다. 



## Javascript nodelist 배열처럼 쓰려면 어떻게 할지 3가지 방법
1. Slice
```javascript
var nodesArray = Array.prototype.slice.call(document.querySelectorAll('div'));
```
2. 배열을 이용해서 하나씩 대입
```javascript
var divs = document.querySelectorAll('div');
var div_arr = [];
for(var i = divs.length; i--; div_arr.unshift(divs[i]));
```
3. Array.from => **가장 많이 사용 될 것**(IE는 지원 안됨)
```javascriptt
var divs = document.querySelectorAll('div')
var div_arr = Array.from(divs)
```
4. ES6 식으로 사용하기
```javascript
var div_arr = [...document.querySelectorAll('div')]
```

## Javascript element.innerHtml - 자주 사용됨, 속도가 적당히 빠름
```
var content = element.innerHTML;
element.innerHTML = content;
// content element에 content HTML 인자로 치환한다.
``` 


## Javascript element.insertAdjacentHTML
```
element.insertAdjacentHTML(position, text);`
```

* `’beforebegin'` : Before the element itself.
* `’afterbegin’` : Just inside the element, before its first child.
* `'beforeend'` : Just inside the element, after its last child.
* `’afterend'`  :After the element itself.





