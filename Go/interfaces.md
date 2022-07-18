# Interfaces

## Purpose of interfaces

- 인터페이스를 처음 접하게 되면, 인터페이스 안에 코딩에 필요한 룰이 들어있다고 하지만, 실제로 개발하는 입장에서는 인터페이스가 정확하게 어떤 일을 하는지 이해하지 못하는 경우가 있음
- 인터페이스를 이해하기 위해서는 인터페이스로 해결가능한 문제의 코드를 살펴보고 직접 해결해보는 방식으로 풀어가보자

### 전제 조건

- 모든 값은 타입이 있음
- 모든 함수는 매개변수가 어떤 타입으로 들어올 것인지 정의해야만 함

<aside>
💡 그럼 지금까지 튜토리얼에서 작성했던 모든 함수들은 로직이 동일함에도 불구하고 다른 타입을 받는다는 목적 하나만을 위해 전부 다 새로 작성해야 하나?

</aside>

### 예를 들면

- 아래와 같이 덱을 섞는 함수를 작성했다고 가정

```go
func (d deck) shuffle() deck {
	source := rand.NewSource(time.Now().UnixNano())
	r := rand.New(source)

	for i := range d {
		newPosition := r.Intn(len(d) - 1)
		d[i], d[newPosition] = d[newPosition], d[i]
	}
	return d
}
```

- 분명 셔플 함수는 덱을 매개변수로 받기 때문에, 다른 타입의 변수를 리시버로 받지 못할 것임
- 그렇다면 아래와 같이 셔플 함수를 전부다 정의해야 하나?

```go
func (d deck) shuffle()
func (s []float64) shuffle()
func (s []string) shuffle()
func (s []int) shuffle()
```

- 샘플 프로그램을 작성해보고 리팩터링을 통해 인터페이스가 어떻게 이러한 문제를 해결하는지 살펴볼 수 있음

### Chatbot

- 두개의 챗봇이 있다고 가정 (English bot, Spanish bot)
- 각각의 챗봇은 getGreeting() 함수를 가지고 있고, 각각 hello, hola를 출력함
- 언어에 따라서 여러가지 봇이 등장할 수 있고, 그에 따라서 함수의 갯수가 증가하게 됨

```go
type englishBot struct
func (englishBot) getGreeting() string
func printGreeting(eb englishBot)

type spanishBot struct
func (spanishBot) getGreeting() string
func printGreeting(sb spanishBot)
```

- 각각의 봇이 가지고 있는 getGreeting() 함수의 구현은 다를지 몰라도, 결국 getGreeting() 이라는 함수가 추구하고 있는 기능은 일치함 (인사말 출력)

```go
package main

import "fmt"

type englishBot struct{}
type spanishBot struct{}

func main() {
	eb := englishBot{}
	sb := spanishBot{}

	printGreeting(eb)
	printGreeting(sb)
}

func printGreeting(eb englishBot) {
	fmt.Println(eb.getGreeting())
}

func printGreeting(sb spanishBot) { // error (함수의 동시선언)
	fmt.Println(sb.getGreeting())
}

func (englishBot) getGreeting() string {
	// VERY custom logic for generating english greeting
	return "hello!"
}

func (spanishBot) getGreeting() string {
	return "hola!"
}
```

<aside>
💡 getGreeting() 함수의 구현은 각 언어에 따라서 특별히 복잡한 로직을 사용한다고 하더라도, printGreeting() 함수의 목적 자체는 getGreeting() 함수에서 가져온 인사말을 출력하는데 있기 때문에 이를 하나의 함수로 재사용할 수 있다면 좋을 것

</aside>

```go
package main

import "fmt"

type bot interface {
	getGreeting() string
}

type englishBot struct{}
type spanishBot struct{}

func main() {
	eb := englishBot{}
	sb := spanishBot{}

	printGreeting(eb)
	printGreeting(sb)
}

func printGreeting(b bot) {
	fmt.Println(b.getGreeting())
}

func (englishBot) getGreeting() string {
	// VERY custom logic for generating english greeting
	return "hello!"
}

func (spanishBot) getGreeting() string {
	return "hola!"
}
```

- interface를 선언하면서 bot 타입을 만들면, interface 내에 선언된 string을 반환하는 getGreeting() 함수는 bot을 리시버로 받을 수 있음

## Interface Notes

- 인터페이스는 제네릭 타입이 아님
  - 인터페이스는 다른 언어 (C#, Java)의 제네릭이 아니며 제네릭을 대체할 수 없음
