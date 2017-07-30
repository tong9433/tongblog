---
layout: default
title:  "[2017-07-27] JavaScript 프로젝트 준비사항 의존성 관리, 웹팩, 바벨"
date:   2017-07-27 12:00:00
categories: "WoowaStudy-Web"
---

## 수업시작 전
* 자바스크립트 테스트와 디버깅 : UI , TDD , Qunit 관련 책
* 프레임 워크

## 프로젝트 준비사항
1. 코딩 컨벤션 ( 이름, 주석, 디렉토리 구조, 소스이름, 스타일)
2. 라이브러리, 프레임워크 ( 크롱님은 뷰만하는 react 를 추천)
3. 테스트 코드 커버리지
4. 코드관리 툴
5. task 관리
6. 브라우저 지원 기기
7. 웹앱 지원기기 범위
8. 코드리뷰 언제 할지 일정 관리

## 의존성 관리 - Module. (import, export) - ES6 스펙
* 한 파일로 javascript 소스가 뭉치면 한 파일만 계속 수정됨. 여러 파일로 나눠 놓으면 10개중에 1개만 수정해야 함.
* 다른 js 간에 필요한 함수를 쓸경우 import 를 이용해서 사용 가능

```javascript
//service.js
import  함수  from './calculate.js';
```

```html
<script type="module" src='./src/service.js'></script>
// 이런식으로 servic.js만 html 소스에는 넣어주면 됨
```

* 각각의 js들의 관계를 그려야 함.
* 우리는 이걸 쓰고 웹팩을 이용해서 트랜스파일링 할 것
* require js 혹은 script를 몽땅 html에 넣거나 하는 방법을 ES6 전에는 사용 
* 더 좋은 문법을 만들어서 이를 배포직전에 javascript 코드로 변환 (coffeescript, typescript 이러한 문법들이 javascript 문법을 발전 시킴)
* 트랜스 파일링 되는 최근의 표준을 이용해서 짜자. ( 크롱님의 clean front - end 개발 = vanila)
[February 29, 2016 : 올해엔 순수 자바스크립트를 써보는 건 어떨까요? · nhnent/fe.javascript Wiki · GitHub](https://github.com/nhnent/fe.javascript/wiki/February-29,-2016-:-%EC%98%AC%ED%95%B4%EC%97%94-%EC%88%9C%EC%88%98-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%A5%BC-%EC%8D%A8%EB%B3%B4%EB%8A%94-%EA%B1%B4-%EC%96%B4%EB%96%A8%EA%B9%8C%EC%9A%94%3F)
-> 이거 잘 읽기


## 웹팩을 이용한 의존성 관리 쉽게하기

```javascript
const _= {
        log(content){
            if(window.console) console.log(content)
            else alert(content)
        }
}
export default _;
```

```javascript 
import _ from "./util.js"

const root = document.querySelector('#root');
root.innerHTML = '<p>Hello tongtong</p>';
_.log(root.innerHTML);
//index.js
``` 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Tongil Project</title>
</head>
<body>
    <div id='root'></div>
    <!--<script src="/src/util.js"></script>-->
    <!--<script src="/src/index.js"></script>-->
    <script src="dist/bundle.js"></script>
</body>
</html>
//util.js
```

* import / export 방식이 대부분 지원이 안되므로 웹팩을 통해 브라우저에서 동작가능한 코드로 변경해줌.

* 빌드 

```
 ./node_module/ .bin/webpack src/index.js dist/bundle.js
 ```

## webpack config 로 빌드 실행 좀 더 쉽게하기

![사진]({{tong9433.github.io}}/image/woowastudy/woowa0727_1.png)

* 간단해진 webpack 빌드실행
`./node_modules/.bin/webpack` 로 bundle.js를 만듬

## 더 간단하게 하기
* package.json의 script에 실행스크립트를 추가

```javascript
"scripts":{
	"start":"webpack"
},
```
-> `npm run start`로 쉽게 빌드 가능

## 자동화면 리로딩을 하고싶음 : 수정이 발생하면 매번 새로고침이 필요함.
1.webpack-dev-server 설치

```
sudo npm install webpack-dev-server --save-dev
```
2.package.json에 추가하기

```javascript
"scripts":{
	"webpackbuild":"webpack",
	"devserver": "webpack-dev-server --inline"
},
```
3.다시 빌드

```
npm run devserver
```
-> 이제 javascript 를 추가하거나 수정할 때 즉각즉각 반영 ( 단 그 소스에 export와 index.js에 import를 해주어야 함)

## css를 import문으로 사용하기 (react 에서 js파일과 css를 함께 모듈화 할때 쓰이는 방법)
1.webpack loader에 css loader를 설치 / 설정해야한다. 각 javascript 마다 이렇게 사용하면 참 좋음
```javascript
//index.jss 파일에
import './index.css';
```

```
npm install —-save-dev css-loader style-loader
```
2.css를 위한 webpack config에 module 추가

![사진]({{tong9433.github.io}}/image/woowastudy/woowa0727_2.png)

3.index.css 파일을 만들어서 src 폴더에 넣기
* 웹팩은 크게 loader와 plugin으로 나뉨. 핵심은 loader와 plugin으로 새로운 syntax를 돌아가게함.
* 디렉토리 구조 

![사진]({{tong9433.github.io}}/image/woowastudy/woowa0727_3.png)

## 바벨 로더 사용하기
* [Babel · The compiler for writing next generation JavaScript](http://babeljs.io/)
* es2015코드를 babel-loader를 사용해서 하위 브라우저 지원 권장.

![사진]({{tong9433.github.io}}/image/woowastudy/woowa0727_4.png)

 * 위와 같이 config 파일에 추가를 하면 bundle.js 가 ES6에 맞게 transfiling 됨
 * devserver는 바로바로 bundle.js 를 바꿔주는게 아니라 저장하고 있다가 나중에 한꺼번에 바꿔주는 방식이다. 왜냐하면 물리적인 파일을 바로바로 바꿔버리는 일을 하면 시간이 너무 오래 걸린다.
 * 지금 우리는 플러그인은 안 했는데 웹팩의 플러그인도 추가적으로 찾아서 하면 좋을 듯 하다.
