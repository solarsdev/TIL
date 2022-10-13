# 오버라이딩 (overriding)

## 오버라이딩이란?

- 조상 클래스로부터 상속받은 메서드의 내용을 상속받는 클래스에 맞게 변경하는 것

## 오버라이딩의 조건

1. 선언부가 같아야 함
   - 이름
   - 매개변수
   - 리턴타입
2. 접근제어자는 좁은 범위로는 변경할 수 없음
   - 조상의 메서드가 protected라면, 범위가 같거나 넓은 protected 혹은 public만 가능
3. 조상 클래스의 메서드보다 더 많은 수의 예외 선언 불가

```java
class Parent {
	void parentMethod() throws IOException, SQLException {}
}

class Child extends Parent {
	void parentMethod() throws IOException
}

class Child2 extends Parent {
	void parentMethod() throws Exception {} // 불가
}

```

## 오버로딩 vs 오버라이딩

- 이름이 비슷해서 헷갈릴 수 있지만, 오버로딩은 메서드를 매개변수만 다르게 동일한 이름으로 여러개를 선언하는 것
- 오버라이딩은 조상 클래스로부터 상속된 메서드를 재정의 하는 것

## super (참조변수)

- this
  - 인스턴스 자신을 가리키는 참조변수
  - 인스턴스의 주소가 저장되어 있음
  - 모든 인스턴스 메서드에 지역변수로 숨겨진 채로 존재
- super
  - this와 같음
  - 조상의 멤버와 자신의 멤버를 구별하는 데 사용

## super() 조상의 생성자

- 자손 클래스의 인스턴스를 생성하면, 자손의 멤버와 조상의 멤버가 합쳐진 하나의 인스턴스가 생성됨
- 조상의 멤버들도 초기화가 필요하기 때문에, 자손의 생성자의 첫 문장에서 조상의 생성자를 호출해야 함
- 만약 정의되어 있지 않을 경우 컴파일러가 자동으로 삽입해주기 때문에 무조건 정의해야 하는 것은 아님
