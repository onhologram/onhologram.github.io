---
title: "Jvascript HTML 링크 & async와 defer"
categories:
  - Github blog
tags:
  - Jvascript
  - async
  - defer
---

## Jvascript를 HTML에 링크할때 효율적인 방법 & async와 defer의 차이점

##### 1.head안에 'script'를 포함하게 되는 경우
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script src="main.js"></script>
</head>
```
사용자가 html을 다운받았을때 브라우저가 한줄 한줄씩 분석(parsing)하게 된다. 
이렇게 한줄씩 분석한걸 css와 병합하여 DOM요소로 변환하게 된다. 
html을 쭉 분석하다가 스크립트 태그가 보이면 분석하는걸 멈추고 필요한 자바스크립트 소스를 다운받아서 그 다음에 다시 분석하게 된다.

단점: js파일의 용량이 크고 인터넷이 느리면 사용자가 웹사이트 보는데 시간소요가 오래걸린다.


##### 2.body안에 끝부분에 'script'를 추가하는 경우
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <div></div>
    <script src="main.js"></script>
</body>
</html>
```
브라우저가 html을 다운받아 쭉 parsing후 페이지가 준비가 된 다음 'script'를 만나 서버에서 받아오고 실행하게 된다.
페이지가 사용자들에게 js를 받기전에 준비가 되어서 사용자가 기본적인 html 페이지 컨텐츠를 빨리 볼수있게된다.

단점: 웹사이트가 자바스크립트에 굉장히 의존적, 자바스크립트를 이용해서 서버에 있는 데이터를 받아 옴, DOM요소를 더 이쁘게 꾸미면서 동작하는 웹사이트라면 
사용자가 정상적인 페이지를 보기 전까지 서버에서 자바스크립트를 받아오는시간(fatching), 실행되는 시간을 기다려야하는 단점이 있다.



##### 3.head안에 'script'에 asyn 속성 값을 사용하는 경우
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script asyn src="main.js"></script>
</head>
```
asyn(에이싱크)는 boolean 타입의 속성값을 가지기 때문에 선언하는 것만으로 true로 설정이 된다.

브라우저가 html을 다운로드 받아서 분석하다 asyn가 있는걸 발견하고 병렬로 js를 다운받으라고 명령한다. 
그럼 다시 분석을 하다 js 다운로드가 완료가 되면 그때 분석하는걸 멈추고 다운로드 된 js파일을 실행한다.
이렇게 실행을 다하고 나서 나머지 html을 분석하게 된다.

body끝에 사용하는것보단 fatching이 parsing하는동안 병렬적으로 일어나기 떄문에 다운로드 받는시간을 절약할 수 있다.

단점: js가 html이 분석되기전에 실행되서 js파일에서 쿼리 셀렉터를 이용해서 DOM요소를 조작하게 된다면 조작시점에 html의 정의가 아직 안될 수 도 있다. 
또 html을 분석하는동안 js 실행을 위해 분석하는걸 언제든지 멈출 수 있기 때문에 사용자가 페이지를 보는데 여전히 시간이 걸리는 단점이 있을 수 있다.



##### 4.head안에 'script'에 defer 옵션을 사용하는 경우(가장 좋은 옵션! 제일 효율적이고 안전!)
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script defer src="main.js"></script>
</head>
```
브라우저가 html을 분석하다 defer가 있는걸 발견하고 js를 다운로드 받으라고 명령만 시켜놓고 나머지 html을 끝까지 분석하게 된다.
그리고 마지막에 분석이 끝난 후 다운로드 받은 js를 실행하게 된다.

html을 분석하는 동안 필요한 자바스크립트를 다 받아놓고 
html 분석을 먼저 해서 사용자에게 페이지를 보여준 다음에
바로 이어서 자바스크립트를 실행한다.

___

##### 다수의 스크립트를 받게 될 경우  asyn 와 defer의 차이
**asyn**
정의된 스크립트 순서에 상관없이 다운로드가 먼저 된 걸 실행하기 때문에
만약 순서에 의존적인 자바스크립트라면 문제가 될 수 있다.

**defer**
분석하는 동안 자바스크립트를 다 다운받아 놓고 순서대로 실행하기 때문에 
정의한 순서가 지켜지기 떄문에 원하는대로 스크립트가 실행 될 거라는걸 예상 할 수 있다.


###### 바닐라 자바스크립트를 사용할 경우
가장 상단에 **'use strict';** 를 선언!   상식적인 범위안에서 바닐라 자바스크립트를 사용할수 있게 해준다.

> ## 출처 : 드림코딩 엘리
<!-- Link -->
Click [Here](https://youtu.be/tJieVCgGzhs)
