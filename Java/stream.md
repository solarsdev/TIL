# Stream

## 스트림(`stream`)

### 다양한 데이터 소스를 표준화된 방법으로 다루기 위한 것

- 데이터 소스
  - 컬렉션 → 데이터셋을 표준화하여 관리하려고 하였으나, 실패 (`List`, `Set`, `Map` 성격이 다름, 사용방법이 다름 = 표준화되지 않음)
  - 배열

```java
List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
Stream<Integer> intStream = list.stream(); // 컬렉션
Stream<String> strStream = Stream.of(new String[]{"a", "b", "c"}); // 배열
Stream<Integer> evenStream = Stream.iterate(0, n->n+2); // 0, 2, 4, 6, ...
Stream<Double> randomStream = Stream.generate(Math::random); // 람다식
IntStream intStream = new Random().ints(5); // 난수 스트림
```

### 스트림이 제공하는 기능 (중간 연산과 최종 연산)

- 중간 연산
  - 연산 결과가 스트림인 연산
  - 반복적으로 적용 가능
- 최종 연산
  - 연산 결과가 스트림이 아닌 연산
  - 단 한번만 적용 가능 (스트림의 요소를 소모)
- 스트림 사용법
  1. 스트림 만들기
  2. 중간 연산(`0~n번`)
  3. 최종 연산(`0~1번`)

```java
stream.distinct().limit(5).sorted().forEach(System.out::println);

// distinct() 중복제거
// limit() 자르기
// sorted() 정렬
// forEach 각요소에 대해 실행
```

## 스트림(`stream`)의 특징

### 스트림은 데이터 소스로부터 데이터를 읽기만 할 뿐 변경하지 않음

```java
List<Integer> list = Arrays.asList(3,1,5,4,2);
List<Integer> sortedList = list.stream.sorted().collect(Collectors.toList()); // 새로운 List에 저장
System.out.println(list); // [3, 1, 5, 4, 2]
System.out.println(sortedList); // [1, 2, 3, 4, 5]
```

### 스트림은 `Iterator`처럼 일회용임 (필요하면 다시 스트림을 생성해야 함)

```java
strStream.forEach(System.out::println); // 모든 요소를 화면에 출력 (최종연산 끝)
int numOfStr = strStream.count(); // 에러, 스트림이 이미 닫힘
```

### 최종 연산 전까지 중간연산이 수행되지 않음 (지연 연산)

```java
IntStream intStream = new Random().ints(1, 46); // 1~45범위의 무한 스트림
intStream.distinct().limit(6).sorted() // 중간 연산
		.forEach(i -> System.out.print(i + ",")); // 최종 연산
```

### 스트림은 작업을 내부 반복으로 처리

```java
for(String str : strList) {
	System.out.println(str);
}

stream.forEach(System.out::println);

void forEach(Consumer<? super T> action) {
	Objects.requireNonNull(action); // 매개변수의 널 체크

	for (T t : src) { // 내부 반복 (for문을 메서드 안으로 넣음)
		action.accept(T);
	}
}
```

### 스트림의 작업을 병렬로 처리 (병렬 스트림) → 멀티 쓰레딩

```java
Stream<String> strStream = Stream.of("dd", "aaa", "CC", "cc", "b");
int sum = strStream.parallel()  // 병렬 스트림으로 전환(속성 변경)
  .mapToInt(s -> s.length()).sum(); // 모든 문자열의 길이의 합
```

### 기본형 스트림 - `IntStream`, `LongStream`, `DoubleStream` …

- 오토박싱 & 언박싱의 비효율이 제거됨 (`Stream<Integer>` 대신 `IntStream`사용)
  - 성능 개선이 필요한 경우에 사용하자
- 숫자와 관련된 유용한 메서드를 `Stream<T>` 보다 더 많이 제공

## 스트림 만들기 (컬렉션)

### `Collection` 인터페이스의 `stream()`으로 컬렉션을 스트림으로 변환

```java
// List, Set
Stream<E> stream(); // Collection 인터페이스의 메서드
```

