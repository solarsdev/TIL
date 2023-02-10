# 테라폼 HCL 기초 문법

## 파일과 디렉토리

### 개요

- 테라폼 언어에서 코드는 평문으로 저장되며 .tf 확장자를 가짐 (JSON기반의 언어도 지원하는데, 이 경우 .tf.json) 통상적으로 .tf 확장자를 사용하고 HCL을 권장함
- 테라폼 코드가 들어있는 문서를 설정 파일이라고 부름

### 파일 인코딩

- 파일은 utf-8 인코딩으로 작성해야 하며, 개행에 관해서는 유닉스, 윈도우 개행을 둘 다 지원하지만 통상적으로 유닉스 개행문자를 사용하는것이 권장됨

### 모듈과 디렉토리

- 모듈은 .tf 또는 .tf.json 파일이 모여있는 디렉토리 자체를 의미
- 테라폼 모듈은 디렉토리 내의 설정 파일들에서 유효하며, 중첩된 디렉토리들은 완전히 별개의 모듈로 취급됨, 따라서 자동으로 읽어들일 수 없음
- 테라폼에서 수행하는 init/plan/apply는 명령어가 수행되는 디렉토리에서 .tf 파일을 찾고, 서브 디렉토리는 무시됨
- 전체 디렉토리의 .tf 파일들은 결국 하나로 합쳐져 한개의 문서로 취급됨, 따라서 파일을 여러개로 나누어 작성해도 결국 처리는 전체에 대해서 동일하게 수행하는 것과 같음
- 테라폼 파일 안에서 다른 모듈을 불러서 수행할 수 있는데, 이 자식 모듈(호출되는 쪽)은 다양한 소스로부터 가져올 수 있음

### 루트 모듈

- 테라폼은 항상 하나의 루트 모듈로부터 수행됨 (모듈의 tf 문서 묶음)
- 테라폼 CLI에서 루트 모듈은 terraform 명령어를 수행한 그 디렉토리가 됨
- 테라폼 클라우드 및 엔터프라이즈에서 루트 모듈은 기본적으로는 최상위 디렉토리지만, 원하는 경우 시작점을 변경할 수 있음

### 파일 덮어쓰기

- 테라폼에서는 같은 디렉토리 내의 .tf 와 .tf.json 파일을 하나의 뭉치로 실행하며, 같은 디렉토리 내의 별개의 .tf 파일이라고 할 지라도 동일한 문서로 통합되어 리소스의 중복 선언 등은 에러가 됨
- 간혹, 기존의 정의 파일을 건드리기 힘든 경우, 새롭게 파일을 정의하여 덮어쓰기로 작업하는 것이 선호되는 경우가 발생할 수 있는데, 이를 위해 override 기능을 제공함
- 이러한 경우에는 \_override.tf 또는 \_override.tf.json 등으로 이름을 지정하면 덮어쓰기를 할 수 있음
- 테라폼은 일단 수행시 이러한 오버라이드 파일을 스킵하고, 실행의 마지막에 기존 리소스를 덮어쓰기 하며 최종 작업을 진행함
- 오버라이드 파일을 특별한 경우에만 활용하는 것이 좋음, 또한 작업자에게 오버라이드 파일에서 어떤 것들을 재정의 하는지 명시하는 것이 좋음

```bash
# example.tf
resource "aws_instance" "web" {
  instance_type = "t2.micro"
  ami           = "ami-408c7f28"
}

# override.tf
resource "aws_instance" "web" {
  ami = "foo"
}

# actual terraform works with
resource "aws_instance" "web" {
  instance_type = "t2.micro"
  ami           = "foo"
}
```

## 문법

- 테라폼의 문법은 HCL이라고 불리는 정형화된 방식이 있음
- HCL을 이용해 다른 어플리케이션에서 제공하는 리소스 및 데이터를 이용 가능

### 인자와 블럭

```bash
# 인자의 경우 다음과 같이 명명됨 (image_id에 abc123을 대입)
image_id = "abc123"
```

- 인자의 경우 값을 입력하는 것도 되지만, 값의 연산을 통한 결과를 입력하는것도 가능함
- 블럭은 여러개의 인자를 묶는 방법인데 중괄호로 인자를 감싸게 됨
- 블럭은 하위 여러개의 블럭을 포함할 수 있음
- 탑 레벨 블럭의 경우 정해진 문법에 따라 정의됨 (resource, output, variable …)

### 구별자

