# Enums

## 열거형 (`enum`)

- 관련된 상수들을 같이 묶어 놓은 것
- `Java`는 타입에 안전한 열거형을 제공

```java
class Card {
	static final int CLOVER = 0;
	static final int HEART = 1;
	static final int DIAMOND = 2;
	static final int SPADE = 3;

	static final int TWO = 0;
	static final int THREE = 1;
	static final int FOUR = 2;

	final int kind;
	final int num;
}

// JDK 1.5부터
class Card {
	enum Kind {CLOVER, HEART, DIAMIND, SPADE}; // 열거형 Kind
	enum Value {TWO, THREE, FOUR}; // 열거형 Value

	final Kind kind;
	final Value value;
}
```

```java
if (Card.CLOVER == Card.TWO) // 열거형 타입 없이 사용할경우 true, 의미상은 false지만
if (Card.Kind.CLOVER == Card.Value.TWO) // 이후에는 컴파일단계에서 잡아줌 (타입이 불일치)
```

## 열거형의 정의와 사용

### 열거형을 정의하는 방법

```java
enum 열거형이름 { 상수명1, 상수명2, ... };
```

### 열거형 타입의 변수를 선언하고 사용하는 방법

```java
enum Direction { EAST, SOUTH, WEST, NORTH };

class Unit {
	int x, y;
	Direction dir;

	void init() {
		dir = Direction.EAST;
	}
}
```

### 열거형 상수의 비교에 `==`와 `compareTo()` 사용가능

```java
if (dir == Direction.EAST) {
	x++;
} else if (dir > Direction.WEST) { // err, 열거형 상수에 비교연산자 사용 불가
	..
} else if (dir.compareTo(Direction.WEST) > 0 { // compareTo는 가능
	..
}
```

## 모든 열거형의 조상 (`java.lang.Enum`)

### 모든 열거형은 `Enum`의 자손이고, 따라서 아래의 메서드가 상속됨

```java
Class<E> getDeclaringClass() // 열거형의 Class객체를 반환
String name() // 열거형 상수의 이름을 문자열로 반환
int ordinal() // 열거형 상수가 정의된 순서를 반환 (0부터 시작)
T valueOf(Class<T> enumType, String name) // 지정된 열거형에서 name과 일치하는 열거형 상수를 반환
```

### `values()`, `valueOf()`는 컴파일러가 자동으로 추가

```java
static E[] values();
static E valueOf(String name);

Directon[] dArr = Direction.values();

for (Direction d : dArr) {
	System.out.printf("%s=%d%n", d.name(), d.ordinal());
}
```

## 열거형에 멤버 추가하기

- 불연속적 열거형 상수의 경우에는 원하는 값을 괄호()안에 적음
- 값은 여러개로 입력 가능 (`ex: EAST(1, “>”)`)

```java
enum Direction {EAST(1), WEST(5), SOUTH(-1), NORTH(10)};
```

- `괄호()`를 사용하려면 인스턴스 변수와 생성자를 새로 추가해줘야 함

```java
enum Direction {
	EAST(1), WEST(5), SOUTH(-1), NORTH(10); // 끝에 ;는 필수임

	private final int value; // 정수를 저장할 필드(인스턴스 변수)를 추가
	Direction(int value) {  // 생성자를 추가
		this.value = value;
	}

	public int getValue() {
		return value;
	}
}
```

- 열거형의 생성자는 묵시적으로 `private`이므로, 외부에서 생성 불가

```java
Direction d = new Direction(1); // 에러
```
