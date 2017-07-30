---
layout: default
title:  "[2017-07-20] Javascript Tab 실습 + 모달창 만들기"
date:   2017-07-20 12:00:00
categories: "WoowaStudy-Web"
---

## 코드 디자인, 설계의 중요성!
* 탭 실습 + 모달창 실습



```javascript

document.addEventListener('DOMContentLoaded', function (event) {
    console.log('DOM fully loaded and parsed');

    // tab
    const basicTab = new TabClick('navi', 'basicTemplate','http://localhost:3000/static/tab', 'tabSec');
    
    //모달
    const tabPopUp = new PopUp('tabSec', 'popup', 'tabPopupTemplate' ,'tabEleImg');

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

class PopUp {

    constructor(container, sec, template, className,url) {
        this.container = container;
        this.template = template;
        this.sec = sec;
        this.url = url;
        this.className = className;
        this.setHtmlInit();

    }

    setHtmlInit() {
        this.container = document.getElementById(this.container);
        this.template = document.getElementById(this.template).innerHTML;
        this.sec = document.getElementById(this.sec);
        this.setEvent();
    }

    getData() {
        const util = new Util();
        util.ajax(this);
    }

    setEvent() {
        this.container.addEventListener('click', function (e) {

            if (e.target.className === this.className) {
                this.url = 'http://52.78.212.27:8080/woowa/detail/' + e.target.getAttribute('name');
                this.getData();
            }
        }.bind(this), false);
    }

    setData(data) {
        this.data = data;

        const util = new Util();
        util.template(this.data.data,this.template,this.sec);

        util.template(this.data.data,
            document.getElementById('popupImgTemplate').innerHTML,
            document.getElementById('PopupImages'))

    }
}

class TabClick {

    constructor(container, template, url, sec) {
        this.container = container;
        this.template = template;
        this.url = url;
        this.sec = sec;
        this.data = [];
        this.setHtmlInit()
    }

    setHtmlInit() {
        this.container = document.getElementById(this.container);
        this.template = document.getElementById(this.template).innerHTML;
        this.sec = document.getElementById(this.sec);
        this.setEvent('selectedTab', 'basic');


    }

    getData() {
        const util = new Util();
        this.url='http://localhost:3000/static/tab'+document.getElementsByClassName('selectedTab')[0].getAttribute('name') + '.json'
        util.ajax(this);
    }

    setEvent(classname, id) {
        this.container.addEventListener('click', function (e) {
            const selectTab = document.getElementsByClassName(classname)[0];
            selectTab.classList.remove(classname);
            e.target.classList.add(classname);
            this.selectedTab = document.getElementsByClassName(classname)[0];
            if (this.selectedTab.getAttribute('id') === id) {
                this.getData();
            }

        }.bind(this), false);
    }

    setData(data) {
        this.data = data;
        const util = new Util();
        util.template(this.data,this.template,this.sec);
    }

}

```