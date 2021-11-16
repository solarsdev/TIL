# CSS : Box model

- 기본적으로 박스(div)는 content-box로 정의됨
- content-box는 컨텐츠 내용에 영향을 미치지 않고, 테두리를 바깥에 정의함
- width와 height가 border를 포함해서 전체 사이즈를 정의하도록 하고 싶을때는 box-sizing: border-box를 이용하면 됨
- [box-sizing - CSS: Cascading Style Sheets | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/box-sizing)

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

[![Run on Repl.it](https://repl.it/badge/github/sherlock-project/sherlock)](https://replit.com/@solarsdev/NotionCSS)
