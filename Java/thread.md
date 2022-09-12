# Thread

## 프로세스와 쓰레드 (`process and thread`)

### 프로세스

- 실행 중인 프로그램
- 자원(`resource`)과 쓰레드(`thread`)로 구성

### 쓰레드

- 프로세스 내에서 실제 작업을 수행
- 모든 프로세스는 최소한 하나의 쓰레드를 가지고 있음

> 💡 process : thread = factory : worker
> 1 worker on 1 process → single thread process
> multiple worker on 1 process → multi thread process

> “하나의 새로운 프로세스를 생성하는 것보다 하나의 새로운 쓰레드를 생성하는 것이 더 적은 비용이 든다.”

### CGI vs Java Servlet (`Tomcat`)

- `CGI`는 사용자의 요청마다 프로세스를 생성
  - 요청 대비 자원의 소모가 많고, 오버헤드가 많아 효율적으로 처리 불가능
- `Java`는 사용자의 요청을 쓰레드를 생성해서 처리
  - 다중 사용자 요청을 기민하게, 효율적으로 처리 가능

## 멀티쓰레드의 장단점

- 대부분의 프로그램이 멀티쓰레드로 작성되어 있음
- 하지만 멀티쓰레드에는 장점만 있는것이 아님

### 장점

- 시스템 자원을 보다 효율적으로 사용할 수 있음
- 사용자에 대한 응답성(`responsibility`)이 향상됨
- 작업이 분리되어 코드가 간결해짐

### 단점

- 동기화(`synchronization`)에 주의해야 함
- 교착상태(`dead-lock`)가 발생하지 않도록 주의해야 함
  - 기아상태가 발생하여 특정 쓰레드가 계속 사용되지 않는 문제
- 각 쓰레드가 효율적으로 고르게 실행될 수 있도록 해야 함

> 프로그래밍 시 고려해야 할 사항이 많음 (유지보수)

## 쓰레드의 구현과 실행

### 멀티쓰레드의 구현 방식

1. `Thread` 클래스의 상속

   ```java
   class MyThread extends Thread {
   	@Override
   	public void run() { // Thread 클래스의 run()을 오버라이딩
   		// 작업내용
   	}
   }
   ```

2. `Runnable` 인터페이스를 구현

   ```java
   class MyThread implements Runnable {
   	@Override
   	public void run() { // Runnable 인터페이스의 run()을 구현
   		// 작업내용
   	}
   }
   ```

- 자바는 단일상속 프로그래밍 언어이기 때문에 `Thread`를 상속받으면 다른 클래스를 상속할 수 없음
- 따라서 `Runnable` 인터페이스를 구현하여 멀티쓰레드를 실현하는 것이 좀 더 나은 방식

### `Runnable` 인터페이스

```java
public interface Runnable {
	public abstract void run();
}
```

### 쓰레드의 실행 방법

- 쓰레드를 생성한 후에 `start()`를 호출하면 쓰레드가 작업을 시작
- OS 스케줄러가 알아서 판단해서 쓰레드의 실행권한을 부여함

```java
// 1. Thread 클래스를 상속 했을 경우
MyThread t1 = new MyThread();
t1.start();

// 2. Runnable 인터페이스를 구현했을 경우
Runnable r = new MyThread();
Thread t2 = new Thread(r); // Thread(Runnable r);
// Thread t2 = new Thread(new MyThread());
t2.start();
```

- 왜 `start()`를 해야 하는가?
  - `start()`는 실제로 새로운 콜스택을 생성한뒤 그곳에 `run()`을 올리는 작업을 수행
  - `instruction set`을 수행하려면 `instruction`을 가리킬 `pc`가 필요하기 때문에 해당 박스(콜 스택)를 새롭게 만들 필요가 있음

## `Main` 쓰레드

- `main` 메서드의 코드를 수행하는 쓰레드
- 쓰레드는 “사용자 쓰레드"와 “데몬 쓰레드" 두 종류가 있음
  - 사용자 쓰레드 → `main` 쓰레드와 같은 것들
  - 데몬 쓰레드 → `main`을 도와 보조로 처리하는 쓰레드
  - `run()`으로 구현된 새로운 콜스택은 **사용자 쓰레드**임

> 💡 실행 중인 “사용자 쓰레드가 하나도 없을 때 프로그램은 종료” 된다.

### 사용자 쓰레드의 처리관계 확인

