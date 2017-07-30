---
layout: default
title:  "[2017-07-18] JavaScript carousel 실습"
date:   2017-07-18 12:00:00
categories: "WoowaStudy-Web"
---


## 코드 디자인, 설계의 중요성!
* carousel 실습

```javascript
document.addEventListener('DOMContentLoaded', function (event) {
    console.log('DOM fully loaded and parsed');

    // roll
    const roll1 = new rollScreen('rollBox1','rollLeft1', 'rollRight1',
        'http://52.78.212.27:8080/woowa/main','rollScreenTemplate');


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

class rollScreen {

    constructor(rollBox, rollLeft, rollRight, url ,template) {
        this.rollBox = rollBox;
        this.rollLeft = rollLeft;
        this.rollRight = rollRight;
        this.url = url;
        this.template = template;
        this.dataArr = [];
        this.count = 0;
        this.index = 4;
        this.dataCount = this.dataArr.length;
        this.init();
    }

    init() {
        this.rollRight = document.getElementById(this.rollRight);
        this.rollLeft = document.getElementById(this.rollLeft);
        this.template = document.getElementById(this.template).innerHTML;
        this.SetLeftRoll();
        this.SetRightRoll();
        this.getData();

    }

    getData() {
        const util = new Util();
        util.ajax(this);
    }

    setData(data){
        this.dataArr = data;
        this.dataCount = this.dataArr.length;
        this.MakeNode(this.dataArr[0], 1);
        this.MakeNode(this.dataArr[1], 1);
        this.MakeNode(this.dataArr[2], 1);
        this.MakeNode(this.dataArr[3], 1);
        const arr = Array.from(document.querySelectorAll(`#${this.rollBox}> .roll_box`));
        arr.forEach(function (ele) {
            ele.setAttribute('class', 'roll_box selected')
        })

    }

    // value 1 이면 오른쪽 makeNode -1이면 왼쪽
    MakeNode(object, value) {
        const para = document.createElement('li');
        para.setAttribute('class', 'roll_box');

        const tmpl = Handlebars.compile(this.template);
        para.innerHTML = tmpl(object);

        if (value === -1) {
            const nextElement = document.getElementById(this.rollBox).firstChild;
            document.getElementById(this.rollBox).insertBefore(para, nextElement);
        } else {
            document.getElementById(this.rollBox).appendChild(para);
        }
    }

    RemoveNode() {
        const arr = Array.from(document.querySelectorAll(`#${this.rollBox}> .selected`));

        arr.forEach(function (ele) {
            document.getElementById(this.rollBox).removeChild(ele)
        }.bind(this))
    }

    SetLeftRoll() {
        this.rollLeft.addEventListener('click', function () {
            const that = this;
            this.rollLeft.disabled = true;
            const roll = document.getElementById(this.rollBox).parentNode;

            this.index = this.index - 4;
            for (let i = this.index - 1; i > this.index - 5; i--) {
                let mIndex;
                if (i < 0) mIndex = this.dataCount + i;
                else if (i >= this.dataCount) mIndex = i - this.dataCount;
                else mIndex = i
                this.MakeNode(this.dataArr[mIndex], -1);
            }

            if (this.index < 0) this.index = this.index + this.dataCount;

            this.count++;
            roll.style.transitionDuration = '0.4s';
            roll.style.marginLeft = '-800px';
            roll.style.transform = 'translateX(800px)';

            setTimeout(function () {
                that.RemoveNode()
                roll.style.transitionDuration = '0s';

                const arr = Array.from(document.querySelectorAll(`#${this.rollBox}> .roll_box`));
                arr.forEach(function (ele) {
                    ele.setAttribute('class', 'roll_box selected')
                })

                roll.style.margin = 'auto';
                roll.style.transform = 'translateX(0px)';

                that.rollLeft.disabled = false;
            }, 400)
        }.bind(this));
    }

    SetRightRoll() {
        this.rollRight.addEventListener('click', function () {
            const that = this;
            this.rollRight.disabled = true;

            for (let i = 0; i < 4; i++) {
                this.MakeNode(this.dataArr[this.index], 1);
                this.index++;
                if (this.index === this.dataCount) this.index = 0;
            }

            this.count++;
            const roll = document.getElementById(this.rollBox).parentNode;
            roll.style.transitionDuration = '0.4s';
            roll.style.transform = 'translateX(-800px)';

            setTimeout(function () {
                that.RemoveNode()
                roll.style.transitionDuration = '0s';
                roll.style.transform = 'none';

                const arr = Array.from(document.querySelectorAll(`#${this.rollBox}> .roll_box`));
                arr.forEach(function (ele) {
                    ele.setAttribute('class', 'roll_box selected')
                })

                that.rollRight.disabled = false;
            }.bind(that), 400)
        }.bind(this));
    }
}
```