---
layout: default
title:  "[JS] 모바일 웹 실습, Mocha를 이용한 Test Code 짜기"
date:   2017-07-26 12:00:00
categories: "Javascript"
---


## 터치 swipe 롤 완성하기

```javascript
const slider = new Slider(document.querySelector('#viewPort'), document.querySelector('#sliderWrapper'))

class Slider {
    constructor (viewPort, sliderWrapper) {
        this.viewPort = viewPort
        this.sliderWrapper = sliderWrapper
        this.firstXPosition = 0
        this.currentWrapperPosition = 0
        this.setEventListener()
    }

    setFirstXPosition (xPosition) {
        this.firstXPosition = xPosition
    }

    getDistance(xPosition) {
        return xPosition - this.firstXPosition
    }

    moveSliderWrapper(movingPixel) {
        this.sliderWrapper.style.transform = `translateX(${movingPixel}px)`
    }

    setEventListener() {
        this.viewPort.addEventListener('touchstart', e => {
            e.preventDefault()
            this.currentWrapperPosition = 0 || Number(this.sliderWrapper.style.transform.replace('translateX(', '').replace('px)', ''))
            const newPosition = e.changedTouches[0].screenX
            this.setFirstXPosition(newPosition)
        })


        this.viewPort.addEventListener('touchmove', e => {
            e.preventDefault()
            this.sliderWrapper.style.transition = null
            const newPixel = e.changedTouches[0].screenX
            this.moveSliderWrapper(this.currentWrapperPosition + this.getDistance(newPixel))
        })



        this.viewPort.addEventListener('touchend', e => {
            const windowWidth = parseInt(window.innerWidth)
            const distance = this.getDistance(e.changedTouches[0].screenX)
            const maxPosition = -(window.innerWidth * (this.sliderWrapper.children.length))
            console.log(maxPosition)
            let newWrapperPosition = 0

            this.sliderWrapper.style.transition = 'ease 0.3s'

            if ((Math.abs(distance)) > (windowWidth / 10)){
                if (distance < 0) {
                    newWrapperPosition = this.currentWrapperPosition - windowWidth
                } else if (distance > 0) {
                    newWrapperPosition = this.currentWrapperPosition + windowWidth
                }

                if ((newWrapperPosition > 0) || (newWrapperPosition <= maxPosition)) {
                    this.sliderWrapper.style.transform = `translateX(${this.currentWrapperPosition}px)`
                    return false
                }
                this.sliderWrapper.style.transform = `translateX(${newWrapperPosition}px)`
            } else {
                this.sliderWrapper.style.transform = `translateX(${this.currentWrapperPosition}px)`
            }
        })


    }
}

```

