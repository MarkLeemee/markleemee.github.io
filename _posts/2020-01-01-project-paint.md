---
title:  "toy_project_paint_webapp"

layout: layout_project

categories:
  - project
tags:
  - project, toy
---
### **\# 토이 프로젝트 - Paint Paint WebApp by vanilla JS**

### **1\. Introduction**

 - vanilla js를 활용하여 웹 페이지에서 구현되는 그림판 앱 클론코딩 및 복기

### **2\. Preview**

[##_Image|kage@nTikc/btqGdEqFZkb/LHGFtrKEjufoBpbLOmnHuk/img.gif|alignLeft|data-filename="Aug-05-2020 12-25-10.gif" data-origin-width="530" data-origin-height="654" width="350" data-ke-mobilestyle="widthContent"|||_##]

### **3\. todo**

 - 마우스 클린된 상태에서 선 그리기

 - 색상 및 선 너비 변경

 - 캔버스 전체 채우기

 - mode 버튼을 활용하여 paint 모드와 fill 모드 변경

 - save 버튼으로 해당 그림 저장

#### ** - html 작업**

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <link rel="stylesheet" href="styles.css" />
    <title>PaintJS</title>
</head>

<body>
    <canvas id="jsCanvas" class="canvas"></canvas>
    <div class="controls">
        <div class="controls__range">
            <input type="range" id="jsRange" min="0.1" max="5" value="2.5" step="0.1" />
        </div>
        <div class="controls__btns">
            <button id="jsMode">NOW : Paint</button>
            <button id="jsSave">Save</button>
        </div>
        <div class="controls__colors" id="jsColors">
            <div class="controls__color jsColor" style="background-color:#2c2c2c"></div>
            <div class="controls__color jsColor" style="background-color:white"></div>
            <div class="controls__color jsColor" style="background-color:#FF3B30"></div>
            <div class="controls__color jsColor" style="background-color:#FF9500"></div>
            <div class="controls__color jsColor" style="background-color:#FFCC00"></div>
            <div class="controls__color jsColor" style="background-color:#4CD963"></div>
            <div class="controls__color jsColor" style="background-color:#5AC8FA"></div>
            <div class="controls__color jsColor" style="background-color:#0579FF"></div>
            <div class="controls__color jsColor" style="background-color:#5856D6"></div>
        </div>
    </div>
    <script src="app.js"></script>
</body>

</html>

```

  \* 자바스크립트는 id, css는 class로 각각 나눠서 활용

  \* html canvas 요소를 활용하여 캔버스 구현

  \* range 타입의 input으로 선 너비 컨트롤바 구현

  \* 각각의 색상은 클래스 네임을 통일시키고, 색상은 css가 아닌 html에 직접 선언

   + 추후 js에서도 색변경시 해당 style 속성을 그대로 사용한다.

#### ** - css 작업**

```css
@import "reset.css";

body {
  background-color: #f6f9fc;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
    Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 50px 0px;
}

.canvas {
  width: 700px;
  height: 700px;
  background-color: white;
  border-radius: 15px;
  box-shadow: 0 4px 6px rgba(50, 50, 93, 0.11), 0 1px 3px rgba(0, 0, 0, 0.08);
}

.controls {
  margin-top: 80px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.controls .controls__btns {
  margin-bottom: 30px;
}

.controls__btns button {
  all: unset;
  cursor: pointer;
  background-color: white;
  padding: 5px 0px;
  width: 80px;
  text-align: center;
  border-radius: 5px;
  box-shadow: 0 4px 6px rgba(50, 50, 93, 0.11), 0 1px 3px rgba(0, 0, 0, 0.08);
  border: 2px solid rgba(0, 0, 0, 0.2);
  color: rgba(0, 0, 0, 0.7);
  text-transform: uppercase;
  font-weight: 800;
  font-size: 12px;
}

.controls__btns button:active {
  transform: scale(0.98);
}

.controls .controls__colors {
  display: flex;
}

.controls__colors .controls__color {
  width: 50px;
  height: 50px;
  border-radius: 25px;
  cursor: pointer;
  box-shadow: 0 4px 6px rgba(50, 50, 93, 0.11), 0 1px 3px rgba(0, 0, 0, 0.08);
}

.controls .controls__range {
  margin-bottom: 30px;
}

```

  \* 기본적으로 [meyerweb.com](http://meyerweb.com) 의 reset.css 셋팅

  \* 폰트 및 그림자 값은 외부값 활용

  \* all: unset; 활용하여 상속

\* border-radius, text-transform 등을 활용하여 모양, 텍스트 변경

\* transform을 활용하여 acrive 되었을데 효과 구현

#### ** - JS : DOM 및 이벤트, 기본 변수 선언**

```javascript
const canvas = document.getElementById("jsCanvas");
const ctx = canvas.getContext("2d");
const colors = document.getElementsByClassName("jsColor");
const range = document.getElementById("jsRange");
const mode = document.getElementById("jsMode");
const saveBtn = document.getElementById("jsSave");

const INITIAL_COLOR = "#2c2c2c";
const CANVAS_SIZE = 700;

canvas.width = CANVAS_SIZE;
canvas.height = CANVAS_SIZE;

ctx.fillStyle = "white";
ctx.fillRect(0, 0, CANVAS_SIZE, CANVAS_SIZE);
ctx.strokeStyle = INITIAL_COLOR;
ctx.fillStyle = INITIAL_COLOR;
ctx.lineWidth = 2.5;

let painting = false;
let filling = false;


// ...


if (canvas) {
    canvas.addEventListener("mousemove", onMouseMove);
    canvas.addEventListener("mousedown", startPainting);
    canvas.addEventListener("mouseup", stopPainting);
    canvas.addEventListener("mouseleave", stopPainting);
    canvas.addEventListener("click", handleCanvasClick);
    canvas.addEventListener("contextmenu", handleCM);
}

Array.from(colors).forEach(item =>
    item.addEventListener("click", handleColorClick)
);

if (range) {
    range.addEventListener("input", handleRangeChange);
}

if (mode) {
    mode.addEventListener("click", handleModeClick);
}

if (saveBtn) {
    saveBtn.addEventListener("click", handleSaveClick);
}

```

  \* 모든 이벤트들은 if문안에 넣어서 존재하는 경우에만 발생하도록 셋팅

  \* 같은 클래스 이름의 colors의 경우 array로 변환하여 이벤트를 호출

#### ** - JS ; 선 그리기 및 채우기 구현**

```javascript
function stopPainting() {
    painting = false;
}

function startPainting() {
    painting = true;
}

function onMouseMove(event) {
    const x = event.offsetX;
    const y = event.offsetY;
    if (!painting) {
        ctx.beginPath();
        ctx.moveTo(x, y);
    } else {
        ctx.lineTo(x, y);
        ctx.stroke();
    }
}

function handleCanvasClick() {
    if (filling) {
        ctx.fillRect(0, 0, CANVAS_SIZE, CANVAS_SIZE);
    }
}
```

  \* startPainting, stopPainting 함수를 활용하여 마우스 클릭 상태를 저장

  \* offeset 메서드를 활용해 evnet target 내의 좌표값 담기 

  \* beginPath, moveTo 메서드로 클릭 비활성화 시에 계속 시작점 셋팅

  \* lineTo, stroke 클릭 활성화 되었을 때에 시작점과 현재점의 라인을 잇고, stroke style로 선을 구현한다.

  \* fillRect로 0,0부터 캔버스 전체사이즈까지 채워버린다

#### ** - JS : 색상 및 컨트롤 바 구현**

```javascript
function handleColorClick(event) {
    const color = event.target.style.backgroundColor;
    ctx.strokeStyle = color;
    ctx.fillStyle = color;
}

function handleRangeChange(event) {
    const size = event.target.value;
    ctx.lineWidth = size;
}

function handleModeClick() {
    if (filling === true) {
        filling = false;
        mode.innerText = "NOW : Paint";
    } else {
        filling = true;
        mode.innerText = "NOW : Fill";
    }
}

function handleSaveClick() {
    const image = canvas.toDataURL();
    const link = document.createElement("a");
    link.href = image;
    link.download = "완성 이미지 파일";
    link.click();
}
```

  \* 간단한 if 문으로 모드변경 버튼 구현

  \* 그림파일 저장은 canvas 와 a 태그 메서드를 통해 구현

4.  study

### **5\. Challenge**

 - canvas, a 등 html의 속성 및 메서드들은 mdn을 통해서 학습

  \* 해당 기능이나 매개변수 등

 - 중복되거나 자주사용하는 값은 변수로 선언하여 활용하자

  \* 어떤 값으로 초기값 설정할지도 유의

 - css선택자 우선순위에 항시 확인하자