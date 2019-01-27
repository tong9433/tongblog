---
layout: default
title:  "[JS]  객체, 배열 + 실습"
date:   2017-07-11 12:00:00
categories: "Javascript"
---


## 객체 (object)
* 객체를 통해서 모든 자료를 보관하는 일을 많이 한다.
* Node.js 도 능숙하게 다룰 수 있음.
* key 와 value 의 자료구조.
* 이름이 있지만, 순서는 없다.
* object 형태는 { } 로 표현
* 보통 json 타입으로 많이 씀.
* const myFriend = { key:"value"}; , key 를 “key” 해도 상관은 없다
* myFriend[“key”] 혹은 myFriend.key 형태로 그 value를 가져 올 수 있음.
* myFriend.age = 34 ; object를 추가 가능.
* Object.keys(obj) : obj 객체의 key값이 배열로 생성
* Object.values(obj): obj 객체의 value이 배열로 생성

```javascript
obj={name:"tongil",
age:32,
hp:"kt"};

for ( var i in obj ){
	if(obj.hasOwnProperty(i)){
    	console.log(obj[i]+"-"+i);
	}
} // for

console.log(Object.keys(obj));
console.log(Object.values(obj));

Object.keys(obj).forEach(function(element){
	console.log(element);
});

Object.values(obj).forEach(function(element){
	console.log(element);
});

console.log(obj.hp);
```


## 객체, 배열 탐색 관련 실습 

* 실습1) 새로운 배열 만들기 : json 파일에서 newslest 항목만 뽑아서 새로운 배열을 반환하는 함수 만들기

```javascript
const news_list = [
    {
        "title": "sbs",
        "imgurl": "http://static.naver.net/newsstand/2017/0313/article_img/9054/173200/001.jpg",
        "newslist": [
            "[가보니] 가상 경주도 즐기고, 내 손으로 자동차도 만들고",
            "리캡차'가 사라진다",
            "갤럭시S8' 출시? '갤노트7' 처리 계획부터 밝혀야",
            "블로코-삼성SDS, 블록체인 사업 '맞손",
            "[블록체인 톺아보기] 퍼블릭 블록체인의 한계와 프라이빗 블록체인"
        ]
    },
    {
        "title": "mbc",
        "imgurl": "http://static.naver.net/newsstand/2017/0313/article_img/9033/220451/001.jpg",
        "newslist": [
            "Lorem ipsum dolor sit amet, consectetur adipisicin",
            "ipsum dolor sit amet, consectetur adipisicin",
            "dolor sit amet, consectetur adipisicin",
            "amet, consectetur adipisicin"
        ]
    },
    {
        "title": "매일경제",
        "imgurl": "http://static.naver.net/newsstand/2017/0314/article_img/9054/134051/001.jpg",
        "newslist": [
            "페이스북, '감시 목적으로 데이터 사용 금지'",
            "구글, ‘저널리즘 360° 챌린지’ 프로젝트 공모전 실시",
            "효과적인 이메일 마케팅을 위한 6가지 방법",
            "amet, consectetur adipisicin"
        ]
    }
];

function newListFunction(array) {
    const result_list = []
    for (const i in array) {
        const news = array[i]
        const news_newslist = news.newslist
        for (const j in news_newslist) {
            result_list.push(news_newslist[j])
        }
    }
    return result_list
}
console.log(newListFunction(news_list))
```


* 실습2) 숫자타입으로만 구성된 요소를 뽑아 배열 만들기

```javascript
const widget = {
    "debug": "on",
    "window": {
        "title": "Sample Konfabulator Widget",
        "name": "main_window",
        "width": 500,
        "height": 500
    },
    "image": {
        "src": "Images/Sun.png",
        "name": "sun1",
        "hOffset": 250,
        "vOffset": 250,
        "alignment": "center"
    },
    "text": {
        "data": "Click Here",
        "size": 36,
        "style": "bold",
        "name": "text1",
        "hOffset": 250,
        "vOffset": 100,
        "alignment": {
            'my_number': 12342315
        },
        "onMouseUp": "sun1.opacity = (sun1.opacity / 100) * 90;"
    }
}

function makeWidget(data) {
    const result_list = []

    function findNumber(obj){
        for (const i in obj) {
            const val = obj[i]
            if (typeof(val) === 'number') {
                result_list.push(i)
            } else if (typeof(val) === 'object'){
                findNumber(val);
            }
        }
    }

    findNumber(data)

    return result_list
}

console.log(makeWidget(widget))
```


* 실습3) type이 ‘sk’인, name으로 구성된 배열 출력하기

```javascript
const newinput = [{
    "id": 1,
    "name": "Yong",
    "phone": "010-2786-9902",
    "type": "sk",
    "childnode": [{
        "id": 11,
        "name": "echo",
        "phone": "010-3923-1333",
        "type": "kt",
        "childnode": [{
            "id": 115,
            "name": "hary",
            "phone": "010-2786-9302",
            "type": "sk",
            "childnode": [{
                "id": 1159,
                "name": "pobi",
                "phone": "010-9302-0009",
                "type": "kt",
                "childnode": [{
                    "id": 11592,
                    "name": "cherry",
                    "phone": "010-1223-9932",
                    "type": "lg",
                    "childnode": []
                },
                    {
                        "id": 11595,
                        "name": "solvin",
                        "phone": "010-534-7843",
                        "type": "sk",
                        "childnode": []
                    }
                ]
            }]
        },
            {
                "id": 116,
                "name": "kim",
                "phone": "010-3796-1102",
                "type": "kt",
                "childnode": [{
                    "id": 1168,
                    "name": "hani",
                    "phone": "010-1223-6713",
                    "type": "sk",
                    "childnode": [{
                        "id": 11689,
                        "name": "ho",
                        "phone": "010-4434-4534",
                        "type": "kt",
                        "childnode": [{
                            "id": 116890,
                            "name": "wonsuk",
                            "phone": "010-3434-1302",
                            "type": "kt",
                            "childnode": []
                        },
                            {
                                "id": 1168901,
                                "name": "chulsu",
                                "phone": "010-3100-9841",
                                "type": "sk",
                                "childnode": []
                            }
                        ]
                    }]
                }]
            },
            {
                "id": 117,
                "name": "hong",
                "phone": "010-2786-9902",
                "type": "lg",
                "childnode": []
            }
        ]
    }]
}]

function findSKUser(arr) {
    const name = [];

    function findType(arr) {

        arr.forEach(function (element) {
            if (element.type === "sk") name.push(element.name);
            findType(element.childnode);
        })
    }

    findType(newinput)
    return name
}

console.log(findSKUser(newinput))
```


* 실습4) 아래와 같이 동작하는 parse 함수를 구현하기

```javascript
const result_obj = {}

function parse(arr, condition) {
    result_obj[condition] = []

    function findType(arr) {
        arr.forEach(function (element) {
            if (element.type === condition) {
                result_obj[condition].push(element.name);
            }
            findType(element.childnode);
        })
    }

    findType(arr)
}

parse(newinput, 'sk')
parse(newinput, 'kt')

console.log(result_obj.sk)
```
