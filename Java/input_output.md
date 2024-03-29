# File Input / Output

## 입출력이란?

- `I/O`의 의미는 `Input Output`으로 입력과 출력을 말함
- 지금까지 사용했던 `System.out.println`이나 `Scanner`의 `next`와 같은 메서드들이 입출력의 예

## 스트림 (`stream`)

- 입출력은 데이터를 주고 받는 통로를 이용해야 가능한데 이 통로를 스트림이라고 부름
- `JDK 1.8` 이후에 등장한 `Stream API`와는 이름만 같을 뿐 내용은 다름
- 스트림은 단방향 통신으로 데이터를 주고, 받기 위해서는 2개의 스트림이 필요

![images/input_output/1.png](images/input_output/1.png)

### 바이트기반 스트림 `InputStream`, `OutputStream`

![images/input_output/2.png](images/input_output/2.png)

- 바이트 기반 스트림은 대상에 따라 선택해서 사용하게 됨
- 각각 `InputStream`, `OutputStream`의 자손으로 추상메서드를 구현한 구현체
- 추상메서드를 구현하는 방식으로 만들어놓은 이유는 입출력 대상이 달라져도 방법은 달라지지 않도록 표준화 하기 위함

![images/input_output/3.png](images/input_output/3.png)

- `InputStream`의 `read()`
- `OutputStream`의 `write()`
- 추상메서드가 아닌 나머지 메서드들 또한 결국 추상메서드를 이용하여 구현해두었으므로 각각의 자손에서 `read`를 구현하지 않으면 의미가 없게 되어 있음

![images/input_output/4.png](images/input_output/4.png)

- 결국 오버로딩한 메서드에서 추상메서드를 참조하고 있기 때문에, 참조된 추상메서드를 각 대상에 맞게 구현하도록 되어 있음

### 보조 스트림

- 대상에 따라 나눠진 기반 스트림 외에도 기반 스트림의 기능을 보조하기 위해 보조 스트림이 제공됨
- 보조 스트림의 역할은 실제로 데이터를 주고받는 통로 역할이 아님
  - 보조 스트림을 생성하기 이전에 기반 스트림을 생성해야 함
  - 보조 스트림은 기반 스트림을 매개변수로 입력받아 생성함

![images/input_output/5.png](images/input_output/5.png)

- 코드상으로는 보조 스트림을 이용해서 입출력을 수행하는 것 처럼 보이지만 실제 `BufferInputStream`은 버퍼만을 제공
- 보조 스트림을 사용하는 이유는 기반 스트림의 성능 개선이라던가, 새로운 기능이 필요하기 때문
  - 실제로 버퍼를 사용한 입출력과 그렇지 않은 경우에는 상당한 성능차이가 있음

![images/input_output/6.png](images/input_output/6.png)

### 문자기반 스트림 Reader, Writer

- 바이트기반은 1바이트를 기준으로 데이터를 처리하는데, 자바에서는 문자를 2바이트 기준으로 처리하기 때문에 바이트 스트림으로는 문자를 처리하기가 곤란
- 따라서 문자데이터를 다루기 위해 별도의 문자기반 스트림을 제공

![images/input_output/7.png](images/input_output/7.png)

<aside>
💡 StringBufferInputStream, StringBufferOutputStream은 StringReader와 StringWriter로 대체됨

</aside>

- 기본적으로 문자기반 스트림은 바이트기반 스트림에서 InputSream을 Reader로, OutputStream을 Writer로 이름 변경하면 동일
  - 단, ByteArrayInputStream에 대응하는 문자기반 스트림의 이름이 CharArrayReader인것에 주의
- 내부적으로 바이트기반 스트림에서 int를 이용해서 데이터를 다루던 것과 달리 문자기반 스트림에서는 char를 기본 단위로 사용한다는 점이 다름

![images/input_output/8.png](images/input_output/8.png)

- 문자보조 스트림 또한 존재하며 사용법은 동일

## 바이트기반 스트림

### `InputStream`과 `OutputStream`

- `InputStream`과 `OutputStream`은 모두 바이트 기반 스트림

![images/input_output/9.png](images/input_output/9.png)

![images/input_output/10.png](images/input_output/10.png)

- 읽기전용 스트림을 사용할 때 `mark()`와 `reset()`을 사용하면 읽던 중이라도 다시 처음으로 돌아가서 읽기가 가능
  - 해당 기능을 지원하는지 확인하는 `markSupported()`가 존재
