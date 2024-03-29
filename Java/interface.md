# Interface

## 인터페이스란?

- 인터페이스는 추상클래스
  - 단, 추상화 정도가 높아서 추상클래스와 달리 몸통을 갖춘 일반 메서드나 멤버변수가 없음
  - 오직 추상메서드와 상수만을 멤버로 가질 수 있음

## 인터페이스의 작성

- 인터페이스는 클래스를 작성하는 방식과 동일하지만, `class` 키워드 대신 `interface`를 사용함
  - 접근제어자도 설정 가능하며, `public` 또는 `default`를 사용 가능 (`default`는 아무것도 없는 상태)

### 인터페이스 멤버의 제약사항

- 모든 멤버변수는 `public static final`이어야 하며, 이를 생략할 수 있음
- 모든 메서드는 `public abstract`이어야 하며, 이를 생략할 수 있음
  - 단, `static`메서드와 디폴트 메서드가 제외됨 (`JDK 1.8~`)

## 인터페이스의 상속

- 인터페이스는 인터페이스를 상속할 수 있으며, 클래스와 달리 다중상속이 허용됨
  - 다중상속이 허용되는 이유는, 클래스와 다르게 몸통이 없는 메서드만이 존재하기 때문에 결국 인터페이스의 구현체에서 그것을 구현해야 하므로, 중복이 발생해도 괜찮기 때문임

## 인터페이스의 구현

- 인터페이스로는 추상클래스처럼 인스턴스를 생성할 수 없음
- 인터페이스를 구현하는 구현체에서 모든 인터페이스의 메서드를 구현해야 함
  - 인터페이스를 일부만 구현하면 아직 구현하지 못한 메서드가 남아버리기 때문에 해당 클래스는 추상클래스가 되며 이 또한 인스턴스 작성이 불가능함

## 인터페이스를 이용한 다중상속

- 두 조상으로부터 상속받는 멤버 중에서 멤버변수의 이름이 같거나 메서드의 선언부가 일치하는 경우
  - 자손이 어떤것을 상속해야 하는지 결정할 수 없음
  - 결국, 한쪽으로부터의 상속을 포기하거나 충돌하지 않게 조상클래스를 수정해야 함
  - 이를 다이아몬드 상속 문제라고 하며, 자바에서는 이를 포기하여 단일상속만을 허용함
- 다만, 인터페이스는 다중상속을 허용하는데, 실제로 이 다중상속으로 구현하는 경우는 거의 없음
- 상속과 포함관계, 인터페이스를 이용하면 다중상속의 이점을 살리는 코딩이 가능함
  ```java
  public class VCR {
  	protected int counter; // VCR의 카운터

  	public void play() {
  		// Tape를 재생
  	}

  	public void stop() {
  		// 재생을 멈춤
  	}

  	public void reset() {
  		counter = 0;
  	}

  	public int getCounter() {
  		return counter;
  	}

  	public void setCounter(int c) {
  		counter = c;
  	}
  }
  ```
  ```java
  public interface IVCR {
  	public void play();
  	public void stop();
  	public void reset();
  	public int getCounter();
  	public void setCounter(int c);
  }
  ```
  ```java
  public class TVCR extends Tv implements IVCR {
  	VCR vcr = new VCR();

  	@Override
  	public void play() {
  		vcr.play();
  	}

  	...

  	public void setCounter(int c) {
  		vcr.setCounter(c);
  	}
  }
  ```

## 인터페이스를 이용한 다형성

- 자손클래스의 인스턴스를 조상타입의 참조변수로 참조하는 것이 가능함
  - 조상타입의 클래스 = 인터페이스와 동일한 개념
  - 즉, 어떠한 인터페이스의 구현체로 만들어진 인스턴스는 인터페이스 타입의 참조변수에 담을 수 있다는 이야기
  ```java
  Fightable f = (Fightable) new Fighter();
  // 업캐스팅은 자동형변환이 가능하므로..
  Fightable f = new Fighter();
  // 단, f는 fightable에 정의된 껍데기 메서드만 호출 가능함

  // 이런식으로 메서드를 작성하는 것도 가능
  // 이렇게 하면 Fightable의 구현체 전부 매개변수로 넘길 수 있음
  void attack(Fightable f) {
  	...
  }

  // 이런식으로 리턴타입에 인터페이스형태를 지정할 수도 있음
  // 이렇게 하면 method()의 반환 객체는 모두 Fightable의 구현체라는 이야기
  Fighable method() {
  	...
  }
  ```

