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