- 인자, 블럭 등의 이름이 여러 단어로 이루어질 경우 구별자를 넣을 수 있음
- 구별자는 특정 영어 글자, 숫자, 밑줄, 하이픈 등으로 이루어질 수 있고 이름의 처음에 숫자는 금지됨
- 엄밀하게 정의하면 유니코드 구별자에 특별히 하이픈을 추가한 형태

### 주석

- 쉘에서 쓰는 #, C와 같은 언어에서 사용하는 //, /\*\*/ 등을 지원
- 내부적으로는 #을 주석으로 사용하고 문법상 처리로 //를 #으로 변경하게 됨 /\*\*/의 경우 전부 앞에 #을 붙임

## 테라폼 코딩 스타일 컨벤션

- 들여쓰기는 2 스페이스를 이용
- 블록 내에 여러가지 인자를 입력할때는 이퀄기호를 기준으로 들여쓰기를 맞춤

```bash
ami           = "abc123"
instance_type = "t2.micro"
```

- 논리적 그룹으로 구분해야 함
- 리소스의 메타 인자의 경우는 단일 인자의 경우 정의문의 제일 위에, 블럭의 경우에는 마지막에 위치

```bash
resource "aws_instance" "example" {
  count = 2 # meta-argument first

  ami           = "abc123"
  instance_type = "t2.micro"

  network_interface {
    # ...
  }

  lifecycle { # meta-argument block last
    create_before_destroy = true
  }
}
```

## 리소스 (resources)

### 개요

- 리소스는 테라폼 언어에서 대부분의 엘리먼트에 해당함 (리소스 정의 언어)
- 각각의 리소스 블럭은 하나 이상의 인프라 객체를 나타냄 (network, instance, dns record 등)

### 리소스 블럭

```bash
resource "aws_instance" "web" {
  ami           = "ami-a1b2c3d4"
  instance_type = "t2.micro"
}
```

- 리소스 블럭 안에는 다양한 기능을 명세할 수 있지만, 기본적으로 필수적인 인자들만 제공하면 사용 가능
- 추가적인 기능으로는 하나의 정의문 블럭 안에서 여러가지의 비슷한 리소스를 한번에 정의한다거나 하는 것임
- 리소스 블럭은 resource 로 시작하고, aws_instance라는 리소스의 종류를 선정한 뒤, 해당 리소스의 이름을 정의하는 것으로 시작함
- 리소스의 이름은 같은 모듈 내에서 다른 리소스들이 인식할 수 있지만, 모듈 외부에서는 알 수 없음
- 블럭 내에는 리소스 고유의 다양한 인자들을 전달할 수 있음, 예를 들어 aws_instance 리소스 타입은 ami와 instance_type이 필수 인자로 지정되어 있고, 나머지는 옵션임
- 리소스의 종류와 인자에 대해서는 테라폼 레지스트리에서 문서를 참고해야 함

### 리소스 타입

- 각각의 리소스는 하나의 리소스 타입과 매치되고 이는 인프라 요소를 어떻게 정의할지와 상통함
- 리소스 타입은 프로바이더라고 불리는 리소스의 집합을 제공하는 벤더들과 연동되어 있음
- 벤더에서 제공하는 리소스를 사용하기 위해서 프로바이더를 제공해야 하고, 프로바이더 블럭에 필요한 요소들은 각각의 벤더에서 문서를 작성한 것을 참고해야 함

### 리소스 인자

- 리소스 블럭 내에는 선택된 리소스 타입에 따라 다양한 인자를 입력하게 됨
- 리소스를 정의함에 있어 HCL의 다양한 표현식을 이용해서 동적으로 리소스를 정의할 수 있음
- 리소스의 인자로 메타 인자가 존재하는데, 이는 리소스의 타입에 관계 없이 모든 리소스에서 사용할 수 있는 인자임

### 메타 인자

- 테라폼 언어에서 모든 리소스에 정의할 수 있는 인자를 메타 인자라고 하는데, 리소스의 의존성이나 갯수와 같은 메타 데이터를 정의하기 위해 사용됨
- 메타 인자 종류
  - depends_on → 리소스간의 의존성을 명시
  - count → 하나의 리소스 정의로 복수의 리소스를 생성
  - for_each → 맵이나 셋의 자료구조를 이용해 복수의 리소스를 정의
  - provider → 리소스의 프로바이더를 명시
  - lifecycle → 리소스의 생성-소멸 주기에 대한 명시
  - provisioner → 리소스 작성 후에 해야 하는 추가적인 액션에 대한 명세

