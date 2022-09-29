# JUnit 5

## 소개

- `JUnit`은 대부분의 자바 프로그래머가 사용하는 대표적인 테스트 도구
  → 단위 테스트를 작성하는 자바 개발자의 `93%`가 `JUnit`을 사용
- 자바 `8` 이상을 필요로 함
- 대체제
  - `TestNG`
  - `Spock`
  - `…`
- `Platform`
  - 테스트를 실행해주는 런처 제공
  - `TestEngine API` 제공
- `Jupiter`
  - `TestEngine API` 구현체로 `JUnit 5`를 제공
- `Vintage`
  - `JUnit 4`와 `3`을 지원하는 `TestEngine` 구현체

## 시작하기

- `JUnit 5` 의존성 추가하기
- 기본 애너테이션
  - `@BeforeAll` (클래스 단위)
    - 모든 테스트를 실행하기 전 전체적으로 한번만 실행
    - `static void <method-name>`
  - `@AfterAll` (클래스 단위)
    - 모든 테스트가 종료된 뒤 한번만 실행
    - `static void <method-name>`
  - `@BeforeEach` (메서드 단위)
    - 각 테스트가 실행되기 전에 실행
    - `void <method-name>`
  - `@AfterEach` (메서드 단위)
    - 각 테스트가 종료된 후에 실행
    - `void <method-name>`
  - `@Test`
    - 테스트 해야할 메서드를 마킹
  - `@Disabled`
    - 테스트를 스킵해야 할 메서드를 마킹
    - 기본적으로는 테스트를 고쳐야 하지만, `ToDo`로 남기거나 소스코드로서는 아직 남겨야 할 필요가 있을 경우

## 테스트 이름 표시하기

[JUnit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/#writing-tests-display-names)

- `@DisplayNameGeneration`
  - `Method`와 `Class` 레퍼런스를 사용해서 테스트 이름을 표기하는 방법 설정
  - 기본 구현체로 `ReplaceUnderscores` 제공
- `@DisplayName` (일반적으로는 메서드별 `TestName`을 지정하는 이 방식을 추천)
  - 어떤 테스트인지 테스트 이름을 보다 쉽게 표현할 수 있는 방법을 제공하는 애너테이션
  - `@DisplayNameGeneration` 보다 우선순위가 높음

## Assertion

- `assertEquals`의 매개변수에 대해서
  - `expected`
    - 기대값
  - `actual`
    - 실제값
  - `message`
    - 람다식을 이용하면 실행이 람다식 내부의 문구들은 람다식이 실제로 실행될때 만들어짐
    - 메시지 `String`을 넘기게 되면 무조건 연산이 진행됨
- `assertAll()`
  - 람다식을 기준으로 여러개의 테스트를 전부 실행하고 그 결과를 묶어서 볼 수 있음
  - 일반적으로 테스트는 직전 테스트가 실패하면 거기서 종료하게 됨
- `assertThrow()`
  - 특정한 상황에서 예외가 예상한대로 발생하는지 확인해야 하는 경우
    ```java
    @Test
    @DisplayName("assertThrows 사용해보기")
    void create_new_study_2() {
        IllegalArgumentException e = assertThrows(IllegalArgumentException.class, () -> new Study(-10));
        e.printStackTrace();
    }
    ```
- `assertTimeout()`
  - 소스코드의 특정한 시간 이내에 완료가 되어야 하는 경우
    - 단, 코드 블럭이 유효시간을 초과할 경우에도 모든 코드가 실행이 될 때까지 기다림
    - 실제로 오래걸리는 테스트가 있다면 그만큼 테스트 전체 시간이 추가되는 것
    ```java
    @Test
    @DisplayName("assertTimeout() 사용해보기")
    void create_new_study_3() {
    	  assertTimeout(Duration.ofMillis(100), () -> {
    	      new Study(10);
    	      Thread.sleep(1000);
    	  });
    }
    ```
  - 이 경우에는 `assertTimeoutPreemptively()` 로 수행하면 초과시 종료
    - `ThreadLocal`을 사용하는 코드블럭일 경우 예상치 못한 결과가 발생할 수 있음
    - `Spring`의 `@Transaction` 등
      - 트랜잭션은 기본적으로 실패 시 롤백이 수행되어야 하지만, 코드가 중간에 끊기고 롤백 수행이 되지 않을 수도 있음
- `assertJ` 등과 같은 기타 라이브러리 들에 대해서도 알아볼 필요가 있음
