---
layout: default
title:  "Firebase를 이용한 Web개발 - DB CRUD"
date:   2017-08-03 
categories: "Firebase"
---

# Firebase를 이용한 Web개발 - DB CRUD
## 초기화 하기 (인증)

---

```javascript
var config = {
    
    생략
    
};
firebase.initializeApp(config);
``` 
* firebaseinit.js 라는 파일을 두어서 중복된 코드를 방지한다.

## Firebase SDK 추가

---

```html
   <script src="https://www.gstatic.com/firebasejs/4.2.0/firebase.js"></script>
```
* html 코드 내에 head 태그 안에 둘 것.

## 데이터 베이스 참조 불러오기

---

```javascript
var database = firebase.database();
```
* 데이터 베이스에서 데이터를 읽기위해  firebase.database.Reference의 인스턴스가 필요


## 데이터 쓰기

---

```javascript
function writeUserData(userId, name, email, imageUrl) {
  firebase.database().ref('users/' + userId).set({
    username: name,
    email: email,
    profile_picture : imageUrl
  });
}
```
* 다음과 같이 `인스턴스.ref ( 지정된 위치  ).set({데이터}) `함수를 이용하면 이 위치의 하위 노드에 {데이터}를 덮어 씌움

## 데이터 읽기 

---

```
1. value
2. child_added
3. child_changed
4. child_removed
5. child_moved
```

![사진]({{ tong9433.gitub.io }}/image/firebase1.png)
![사진]({{ tong9433.gitub.io }}/image/firebase2.png)


### 값 이벤트 수신 대기(데이터 변화 감지)
* `firebase.database.Reference `의 `on()`또는 `once()` 메소드를 사용하여 이벤트를 관찰할 수 있다.
* `value` 이벤트를 사용하여 이벤트 발생 시점에 특정 경로에 있던 내용의 정적 스냅샷을 읽을 수 있음.
* `on()` 또는 `once()` 메소드는 리스너가 연결될 때 한 번 호출된 후 하위를 포함한 데이터가 변경될 때마다 다시 호출됨. 
* 하위 데이터를 포함하여 해당 위치의 모든 데이터를 포함하는 스냅샷이 이벤트 콜백함수를 통해 전달
* 데이터가 없는 경우 반환 되는 스냅샷은 `null` 이다.
* 다음 예시를 통해 이해를 해보자!

```javascript
var starCountRef = firebase.database().ref('posts/' + postId + '/starCount');
starCountRef.on('value', function(snapshot) {
  updateStarCount(postElement, snapshot.val());
});
```


* 이 예제는 특정 게시물의 별표 수를 검색하는 예제이다.
* 이 메소드는 스냅샷의 크기를 줄이기 위해 최하위 위치에 연결하는 것이 좋음,
* 예를 들면 데이터베이스 루트에 이 리스너를 연결하는 것은 좋은 방법이 아님.


### 단순히 데이터만 한번읽기
* 변경을 수신 대기하지 않고 단순히 데이터의 스냅샷만 필요한 경우

```javascript
var userId = firebase.auth().currentUser.uid; // user의 아이디 불러오기
return firebase.database().ref('/users/' + userId).once('value').then(function(snapshot) {
  var username = snapshot.val().username;
  // ...
});
```

* val() 메소드로 snapshot의 데이터를 검색할 수 있음.
* once()는 내부적으로 on()을 한 번 수행한 후 리스너를 제거하는 off()를 수행하도록 구현됨



## 데이터 업데이트
---
* 다른 하위 노드를 덮어 쓰면서 데이터 업데이트 가능하다. 
* 하지만 그렇지 않고 특정 하위노드를 업데이트 하기위해 update() 메소드를 사용하자.
* update()를 호출할 때 키의 경로를 지정하여 하위 요소의 값을 업데이트 할 수 있음.
* 다음 예시를 살펴보자.

```javascript
function updateNewPost(uid, username, picture, title, body) {
  var postData = {
    author: username,
    uid: uid,
    body: body,
    title: title,
    starCount: 0,
    authorPic: picture
  };
  var newPostKey = firebase.database().ref().child('posts').push().key;
  var updates = {};
  updates['/posts/' + newPostKey] = postData;
  updates['/user-posts/' + uid + '/' + newPostKey] = postData;
// 두곳을 업데이트 하기
  return firebase.database().ref().update(updates);
}
```

* push()를 사용하여 모든 사용자의 게시물을 포함하는 노드에 게시물을 만드는 동시에 키를 검색함.
* id 값만 가져오고 그 최종하위값에 다른 값을 대입해서 update도 할 수 있음. 마찬가지로 그경우에는 `postData = '값'`으로 해서 그 값을 넣어주면 될 것

```javascript
firebase.database().ref('item/PR0008/allergy").set("감자");
```

* 단순한 단일 항목을 이런식으로 수정하는 것이 가장 편함.

## 데이터 삭제

---

* 데이터를 삭제하는 가장 간단한 방법은 해당 데이터 위치의 참조에 대해 remove()를 호출 하는 것.
* set() 또는 update 등의 다른 쓰기 작업에 대한 값으로 null을 지정하여 삭제가능.

## key 이름 규칙

---

* UTF-8 인코딩 사용 ( 한글 및 유니코드 기호 사용 가능)
* key의 이름에는 $, . # [ ] / 일부 ASKII 문자는 사용 불가능


참조 : [firebase 공식홈페이지](https://firebase.google.com/docs/database/web/read-and-write?hl=ko)


## 유용한 사이트

---

* [웹개발을 위한 firebase]( https://www.slideshare.net/sungbeenjang/firebase-for-web-3-realtime-database?next_slideshow=1)


