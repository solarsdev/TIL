## 클래스와 오브젝트의 차이점(class vs object), 객체지향 언어 클래스 정리

- 클래스는 연관있는 데이터를 한데 묶어놓는 컨테이너
- 속성fields과 메소드methods로 이루어져 있음
- 속성만으로 이루어져 있는 클래스를 데이터 클래스라고 부름
- 데이터를 외부에서 직접 접근할수 없도록 하는것이 캡슐화

### class, object

- 클래스는 붕어빵을 만들수 있는 붕어빵 틀에 비유 가능
  - 템플릿으로 정의됨
  - 한번만 선언함
  - 실제 데이터는 없음
- 클래스를 이용해서 만드는것이 오브젝트
  - 클래스를 이용해서 여러번 만드는것이 가능
  - 실제 데이터가 들어있음

### Class

- 템플릿
- ES6 이전에는 클래스가 존재하지 않음
- 그 이전에는 클래스를 정의하지 않고 바로 오브젝트를 생성했음
- 완전하게 새로 들어온 개념이 아니기 때문에, 기존 프로토타입 베이스의 상속의 형태로 만들었음
  - syntactical sugar (문법적으로 달달할뿐 실제 개념이 아님)

### Class 선언

```jsx
class Person {
  // constructor
  constructor(name, age) {
    //fields
    this.name = name;
    this.age = age;
  }
  // methods
  speak() {
    console.log(`${this.name}: hello!`);
  }
}
```

- Person 클래스를 정의
- 생성자에 name, age를 매개변수로 입력받아 속성을 정의
- speak() 메소드를 이용하여 hello! 라는 문자열을 출력

### Object 생성

```jsx
const ellie = new Person('ellie', 20);
console.log(ellie.name);
console.log(ellie.age);
ellie.speak();
```

- ellie 라는 Person을 이용한 오브젝트를 생성하고, 속성과 메소드를 테스트

### Getter & Setter

- 클래스를 정의할때 Getter, Setter 개념이 있음
- Getter
  - 속성값을 가져오는 메소드
  - get props()
- Setter
  - 속성값을 설정하는 메소드
  - set props()
- 속성을 직접 호출하거나 할당이 가능한데 왜 굳이 Getter와 Setter를 사용할까?
  - 사용자가 클래스를 사용할때 클래스 제작자의 의도 내에서 사용할 수 있도록 동작을 제어하기 위함
- Getter나 Setter가 정의되어 있으면 메모리에 직접 접근해서 할당하거나 값을 가져오는것이 아니라, get set 메소드를 호출해서 작업을 처리함

```jsx
class User {
  constructor(firstName, lastName, age) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.age = age;
  }

  get age() {
    return this._age;
  }

  set age(value) {
    this._age = value < 0 ? 0 : value;
  }
}

const user1 = new User('steve', 'jobs', -1);
console.log(`${user1.firstName} ${user1.lastName}: age ${user1.age}`);
// output age is 0 not -1
```

### Public & Private

- private 속성에는 # 기호를 붙이면 되지만, 최근에 추가된 개념이기 때문에 사용하기에는 시기상소

### Static

- static 이 붙은 속성은 클래스 고유의 변수가 됨
- 오브젝트에 상관없이 클래스에서 공통적으로 사용되는것이라면 static을 사용하는것이 좋음

### 상속과 다형성

- 상속은 클래스의 코드 중복처리를 줄이는데 도움이 됨
- 관련있는 클래스의 공통 부분을 상위 개념으로 모아서 부모 클래스를 만들고, 구체화된 클래스가 공통 개념을 가진 클래스를 상속하게 하면 됨

```jsx
class Shape {
  constructor(width, height, color) {
    this.width = width;
    this.height = height;
    this.color = color;
  }

  draw() {
    return `drawing ${this.color} color`;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Rectangle extends Shape {
  draw() {
    return '▬';
  }
}
class Triangle extends Shape {
  draw() {
    return '🔺';
  }
  getArea() {
    return (this.width * this.height) / 2;
  }
}

const rectangle = new Rectangle(20, 20, 'blue');
const triangle = new Triangle(20, 20, 'red');

console.log(rectangle.draw());
console.log(rectangle.getArea());
console.log(triangle.draw());
console.log(triangle.getArea());
```

- 상기 코드는 삼각형과 사각형을 구현함에 있어, 공통이 되는 면적 구하기나 도형 그리기를 shape 클래스로 집약시켜 공통화 시키고, 이를 상속한 삼각형 클래스와 사각형 클래스에서 메소드의 오버라이드 기능을 이용해서 각각의 도형에 맞는 방식으로 메소드의 재구현을 실행함

### instanceOf 연산자

- {object} instanceOf {class}
- 특정 오브젝트가 클래스에서 만들어진 인스턴스인지 확인하는 연산자

### JavaScript Refference

[JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript)