```java
package com.solarsdev;

public class ThreadEx1 {
    static long startTime = 0;

    public static void main(String[] args) throws InterruptedException {
        Thread th1 = new Thread(new ThreadEx1_1());
        Thread th2 = new Thread(new ThreadEx1_2());

        th1.start();
        th2.start();
        startTime = System.currentTimeMillis();

				// join없이 수행할 경우 소요시간이 바로 출력됨
				// main 함수는 비동기로 소요시간을 출력하고 바로 종료되지만
				// 프로그램(프로세스)는 쓰레드가 전부 종료될때까지 기다렸다는 것을 알 수 있음
        th1.join();
        th2.join();

        System.out.println();
        System.out.println("소요시간: " + (System.currentTimeMillis() - startTime));
    }
}

class ThreadEx1_1 implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 300; i++) {
            System.out.print("-");
        }
    }
}

class ThreadEx1_2 implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 300; i++) {
            System.out.print("|");
        }
    }
}
```

## 싱글쓰레드와 멀티쓰레드

### 싱글쓰레드

```java
class ThreadTest {
	public static void main(String[] args) {
		for (int i=0; i<300; i++) {
			System.out.println("-");
		}

		for (int i=0; i<300; i++) {
			System.out.println("|");
		}
	}
}
```

![images/thread/1.png](images/thread/1.png)

### 멀티쓰레드

```java
package com.solarsdev;

public class ThreadTest {
    public static void main(String[] args) throws InterruptedException {
        Thread th1 = new Thread(new ThreadEx1_1());
        Thread th2 = new Thread(new ThreadEx1_2());
        th1.start();
        th2.start();
    }
}

class ThreadEx1_1 implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 300; i++) {
            System.out.print("-");
        }
    }
}

class ThreadEx1_2 implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 300; i++) {
            System.out.print("|");
        }
    }
}
```

![images/thread/2.png](images/thread/2.png)

## 쓰레드의 `I/O` 블록킹 (`blocking`)

- 싱글쓰레드로 작성한 코드의 경우 인풋을 기다리는 동안 아무 작업도 하지 않음

  ```java
  import javax.swing.*;

  public class MultiThreadTest {
      public static void main(String[] args) {

          String input = JOptionPane.showInputDialog("아무 값이나 입력하세요.");

          for (int i = 10; i > 0; i--) {
              System.out.println(i);
              try {
                  Thread.sleep(1000);
              } catch (InterruptedException e) {
                  throw new RuntimeException(e);
              }
          }

          System.out.println("입력하신 값은 " + input + "입니다.");
      }
  }
  ```

- 멀티쓰레드의 경우 `I/O`로 발생한 텀 동안 다른 쓰레드에 실행권한을 할당해서 계속 수행

  ```java
  import javax.swing.*;

  public class MultiThreadTest {
      public static void main(String[] args) {
  				String input = JOptionPane.showInputDialog("아무 값이나 입력하세요.");

  				Thread th1 = new Thread(new MultiThreadTest_1());
          th1.start();

          System.out.println("입력하신 값은 " + input + "입니다.");
      }
  }

  class MultiThreadTest_1 implements Runnable {
      @Override
      public void run() {
          for (int i = 10; i > 0; i--) {
              System.out.println(i);
              try {
                  Thread.sleep(1000);
              } catch (InterruptedException e) {
                  throw new RuntimeException(e);
              }
          }
      }
  }
  ```

## 쓰레드의 우선순위(priority of thread)

- 작업의 중요도에 따라 쓰레드의 우선순위를 다르게 하여 특정 쓰레드가 더 많은 작업시간을 같게 할 수 있음

  ```java
  void setPriority(int newPriority); // 쓰레드의 우선순위를 지정한 값으로 변경
  int getPriority(); // 쓰레드의 우선순위를 반환

  public static final int MAX_PRIORITY = 10; // 최대 우선순위
  public static final int MIN_PRIORITY = 1; // 최소 우선순위
  public static final int NROM_PRIORITY = 5; // 보통 우선순위
  ```

![images/thread/3.png](images/thread/3.png)

![images/thread/4.png](images/thread/4.png)

- 우선순위가 높은 작업이 보다 더 많은 실행시간을 보장받음
- 하지만 결국 OS 스케줄러에 의해 판단되기 때문에 반드시 그렇다고는 할 수 없음
  - OS 내에 다양한 프로세스, 쓰레드들이 항상 대기 상태에 있고 OS 스케줄러는 모든것을 고려함
  - 그래서 심지어 우선순위가 낮은 쓰레드가 먼저 종료되는 경우도 있음