## CSS Preprocess 공부하기 : less sass
[An Introduction to CSS Pre-Processors: SASS, LESS and Stylus HTML Mag](https://htmlmag.com/article/an-introduction-to-css-preprocessors-sass-less-stylus)

![사진]({{tong9433.github.io}}/image/woowastudy/woowa0726_1.png)

![사진]({{tong9433.github.io}}/image/woowastudy/woowa0726_2.png)

![사진]({{tong9433.github.io}}/image/woowastudy/woowa0726_3.png)


## media query 
* 어떤 모바일,해상도 조건에 따라 css 값을 다르게 줌.

## 애니메이션
* CSS3 속성을 잘 쓰면 좋음.

## Software Test
* 유닛 테스트 ( 작은 단위, 함수 )
* 통합 테스트 ( 단위 기능이 합쳐진 기능에 대한 테스트 )
* 시스템 테스트 ( 전체 시스템에 대한 동작 테스트 )
* Acceptance Test : 고객이 ok할 수 있는지 판단하기 위한 테스트
* 누구나 보고 테스트 할 수 있게 해야 함.
* 많은 테스트 코드의 결과를 쉽게 확인하기 위해서 추가적인 장치가 필요.
=> Qunit, Mocha 와 같은 테스트 프레임 워크를 사용하면 편리.

##  단위 테스트 실습
* nodeJS 기반의 프로젝트를 만들고 그 곳에서 웹프론트 개발을 할 것.
* npm 기반의 프로젝트가 좋음
* mocha 를 이용해서 테스트 하기

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Mocha Tests</title>
    <Link rel="stylesheet" href="./node_modules/mocha/mocha.css">
</head>
<body>
    <div id = "mocha"></div>
    <script src="./node_modules/mocha/mocha.js"></script>
    <script src="./node_modules/chai/chai.js"></script>
    <script>mocha.setup('bdd')</script>
    <script src="main.test.js"></script>
    <script>mocha.run()
    </script>

</body>
</html>
```

```javascript
const assert = chai.assert;

describe('equal',function(){
    it('should not equal',function(){
        assert.equal(true, false);
    });
})

describe('array test1',function(){
    it('equal dummy test',function(){
        //given
        var arr = [];
        //when
        arr.push(1,2,'3');
        //then
        assert.equal(arr.length,3);
    });
})

describe('array test2',function(){
    it('equal dummy test',function(){
        //given
        var arr = [];
        //when
        arr.push(1,2,'3');
        //then
        assert.equal(arr.length,4);
    });
})
```

* 위와 같이 테스트 환경 만들기
* 결과 화면
![사진]({{tong9433.github.io}}/image/woowastudy/woowa0726_4.png)

* describe 와 it 은 모카에서 제공하는 메소드 이다.
* 테스트 코드 보편적으로 짜는 법 given -> when -> then  (마틴 파울러)
https://martinfowler.com/bliki/GivenWhenThen.html
* 많은 회사들은 테스트를 잘 못하는 경우가 많다. 하지만 자발적으로 점진적으로 하는 것이 소프트웨어의 품질을 높인다.
* 소프트웨어의 품질에 대해서 깊이 있게 학습하면 좋은 직장에 근무할 수 있을 것이다. 품질에 대한 고민을 많이 해보자.
-> 도전적, 열정적, 기업문화를 바꿔줄 수 도 있는 사람으로 성장
* 실습2 checkType 함수, addClass 함수 테스트 하기

```javascript
describe('function test',function(){
    it('equal dummy test',function(){
        //given
        const text = 3;
        //when
        const a = checkType(3);
        //then
        assert.equal(a,'string');
    });
})

describe('addClass test',function(){
    it('equal dummy test',function(){
        //given
        const element = document.createElement('div');
        //when
        addClass(element, 'newClass');
        //then
        assert.equal(element.classList.contains('newClass'),true);
    });
})
```

![사진]({{tong9433.github.io}}/image/woowastudy/woowa0726_5.png)


* eventHandler test하기

```javascript
function clickClearClassHandler(evt){
    evt.preventDefault();
    evt.target.className = "" ;
}
```
```javascript
describe('handler test',function(){
    it('equal dummy test',function(){
        //given
        const element = document.createElement("h3");
        const event = {
            target : element,
            preventDefault(){
                return true;
            }
        }
        const className = 'newClass';
        //when
        element.classList.add('newClass');
        clickClearClassHandler(event)
        //then
        assert.equal(element.classList.contains(className),false);
    });
})
```
-> 다음과 같이 간단하게 테스트를 할 수 있음. ( fake 방법, 가짜 객체 만들기)

* 클릭 이벤트 실행시키기

``` javascript
const event = new Event("click');
document.querySelector("div").dispatchEvent(event);
```

* 비동기 ajax() 테스트 ( done이 없을 때)

```javascript
function xhr(url, cb){
    const xhr = new XMLHttpRequest();
    xhr.addEventListener('load',function(){
        cb(JSON.parse(this.responseText));
        console.log("받았다")
    });
    xhr.open('get', url);
    xhr.send();
}

describe('async ajax test',function(){
    it('should get',function(){//매개변수로 done을 추가
        const url = 'http://52.78.212.27:8080/woowa/best';
        const fn = function(result){
            const id= result[0].category_id;
            console.log(id);
            assert.equal(id, '17011200');
            console.log(1);
            // done();
        }
        xhr(url,fn);
    })
})

describe('equal dummy',function(){
    it('should equal',function(){
        assert.equal(true,true);
        console.log(2);
    })
})
```
* 결과 
![사진]({{tong9433.github.io}}/image/woowastudy/woowa0726_6.png)


* done() 추가 후 결과 -> 비동기 데이터가 올 때까지 기다리고 동기적인 소스를 실행.
![사진]({{tong9433.github.io}}/image/woowastudy/woowa0726_7.png)


* TDD 란 : 테스트 코드를 작성하고 중복을 제거하라!
> 테스트 주도 개발(Test-driven development TDD)은 매우 짧은 개발 사이클을 반복하는 소프트웨어 개발 프로세스 중 하나이다. 우선 개발자는 바라는 향상 또는 새로운 함수를 정의하는 (초기적 결함을 점검하는) 자동화된 테스트 케이스를 작성한다. 그런 후에, 그 케이스를 통과하기 위한 최소한의 양의 코드를 생성한다. 그리고 마지막으로 그 새 코드를 표준에 맞도록 리팩토링한다. 이 기법을 개발했거나 '재발견' 한 것으로 인정되는 Kent Beck은 2003년에 TDD가 단순한 설계를 장려하고 자신감을 불어넣어준다고 말하였다. 출처:위키

![사진]({{tong9433.github.io}}/image/woowastudy/woowa0726_8.png)



* TDD는 설계 패턴임.