### 커스텀 컨디션 체크

- precondition 혹은 postcondition 블럭을 통해 리소스의 동작 양식을 적시할 수 있음 예를 들어 다음의 예제에서는 precondition 체크를 통해 AMI가 x86_64 아키텍쳐인지를 체크하게 됨

```bash
resource "aws_instance" "example" {
  instance_type = "t2.micro"
  ami           = "ami-abc123"

  lifecycle {
    # The AMI ID must refer to an AMI that contains an operating system
    # for the `x86_64` architecture.
    precondition {
      condition     = data.aws_ami.example.architecture == "x86_64"
      error_message = "The selected AMI must be for the x86_64 architecture."
    }
  }
}
```

- 커스텀 컨디션은 정의를 통해 리소스 작성자 이후에 유지보수를 해야 하는 사람에게도 정보를 줌과 동시에 인프라상의 문제 발생시 해결 실마리를 제공해주기 위한 목적도 존재함

### 실행 타임아웃

- 특정 인스턴스 타입에 timeouts 중첩 블럭을 정의하는 것으로 리소스의 작성에 어느정도의 시간을 할애 가능한지 정의할 수 있음

```bash
resource "aws_db_instance" "example" {
  # ...

  timeouts {
    create = "60m"
    delete = "2h"
  }
}
```

- timeouts 블럭은 리소스 유형에 따라서 지원하지 않는 경우가 많으므로, 지원 여부를 리소스 문서에서 확인 한 뒤에 사용하는 것이 바람직함

## 변수 (variable)

### 인풋 변수 (Input Variables)

- 인풋 변수는 모듈 자체의 소스 코드를 변경하지 않고 커스터마이징 기능을 활용 할 수 있게 해줌
- 이 기능을 통해 다른 테라폼 설정과 모듈을 재사용 가능하게 함
- 루트 모듈에 변수를 사용하게 되면 환경 변수나 CLI를 통한 주입이 필요하고 자식 모듈에서 선언된 변수의 경우에는 루트 모듈에서 module 블럭에 주입을 해주어야 함
- 프로그래밍 언어와 비교 (테라폼 <> 함수)
  - 인풋 변수와 함수의 인자
  - 아웃풋 값과 함수의 리턴값
  - 로컬 값과 함수의 로컬 변수

### 인풋 변수의 선언방법

```bash
variable "image_id" {
  type = string
}

variable "availability_zone_names" {
  type    = list(string)
  default = ["us-west-1a"]
}

variable "docker_ports" {
  type = list(object({
    internal = number
    external = number
    protocol = string
  }))
  default = [
    {
      internal = 8300
      external = 8300
      protocol = "tcp"
    }
  ]
}
```

- variable 키워드 뒤에 변수 이름을 명시하고, 동일 모듈 내 이름은 유일해야 함
- 선언된 이름을 통해 모듈 내 다른 리소스 및 외부에서 주입이 가능해짐
- 이름으로 사용할 수 없는 것들이 존재하는데, 테라폼 키워드로 지정된 것들은 사용 불가
  - source
  - version
  - providers
  - count
  - for_each
  - lifecycle
  - depends_on
  - locals

### 인풋 변수의 인자들

- default
  - 인풋 변수에 값이 지정되지 않을 경우 기본으로 지정될 값
  - 인풋 변수에 default가 적시될 경우 인풋 변수는 optional이 되는 것과 동일함
- type
  - 인풋 변수의 타입
- description
  - 인풋 변수의 문서적 설명
- validation
  - 인자의 검증 블럭 (추가적 타입 제약조건을 검사하는데 보통 사용됨)
- sensitive
  - 테라폼 UI상에서 노출되지 말아야 함을 적시
- nullable
  - 인풋 변수가 null 상태를 가질 수 있음을 적시

### 인풋 변수의 타입 제약사항

- type 인자는 variable 블럭에 어떤 타입의 값이 들어올지 정하는 것
- 만약 타입이 지정되지 않으면 어떤 타입의 값도 허용됨
- type은 꼭 지정할 필요는 없으나 지정하는 것이 권장됨, 모듈 사용자와 에러 메시지의 원인 규명에 도움이 되기 때문임
- 현재 테라폼에서 지정 가능한 타입은 총 3가지가 존재함
  - string
  - number
  - bool
