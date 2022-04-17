# 클래스, 최신 문법

## 클래스 소개

- 생성자 함수를 이용해서 다양한 객체를 생성할 수 있음
- 생성자 함수는 공통된 인수를 전달하면 비슷한 형태의 객체를 만들어주는 함수를 말함
- `생성자 함수는 일종의 프로토타입`이라고 부를 수 있음
- 자바스크립트에서는 최근에는 클래스를 이용한 객체지향형 프로그래밍을 지원하고 있음
- 클래스는 생성자 함수와 같이 `객체를 생성할수 있는 일종의 템플릿` (청사진, 틀)
- `객체지향 프로그래밍`을 수행할 수 있음
- 생성자 함수를 이제 클래스로 대체해서 사용
- 클래스에서 생성된 객체들을 인스턴스라고 부름

## 클래스의 기본

[class - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/class)

```jsx
// 객체를 손쉽게 만들수 있는 템플릿
// 1. 생성자 함수 (오래된 고전적인 방법)
// 2. 클래스

// 클래스 class
class Fruit {
  // 생성자: new 키워드로 클래스를 생성할때 호출되는 함수
  constructor(name, emoji) {
    this.name = name;
    this.emoji = emoji;
  }

  display = () => {
    console.log(`${this.name}: ${this.emoji}`);
  };
}

// apple은 Fruit 클래스의 인스턴스이다.
const apple = new Fruit('apple', '🍎');
// orange은 Fruit 클래스의 인스턴스이다.
const orange = new Fruit('orange', '🍊');
console.log(apple);
console.log(orange);
apple.display();
```

## 재사용성을 높이는 방법

- 클래스 내에 정의된 프로퍼티와 메소드는 인스턴스 레벨의 프로퍼티임
- 인스턴스 레벨의 프로퍼티?
  - 인스턴스를 생성할때마다 해당 내용이 중복적으로 생성되기 때문
- 각각의 객체마다 다른 속성값을 가져야 한다면 인스턴스 레벨로 생성해도 괜찮으나, 모든 객체들이 공유해야 하는 속성값이 있다면?
  - 클래스 레벨의 프로퍼티와 메소드를 생성하면 됨
  - 이때는 지시어 static을 이용해서 프로퍼티와 메소드를 생성하면 되는데, 이렇게 생성된 프로퍼티와 메소드는 클래스상에 존재하게 됨
  - 이 경우에는 호출방식도 달라지게 되는데, 인스턴스를 생성한뒤에 호출하는것이 아닌, 클래스명.속성과 같이 사용하게 됨
    ```jsx
    let apple = {
      name: 'apple',
      emoji: '🍎',
    };
    Fruit.make();
    ```

```jsx
// static 정적 프로퍼티, 메소드
class Fruit {
  static MAX_FRUITS = 4;
  constructor(name, emoji) {
    this.name = name;
    this.emoji = emoji;
  }

  // 클래스 레벨의 메소드
  static makeRandomFruit() {
    // 클래스 레벨의 메소드에서는 this를 참조할 수 없음
    // 왜? -> 클래스 레벨에서는 아무것도 채워진것이 없는 템플릿 상태이기 때문임
    return new Fruit('banana', '🍌');
  }
  // 인스턴스 레벨의 메소드
  display = () => {
    console.log(`${this.name}: ${this.emoji}`);
  };
}

const banana = Fruit.makeRandomFruit();
console.log(banana);
console.log(banana.display());

Math.pow();
Number.isFinite(1);
```

## 필드란

```jsx
// 접근제어자 - 캡슐화
// private(#), public(기본), protected
class Fruit {
  #name;
  #emoji;
  #type = '과일';
  constructor(name, emoji) {
    this.name = name;
    this.emoji = emoji;
  }
  display = () => {
    console.log(`${this.name}: ${this.emoji}`);
  };
}

const apple = new Fruit('apple', '🍎');
//apple.#name = '오렌지'; // #field는 외부에서 접근이 불가능함
console.log(apple);
```

## 세터와 게터

```jsx
// 접근자 프로퍼티 (Accessor Property)
class Student {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  get fullName() {
    return `${this.lastName} ${this.firstName}`;
  }

  set fullName(value) {
    console.log(value);
  }
}

const student = new Student('수지', '김');
console.log(student.firstName);
student.firstName = '안나';
console.log(student.fullName);
student.fullName = '이상국';
```

