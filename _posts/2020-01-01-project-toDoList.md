---
title:  "toy_project_toDoList_webapp"

layout: layout_project

categories:
  - project
tags:
  - project, toy
---
### **\# 토이 프로젝트 - ToDoList WebApp**

### **1\. Introduction**

 - vanilla js를 활용하여 웹 페이지에서 ToDoList 및 시계, 날씨 API 구현 클론코딩 및 복기

### **2\. Preview**

[##_Image|kage@bfvzjY/btqGiUfbWRT/8tW54MuGIW23Ea2GAxrZFK/img.gif|alignLeft|data-filename="Aug-07-2020 14-50-29.gif" data-origin-width="320" data-origin-height="279" width="457" height="NaN" data-ke-mobilestyle="widthContent"|||_##]

### **3\. todo**

 - 실시간 시계 구현

 - 유저네임 입력 및 저장

 - ToDoList 구현

 - 무작위 배경사진 출력

 - 현 지역 날씨 API 구현

#### ** - html 작업**

```html
<!DOCTYPE html>
<html lang="ko">

<head>
    <meta charset="UTF-8">
    <title>Hello, Javascript</title>
    <link rel="stylesheet" href="style.css">
</head>

<body>
    <div class="center">
        <span class="js-weather weather"></span>
        <div class="js-clock clock">
            <h1 class="js-title"></h1>
        </div>
        <form class="js-form form">
            <input class="input" type="text" placeholder="What is your name?">
        </form>
        <h4 class="js-greetings greetings"></h4>
        <form class="js-toDoForm toDo">
            <input type="text" placeholder="Write a to do">
        </form>
        <ul class="js-toDoList list"></ul>
    </div>
    <script src="js/clock.js"></script>
    <script src="js/greeting.js"></script>
    <script src="js/todo.js"></script>
    <script src="js/background.js"></script>
    <script src="js/weather.js"></script>
</body>

</html>
```

  \* 크게 날씨, 시계, 이름, ToDoList 4개로 분리하여 선언

  \* js에서 DOM을 다룰 때는 클래스 명 앞에 "js"를 설정

#### ** - css 작업**

```css
body,
html {
  overflow: hidden;
  height: 100%;
  width: 100%;
  margin: 0;
  padding: 0;
  font-family: "Open Sans", sans-serif;
}

body {
  background-size: cover;
  background-position: center center;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
}

h1,
h2,
h3,
h4,
h5,
h6 {
  margin: 0;
}

input {
  display: none;
  appearance: none;
  background: none;
  border: none;
  color: white;
  font-size: 36px;
  border-bottom: 3px solid rgba(255, 255, 255, 0.8);
  margin-bottom: 20px;
}

input::placeholder {
  color: white;
  text-align: center;
  opacity: 0.7;
}

input:focus,
input:active {
  outline: none;
}

ul {
  list-style-type: none;
  margin-top: 20px;
}

li {
  margin: 10px;
}

.center {
  display: flex;
  align-items: center;
  flex-direction: column;
}

.clock {
  font-size: 5em;
}

.greetings {
  display: none;
  font-size: 36px;
  margin-bottom: 20px;
  margin-bottom: 30px;
}

.toDo {
  font-size: 22px;
}

.toDos_btn {
  margin-left: 10px;
  cursor: pointer;
}

.weather {
  font-style: italic;
  font-size: 40px;
}

.showing {
  display: block;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.bgImage {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
  animation: fadeIn 0.5s linear;
}

```

  \* 이미지를 배경을 설정하기 위해 "z-index : -1" 활용

#### ** - JS : 실시간 시계 구현**

```javascript
const clockContainer = document.querySelector(".js-clock"),
    clockTitle = clockContainer.querySelector(".js-title");

function getTime() {
    const date = new Date();
    const minutes = date.getMinutes();
    const hours = date.getHours();
    const seconds = date.getSeconds();
    clockTitle.innerText = `${hours < 10 ? `0${hours}` : hours}:${minutes < 10 ? `0${minutes}` : minutes}:${seconds < 10 ? `0${seconds}` : seconds}`;
}

function init() {
    getTime();
    setInterval(getTime, 1000);
}

init();

```

  \* 내장된 Date 생성자 함수를 통해서 현시간 데이터 생성

  \* 삼항연산자를 통해 시분초가 한자리 단위일 때, 앞에 0 표시

  \* setInterval 메서드를 통해 1초마다 실시간 시간 갱신

  \* init에서 getTime 함수를 최초 1회 호출함으로써, 페이지 로딩과 동시에 화면에 시계 출력

#### ** - JS : 유저네임 입력 및 저장**

```javascript
const form = document.querySelector(".js-form"),
    input = form.querySelector("input"),
    greeting = document.querySelector(".js-greetings");

const USER_LS = "currentUser",
    SHOWING_CN = "showing";

function saveName(text) {
    localStorage.setItem(USER_LS, text);
}

function handleSubmit(event) {
    event.preventDefault();
    const currentValue = input.value;
    paintGreeting(currentValue);
    saveName(currentValue);
}

function askForName() {
    form.classList.add(SHOWING_CN);
    form.addEventListener("submit", handleSubmit);
}

function paintGreeting(text) {
    form.classList.remove(SHOWING_CN);
    greeting.classList.add(SHOWING_CN);
    greeting.innerText = `Hello ${text}`;
}

function loadName() {
    const currentUser = localStorage.getItem(USER_LS);
    if (currentUser === null) {
        askForName();
    } else {
        paintGreeting(currentUser);
    }
}

function init() {
    loadName();
}

init();

```

\* USER\_LS, SHOWING\_CN 변수를 별도로 만들어서 반복되는 key와 class를 효율적으로 관리

  \* localStorage의 setItem, getItem을 통해 localStorage의 데이터를 저장 및 가져올 수 있다.

  \* 태그 a 또는 form 에서 이벤트 발생 시 페이지가 이동되거나 데이터가 전송된다. 이런한 것들을 기본 이벤트들이 발생되면 페이지가 reload 된다. 그렇기 때문에 preventDefault 메서드를 통해 event가 상위로 올라가는 것을 막아 새로고침을 방지할 수 있다.  

  \* SHOWING\_CN 클래스를 추가 제거함으로써, 이름 입출력 창을 컨트롤한다.

#### ** - JS : ToDoList 구현**

```javascript
const toDoForm = document.querySelector(".js-toDoForm"),
    toDoInput = toDoForm.querySelector("input"),
    toDoList = document.querySelector(".js-toDoList");

const TODOS_LS = "toDos";

let toDos = [];

function deleteToDo(event) {
    const btn = event.target;
    const li = btn.parentNode;
    toDoList.removeChild(li);
    const cleanToDos = toDos.filter(function (toDo) {
        return toDo.id !== parseInt(li.id);
    });
    toDos = cleanToDos;
    saveToDos();
}

function saveToDos() {
    localStorage.setItem(TODOS_LS, JSON.stringify(toDos));
}

function paintToDo(text) {
    const li = document.createElement("li");
    li.className = "toDo";
    const delBtn = document.createElement("button");
    const span = document.createElement("span");
    delBtn.innerText = "X";
    delBtn.className = "toDos_btn";
    delBtn.addEventListener("click", deleteToDo);
    span.innerText = text;
    li.appendChild(span);
    li.appendChild(delBtn);
    toDoList.appendChild(li);
    const newId = toDos.length + 1;
    li.id = newId;
    const toDoObj = {
        text: text,
        id: newId
    };
    toDos.push(toDoObj);
    saveToDos();
}

function handleSubmit(event) {
    event.preventDefault();
    const currentValue = toDoInput.value;
    paintToDo(currentValue);
    toDoInput.value = "";
}

function loadToDos() {
    const loadedToDos = localStorage.getItem(TODOS_LS);
    if (loadedToDos !== null) {
        const parsedToDos = JSON.parse(loadedToDos);
        parsedToDos.forEach(function (toDo) {
            paintToDo(toDo.text);
        });
    }
}

function init() {
    toDoInput.className="showing";
    loadToDos();
    toDoForm.addEventListener("submit", handleSubmit);
}

init();

```

  \* loadToDos : 페이지를 새로고침 할 때마다 localStorage에 저장된 텍스트 값들이 painToDo 함수를 걸쳐 출력되고 저장되기에, 새로운 id를 받아 새로운 객체로 형성되어 toDos에 저장된다. (즉, deleteToDo로 출력과 id순서가 복잡해져도, reload하면 다시 정리된다)

   + JSON.parse 메서드로 localStorage에 저장된 배열꼴의 string 타입을 배열타입으로 변환시킨다.

   + forEach를 통해 불러온 배열의 각각 값(객체타입)의 text프로퍼티를 paintToDo 함수 인자로 보내, localStorage에 저장된 리스트를 출력

  \* paintToDo : DOM li, delBtn, span을 활용하여 리스트를 출력 및 추가시킨다. 이때, toDos 배열의 값들을 입력값과 id키를 가진 객체타입으로 지정해주며, id는 "ToDos.length + 1 "로 지정해줌으로 구분과 중복을 막는다. 

  \* saveToDos : localStorage안에는 string타입으로 저장되기에 stringfify 메서드로 toDos 리스트들을 string으로 변환시켜 저장한다.

  \* deleteToDo : 클릭 되어지는 버튼 자체는 li의 부모요소를 가지고 있다. 이 부모 li는 버튼과 텍스를 가지고 있음으로, 이 li를 removeChild하면 해당 리스트가 삭제된다. filter 메서드를 활용하여 해당 li 값을 삭제하여 새로운 toDos배열을 만들어 저장한다.

#### ** - JS : 랜덤 배경사진 구현**

```javascript
const body = document.querySelector("body");

const IMG_NUMBER = 5;

function paintImage(imgNumber) {
    const image = new Image();
    image.src = `images/${imgNumber+1}.jpg`;
    image.classList.add("bgImage");
    body.appendChild(image);
}

function genRandom() {
    const number = Math.floor(Math.random() * IMG_NUMBER);
    return number;
}

function init() {
    const randomNumber = genRandom();
    paintImage(randomNumber)
}

init();
```

  \* math 메서드 floor, ramdom을 활용해 0~IMG\_NUMBER 사이의 정수 생성

  \* Image 생산자 함수로 image tag 객체 생성

  \* 클래스를 활용하여 css 컨트롤

#### ** - JS : 현 지역 날씨 API 구현**

```javascript
const weather = document.querySelector(".js-weather");

const API_KEY = "aaab2e1dc827874c77c629423de9b97e";
const CORDS = 'cords';

function getWeather(lat, lon) {
    fetch(`https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${API_KEY}&units=metric`)
        .then(function (response) {
            return response.json();
        })
        .then(function (json) {
            const temperature = json.main.temp;
            const place = json.name;
            weather.innerText = `${temperature}℃ @ ${place}`;
        });
}

function saveCoords(coordsObj) {
    localStorage.setItem(CORDS, JSON.stringify(coordsObj));
}

function handleGeoSucces(position) {
    const latitude = position.coords.latitude;
    const longitude = position.coords.longitude;
    const coordsObj = {
        latitude,
        longitude
    };
    saveCoords(coordsObj);
    getWeather(latitude, longitude);
}

function handleGeoError() {
    console.log("eroorrr");
}

function askForCords() {
    navigator.geolocation.getCurrentPosition(handleGeoSucces, handleGeoError);
}

function loadCords() {
    const loadedCords = localStorage.getItem(CORDS);
    if (loadedCords === null) {
        askForCords();
    } else {
        const pareCords = JSON.parse(loadedCords);
        getWeather(pareCords.latitude, pareCords.longitude);
    }
}

function init() {
    loadCords();
}

init();
```

  \* navigator : geolocation API의 getCurrentPosion 메소드를 활용하여 현재 위치를 좌표값을 불러 올 수 있다.

  \* weather API : API의 매개변수 및 호출하는 방법은 해당 API 사이트를 참고 (openweathermap.org)

  \* fetch, then 문을 이용하여서 비동기 작업으로  API 데이터를 요청 및 응답을 가져와 활용

   + 응답받은 body 스트림을 json() 메서드를 활용하여 JSON 데이터로 변환시켜주고,

   + then 문을 한번 더 사용하여 JSON 데이터가 완전히 준비된 상태에서 필요한 속성들을 활용한다

   + 변환한 response 객체의 속성들을 확인하여 필요한 온도와 지역명만 추출하였다.

### **4\. Study**

 - quertySelector : 클래스, 아이디명을 명확하게 하여 효율적으로 관리해보자

 - event.preventDefault : input 등 데이터가 전송되어 페이지가 이동되는 기본이벤트들을 방지 

 - event.target : 해당 이벤트 타켓이 어떤 요소인지 명확히 확인

 - localStorage.getItem, setItem : 데이터를 string 타입으로 관리하오니, JSON 메서드를 활용해야 한다.

  \* JSON.parse, stringfy

 - fetch / then 구문 : 비동기로 refresh 없이 정보를 받아와 적용시키는 큰 장점 (async/await)

 - 스트림과 json() : 네트웍상 스트림으로 오가는 데이터들을 JSON화 하여 js에서 활용할 수 있다.

 - forEach : 처음부터 끝까지 함수호출하는 구문은 for문보다는 forEach문을 활용해보자.

 -appendChild,removeChild : 해당 메서드를 활용하여 보다 효율적으로 html 관리해보자

 -parseInt : 숫자형태의 문자열을 정수화

-filter : 매개변수 callback 함수의 return 값은 조건문으로 주어야 한다.

 - 기본 생성자 함수 Date, Image, Math

 - 객체의 키와 값이 동일하면 한번만 명기하여도 된다.

 - navigator API : window와 같이 기본적으로 제공되는 객체형태의 API

  \* geolocation API의 getCurrentPosion 메서드는 비동기 함수이며, 일반 콜백함수와 오류콜백함수를 지정 가능하다.

### **5\. Challenge**

 - 처음 접해보는 API와 메서드들은 개발자 도구를 통해 어떤 속성과 값을 갖는지 조사해보자.

 - class를 관리할 때, 앞에 js 등을 붙여줌으로써 css와 구분하여 효율적 관리가 가능하다.

 - localStorage에 데이터를 불러오거니 저장할 때에는 데이터가 중복되지 않도록 id 등 속성으로 관리하자.