## 쓰레드 그룹

- 서로 관련된 쓰레드를 그룹으로 묶어서 다루기 위한 것
- 모든 쓰레드는 반드시 하나의 쓰레드 그룹에 포함되어 있어야 함
- 쓰레드 그룹을 지정하지 않고 생성한 쓰레드는 “main 쓰레드 그룹”에 속함
- 자신을 생성한 쓰레드(부모 쓰레드)의 그룹과 우선순위를 상속받음

```java
// 생성자들
Thread(ThreadGroup group, String name);
Thread(ThreadGroup group, Runnable target);
Thread(ThreadGroup group, Runnable target, String name);
Thread(ThreadGroup group, Runnable target, String name, long stackSize);
```

```java
ThreadGroup getThreadGroup(); // 쓰레드 자신이 속한 쓰레드 그룹을 반환
void uncaughtException(Thread t, Throwable e); // 처리되지 않은 예외에 의해 쓰레드 그룹의 쓰레드가 종료되었을때, JVM에 의해 이 메서드가 자동으로 호출됨
```

## 데몬 쓰레드 (`daemon thread`)

- 일반 쓰레드(`non-daemon thread`)의 작업을 돕는 보조적인 역할을 수행
- 일반 쓰레드가 모두 종료되면 자동적으로 종료됨
- 가비지 컬렉터, 자동저장, 화면 자동갱신 등에 사용됨
- 무한루프와 조건문을 이용해서 실행 후 대기하다가 특정조건이 만족되면 작업을 수행하고 다시 대기하도록 작성

```java
@Override
public void run() {
	while (true) { // 무한루프
		try {
			Thread.sleep(3 * 1000); // 3초마다
		} catch (InterruptedException e) {
			// autoSave의 값이 true이면 autoSave()를 호출
			if (autoSave) {
				autoSave();
			}
		}
	}
}
```

```java
boolean isDaemon(); // 쓰레드가 데몬 쓰레드인지 확인함 (데몬 쓰레드이면 true를 반환)
void setDaemon(boolean on); // 쓰레드를 데몬 쓰레드로, 또는 사용자 쓰레드로 변경 (on을 true로 설정하면 데몬 쓰레드가 됨)
```

- `setDaemon(boolean on);` 은 반드시 `start()`를 호출하기 전에 실행되어야 함
- 실행중에 `daemon` 상태를 변경하면 `IllegalThreadStateException`이 발생

### 데몬쓰레드를 만드는 이유

- 일반 쓰레드를 사용하면 사용자 쓰레드로 올라가기 때문에 일일히 작업종료를 선언해줘야 함
- 사용자 쓰레드가 종료(프로그램 종료)시 자동으로 종료되는 쓰레드를 운영하기 위해 데몬 쓰레드를 선언

## 쓰레드의 상태

![images/thread/5.png](images/thread/5.png)

### `NEW`

- 쓰레드가 생성되고 아직 `start()`가 호출되지 않은 상태

### `RUNNABLE`

- 실행 중 또는 실행 가능한 상태
  - 실행 가능한 상태란, 쓰레드가 실행 중 `OS` 스케줄러에 의해 작업권을 빼앗긴 상태임
  - 언제든 다시 `OS` 스케줄러에 의해 작업권을 부여받을 수 있음 (순서 or 조건에 의해)

### `BLOCKED`

- 동기화 블럭에 의해서 일서 정지된 상태 (`lock`이 풀릴때 까지 기다리는 상태)

### `WAITING`, `TIMED_WAITING`

- 쓰레드의 작업이 종료되지는 않았지만, 실행가능하지 않은(`runnable` 하지 않은) 상태
- `TIMED_WAITING`은 일시정지 시간이 지정된 경우를 의미
- `sleep`등으로 정지중인 상태

### `TERMINATED`

- 쓰레드의 작업이 종료된 상태

## 쓰레드의 실행제어

- 쓰레드의 실행을 제어할 수 있는 메서드가 제공됨

![images/thread/6.png](images/thread/6.png)

