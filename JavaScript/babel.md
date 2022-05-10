# 바벨 맛보기

## 바벨이란?

[ECMAScript 6 compatibility table](https://kangax.github.io/compat-table/es6/)

- 바벨을 사용하면 바닐라 자바스크립트의 최신 버전에서 구버전으로 빌드가 가능
- 타입스크립트를 사용하면 컴파일 기능 + 구버전 자바스크립트로 빌드가 가능
- 바벨을 사용해서 불필요하게 너무 하위 버전까지 지원하려고 하면 코드 자체의 양이 늘어나므로 주의가 필요함 (퍼포먼스에 영향이 있음)

## 프로젝트에 바벨 셋업하기

```bash
npm init --yes
npm install --save-dev @babel/core @babel/cli @babel/preset-env
```

```json
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "targets": {
          "ie": "8",
          "firefox": "12",
          "chrome": "12",
          "safari": "11.1"
        },
        "useBuiltIns": "usage",
        "corejs": "3.6.5"
      }
    ]
  ]
}
```

```json
{
  "name": "babel",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "build": "babel index.js -w -o build/index.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@babel/cli": "^7.17.10",
    "@babel/core": "^7.17.10",
    "@babel/preset-env": "^7.17.10"
  }
}
```
