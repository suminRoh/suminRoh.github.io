---
layout: post
title: HTML
color: rgb(239, 192, 80)
tags: [websolute,team,HTML]
---

#### Java Script 
* 동적 (html은 정적)
* `<script></script>` : 이 태그 안 쪽에 있는 코드를 javascript로 해석

*  `<input type="button" value="hi" onclick="alert('hi')">`
    * button 타입, 버튼안에 들어가 있는 내용 hi, 
    * onclick의 속성값은 반드시 javascript
    * onclick의 속성값은 웹브라우저가 기억하고 있다가 사용자가 클릭했을 때 문법에 따라 해석해서 웹브라우저가 동작

* event(사건): 웹브라우저 위에서 일어나는 일들 ex) onclick 
    * (이벤트가 실행되었을 떄 어떠한 JS가 일어남)
    * 숫자 데이터 타입 -산술 연산자(+, -, *, /)
    * 문자 데이터 타입- `string.length()` , `string.indexof(' ')` 등등
    * 변수 쓸 때 var 앞에 쓰기

->body 태그의 스타일 바꿈
```javascript 
<input type="button" value="night" onclick="
    document.querySelector('body').style.backgroundColor='black';
    document.querySelector('body').style.color='white';
">
```
    



* 버튼을 만들어서 낮과 밤에 맞는 스타일링
```javascript
<input type="button" value="night" onclick="
    document.querySelector('body').style.backgroundColor='black';
    document.querySelector('body').style.color='white';  
">
<input type="button" value="day" onclick="
    document.querySelector('body').style.backgroundColor='white';
    document.querySelector('body').style.color='black';
">
```
* 토글 버튼을 상황에 따라 바꿈
```javascript  
<input id="night_day" type="button" value="night" onclick="
    if (document.querySelector('#night_day').value==='night'){
        document.querySelector('body').style.backgroundColor='black';
        document.querySelector('body').style.color='white';
        document.querySelector('#night_day').value='day';
    }else{
        document.querySelector('body').style.color='black';
        document.querySelector('body').style.backgroundColor='white';
        document.querySelector('#night_day').value='night';
    }
">
```


* 리펙토링 : 코드 자체를 효율적으로 만들어 가독성을 높이는 작업
1. this : 자기 자신을 가리키는 아이로 가독성을 높여줌
    ex)  `document.querySelector('#night_day').value` -> `this.value`
2. 변수 설정 
    ex)   `var target=document.querySelector('body');`
3. 반복문과 배열
```javascript
  <input id="night_day" type="button" value="night" onclick="
    var target=document.querySelector('body');
    if (this.value==='night'){
      target.style.backgroundColor='black';
      target.style.color='white';
      this.value='day';

      var alist=document.querySelectorAll('a');
      var i=0;
      while(i<alist.length){
        alist[i].style.color='powderblue';
        i=i+1;
      }
    }else{
      target.style.color='black';
      target.style.backgroundColor='white';
      this.value='night';

      var alist=document.querySelectorAll('a');
      var i=0;
      while(i<alist.length){
        alist[i].style.color='blue';
        i=i+1;
      }
    } 
  ">
```
->반복문과 배열을 통해 원하는 글자 색 변경시켜줌


4. 함수 : 함수를 이용해 중복 제거하는 코드 만들기
```javascript
    function setColor(color){
        var alist=document.querySelectorAll('a');
        var i=0;
        while(i<alist.length){
            alist[i].style.color=color;
            i=i+1;
        }
    }
    function nightDayHandler(self){
        var target=document.querySelector('body');
        if (self.value==='night'){
            target.style.backgroundColor='black';
            target.style.color='white';
            self.value='day';
            setColor('powderblue');
        }else{
            target.style.color='black';
            target.style.backgroundColor='white';
            self.value='night';
            setColor('blue');
        }
    }
```
5. 객체
*  객체: { } 로 표현 (배열: [ ]로 표현)
ex) `person={"name":"sumin","age":"22"}`
-> key : property 관계로 표현 

* property (속성) : 객체에 소속된 변수
객체 모든 키 값 & 속성 출력 ->
`for( var key in coworkers){document.write(key+" : "+coworkers[key]+'<br>')}`

* 함수 정의 법
1. coworkers.showAll=function(){} 
    * 객체에 소속된 변수의 값으로 함수를 지정
    * 객체에 소속된 함수를 메소드(method)라고 함
2. function showAll(){}
3. var showAll=function(){}
```javascript
  var Body={
      setColor:function(color){
          document.querySelector('body').style.color=color;
      }
 }
 ```
-> 이런 형식으로 객체 안의 변수를 함수로 지정하고 그 변수의 속성에 함수를 정의함
  
6. 파일로 묶어서 그룹핑
* `<script></script>` 내부의 코드들을 color.js라는 파일에 저장
-> `<script src="color.js"></script>` 만 있으면 사용 가능

7. 라이브러리와 프레임워크
* library : 내가 만들고자 하는 프로그램에 필요한 소프트웨어를 잘 정리
* framework : 만들고자하는 것에서 공통적인 부분을 frame work로 놓고 다른 부분들만 설정 
->jquery 를 이용 ( script로 src 가져옴) 
`$('a').css()` : 모든 a 태그를 jquery로 제어


* UI (User Interface) : 사용자가 시스템을 제어하기 위해서 사용하는 조작 장치
* API (Application Programming Interface) : 애플리케이션을 만들기 위해서 프로그래밍을 할 때 사용하는 조작 장치 