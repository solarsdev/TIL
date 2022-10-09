# 오버로딩 (`overloading`)

## 오버로딩이란?

- 메서드도 변수와 동일하게 이름으로 서로를 구별 가능
- 동일한 기능을 수행하는 메서드가 다양한 매개변수를 처리하기 위해 오버로딩이 지원됨

## 오버로딩의 조건

1. 메서드 이름이 같아야 함
2. 매개변수의 개수 또는 타입이 달라야 함

- 메서드의 이름이 같아도 매개변수의 개수 또는 타입이 다르기 때문에 구별이 가능
- 리턴 타입은 오버로딩의 조건이 아니므로, 리턴 타입은 상관이 없음

## 오버로딩의 예

- 가장 대표적인 오버로딩의 예로는 `println`이 있음
- `println` 메서드에 다양한 값을 입력해도 결국 콘솔(기본)에 매개변수를 출력하는 기능에는 차이가 없음
- 실제로 `println` 메서드는 10개의 서로 다른 매개변수로 오버로딩을 구현하고 있음

```java
void println();
void println(boolean x);
void println(char x);
void println(char[] x);
void println(double x);
void println(float x);
void println(int x);
void println(long x);
void println(Object x);
void println(String x);
```

### 오버로딩에서 자주 착각하는 부분

1. 매개변수의 이름이 다른 경우

   ```java
   int add(int a, int b) { return a + b; }
   int add(int x, int y) { return x + y; }
   ```

   - 위 메서드는 매개변수의 이름이 다를 뿐 타입은 동일하므로 오버로딩이 ❌ 성립하지 않음

2. 리턴타입이 다른 경우

   ```java
   int add(int a, int b) { return a + b; }
   long add(int a, int b) { return a + b; }
   ```

   - 리턴타입은 오버로딩의 조건이 아니므로, 역시 오버로딩이 ❌ 성립하지 않음

3. 매개변수의 순서가 다름

   ```java
   long add(int a, long b) { return a + b; }
   long add(long a, int b) { return a + b; }
   ```

   - 매개변수의 순서가 다르면, 호출 시 매개변수 값에 의해 호출될 메서드가 구분 가능하므로 이경우는 ✅ **성립**
   - 단, 호출하는 측에서 이러한 부분에서는 순서에 따라 다른 메서드가 호출된다는 사실을 인지하지 못하고 있을 경우에는 헷갈리기 때문에 단점이 될 수 있음

## 오버로딩의 장점

- 메서드도 단순히 이름으로만 구별한다고 생각하면 `println`의 예는 아래와 같이 구현되어야 할 것임
  ```java
  void println();
  void printlnBoolean(boolean x);
  void printlnChar(char x);
  void printlnChatArray(char[] x);
  void printlnDouble(double x);
  ...
  ```
- 근본적으로는 같은 기능을 하는 메서드이지만 매개변수에 따라 불필요한 이름이 추가되므로 비효율적
  - 이름을 짓는 쪽에서도 비효율이지만, 사용하는 측에서도 이름을 기억해야 하므로 비효율

## 가변인자(varargs)와 오버로딩

- `JDK 1.5`부터 메서드의 매개변수를 가변적으로 지정해줄 수 있게 되었음
- 가변인자(`variable arguments`)라고 하는 기능인데 사용법은 `타입… 변수명`
  ```java
  public PrintStream printf(String format, Object... args) {...}
  ```
- 가변인자는 항상 매개변수의 마지막에 존재해야 하는데, 매개변수의 앞에 있으면 뒤의 매개변수를 언제 취득해야 하는지 알 수 없기 때문임
  ```java
  // format을 언제 복사해야 하는지 알 수 없음
  public PrintStream printf(Object... args, String format) {...}
  ```
- 가변인자로 여러개의 오버로딩된 메서드를 하나로 줄일 수 있게 되었음
  ```java
  String concatenate(String s1, String s2) { ... }
  String concatenate(String s1, String s2, String s3) { ... }
  String concatenate(String s1, String s2, String s3, String s4) { ... }

  String concatenate(String... str) { ... }
  ```
- 가변인자는 내부적으로 배열을 이용하여, 호출시에 지정된 매개변수 개수만큼의 배열을 생성하여 값을 넘겨줌
- 가변인자는 매개변수를 아예 전달하지 않아도 되며, 이 경우에는 자동으로 배열의 길이를 0으로 지정하여 처리
- 가변인자를 사용한 메서드는 가급적이면 오버로딩 하지 않는 것이 좋음
  ```java
  public class VarArgsEx {
      public static void main(String[] args) {
          String[] strArr = {"100", "200", "300"};

          //System.out.println(concatenate("", "100", "200", "300"));
          System.out.println(concatenate("-", strArr));
          System.out.println(concatenate(",", new String[]{"1", "2", "3"}));
          System.out.println("[" + concatenate(",", new String[0]) + "]");
          //System.out.println("[" + concatenate(",") + "]");
      }

      static String concatenate(String delim, String... args) {
          String result = "";

          for (String str : args) {
              result += str + delim;
          }

          return result;
      }

      static String concatenate(String... args) {
          return concatenate("", args);
      }
  }
  ```
  - `String`의 가변인자와 `delim`은 같은 타입이기 때문에, 어떤것을 취해야 할지 구분이 안되는 타이밍이 있음
