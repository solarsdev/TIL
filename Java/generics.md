# Generics

## 제네릭이란?

- 컴파일시 타입을 체크해 주는 기능 (`compile time type check`)
- `JDK 1.5`부터 도입됨

### 컴파일러의 한계

- 컴파일 단계에서 문제없는 코딩이지만, 런타임에 에러가 발생할 수 있는 여지가 있음
- 컴파일러의 한계로 런타임에서 발생하는 에러는 잡아낼 수 없음

```java
public class GenericTest {
	public static void main(String[] args) {
		ArrayList list = new ArrayList();
		list.add(10); // String -> Integer 자동 업캐스팅
		list.add(20); // String -> Integer 자동 업캐스팅
		list.add("30"); // String -> Object 자동 업캐스팅

		Integer i = (Integer) list.get(2); // Compile OK -> 컴파일러의 한계
	  // list는 명시적 타입 지정 없이는 Object로 초기화되기 때문에 가능한 문제
	}
```

- 컴파일러에게 추가정보를 제공해줘서 이러한 문제를 최대한 해결하고자 함

```java
public class GenericTest {
	public static void main(String[] args) {
		ArrayList<Integer> list = new ArrayList<Integer>();
		list.add(10); // int -> Integer 래핑
		list.add(20); // int -> Integer 래핑
		list.add("30"); // 컴파일 에러
	}
```

```java
ArrayList list = new ArrayList();  // JDK 1.5이전
ArrayList<Object> list = new ArrayList<>();  // JDK 1.5이후
```

### 제네릭의 장점

- 객체의 타입 안정성을 높이고 형변환의 번거로움을 줄여줌
- 타입체크와 형변환을 생략할 수 있으므로 코드가 간결해짐

## 제네릭스는 런타임 예외를 방지

![images/generics/1.png](images/generics/1.png)

예외의 상속계층도

- 예외에는 컴파일 타임에 발생할 수 있는 에러와 런타임에 발생할 수 있는 에러로 나뉨
- 컴파일 타임에 발생하는 에러는 컴파일러가 미리 잡아주기 때문에 실행중에는 발생하지 않지만, 런타임 에러는 프로그래머의 코딩 실수로 프로그램을 종료시킬 수 있기 때문에 조심해야 함
- 위 예제와 같이 제네릭스는 런타임 예외 중 `ClassCastException`을 방지해주기 때문에 유용함