## 상속

[class - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/class)

```jsx
// class Tiger {
//   constructor(color) {
//     this.color = color;
//   }
//   eat() {
//     console.log('먹자!');
//   }
//   sleep() {
//     console.log('잔다');
//   }
// }

// class Dog {
//   constructor(color) {
//     this.color = color;
//   }
//   eat() {
//     console.log('먹자!');
//   }
//   sleep() {
//     console.log('잔다');
//   }
//   play() {
//    console.log('놀자');
//  }
// }

class Animal {
  constructor(color) {
    this.color = color;
  }
  eat() {
    console.log('먹자!');
  }
  sleep() {
    console.log('잔다');
  }
}

class Tiger extends Animal {}
const tiger = new Tiger('노랑이');
console.log(tiger);
tiger.eat();
tiger.sleep();

class Dog extends Animal {
  constructor(color, ownerName) {
    super(color);
    this.ownerName = ownerName;
  }
  // 오버라이딩 overriding
  // 부모의 함수 내용을 자식 클래스에 맞게 덮어씌우기
  eat() {
    console.log('강아지가 먹는다!');
  }
  // 오버라이딩의 다른 예
  // 부모의 메소드를 사용하면서 추가적으로 특정 기능을 구현해야 할 때
  sleep() {
    super.sleep();
    console.log('강아지는 3시간만 잡니다');
  }
  // 강아지에 맞는 메소드를 따로 구현해줌
  play() {
    console.log('놀자');
  }
}
const dog = new Dog('빨강이', '주인님');
dog.eat();
dog.sleep();
dog.play();
```

## 퀴즈1 → 카운터 클래스 작성

```jsx
// 카운터를 만들기
// 0 이상의 값으로 초기화 한 뒤 하나씩 숫자를 증가할 수 있는 카운터를 만들기
// Counter 클래스 만들기

class Counter {
  #value;
  constructor(num) {
    if (isNaN(num) || num < 0) {
      this.#value = 0;
      return;
    }
    this.#value = num;
  }
  increase() {
    this.#value++;
  }
  get value() {
    return this.#value;
  }
}

const counter = new Counter(5);
counter.increase();
counter.increase();
console.log(counter.value);
```

## 퀴즈2 → 사원 클래스 작성

```jsx
// 정직원과 파트타임 직원을 나타낼 수 있는 클래스를 만들어보자
// 직원들의 정보: 이름, 부서이름, 한달 근무 시간
// 매달 직원들의 정보를 이용해서 한달 월급을 계산할 수 있다
// 정직원은 시간당 10000원
// 파트타임 직원은 시간당 8000원
class Employee {
  _name;
  _departmentName;
  _monthlyWorkingHour;
  _payForWorkingHour;

  constructor(name, departmentName, monthlyWorkingHour, payForWorkingHour) {
    this._name = name;
    this._departmentName = departmentName;
    this._monthlyWorkingHour = monthlyWorkingHour;
    this._payForWorkingHour = payForWorkingHour;
  }

  get name() {
    return this._name;
  }

  get departmentName() {
    return this._departmentName;
  }
  get monthlyWorkingHour() {
    return this._monthlyWorkingHour;
  }

  get payForWorkingHour() {
    return this._payForWorkingHour;
  }

  get salary() {
    return this.monthlyWorkingHour * this.payForWorkingHour;
  }
}

class EmployeeFullTime extends Employee {
  static #PAY_FOR_WORKING_HOUR = 10000;

  constructor(name, departmentName, monthlyWorkingHour) {
    super(
      name,
      departmentName,
      monthlyWorkingHour,
      EmployeeFullTime.#PAY_FOR_WORKING_HOUR
    );
  }
}

class EmployeePartTime extends Employee {
  static #PAY_FOR_WORKING_HOUR = 8000;

  constructor(name, departmentName, monthlyWorkingHour) {
    super(
      name,
      departmentName,
      monthlyWorkingHour,
      EmployeePartTime.#PAY_FOR_WORKING_HOUR
    );
  }
}

const fulltime = new EmployeeFullTime('이상국', '엔지니어링', 720);
console.log(fulltime.salary);
```
