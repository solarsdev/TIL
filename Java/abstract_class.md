# Abstract Class

## 추상 클래스

### 추상 클래스란?

- 미완성 설계도로서 미완성 메서드를 갖고 있는 클래스

```java
abstract class Player {    // 추상 클래스(미완성 클래스)
	abstract void play(int pos);    // 추상 메서드(몸통{}이 없는 미완성 메서드)
	abstract void stop();    // 추상 메서드
}
```

- 추상 클래스는 다른 클래스 작성에 도움을 주기 위한 것으로, 인스턴스를 생성할 수는 없음

```java
Player p = new Player();    // Error, 추상 클래스의 인스턴스 생성 불가
```

- 상속을 통해 추상 메서드를 완성(오버라이딩)해야 인스턴스 생성 가능

```java
class AudioPlayer extends Player {
	@Override
	void play(int pos) { ... }    // 추상 메서드를 구현

	@Override
	void stop() { ... }    // 추상 메서드를 구현
}

AudioPlayer ap = new AudioPlayer();    // OK
```

## 추상 메서드

### 추상 메서드란?

- 미완성 메서드로 구현부(`Body {}`)가 없는 메서드

```java
abstract Type methodName();
```

- 추상 메서드는 꼭 필요지만, 자손마다 다르게 구현될 것으로 예상되는 경우에 사용
- 추상 클래스의 자손이 꼭 바로 위의 부모로부터 물려받은 메서드를 구현해야 할 필요는 없으며, 일부만 구현하고 다시 상속으로 남겨둘 수 있음 (단, 이때는 해당 클래스도 추상 클래스가 됨)

```java
abstract class Player {    // 추상 클래스(미완성 클래스)
	abstract void play(int pos);    // 추상 메서드(몸통{}이 없는 미완성 메서드)
	abstract void stop();    // 추상 메서드
}

class AudioPlayer extends Player {
	@Override
	void play(int pos) { ... }    // 추상 메서드를 구현

	void stop() { ... }    // 추상 메서드를 구현
}

abstract class AbstractPlayer extends Player {
	@Override
	void play(int pos) { ... }    // 추상 메서드를 구현

	// abstract void stop(); // 생략되어 있음
}
```

- 추상 메서드는 최종적으로 완성될 클래스에서 반드시 존재할것이 보장되기 때문에, 미구현 상태인 추상 클래스 내에서 활용이 가능함

```java
abstract class Player {
	boolean pause;
	int currentPos;

	Player() {
		pause = false;
		currentPost = 0;
	}

	abstract void play(int pos);    // 추상 메서드
	abstract void stop();    // 추상 메서드

	void play() {
		play(currentPos);    // 오버로딩된 추상 메서드를 호출함
	}
}
```

## 추상 클래스의 작성

- 여러 클래스에 공통적으로 사용될 수 있는 추상 클래스를 바로 작성
- 기존 클래스의 공통 부분을 뽑아서 추상 클래스를 만듬

```java
class Marine {
	int x, y;
	void move(int x, int y)  { /* 지정된 위치로 이동 */ }
	void stop()              { /* 현재 위치에 정지 */ }
	void stimPack()          { /* 스팀팩 사용 */ }
}

class Tank {
	int x, y;
	boolean siegeMode;
	void move(int x, int y)  { /* 지정된 위치로 이동 */ }
	void stop()              { /* 현재 위치에 정지 */ }
	void changeMode()        { /* 공격모드 변환 */ }
}

class Dropship {
	int x, y;
	void move(int x, int y)  { /* 지정된 위치로 이동 */ }
	void stop()              { /* 현재 위치에 정지 */ }
	void load()              { /* 선택된 대상을 태움 */ }
	void unload()            { /* 선택된 대상을 내림 */ }
}

```

```java
// 공통된 부분을 뽑아서 Unit 클래스 작성
abstract class Unit {
	int x, y;
	abstract void move(int x, int y);
	void stop()              { /* 현재 위치에 정지 */ }
}

class Marine extends Unit {
	@Override
	void move(int x, int y)  { /* 지정된 위치로 이동 */ }

	void stimPack()          { /* 스팀팩 사용 */ }
}

class Tank extends Unit {
	boolean siegeMode;

	@Override
	void move(int x, int y)  { /* 지정된 위치로 이동 */ }

	void changeMode()        { /* 공격모드 변환 */ }
}

class Dropship extends Unit {
	@Override
	void move(int x, int y)  { /* 지정된 위치로 이동 */ }

	void load()              { /* 선택된 대상을 태움 */ }
	void unload()            { /* 선택된 대상을 내림 */ }
}
```

- 공통된 조상을 만들면 다형성을 이용해서 그룹별로 활용이 가능한 장점이 있음
- 조상 클래스의 메서드를 호출해도 다형성의 장점을 이용해서 자손 객체들의 메서드를 호출하게 됨 (가상 메서드)

```java
Unit[] group = new Unit[3];
group[0] = new Marine();
group[1] = new Tank();
group[2] = new Dropship();

for(int i = 0; i < group.length; i++) {
	group[i].move(100, 200);
}
```