- 출력전용 스트림을 사용할 때 `flush()`는 버퍼가 있는 출력 스트림의 경우에만 의미가 있고, 버퍼 없이 사용하는 `flush()`는 아무 동작을 하지 않음
- 스트림을 사용하면 기본적으로 `close()`를 이용해서 닫는 습관을 들일것
  - 프로그램이 종료되기 전 `JVM`이 자동으로 닫긴 하지만, 그 이전에 계속 남겨두는 스트림은 계속 남음
  - `ByteArrayInputStream`과 같은 메모리를 사용하는 스트림과 `System.in`, `System.out`과 같은 표준 입출력 스트림은 닫을 필요 없음

### `ByteArrayInputStream`과 `ByteArrayOutputStream`

- 바이트 배열에 데이터를 입출력할 때 사용
- 주로 다른 곳에 입출력하기 전 데이터를 임시로 바이트 배열에 담아서 변환등을 하는데 사용

```java
byte[] inSrc = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
byte[] outSrc = null;

ByteArrayInputStream input = new ByteArrayInputStream(inSrc);
ByteArrayOutputStream output = new ByteArrayOutputStream();

int data = 0;

while ((data = input.read()) != -1) {
    output.write(data);
}

outSrc = output.toByteArray();

System.out.println("input source: " + Arrays.toString(inSrc));
System.out.println("output source: " + Arrays.toString(outSrc));
```

```java
while ((data = input.read()) != -1) {
    output.write(data);
}

// 1. data = input.read() // read()를 호출한 반환값을 변수 data에 저장
// 2. data != -1 // data에 저장된 값이 -1이 아닌지 비교
```

- 바이트 배열은 사용하는 자원이 메모리밖에 없으므로 가비지 컬렉터에 의해 자동으로 회수됨 → close()하지 않아도 됨
- 상기 샘플은 한번에 1바이트씩 읽어서 쓰기 때문에 효율이 떨어짐

```java
byte[] temp = new byte[10];

input.read(temp, 0, temp.length);
output.write(temp, 5, 5);
```

- 한번에 여러개를 읽어올 수 있도록 버퍼(temp)를 마련하면 효율이 올라감
  - 단, 버퍼는 메모리를 소모하므로 버퍼의 크기가 무한정 증가하는 것은 또 다른 문제를 야기

### `FileInputStream`과 `FileOutputStream`

- 파일을 입출력 하기 위한 스트림으로, 실제 코딩에서 많이 사용되는 스트림 중 하나

![images/input_output/11.png](images/input_output/11.png)

```java
import java.io.FileInputStream;
import java.io.IOException;

public class FileViewer {
    public static void main(String[] args) throws IOException {
        FileInputStream fis = new FileInputStream(args[0]);
        int data = 0;

        while ((data = fis.read()) != -1) {
            char c = (char) data;
            System.out.print(c);
        }
    }
}
```

- 심플한 파일 읽기 프로그램으로, `fis.read()`가 반환하는 값은 파일 내 문자열이므로 `0~255`와 값이 없을때 읽어오는 `-1`이면 충분하기 때문에 `int`로 입력받아 `char`로 형변환 하는것은 문제가 없음

## 바이트기반의 보조스트림

### `FilterInputStream`과 `FilterOutputStream`

- 모든 보조스트림의 조상
- 각각 `InputStream`과 `OutputStream`의 자손

```java
protected FilterInputStream(InputStream in)
public FilterOutputStream(OutputStream out)
```

- `FilterInputStream`은 생성자가 `protected`로 설정되어 있으므로 직접 생성할 수 없고, 상속을 통해 오버라이딩한 클래스의 인스턴스를 생성해야 함
- `FilterInputStream`의 자손
  - `BufferedInputStream`
  - `DataInputStream`
  - `PushbackInputStream`
  - …
- `FilterOutputStream`의 자손
  - `BufferedOutputStream`
  - `DataOutputStream`
  - `PrintStream`
  - …

### `BufferedInputStream`과 `BufferedOutputStream`

- 스트림의 입출력 효율을 높이기 위해 버퍼를 사용하는 보조스트림
- 한 바이트씩 입력/출력을 반복하는 것 보다는 버퍼(바이트배열)을 이용해서 한번에 여러 바이트씩 입출력 하는것이 빠름
- 버퍼 크기를 정하는 것이 핵심인데, 일반적으로는 입력소스가 파일인 경우 8K(8192)를 기본으로 지정하며, 버퍼의 크기를 변경해가면서 최적화가 가능함

![images/input_output/12.png](images/input_output/12.png)

- 버퍼는 외부 입력소스에서 `InputStream`으로 계속 채워지고 (`read`) `OutputStream`의 `write`는 출력 내용을 계속해서 버퍼에 저장하고, 전부 채워지면 출력 소스에 저장
- 출력 소스에 저장한 뒤에는 버퍼를 클리어하게 됨
- 버퍼가 가득 찼을 때만 출력소스에 쓰기 때문에, 마지막 단계에서 버퍼 크기를 다 채우지 못하면 출력 소스에 해당 버퍼 부분만큼의 여분이 출력되지 않을 수 있음
  - 이럴때는 `flush()`를 통해 강제로 버퍼를 비우고, 출력소스에 기록해야 함