```java
List<Integer> list = Arrays.asList(1,2,3,4,5);
Stream<Integer> intStream = list.stream(); // list를 stream으로 변환

// 스트림 출력
intStream.forEach(System.out::print); // 12345
intStream.forEach(System.out::print); // 에러, 스트림이 한번 최종연산 된 이후에는 사용 불가
```

## 스트림 만들기 (배열)

### 객체 배열로부터 스트림 생성

```java
Stream<T> Stream.of(T... values) // 가변 인자
Stream<T> Stream.of(T[])
Stream<T> Arrays.stream(T[])
Stream<T> Arrays.stream(T[] array, int startInclusive, int endExclusive)

// example
Stream<String> strStream = Stream.of("a", "b", "c");
Stream<String> strStream = Stream.of(new String[]{"a", "b", "c"});
Stream<String> strStream = Arrays.stream(new String[]{"a", "b", "c"});
Stream<String> strStream = Arrays.stream(new String[]{"a", "b", "c"}, 0, 3); // 0, 1, 2
```

### 기본형 배열로부터 스트림 생성

```java
IntStream IntStream.of(int... values)
IntStream IntStream.of(int[])
IntStream Arrays.stream(int[])
IntStream Arrays.stream(int[] array, int startInclusive, int endExclusive)
```

## 스트림 만들기 (난수)

### 난수를 요소로 갖는 스트림 생성

```java
IntStream intStream = new Random().ints(); // 무한 스트림
intStream.limit(5).forEach(System.out::println); // 5개의 요소만 출력

IntStream intStream = new Random().ints(5); // limit 5가 적용된 유한 스트림

// new Random()의 범위
Integer.MIN_VALUE <= ints() <= Integer.MAX_VALUE
Long.MIN_VALUE <= longs() <= Long.MAX_VALUE
0.0 <= doubles() < 1.0
```

### 지정된 범위의 난수를 요소로 갖는 스트림을 생성하는 메서드 (`Random` 클래스)

```java
// 무한 스트림
IntStream ints(int begin, int end);
LongStream longs(long begin, long end);
DoubleStream doubles(double begin, double end);

// 유한 스트림
IntStream ints(long streamSize, int begin, int end);
LongStream longs(long streamSize, long begin, long end);
DoubleStream doubles(long streamSize, double begin, double end);
```

## 스트림 만들기 (특정 범위의 정수)

### 특정 범위의 정수를 요소로 갖는 스트림 생성 (`IntStream`, `LongStream`)

```java
IntStream IntStream.range(int begin, int end);
IntStream IntStream.rangeClosed(int beginm int end);

// example
IntStream intStream = IntStream.range(1, 5); // 1,2,3,4
IntStream intStream = IntStream.rangeClosed(1,5); // 1,2,3,4,5
```

## 스트림 만들기 (람다식) - `iterator()`, `generate()`

### 람다식을 소스로 하는 스트림 생성 (동적 생성)

```java
static <T> Stream<T> iterator(T seed, UnaryOperator<T> f) // 이전 요소에 종속
static <T> Stream<T> generate(Supplier<T> s) // 이전 요소에 독립
```

### `iterator()`는 이전 요소를 `seed`로 해서 다음 요소를 계산

```java
Stream<Integer> evenStream = Stream.iterator(0, n->n+2);
```

![images/stream/1.png](images/stream/1.png)

### `generate()`는 `seed`를 사용하지 않음

```java
Stream<Double> randomStream = Stream.generate(Math::random); // Math.random() 무한
Stream<Integer> oneStream = Stream.generate(()->1); // 무한 1
```

## 스트림 만들기 (파일과 빈 스트림)

### 파일을 소스로 하는 스트림 생성

```java
Stream<Path> Files.list(Path dir) // Path는 파일 또는 디렉토리

Stream<String> Files.lines(Path path)
Stream<String> Files.lines(Path path, Charset cs)
Stream<String> lines() // BufferedReader클래스의 메서드
```

### 빈 스트림 생성

```java
Stream emptyStream = Stream.empty(); // empty()는 빈 스트림을 생성해서 반환
long count = emptyStream.count(); // 0
```
