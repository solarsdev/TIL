# CSS : Box model

- 기본적으로 박스(div)는 content-box로 정의됨
- content-box는 컨텐츠 내용에 영향을 미치지 않고, 테두리를 바깥에 정의함
- width와 height가 border를 포함해서 전체 사이즈를 정의하도록 하고 싶을때는 box-sizing: border-box를 이용하면 됨

[box-sizing - CSS: Cascading Style Sheets | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/box-sizing)

```css
.box {
  width: 100px;
  height: 100px;
  background-color: red;
}
.inner {
  width: 100%;
  height: 100%;
  background-color: blue;
}
.box2 {
  padding: 20px;
  box-sizing: content-box;
  border: 10px solid black;
}
.box3 {
  padding: 20px;
  box-sizing: border-box;
  border: 10px solid black;
}
```

[![Run on Repl.it](https://repl.it/badge/github/sherlock-project/sherlock)](https://replit.com/@solarsdev/box-sizing)

# Absolute vs Static

- 블럭의 포지션 기본값은 static인데, 포지션이 static인 블럭의 top,right,bottom,left는 의미가 없음
- relative는 static일때의 기준으로 정해진 상대적인 위치만큼 이동하는것을 말함
- position을 absolute로 주게 되면, 인접한 **static이 아닌** 부모의 기준에서 위치가 정해짐

[position - CSS: Cascading Style Sheets | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/position)

[Layout and the containing block - CSS: Cascading Style Sheets | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Containing_Block)

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <!-- https://developer.mozilla.org/en-US/docs/Web/CSS/position -->
    <!-- https://developer.mozilla.org/en-US/docs/Web/CSS/Containing_Block -->
    <style>
      .outer {
        width: 200px;
        height: 200px;
        margin-bottom: 20px;
        background-color: green;
      }
      .box {
        width: 100px;
        height: 100px;
        background-color: red;
        position: relative;
      }
      .inner {
        width: 100%;
        height: 50%;
      }
      .inner1 {
        background-color: blue;
      }
      .inner2 {
        background-color: yellow;
      }

      .box1 .inner1 {
        position: relative;
        top: 30px;
        left: 100px;
      }

      .box2 .inner1 {
        position: absolute;
        top: 30px;
        left: 100px;
      }
    </style>
  </head>
  <body>
    <div class="outer">
      <div class="box box1">
        <div class="inner inner1">inner1</div>
        <div class="inner inner2">inner2</div>
      </div>
    </div>
    <div class="outer">
      <div class="box box2">
        <div class="inner inner1">inner1</div>
        <div class="inner inner2">inner2</div>
      </div>
    </div>
  </body>
</html>
```

[![Run on Repl.it](https://repl.it/badge/github/sherlock-project/sherlock)](https://replit.com/@solarsdev/position)

# Sticky vs Fixed

- static, relative, sticky는 감싸고 있는 컨테이너 블록을 기준으로 위치변경이 일어남
- sticky는 static이 아닌 부모를 기준으로 화면상에 위치를 자리하다가, 화면밖으로 벗어나면 따라옴
- fixed의 경우에는 부모와 상관없이 뷰포트를 기준으로 정렬하게 됨

[position - CSS: Cascading Style Sheets | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/position)

[Layout and the containing block - CSS: Cascading Style Sheets | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Containing_Block)

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <!-- https://developer.mozilla.org/en-US/docs/Web/CSS/position-->
    <!-- https://developer.mozilla.org/en-US/docs/Web/CSS/Containing_Block -->
    <style>
      .container {
        position: relative;
        top: 100px;
        left: 100px;
        background-color: beige;
      }

      .box {
        width: 80px;
        height: 80px;
        margin-bottom: 20px;
        background-color: chocolate;
      }

      .box2 {
        background-color: hotpink;
        position: sticky;
        top: 20px;
        left: 20px;
      }

      .box3 {
        background-color: blue;
        position: fixed;
        top: 20px;
        left: 20px;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="box box1"></div>
      <div class="box box2"></div>
      <div class="box box3"></div>
      <div class="box box4"></div>
      <div class="box box5"></div>
      <div class="box box6"></div>
      <div class="box box7"></div>
      <div class="box box8"></div>
      <div class="box box9"></div>
      <div class="box box10"></div>
    </div>
  </body>
</html>
```

[![Run on Repl.it](https://repl.it/badge/github/sherlock-project/sherlock)](https://replit.com/@solarsdev/position-sticky)