### `DataInputStream`과 `DataOutputStream`

- `FilterInputStream`, `FilterOutputStream`의 자손
- `DataInput` 인터페이스, `DataOutput` 인터페이스의 구현체
- 데이터를 읽고 쓰는것이 바이트 단위가 아닌, 8가지 기본 자료형 단위로 가능
  - 출력 형식은 각 기본 자료형을 16진수로 저장
  - 예를 들어 `int`값을 출력하면 4바이트 16진수로 출력
- 각 기본 자료형별로 크기가 다르기 때문에 입력과 출력시 염두해두어야 함

![images/input_output/13.png](images/input_output/13.png)

### `SequenceInputStream`

![images/input_output/14.png](images/input_output/14.png)

- 여러개의 입력 스트림을 연속적으로 연결해서 하나의 스트림으로부터 데이터를 읽는것과 같이 만들어줌
- `Vector`에 연결할 입력스트림을 저장한 뒤 `Vector`의 `elements()`를 매개변수로 생성자를 호출

```java
Vector files = new Vector();
files.add(new FileInputStream("file.001");
files.add(new FileInputStream("file.002");
SequenceInputStream in = new SequenceInputStream(files.elements());
```

- `Vector`에 추가된 입력스트림의 순서대로 입력되는 것에 주의

### `PrintStream`

![images/input_output/15.png](images/input_output/15.png)

- 기반스트림의 데이터를 다양한 형태로 출력할수 있도록 `print`, `println`, `printf`와 같은 메서드를 제공
- 자바에서 자주 사용하고 있던 `System.out`이 대표적인 `PrintStream`
- 다만, `PrintWriter`가 더 다양한 문자를 처리하는데 적합하기 때문에 가능하면 `PrintWriter`를 사용하는것이 좋음

### `printf`의 다양한 표현식

![images/input_output/16.png](images/input_output/16.png)

## 문자기반 스트림

### `Reader`와 `Writer`

![images/input_output/17.png](images/input_output/17.png)

- 바이트기반 스트림의 `InputStream`/`OutputStream`과 같이 문자기반 스트림에서의 최고조상
- `byte`대신 `char`를 사용하는것이 특징이며 기타는 바이트 기반 스트림과 다른것이 없음
- 문자기반이라는 특징에는 단순히 2바이트 스트림을 처리한다는 것이 아닌, 문자가 가진 특성을 읽고 쓸수 있도록 인코딩을 지원하는것에 있음
  - `Reader`는 특정 인코딩을 읽어서 유니코드로 변환
  - `Writer`는 유니코드를 특정 인코딩으로 변환

### `FileReader`와 `FileWriter`

- 파일로부터 텍스트를 읽고 파일을 쓰는데 사용
- 기본적인 사용법은 `FileInputStream`/`FileOutputStream`과 다른것이 없음

### `PipedReader`와 `PipedWriter`

- 쓰레드 간에 데이터를 주고 받을때 사용
- 입력과 출력스트림을 하나의 스트림으로 연결해서 데이터를 주고 받음
- 스트림을 생성 후 어느 한쪽 쓰레드에서 `connect()`를 호출하면 연결됨
- 연결된 이후에는 입력이나 출력 어느 한쪽에서 스트림을 닫으면 반대편은 자동으로 닫힘

### `StringReader`와 `StringWriter`

![images/input_output/18.png](images/input_output/18.png)

- `CharArrayReader`, `CharArrayWriter`처럼 메모리의 입출력에 사용
- `StringWriter`에 출력되는 데이터는 내부의 `StringBuffer`에 저장됨

## 문자기반의 보조스트림

### `BufferedReader`와 `BufferedWriter`

![images/input_output/19.png](images/input_output/19.png)

- 버퍼를 이용해서 입출력의 효율을 증가시킴 (효율성이 비교할수 없을 정도이므로 가능한한 사용)
- `BufferedReader`의 `readLine()`은 라인단위로 데이터를 읽을 수 있는 메서드
- `BufferedWriter`의 `newLine()`은 출력시 줄바꿈을 자동으로 수행해줌

### `InputStreamReader`와 `OutputStreamWriter`

- 바이트기반 스트림을 문자기반 스트림처럼 쓸 수 있게 도와줌
- 인코딩(`encoding`)을 변환하여 입출력이 가능하도록 해줌

![images/input_output/20.png](images/input_output/20.png)

```java
// 콘솔(console,화면)로부터 라인단위로 입력받기
InputStreamReader isr = new InputStreamReader(System.in);
BufferedReader br = new BufferedReader(isr);
String line = br.readLine();

// 인코딩 변환하기
FileInputStream fis = new FileInputStream("korea.txt");
InputStreamReader isr = new InputStreamReader(fis, "KSC5601");
```

