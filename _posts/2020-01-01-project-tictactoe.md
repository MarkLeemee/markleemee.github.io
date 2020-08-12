---
title:  "toy_project_tictactoe"

layout: layout_project

categories:
  - project
tags:
  - project, toy
---
# 토이 프로젝트 - Tic Tac Toe Game

## 1\. Introduction

-   X/O 2명의 플레이어가 참여하는 Tic Tac Toe 게임 구현

## 2\. Preview

[##_Image|kage@sYfMs/btqGqPKbivN/Woed5kKUDEuaq9yXZd0Utk/img.gif|alignLeft|data-filename="Aug-07-2020 17-32-22.gif" data-origin-width="320" data-origin-height="389" width="354" height="NaN" data-ke-mobilestyle="widthContent"|||_##]

## 3\. todo

-   Start & Restart 버튼 구현
-   빈칸 클릭 시 해당 플레이어 체크 표시
-   이미 채워진 칸 클릭 시, 오류 메세지 발생
-   플레이어가 빙고가 되었을 때, 게임 결과 표시 및 종료
-   빙고 없이 모든 칸이 찼을 때, 무승부 표시

### html 작업

```html
<!doctype html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width">
  <title>Tic Tac Toe</title>
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@100;300;400;500;700;900&display=swap" rel="stylesheet" />
  <link href="./styles/index.css" rel="stylesheet" />
</head>
<body>
  <h1>Tic Tac Toe</h1>
  <div class="message">Start 버튼을 눌러 게임을 시작하세요.</div>
  <section>
    <div class="board">
      <div class="empty"></div>
      <div class="empty"></div>
      <div class="empty"></div>
      <div class="empty"></div>
      <div class="empty"></div>
      <div class="empty"></div>
      <div class="empty"></div>
      <div class="empty"></div>
      <div class="empty"></div>
    </div>
  </section>
  <div class="bottom">
    <button class="start"> Start </button>
    <button class="restart"> Restart </button>
  </div>
  <script src="./app/index.js"></script>
</body>
</html>
```

-   `message` 클래스에 현재 상태 및 결과 출력
-   게임에 필요한 9칸을 `empty` 클래스로 생성
-   `start` & `restart` 버튼은 각각 생성

### css 작업

```css
html, body {
  box-sizing: border-box;
  background-color: #FFFFFF;
  font-family: 'Roboto', sans-serif;
}

*, *:before, *:after {
  box-sizing: inherit;
}

/* Top */
h1 {
  color: blue;
  font-size: 2em;
  font-weight: 300;
  text-align: center;
}

.main-logo {
  display: block;
  margin: 2em auto 0;
  height: 100px;
}

.message{
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 20px auto;
}

/* Section */
.board {
  display: grid;
  grid-template-rows: repeat(3,1fr);
  grid-template-columns: repeat(3,1fr);
  width: 400px;
  height: 400px;
  border: 1px solid black;
  margin: 0 auto;
}

.empty {
  border:2px solid black;
}

.empty:hover{
  border:2px solid black;
  background-color: grey;
  cursor: pointer;
}

.X{
  font-size: 100px;
  border:2px solid black;
  justify-content: center;
  align-items: center;
  display: flex;
}

.O{
  font-size: 100px;
  border:2px solid black;
  justify-content: center;
  align-items: center;
  display: flex;
}

/* Bouttom */
.bottom{
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
}

.restart{
  display: flex;
}

.restart,
.start {
  background-color: grey;
  color: white;
  padding: 2em 3em;
  border: none;
  transition: all .3s ease;
  border-radius: 5px;
  letter-spacing: 2px;
  text-transform: uppercase;
  outline: none;
  align-self: center;
  cursor: pointer;
  font-weight: bold;
}

.restart:hover,
.start:hover {
  background-color:rgb(160, 182, 255);
  animation: random-bg .5s linear infinite, grow 1500ms ease infinite;
}

@keyframes random-bg {
  from {
    filter: hue-rotate(0)
  }
  to {
    filter: hue-rotate(360deg)
  }
}

@keyframes grow {
  0% {
    transform: scale(1);
  }
  14% {
    transform: scale(1.3);
  }
  28% {
    transform: scale(1);
  }
  42% {
    transform: scale(1.3);
  }
  70% {
    transform: scale(1);
  }
}

.showing{
  display: flex;
}

.hiding{
  display: none;
}

```

-   class 네임을 활용하여 `showing`과 `hiding` 컨트롤
-   버튼 활성화 효과를 넣어서 선택되어지는 버튼 구분
-   키프레임을 활용한 버튼 효과

### JS : DOM 및 이벤트, 기본 변수 선언

```javascript
const items = document.querySelectorAll('.empty');
const button = document.querySelector('.start');
const rebutton = document.querySelector('.restart');
const message = document.querySelector('.message');

const HIDING = 'hiding';

let current;
let flag;
let winMap = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
];


// ...


function init() {
    current = 'X';
    rebutton.classList.add(HIDING);
    button.addEventListener('click', action);
    rebutton.addEventListener('click', reset);
}

init();
```

-   `querySelectorAll`로 9개의 `empty` 클래스를 가져온다.
-   `HIDING` 변수를 통해 버튼 표시 컨트롤
-   `current`는 현재 순서, `flag`와 `winMap`을 활용하여 빙고를 확인
-   첫 순서로 X 플레이어 선언 및 버튼 활성화

### JS ; 초기 시작 및 재시작

```javascript
function action() {
    button.classList.add(HIDING);
    message.textContent = `${current} 차례입니다.`;
    items.forEach(function (item) {
        item.addEventListener('click', paint);
    });
}

function finish() {
    rebutton.classList.remove(HIDING);
    items.forEach(function (item) {
        item.removeEventListener('click', paint);
    });
}

function reset() {
    rebutton.classList.add(HIDING);
    items.forEach(function (item) {
        item.className = 'empty';
        item.textContent = '';
    });
    action();
}
```

-   `HIDING` 클래스 네임을 통해 버튼 활성화&비활성화
-   `message` 클래스에 현재 상태 표시
-   게임 시작 시에 `querySelectAll`로 가져온 모든 버튼에 이벤트 활성화 (종료 시에 비활성화)

### JS : 플레이어 체크 표시

```javascript
function paint(event) {
    if (event.target.className === 'empty') {
        event.target.className = current;
        event.target.textContent = current;

        if (checkWin(event)) {
            message.textContent = `${current}의 승리입니다.`;
            return finish();
        }
        if (checkDraw() === 1) {
            message.textContent = '무승부 입니다.';
            return finish();
        }
        current === 'X' ? current = 'O' : current = 'X';
        return action();
    }
    alert('이미 선택된 곳입니다.');
}
```

-   if문을 통해서 이미 선택되어진 칸인지 구분
-   빈칸일 시 선택 후 승패 및 무승부 순차적 확인 (`checkWin`, `checkDraw` 함수)
-   결과가 안났을 시에는 순서를 체인지하고 다시 `action` 함수 호출

### JS : 승패 및 무승부 확인

```javascript
function checkMap(dy) {
    flag = 0;
    for (let i = 0; i < 3; i++) {
        if (items[winMap[dy][i]].className !== current) {
            flag = 1;
        }
    }
}

function checkWin(event) {
    for (let y = 0; y < 8; y++) {
        for (let x = 0; x < 3; x++) {
            let currentMap = Array.from(items).indexOf(event.target);
            if (currentMap === winMap[y][x]) {
                checkMap(y);
                if (flag === 0) {
                    return true;
                }
            }
        }
    }
}

function checkDraw() {
    for (let i = 0; i <= 8; i++) {
        if (items[i].className === 'empty') return;
    }
    return 1;
}
```

-   전체 `empty` 클라스를 배열로 만들어 `Array` 메서드 활용
-   `indexOf`를 활용하여 현재 클릭된 타겟이 9개의 `empty` 클래스 중 몇 번째인지 확인
-   승리하는 8가지 경우의 수인 `winMap`과 비교하여 승리 확인
-   하나라도 일치하는 않으면 `flag = 1`로 선언하여 계속하여 게임 진행
-   승패 확인 후 모든 칸이 `empty`가 아닐 시에는 무승부

## 4\. Study & Challenge

-   승패를 확인하기 위하여, 총 9개의 칸들을 어떻게 구분하고 어떻게 확인할지가 키포인트 였습니다.
-   `querySelectAll`로 가져온 유사배열 형태의 `empty` 클래스들을 배열로 전환하여 각각의 index로 구분
-   또한, 승패를 확인하기 위해 대각선까지 확인하는 direct 배열 보다는, 빙고가 되는 경우의 수 맵(`winMap`)을 만들어 활용
-   게임이 끝난 뒤에는 `removeEventListener`로 버튼 이벤트 비활성화를 하여 오류를 방지