- `static` sleep → 잠들기
- join → 기다리기
- interrupt → 잠들어있거나 기다리는 쓰레드를 깨움
- stop → 쓰레드 즉시 종료
- suspend → 일시정지
- resume → 재개
- `static` yield → 양보 (실행중인 쓰레드에게 즉시 양보하고 넘어감)
- `static`이 붙어있는 메서드의 경우에는 자기 자신에게만 수행 가능함

## `sleep()`

- 현재 쓰레드를 지정된 시간동안 멈추게 함
- `static` 메서드로 **자기 자신에 대해서만** 적용이 가능

```java
static void sleep(long millis); // 천분의 일초
static void sleep(long millis, int nanos); // 천분의 일초+ 나노초
```

- 예외처리를 해야 함

  - `InterruptedException`이 발생하면 깨어남
  - 실제로 Thread.sleep()으로 자고 있는 쓰레드의 구문에 interrupt()를 던지게 되면 예외를 발생시킴
    ```java
    throw new InterruptedException();
    ```
  - 예외처리를 해야 하기 때문에 코드가 지저분해지고, 그래서 직접 사용하기보다는 별도 메서드를 작성

    ```java
    void delay(long millis) {
    	try {
    		Thread.sleep(millis);
    	} catch (InterruptedException e) {
    		// 예외캐치
    	}
    }

    void test() {
    	delay(1000);
    }
    ```

- `sleep` 상태의 쓰레드는 지정된 시간이 지나가거나 `Interrupt`가 발생하지 않으면 깨어나지 않음

## `interrupt()`

- 대기상태(`WAITING`)인 쓰레드를 실행대기 상태(`RUNNABLE`)로 만듬

```java
void interrupt(); // 쓰레드의 interrupted상태를 false에서 true로 변경
boolean isInterruped(); // 쓰레드의 interrupted 상태를 반환
static boolean interruped(); // 현재 쓰레드의 interruped상태를 알려주고, false로 초기화
```

- `isInterrupted`와 `interrupted`의 차이
  - `isInterrupted`는 수행중인 쓰레드가 인터럽트에 의해서 깨어나 동작하고 있는지를 확인만 함
  - `interrupted`는 현재 상태를 반환함과 동시에 `interrupted`를 `false`로 변경

## `suspend()`, `resume()`, `stop()`

- 쓰레드의 실행을 일시정지

```java
void suspend(); // 쓰레드를 일시정지(Waiting) 시킴
void resume(); // suspend()에 의해 일시정지된 쓰레드를 실행대기(Runnable)상태로 만듬
void stop(); // 쓰레드를 즉시 종료시킴
```

- 상기 3개의 메서드는 `deprecated`됨
  - 교착상태(`dead-lock`)를 발생시키기 쉬운 메서드들

## `join()`

- 지정된 시간동안 특정 쓰레드가 작업하는 것을 기다림

```java
void join(); // 작업이 끝날 때 까지
void join(long millis); // 기간 지정
void join(long millis, int nanos); // 나노초 개념 도입
```

```java
// sample code
@Override
public void run() {
	while (true) {
		try {
			Thread.sleep(10 * 1000); // 10초 대기
		} catch(InterruptException e) {
			System.out.println("awaken by interrupt().");
		}

		gc(); // garbage collection을 수행
		System.out.println("Garbage Collected. Free memory : " + freeMemory());
	}
}

//
for (int i=0; i <20; i++) {
	requiredMemory = (int) (Math.random() * 10) * 20;
	// 필요한 메모리가 사용할 수 있는 양보다 적거나 전체 메모리의 60%이상 사용했을 경우 gc를 깨움
	if(gc.freeMemory() < requiredMemory || gc.freeMemory() < gc.totalMemory * 0.4) {
		gc.interrupt();
	}
	gc.usedMemory += requiredMemory;
	System.out.println("usedMemory: " + gc.usedMemory);
}
```

## `yield()`

- 남은 시간을 다음 쓰레드에게 양보하고, 자신(현재 쓰레드)는 실행대기 함
- `yield()`와 `interrupt()`를 적절히 활용하면, 응답성과 효율을 높일 수 있음
  - 응답을 대기할때는 `yield()`를 호출
  - 필요할때는 바로 `interrupt()`를 이용해서 잠자는 쓰레드를 깨우기
  - 프로그램적으로 판단하기 때문에 `OS` 스케줄러보다 영리한 배분이 가능
  - `OS` 스케줄러에게 현재 상황을 메서드를 이용해서 바로바로 전달할 수 있음
