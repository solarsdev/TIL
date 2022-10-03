# Ruby.new

## 객체 지향 언어 루비

- 루비에서 프로그래머가 다루는 모든 것이 객체
- 클래스의 생성자를 이용해 객체를 생성 (new 메서드로 생성자 호출 가능)

### 다른 객체 지향 언어와의 차이점

- 자바와 같은 객체지향 언어들은 기본형의 경우에는 객체가 아님

```java
num = Math.abs(num) // 자바 코드에서 절대값 구하기
```

- 루비는 숫자 자체가 객체이므로 이미 메서드를 가지고 있음

```ruby
num = -1234 # => -1234
positive = num.abs # => 1234
```

## 루비 기초

- 메서드의 정의는 `def` 키워드를 이용
- 중괄호는 사용하지 않고 정의에 끝부분에 `end` 키워드를 사용하면 됨
- 메서드의 호출에 괄호는 생략이 가능하지만, 대부분의 경우에는 사용이 권장됨

### 루비에서 문자열의 처리방법

- 리터럴을 정의할때 작은 따옴표와 큰 따옴표를 둘 다 사용 가능
- 작은 따옴표를 사용하면 문자 그대로 리터럴이 됨
- 큰 따옴표를 사용하면 내부에서 처리할수 있는 특수 문자열이 발견되면 처리를 수행
  - 예를 들어 `\n`은 줄 바꿈 문자로 변경됨
  - 또 표현식 보간(`expression interpolation`)이라고 불라는 평가값 대체를 수행
  ```ruby
  def say_goodnight(name)
  	result = "Good night, #{name}"
  	return result
  end
  puts say_goodnight('Pa')
  ```
  - 표현식 보간은 일종의 템플릿 스트링으로 안의 내용을 프로그래밍적 처리의 결과로 치환함

### 리턴의 생략

- 루비에서는 메서드의 반환값이 표현식의 결과값일때 리턴을 생략 가능

```ruby
def say_goodnight(name)
	"Good night, #{name.capitalize}"
end
puts say_goodnight('ma')
```

### 루비에서의 `naming conventions`

- 지역 변수, 메서드 매개 변수, 메서드 이름 → 소문자나 밑줄로 시작
- 전역 변수 → `$` (달러사인)으로 시작
- 인스턴스 변수 → `@` (앳사인)으로 시작
- 클래스 이름, 모듈 이름, 상수 → 대문자로 시작
- 여러 단어로 이루어져 있을 경우
  - 인스턴스 변수는 `snake_case`
  - 클래스 이름의 경우에는 `MixedCase`
  - 메서드의 이름은 `?`, `!`, `=` 기호로 끝날 수 있음

## 배열과 해시

- 루비의 배열과 해시는 색인된 컬렉션 (`indexed collection`)
- 배열과 해시의 차이점은 키값으로 어떤것을 사용할수 있는지 여부
  - 배열은 정수만 가능 (속도에서 어드밴티지)
  - 해시는 어떤 객체든 사용 가능

```ruby
a = [ 1, 'cat', 3.14 ] # 세 개의 요소를 가지는 배열
puts "The first element is #{a[0]}"
a[2] = nil
puts "The array is now #{a.inspect}"
```

- `nil`은 “아무것도 아님”을 표현하는 또 하나의 객체로 자바에서의 `null`과는 결이 다름
- 루비에서는 단축 문법 `%w`를 이용하면 쉼표를 넣을 필요가 없음

```ruby
a = [ 'ant', 'bee', 'cat', 'dog', 'elk' ]
a[0] # => "ant"
a[3] # => "dog"
# 다음과 같음
a = %w{ ant bee cat dog elk }
a[0] # => "ant"
a[3] # => "dog"
```

- 해시 리터럴은 대괄호 대신 중괄호를 사용
- 해시는 키와 값으로 구분해야 함

```ruby
inst_section = {
	'cello' => 'string',
	'clarinet' => 'woodwind',
	'drum' => 'percussion',
	'oboe' => 'woodwind',
	'trumpet' => 'brass',
	'violin' => 'string'
}
```

- 왼쪽이 키, 오른쪽이 값
- 특정 해시 안에서 키는 유일해야 함
- 해시의 값을 얻기 위해서는 대괄호를 사용

```ruby
p inst_section['oboe']
p inst_section['cello']
p inst_section['bassoon']

# "woodwind"
# "string"
# nil
```

- 주어진 키가 존재하지 않을때 `nil`을 반환하며, 이는 거짓을 의미함 (조건문에서 유용)
- 해시는 기본값을 가질 수 있으며, 이를 통해 연산을 수행할수 있음
  - 예를 들어 단어가 존재하는지 확인하는 프로그램에서 `nil`이 아닌 `0`으로 기본값을 주고 단어가 등장할때마다 `+1`을 해준다거나
-
