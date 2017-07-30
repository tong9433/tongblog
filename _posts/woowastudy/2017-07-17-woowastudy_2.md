---
layout: default
title:  "[2017-07-17] JavaScript 슬라이드 스크린 실습"
date:   2017-07-17 12:00:00
categories: "WoowaStudy-Web"
---

## 코드 디자인, 설계의 중요성!
* 슬라이드 스크린 실습

```javascript
document.addEventListener('DOMContentLoaded', function (event) {
    console.log('DOM fully loaded and parsed');

    // slide
    const screen = new slideScreen('circleBox','slidesPrev', 'slidesNext');

});

class Util {

    ajax(func) {
        const oReq = new XMLHttpRequest();
        oReq.addEventListener('load', function (e) {
            const data = JSON.parse(oReq.responseText);
            func.setData(data);
        });

        oReq.open('GET', func.url);
        oReq.send();
    }

    template(data,template,section){
        const context = data;
        const tmpl = Handlebars.compile(template);
        section.innerHTML = tmpl(context);
    }
}

class slideScreen {

    constructor(container, prevButton, nextButton) {
        this.container = container;
        this.prevButton = prevButton;
        this.nextButton = nextButton;
        this.circleArr = [];
        this.init();
    }

    init() {
        this.container = document.getElementById(this.container);
        this.prevButton = document.getElementById(this.prevButton);
        this.nextButton = document.getElementById(this.nextButton);
        this.circleArr = ['s1', 's2', 's3', 's4', 's5', 's6', 's7', 's8', 's9', 's10', 's11', 's12'];
        this.setSwitchScreen();
        this.setNextSlideButton();
        this.setPrevSlideButton();
    }

    setSwitchScreen() {
        this.container.addEventListener("click", function (e) {
            if (e.srcElement.nodeName === "A") {
                this.removeSelect();
                e.target.classList.add('selectedCircle');
                this.addSelect('selectedCircle');
            }
        }.bind(this));
    }

    setNextSlideButton() {
        this.nextButton.addEventListener('click', function (e) {
            const beforeEle = this.removeSelect();

            let index = this.circleArr.indexOf(beforeEle.getAttribute('name'))
            if (index == this.circleArr.length - 1) index = 0
            else index = index + 1;
            document.getElementsByClassName('c' + (index + 1))[0].classList.add('selectedCircle');
            this.addSelect('selectedCircle')
        }.bind(this));
    }

    setPrevSlideButton() {
        this.prevButton.addEventListener('click', function (e) {
            const beforeEle = this.removeSelect();
            let index = this.circleArr.indexOf(beforeEle.getAttribute('name'))
            if (index == 0) index =this.circleArr.length - 1
            else index = index - 1;
            document.getElementsByClassName('c' + (index + 1))[0].classList.add('selectedCircle');
            this.addSelect('selectedCircle')

        }.bind(this));
    }

    removeSelect() {
        const ele = document.getElementsByClassName("selectedCircle")[0];
        ele.classList.remove("selectedCircle");
        ele.style.backgroundColor = 'white';
        const eleName = ele.getAttribute('name')
        this.setScreenAnimation(eleName, 1)

        return ele;
    }

    addSelect(className) {
        const ele = document.getElementsByClassName(className)[0];
        ele.style.backgroundColor = 'skyblue';
        const eleName = ele.getAttribute('name')
        this.setScreenAnimation(eleName, 0)
    }

    setScreenAnimation(classname, value) {
        const element = document.getElementsByClassName(classname)[0];
        let count = 0;
        const func = () => {
            if (count === 51) return;
            if (value == 1) element.style.opacity = 1 - count * 0.02; //fadein , 1
            else element.style.opacity = count * 0.02; // fadeout , 0
            count++;
            requestAnimationFrame(func);
        }
        requestAnimationFrame(func);
        element.style.opacity = 1 - value;
    }

    autoScreen(time) {
        setInterval(function () {
            this.nextScreen();
        }.bind(this), time)
    }

    nextScreen() {
        const beforeEle = this.removeSelect();
        let index = this.circleArr.indexOf(beforeEle.getAttribute('name'))
        if (index == 11) index = 0
        else index = index + 1;
        document.getElementsByClassName('c' + (index + 1))[0].classList.add('selectedCircle');
        this.addSelect('selectedCircle')
    }
}


```