## 표준입출력과 File

### 표준입출력 `System.in` `System.out` `System.err`

![images/input_output/21.png](images/input_output/21.png)

- 콘솔(`console`,화면)을 통한 데이터 입출력을 “표준 입출력”이라 부름
- `JVM`이 시작되면 자동으로 생성되는 스트림으로 별도의 생성없이 바로 사용 가능

### 표준입출력의 대상 변경 `setOut()`, `setErr()`, `setIn()`

- 기본적으로는 `System.in`, `out`, `err`의 입출력대상은 콘솔이지만, `setOut()`, `setErr()`, `setIn()`등을 이용해 해당 대상을 다른것으로 변경 가능

```java
static void setOut(PrintStream out)
static void setErr(PrintStream err)
static void setIn(InputStream in)
```

- 예를 들어 예외의 경우에는 콘솔이 아닌 파일로 별도 로깅을 하고 싶을 때

```java
FileOutputStream fos = new FileOutputStream("error.log");
ps = new PrintStream(fos);
System.setOut(ps);
```

### `RandomAccessFile`

- 하나의 스트림으로 입력과 출력을 모두 수행할 수 있는 스트림
- 다른 스트림과 달리 `Object`를 상속
- `DataInput`과 `DataOutput` 인터페이스의 구현체

![images/input_output/22.png](images/input_output/22.png)

### `File`

- 자바에서는 파일을 다룰 수 있는 클래스를 별도 제공

![images/input_output/23.png](images/input_output/23.png)

- `OS`에 따라서 파일의 경로나 디렉토리의 이름을 구분하는 구분자가 다름
- 즉, 세퍼레이터를 메서드로 사용하지 않고 직접 코드에 입력하면 `OS`에 종속적인 프로그램이 되어버림

### 파일 클래스의 메서드들

![images/input_output/24.png](images/input_output/24.png)

## 직렬화 (`serialization`)

### 직렬화(`serialization`)란?

- 객체를 “연속적인 데이터”로 변환하는 것
- 반대과정을 “역직렬화”라고 함
- 객체들의 인스턴스 변수들의 값을 일렬로 나열하는 것
- 객체를 저장하기 위해서는 객체를 직렬화 해야 함
- 객체를 저장한다는 것은 객체가 가진 모든 인스턴스 변수의 값을 저장하는 것

### `ObjectInputStream`, `ObjectOutputStream`

- 객체를 직렬화하여 입출력 가느앟게 해주는 보조스트림

```java
ObjectInputStream(InputStream in);
ObjectOutputStream(OutputStream out);
```

- 객체를 파일에 저장하는 방법

```java
FileOutputStream fos = new FileOutputStream("objectfile.ser");
ObjectOutputStream out = new ObjectOutputStream(fos);

out.writeObject(new UserInfo());
```

- 파일에 저장된 객체를 다시 읽어오는 법

```java
FileInputStream fis = new FileInputStream("objectfile.ser");
ObjectInputStream in = new ObjectInputStream(fis);

UserInfo info = (UserInfo) in.readObject();
```

### 직렬화 가능한 클래스 만들기

- `java.io.Serializable`을 구현한 클래스만 직렬화가 가능

```java
public class UserInfo implements java.io.Serializable {
	String name;
	String password;
	int age;
}
```

- 제어자 `transient`가 붙은 인스턴스 변수는 직렬화 대상(저장대상)에서 제외

```java
public class UserInfo implements java.io.Serializable {
	String name;
	transient String password; // 직렬화 대상에서 제외됨
	int age;
}
```

- `Serializable`을 구현하지 않은 클래스의 인스턴스도 직렬화 대상에서 제외

```java
public class UserInfo implements java.io.Serializable {
	String name;
	transient String password; // 직렬화 대상에서 제외됨
	int age;

	Object obj = new Object(); // Object객체는 직렬화할 수 없음
}
```

- `Serializable`을 구현하지 않은 조상의 멤버들은 직렬화 대상에서 제외

```java
public class SuperUserInfo {
	String name; // 직렬화 하지 않음
	String password; // 직렬화 하지 않음
}

public class UserInfo extends SuperUserInfo implements Serializable {
	int age;
}
```

- 직렬화 했을 때와 역직렬화했을때의 클래스가 같은지 확인할 필요가 있음
- 직렬화 할 때 클래스의 버전 (`serialVersionUID`)를 자동계산해서 저장
- 클래스의 버전을 수동으로 관리하려면 클래스 내에 정의해야 함
- `serialver.exe`는 클래스의 `serialVersionUID`를 자동으로 생성해줌
