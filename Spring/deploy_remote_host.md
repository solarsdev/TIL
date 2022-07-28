# 원격 프로그램 실행

## Spring에서 클래스 생성 후 등록하기

- `Spring`에서 클래스를 웹페이지에서 호출 가능하도록 하려면 `@Controller` 어노테이션을 등록함
- `@Controller` 어노테이션이 붙은 클래스 내부의 메서드를 URL과 맵핑할 수 있는데, 이때 사용하는 것이 `@RequestMapping` 어노테이션
- 톰캣은 기본적으로 `@RequestMapping` 이 붙은 메서드를 실행하기 위해 `@Controller` 로 지정된 객체를 생성한 뒤 메서드를 실행하기 때문에, 모든 인스턴스 변수에 접근 가능한 인스턴스 메서드로 작성하는 것이 좋음

```java
// 1. 원격 호출가능한 프로그램으로 등록
@Controller
public class Hello {

	public static int cv = 20;
	public int iv = 10;

	// 2. URL과 메서드를 연결
	@RequestMapping("/main")
	public void main() {
		System.out.println("main");
		System.out.println(cv); // OK
		System.out.println(iv); // OK
	}

	@RequestMapping("/main3")
	private void main3() {
		System.out.println("main3"); // OK
	}

	@RequestMapping("/main2")
	private static void main2() {
		System.out.println("main2");
		System.out.println(cv); // OK
		// System.out.println(iv); // NOT OK
	}
}
```

### 웹상에서 `private` 메서드를 호출 가능한 이유

- `Reflection API`를 이용 - 클래스 정보를 얻고 다룰 수 있는 강력한 기능 제공
- `java.lang.reflect` 패키지를 제공
- `Hello`클래스의 `Class`객체(클래스의 정보를 담고 있는 객체)를 얻어옴

```java
public class Main {
	public static void main(String[] args) throws Exception {
		// Hello hello = new Hello();
		// hello.main3(); // private이기 때문에 기본적으로 호출 불가
		Class helloClass = Class.forName("Hello");
		Hello hello = (Hello) helloClass.newInstance(); // Class객체가 가진 정보로 객체 생성
		Method main3 = helloClass.getDeclaredMethod("main3");
		main3.setAccessible(true); // private인 main3()을 호출 가능하게 한다.
		main3.invoke(hello);
	}
}
```

## Docker로 빌드 후 배포

- 만들어진 프로젝트를 WAR 파일로 추출

```docker
FROM tomcat:9-jdk11-corretto

COPY ch2.war /usr/local/tomcat/webapps/
```

```bash
$ docker build -t solarsdev/spring-app
$ docker run --rm -p 8080:8080 --name spring-app solarsdev/spring-app
```

- 웹주소 `http://localhost:8080/ch2/main2`
