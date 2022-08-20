# Regex

## 공부용 사이트

[RegexOne - Learn Regular Expressions - Lesson 1: An Introduction, and the ABCs](https://regexone.com/)

## Groups and ranges

`|` 또는

`()` 그룹

`[]` 문자셋, 괄호안의 어떤 문자든

`[^]` 부정 문자셋, 괄호안의 어떤 문자가 아닐때

`(?:)` 찾지만 기억하지는 않음

## Quantifiers

`?` 없거나 있거나 (zero or one)

`*` 없거나 있거나 많거나 (zero or more)

`+` 하나 또는 많이 (one or more)

`{n}` n번 반복

`{min,}` 최소

`{min,max}` 최소, 최대

## Boundary-type

`\b` 단어 경계

`\B` 단어 경계가 아님

`^` 단어의 시작

`$` 단어의 끝

## Character classes

`\` 특수 문자가 아닌 문자

`.` 어떤 글자 (줄바꿈 문자 제외)

`\d` digit 숫자

`\D` digit 숫자 아님

`\w` word 문자

`\W` word 문자 아님

`\s` space 공백

`\S` space 공백 아님
