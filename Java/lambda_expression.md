# Lambda Expression

## 함수형 언어

- 자바는 객체지향(`OOP`) 언어지만, `JDK 1.8`부터 함수형 언어의 기능을 포함했음
- 함수형 언어로 유명한것들 `Haskell`, `Scala`, …
- 빅데이터로 인해 함수형 언어들이 점점 필요하게 되고 있음
- `Python`, `JavaScript`등이 `OOP`와 함수형 언어의 기능을 동시에 포함하고 있음

## 람다식

### 함수(메서드)를 간단한 “식(`expression`)”으로 표현하는 방법

```java
// normal method
int max(int a, int b) {
	return a > b ? a : b;
}

// to expression
(a, b) -> a > b ? a : b
```

### 익명 함수 (이름이 없는 함수, `anonymous function`)

```java
// normal method
int max(int a, int b) {
	return a > b ? a : b;
}

// to expression
~~int max~~(int a, int b) -> {
	return a > b ? a : b;
}
```

### 함수와 메서드의 차이

- 근본적으로는 동일
- 함수는 일반적 용어, 메서드는 객체지향개념 용어
- 함수는 클래스에 독립적, 메서드는 클래스에 종속적

## 람다식 작성하기

1. 메서드의 이름과 반환타입을 제거하고 “`→`”를 블록 `{}` 앞에 추가
   - 메서드를 간단하게 표현하기 위한 것이 람다식
2. 반환값이 있는 경우, 식이나 값만 적고 `return` 문을 생략 가능 (끝에 “`;`” 안붙임)

   ```java
   (int a, int b) -> {
   	return a > b ? a : b;
   }

   // to one line
   (int a, int b) -> a > b ? a : b
   ```

3. 매개변수의 타입이 추론 가능하면 생략 가능 (대부분의 경우 생략가능)

   ```java
   (int a, int b) -> a > b ? a : b

   // simplify
   (a, b) -> a > b ? a : b
   ```

### 주의사항

1. 매개변수가 하나인 경우 괄호`()` 생략 가능 (타입이 없을 때만)

   ```java
   (a) -> a * a
   (int a) -> a * a

   a -> a * a // OK
   int a -> a * a // Not OK
   ```

2. 블록안의 문장이 하나뿐일때, 블럭`{}` 생략 가능 (끝에 “`;`” 안붙임)
3. 단, 하나뿐인 문장이 `return`문이면 블럭`{}` 생략불가

## 람다식은 익명 함수가 아닌 익명 객체

### 람다식은 익명 함수가 아니라 익명 객체

```java
(a, b) -> a > b ? a : b

// Originally
new Object() {
	int max(int a, int b) {
		return a > b ? a : b;
	}
}
```

### 람다식(익명 객체)을 다루기 위한 참조변수의 타입은?

```java
Object obj = new Object() {
	int max(int a, int b) {
		return a > b ? a : b;
	}
}

int value = obj.max(3,5); // error
```

- `obj`는 `Object` 타입의 참조변수로 다형성의 원칙에 의해 모든 객체를 담을수는 있겠지만, `max()`는 새로운 익명 객체에서 추가된 메서드이기 때문에 호출이 불가함
- 자바에서의 룰에 의해 메서드는 항상 클래스 종속(객체 종속)이기 때문에 메서드 단일로만 존재할 수 없음
- 그래서 자바에서 람다식은 익명 객체를 생성하여 객체의 생성부를 생략하는 방식(컴파일러가 서포트)을 통해 람다식을 구현
- 하지만 상기의 에러를 해결하기 위해 함수형 인터페이스가 필요