- 자바스크립트 빌트인 오브젝트들을 확인할수 있음

## What is Object?

- 먼저 오브젝트에 대한 자바스크립트 레퍼런스를 확인할 수 있는 링크
  [Object - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)

### Literals and Properties

- primitive 타입은 변수 하나당 값 한개만 저장 할 수 있음
- 따라서 함수의 매개변수에도 각각의 매개변수를 따로 따로 전달해주어야 함
  - 이렇게 할 경우, 매개변수가 많아질 경우 관리가 어려워짐
  - 이를 개선하려면 매개변수에 object 타입의 객체를 전달해서 여러가지 정보를 한번에 처리 가능
- 오브젝트를 작성하려면 중괄호를 이용해서 직접 만들수 있음 (object literal)
- 또는 new Object(); 를 이용해서 오브젝트를 만들 수도 있음 (object constructor syntax)
  - 클래스를 만들어서 해당 클래스의 constructor를 호출하는 방식
- 자바스크립트의 특징으로 동적 타입 언어가 있는데, 이를 통해 오브젝트를 생성하고 난 뒤에 추가로 속성을 정의하는것이 가능함
  ```jsx
  const user = { name: 'ellie', age: 20 };
  user.hasJob = true;
  // 이와 같이 나중에 hasJob 속성을 추가하는것도 가능
  // 권장되는 방법은 아님 (유지보수의 문제)
  delete user.hasJob;
  // 추후 삭제도 가능
  ```
- Object는 키와 밸류로 이루어진 집합체

### Computed Properties

- Object의 속성에 접근하기 위한 2가지 방법
  - object.propName
  - object[’propName’] - 키는 항상 스트링으로 지정해야 함
- 2가지 방법을 어떤 타이밍에 사용해야 할까
- dot 방식은 코딩시에 사용
- 실시간으로 원하는 키의 값을 받아오고 싶다면 [’propName’]을 사용
  ```jsx
  function printValue(obj, key) {
    console.log(obj.key);
  }
  printValue(user, 'name');
  // output: undefined
  ```
  - 키의 입력이 런타임에서 정해지는 경우에는 [’’]를 통해서 코딩시점에 존재하지 않는 키를 명시적으로 사용할 수 있음
  - 정리하면 동적으로 key에 대한 값을 받아와야 할때 사용

### Property value shorthand

```jsx
function makePerson(name, age) {
  return {
    name, // name: name
    age, // age: age
  };
}
```

### Constructor Function

- 상기 makePerson과 같이 매개변수에서 입력받은 값을 토대로 오브젝트를 반환하는 함수의 경우
  - 클래스에서 인스턴스를 작성하는것과 의미 동일
  - ES6 이전에는 클래스의 개념이 존재하지 않았음
- 이러한 이유로 절차지향적 방식으로 클래스 흉내를 냈는데, 이러한 클래스 like 함수를 constructor function이라고 부름

```jsx
function Person(name, age) {
  // this = {};
  this.name = name;
  this.age = age;
  // return this;
}
```

### In Operator

- in 연산자의 경우에는 오브젝트 안에 해당 속성이 존재하는지 확인할때 사용함

```jsx
console.log('name' in ellie); // true
console.log('age' in ellie); // true
console.log('random' in ellie); // false
console.log(ellie.random); // undefined
```

### for..in vs for..of

- for ... in
  - for (key in obj)
  - 오브젝트에 있는 모든 키를 나열하는 방법
  ```jsx
  const ellie = new Person('ellie', 20);
  for (let key in ellie) {
    console.log(key);
  }
  ```
- for ... of
  - for (value of iterable)
  - 배열에 있는 모든 값을 나열하는 방법
  ```jsx
  const array = [1, 2, 3, 4, 5];
  for (let value of array) {
    console.log(value);
  }
  ```

### Cloning

- 일반적으로 변수로 관리하는 오브젝트를 다른 변수에 할당하게 되면 오브젝트가 복사되어 할당되는것이 아닌 ref값이 동일하게 됨
- 따라서 새롭게 할당된 변수에서 값을 조작하면 기존 변수에 할당되어 있는 오브젝트 또한 값이 변경됨
- 오브젝트를 복사해서 복사된 오브젝트의 ref값을 참조하도록 하는 방법

```jsx
// old way
const user = new Person('user', 20);
const user2 = {};
for (let key in user) {
  user2[key] = user[key];
}
console.log(user2);

// new way
const user3 = Object.assign({}, user);
console.log(user3);
```

- 오브젝트를 assign 할때 속성이 동일한 두개의 오브젝트를 결합할때 결과

```jsx
const fruit1 = { color: 'red' };
const fruit2 = { color: 'blue', size: 'big' };
const mixed = Object.assign({}, fruit1, fruit2);
console.log(mixed.color); // blue
console.log(mixed.size); // big
// 뒤에서 결합될때 동일한 속성을 덮어씌운다
```
