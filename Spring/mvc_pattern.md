# 관심사(Concern)의 분리, MVC 패턴

## 관심사의 분리 (Separation of Concerns)

- 자바 서블릿 메서드의 내용을 보면 각 관심사(Concern)가 조금 다른 부분이 있다는 점을 찾을 수 있음
  - 관심사는 각각 함수의 기본적인 구조와 동일
    1. 입력
    2. 처리
    3. 출력

## OOP의 5대 원칙

- SOLID
  - 단일 책임 원칙 (SRP)
    - 하나의 메서드는 하나의 책임만 진다 (= 하나의 관심사만 가진다)
    - 코드의 분리가 중요
      - 관심사의 분리
      - 변하는것, 변하지 않는것의 분리
      - 공통(중복) 코드의 분리 (Common, Uncommon)

## 입력의 분리

```java
@RequestMapping("/getYoil")
public void main(HttpServletRequest request, HttpServletResponse response) throws IOException {
	// 1. Input
	String year = request.getParameter("year");
	String month = request.getParameter("month");
	String day = request.getParameter("day");

	int yyyy = Integer.parseInt(year);
	int mm = Integer.parseInt(month);
	int dd = Integer.parseInt(day);
```

```java
@RequestMapping("/getYoil")
public void main(String year, String month, String day, HttpServletResponse response) throws IOException {
```

```java
@RequestMapping("/getYoil")
public void main(int year, int month, int day, HttpServletResponse response) throws IOException {
```

## 출력의 분리

```java
@RequestMapping("/getYoil")
public void main(int year, int month, int day, HttpServletResponse response) throws IOException {
	// 2. Process
	Calendar cal = Calendar.getInstance();
	cal.set(year, month - 1, day);

	int dayOfWeek = cal.get(Calendar.DAY_OF_WEEK);
	char yoil = " 일월화수목금토".charAt(dayOfWeek);

	// 3. Output
	response.setContentType("text/html");
	response.setCharacterEncoding("utf-8");
	PrintWriter out = response.getWriter(); // response객체에서 브라우저로의 출력 스트림을 얻는다.
	out.println(year + "년 " + month + "월 " + day + "일은 ");
	out.println(yoil + "요일입니다.");
}
```

- 처리부를 `Controller`, 표현부를 `View`로 분리하고, `Controller`에서 `View`로 데이터를 넘기기 위해 `Model` 객체를 이용 → MVC 패턴

### Controller

```java
// 2. Process
Calendar cal = Calendar.getInstance();
cal.set(year, month - 1, day);

int dayOfWeek = cal.get(Calendar.DAY_OF_WEEK);
char yoil = " 일월화수목금토".charAt(dayOfWeek);
```

### View

```java
// 3. Output
response.setContentType("text/html");
response.setCharacterEncoding("utf-8");
PrintWriter out = response.getWriter(); // response객체에서 브라우저로의 출력 스트림을 얻는다.
out.println(year + "년 " + month + "월 " + day + "일은 ");
out.println(yoil + "요일입니다.");
```

## 출력의 분리 (변하는 것과 변하지 않는 것의 분리)

### 단순화된 요청의 처리 순서

- 스프링에서는 요청이 오면 `DispatcherServlet`이 요청을 객체화해서 입력부를 자동으로 처리해줌
- 객체화된 것을 `Model`이라고 하는데, 이 `Model`을 `Controller`에게 전달해줌
- `Controller`에서는 정의된 사양에 의해 데이터를 처리하고, 결과를 `Model` 객체에 담아둠
- `Controller`는 처리가 끝나면 `DispatcherServlet`에게 결과를 출력한 `View`의 이름을 반환
- `DispatcherServlet`은 결과가 담긴 모델 객체를 `View`에 전달하고 실행
- `View`는 `Model`을 해석하여 템플릿 스트링으로 치환하고 응답을 작성, 응답은 클라이언트에게 전달

## MVC 패턴의 정확한 원리

- `DispatcherServlet`이 내부적으로 처리하는 내용을 직접 자바로 구현해보면서 실질적으로 어떤 방식으로 `MVC`가 유기적으로 연계되는지 확인

```
[txtView1.txt]
id=${id}, pwd=${pwd}

[txtView2.txt]
id:${id}
pwd:${pwd}
```

```java
// MethodCall1.java

class ModelController {
	public String main(HashMap map) {
		map.put("id", "asdf");
		map.put("pwd", "1111");

		return "txtView1";
	}
}

public class MethodCall {
	public static void main(String[] args) throws Exception {
		HashMap map = new HashMap();
		System.out.println("before:" + map);

		ModelController mc = new ModelController();
		String viewName = mc.main(map);

		System.out.println("after :" + map);

		render(map, viewName);
	}

	static void render(HashMap map, String viewName) throws IOException {
		String result = "";

		// 1. 뷰의 내용을 한줄씩 읽어서 하나의 문자열로 만든다.
		Scanner sc = new Scanner(new File(viewName + ".txt"));

		while (sc.hasNextLine())
			result += sc.nextLine() + System.lineSeparator();

		// 2. map에 담긴 key를 하나씩 읽어서 template의 ${key}를 value바꾼다.
		Iterator it = map.keySet().iterator();

		while (it.hasNext()) {
			String key = (String) it.next();

			// 3. replace()로 key를 value 치환한다.
			result = result.replace("${" + key + "}", (String) map.get(key));
		}

		// 4.렌더링 결과를 출력한다.
		System.out.println(result);
	}
}
```

- MethodCall 클래스의 main() 함수는 HashMap을 통해서 정보를 전달할 객체를 생성함
- 여기서 main() 함수가 수행하는 것이 `DispatcherServlet` 이 수행하는 로직
- `HashMap map` 변수는 `Model model` 객체
- `ModelController mc`는 실질적으로 `model`에 데이터를 담는 `Controller`
- `render()` 함수는 `View`의 이름을 받아서 `jsp` 파일을 불러온 뒤, `템플릿 스트링을 치환`하는 처리로직