## 인터페이스의 장점

- 개발시간을 단축시킬 수 있음
  - 인터페이스를 이용해서 프로그램을 작성하면, 인터페이스의 몸통 없이 일단 개발이 가능하기 때문
- 표준화 가능
  - 어떠한 코드의 기본 틀을 인터페이스로 구현해서 정형화된 프로그래밍 가능
- 서로 관계없는 클래스들에게 관계를 맺어 줄 수 있음 (구현체들간의 집합)
  - 어떠한 인터페이스의 구현체라는 관계, 하나의 조상을 가진 어떠한 그룹을 만들 수 있음
- 독립적인 프로그래밍이 가능
  - 주로 예시로 올라오는 것이 데이터베이스 커넥터
  - 커넥션에 필요한 메서드를 인터페이스로 정의하고 실제 내부 구현은 각 `DBMS` 회사별(`oracle`, `mysql`, `mssql`등)로 별도로 구현함으로써 프로그래머는 `DB`를 유연하게 변경 가능하게 됨

## 인터페이스의 이해

### 인터페이스의 근본적인 이해를 위한 전제조건

- 클래스를 사용하는 쪽(`User`)과 클래스를 제공하는 쪽(`Provider`)이 있음
- 메서드를 사용(호출)하는 쪽(`User`)에서는 사용하려는 메서드(`Provider`)의 선언부만 알면 됨

### 직접적인 클래스 관계

```java
class A {
	public void method(B b) {
		b.methodB();
	}
}

class B {
	public void methodB() {
		System.out.println("methodB()");
	}
}

class InterfaceTest {
	public static void main(String args[]) {
		A a = new A();
		a.method(new B());
	}
}
```

- 클래스A는 클래스B의 인스턴스를 생성하고 메서드를 호출하기 때문에, 직접적인 관계에 있음
  - A-B
- 클래스A를 작성하기 위해서는 클래스B가 작성되어 있어야 함
- 클래스B의 선언부가 변경되면 클래스A에도 수정이 필요함

### 두 클래스의 관계를 간접적으로 변경하기

```java
class B Implements I {
	public void methodB() {
		System.out.println("methodB in B class");
	}
}

class A {
	public void methodA(I i) {
		i.methodB();
	}
}
```

- 이제 클래스A는 클래스B가 아닌 인터페이스를 필요로 하므로, A-I의 관계가 됨
  - A는 클래스B의 존재를 몰라도 상관없으며 변경에도 영향을 받지 않게 되었음

## 디폴트 메서드와 `static` 메서드

- 인터페이스에 디폴트 메서드, `static` 메서드 추가 가능 (`JDK 1.8부터`)
- 인터페이스에 새로운 메서드(추상 메서드)를 추가하기 어려움
  - 인터페이스에 추상 메서드를 추가하면, 인터페이스를 구현한 클래스 혹은 해당 클래스의 자손이 반드시 구현해야 하기 때문
  - 구현부가 동일한 작은 메서드를 인터페이스에 추가하고 싶을 뿐인데도, 해당 내용을 모든 곳에 전파해야 함 (코드 중복)
  → 디폴트 메서드의 등장 배경

```java
interface MyInterface {
	void method();
	default void newMethod() {}
}
```

### 디폴트 메서드가 기존의 메서드와 충돌할 때의 해결책

- 디폴트 메서드를 선언해서 간단한 구현부가 존재하는 메서드를 인터페이스에 추가할 수 있게 된것은 좋음
- 인터페이스는 다중상속을 허용하고 있는데, 이 경우에 메서드의 구현부가 다른 것들이 충돌할 가능성이 생김
  1. 여러 인터페이스 디폴트 메서드간의 충돌
     - 인터페이스를 구현한 클래스에서 디폴트 메서드를 오버라이딩 해야 함
     - 오버라이딩을 통해 다시한번 구현부를 작성해서 덮어쓰기
  2. 디폴트 메서드와 조상 클래스의 메서드 간의 충돌
     - 조상 클래스의 메서드가 상속되고, 디폴트 메서드는 무시됨
     - extends로 상속받은 조상 클래스의 메서드가 우선되며 인터페이스는 우선순위에서 밀림
     - 그게 싫으면 오버라이딩으로 구현부를 작성해서 덮어쓰기
