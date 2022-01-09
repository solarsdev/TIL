# 코딩의 기본 operator, if, for loop 코드리뷰 팁

- 다른 프로그래밍 언어에서도 활용할수 있는 연산을 공부해보자

## String concatenation (문자열 합치기)

- 기본적으로는 + 기호를 이용해서 더하기 연산을 할 경우 문자열이 합쳐짐
- string과 다른 타입을 + 연산으로 더하면 결과 string이 됨
- 백틱으로 string literal을 만들수 있음
  - string literal은 중간에 탭을 넣거나 띄어쓰기나 줄바꿈을 하더라도 그대로 전부 들어감
  - 따옴표 안에서는 줄바꿈은 \n 탭은 \t 등으로 정해진 리터럴이 존재함

## Numeric operators (사칙연산)

- -
- -
- /
- -
- %
- \*\* (제곱)

## Increment and Decrement operators (증감연산)

- ++
- —
- 변수 앞에 쓰느냐 뒤에 쓰느냐에 따라서 pre, post로 나누어 지며 pre는 변수값을 먼저 증감시키고 다음 연산을 진행, post는 한줄이 끝나고 나서 변수를 증감시킴

## = operators (equal, 할당 연산자)

- = 할당

## logical operators (논리연산자)

- ||
- &&
- !

## Equality operators

- ==
  - loose equality : 타입을 변경해서 비교함
- ===
  - strict equality : 타입을 신경써서 비교함
- object에서의 equality
  - object에서는 reference를 저장하게 되므로, 각각의 ref는 값이 같다고 하더라도 다른 레퍼런스를 참조할 수 있음
  - 따라서 object에 equality는 ref값이 같은지 아닌지를 체크하게 되마

## Conditional operators

- if
- ?
- switch

## Loops

- while
- do while
- for