- 세가지 타입을 다음과 같은 자료구조로 묶어줄 수 있음
  - list
  - set
  - map
  - object
  - tuple
- 키워드 any를 사용할 수 있으나 지정하지 않는것과 동일
- type과 default를 둘 다 지정할 경우 default 값은 type의 제약조건을 만족시켜야 함

### 인풋 변수를 위한 문서화

- description은 모듈 사용자 관점에서 작성해야 함
  - 모듈 사용자에게 CLI등으로 입력을 받을 때 표시될 수 있기 때문임
- 모듈 관리자/유지보수자에게 전달하기 위한 문서의 경우 주석으로 적어줄 것

### 인풋 변수의 sensitive 설정과 CLI 표시에 대해서

- 변수에 sensitive를 표시할 경우 terraform plan apply시 값이 표시되지 않음
- 단, 여전히 상태 정보에는 평문으로 기재되기 때문에 모든 노출로부터 안전해지는 것은 아님
- 또한 변수가 sensitive로 선언되어 plan 및 apply에서 숨겨졌다고 하더라도, 실제로 apply 작업에서 발생하는 특정 값에 해당 변수가 이용된다면 의도와 다르게 표시될 수도 있음

```bash
# random_pet.animal will be created
  + resource "random_pet" "animal" {
      + id        = (known after apply)
      + length    = 2
      + prefix    = (sensitive value)
      + separator = "-"
    }

Plan: 1 to add, 0 to change, 0 to destroy.

...

random_pet.animal: Creating...
random_pet.animal: Creation complete after 0s [id=jae-known-mongoose]
# jae was sensitive value
```

### 변수 선언 우선도

- 테라폼에서는 여러가지 변수 주입 방법을 지원하는데, 방법에 따라 우선도가 결정됨
- 나중에 기재된 것이 먼저 기재된 것을 덮어씀
  - 환경 변수
  - terraform.tfvars (존재하면)
  - terraform.tfvars.json (존재하면)
  - _.auto.tfvars 또는 _.auto.tfvars.json (파일이름에 따라 순서대로 적용됨)
  - CLI상에서 -var 또는 -var-file로 주입된 파일이나 변수

## 로컬 값 (local value)

- 테라폼 코드를 작성하다 보면, 여러번 반복해서 사용하는 이름이 있음
- 로컬 변수로 해당 이름을 선언하고 값을 주입할 수 있음

### 로컬 값의 선언

```bash
locals {
  service_name = "forum"
  owner        = "Community Team"
}
```

- 로컬 값의 선언은 리터럴에 국한되지 않고, 다양한 포맷으로 선언이 가능함

```bash
locals {
  # EC2 인스턴스의 ID집합을 연결
  instance_ids = concat(aws_instance.blue.*.id, aws_instance.green.*.id)
}

locals {
  # 모든 리소스에 부여할 기본 태그
  common_tags = {
    Service = local.service_name
    Owner   = local.owner
  }
}
```

## 출력 값 (output value)

- 출력은 테라폼 코드에서 작성된 리소스의 값을 외부로 노출시키는 용도로 활용됨
- 부모 모듈은 자식 모듈에서 노출된 값을 활용하여 리소스를 정의할 수 있음
- terraform apply를 이용해서 적용된 출력 값은 CLI의 마지막에 출력됨

### 출력 값의 선언

```bash
output "instance_ip_addr" {
  value = aws_instance.server.private_ip
}
```

### 자식 모듈에서 정의된 출력 값에 접근

- 부모 모듈에서 module.<모듈명>.<출력값이름>을 통해 자식 모듈에서 정의된 값에 접근 가능

## 테라폼 워크스페이스

- Use Cases
  - Dev/Staging/Prod
  - kr/jp/us
  - 샘플
    - 네트워크 관리 코드
    - dev/staging/prod에 해당하는 워크스페이스 생성
- 워크스페이스 관련 terraform 명령어
- 주의사항
  - workspace 기능은 테라폼 클라우드 remote backend에서는 workspace가 다르게 동작
  -

## 테라폼 클라우드

### 워크스페이스

- 실행 모드
  - 테라폼 명령어를 어디서 실행할지 선택
  - Remote → 테라폼 클라우드 인프라에 위치한 terraform runner에서 실행
    - 테라폼에서 인트라넷에 접근할 수 있는 터널링을 해줘야 하는 경우가 있음
  - Local → 작업자 PC에서 테라폼 명령어를 수행 (테라폼 클라우드는 상태 저장만 수행)
