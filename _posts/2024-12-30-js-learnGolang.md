---
title: "[Go] Go언어 배워보기"
excerpt: "Golang(Go)언어 배워보기 vscode"

categories:
  - BE
tags:
  - [tag1, tag2]

permalink: /BE/learnGolang/

toc: true
toc_sticky: true

date: 2024-12-30
last_modified_at: 2024-12-30
---

### 1. Go는 무엇인가?
>Go(고)는 구글에서 개발한 프로그래밍 언어로 C++의 복잡함과 긴 컴파일 시간을 줄일 수 있는 간결한 언어, 또한 사용자가 배우기 쉬운 언어를 만들어보자는 취지로 만들어졌습니다. 안전하고 확장 가능한 시스템을 간단하게 구축할 수 있도록 돕는 오픈 소스 프로그래밍 언어입니다. [Go 공식문서](https://go.dev/)

#### C언어와 차이
 C의 복잡한 문법과 부족한 기능을 개선하려는 목적에서 설계
```c
// C의 선언문
int x; //x는 int 타입
int *p; // p는 int 타입의 포인터
int a[3]; // a는 int 타입의 배열
```
```go
// Go의 선언문
x int // x는 int 타입, 파라미터 뒤에 타입이 옴
p *int // p는 int 타입의 포인터
a [3]int // a는 int 타입의 배열
```
Go 선언문은 좌에서 우로 읽기 쉬워 복잡한 타입도 가독성이 좋으며, C보다 간단하고 유지보수가 용이합니다.

<br>

### 2. Golang 설치하기
1. OS에 맞는 [Go 설치](https://go.dev/dl/)

2. 설치 후 cmd에서 `go version`으로 설치 확인    
버전 안 나오면 사용자 환경변수 세팅 `[GOPATH] %USERPROFILE%\go`

3. 폴더와 hello.go 파일 생성

4. go.mod 생성 `go mod init [폴더명]`

5. vsCode - `Go` extensions install  
　　　　- ▶️ Run and Debug   
　　　　- ✅install 알림창 뜨면 설치  
6. 실행

<br>

### 3. Go 배우기 > [Go 컴파일러](https://go.dev/tour/welcome/1)

#### Hello World 출력
```golang
package main
import "fmt" //표준 출력과 입력 기능을 제공하는 표준 패키지

// func 키워드를 사용하여 함수 정의
func main() {
    fmt.Println("Hello World!") // "Hello, World!"를 출력
}
```

#### 인자 여러개 받기
```golang
package main
import "fmt"

// 동일한 타입의 인자 생략 가능
func addNumbers(a, b, c int) int {
    return a + b + c
}

func main() {
    // var x, y, z int  입력 값 받으려면
    // fmt.Scanln(&x, &y, &z) 
    // result := addNumbers(x, y, z)

    // :=는 주로 새로운 변수를 선언할 때 사용되며, 
    // 타입을 명시하지 않고 컴파일러가 자동으로 타입을 추론합니다.
    result := addNumbers(1, 2, 3)
    fmt.Println("결과:", result) // 결과: 6
}
```
#### 반복문
```go
package main

import "fmt"

func main() {

    for i := 0; i < 5; i++ {
        fmt.Println(i) // 0, 1, 2, 3, 4 출력
    }
	
    nums := []int{1, 2, 3}
	for _, num := range nums { // _ , 인덱스는 무시하고 값만 사용
        fmt.Println(num) // 1, 2, 3 출력
    }
	for _, _ = range nums {
        // 반복 성능 테스트 Go에서는 사용하지 않는 변수가 있으면 컴파일 에러
    }

}
```

#### Go에서의 재귀 계산
```go
// 0~9까지 팩토리얼 계산
package main
import "fmt"
type Number *Number

func zero() *Number {
	return nil
}

func isZero(x *Number) bool {
	return x == nil
}

func add1(x *Number) *Number {
	e := new(Number)
	*e = x
	return e
}

func sub1(x *Number) *Number {
	return *x
}

func add(x, y *Number) *Number {
	if isZero(y) {
		return x
	}
	return add(add1(x), sub1(y))
}

func mul(x, y *Number) *Number {
	if isZero(x) || isZero(y) {
		return zero()
	}
	return add(mul(x, sub1(y)), x)
}

func fact(n *Number) *Number {
	if isZero(n) {
		return add1(zero())
	}
	return mul(fact(sub1(n)), n)
}

func gen(n int) *Number {
	if n > 0 {
		return add1(gen(n - 1))
	}
	return zero()
}

func count(x *Number) int {
	if isZero(x) {
		return 0
	}
	return count(sub1(x)) + 1
}

func main() {
	for i := 0; i <= 9; i++ {
		f := count(fact(gen(i)))
		fmt.Println(i, "! =", f)
	}
}
```

#### 구조체 정의, Getter&Setter
```go
package main

import "fmt"

// Person 구조체 정의
type Person struct {
	name  string // 비공개 필드 (소문자)
	Age   int    // 공개 필드 (대문자)
}

func NewPerson(name string, age int) *Person {
	return &Person{name: name, Age: age}
}

// 비공개 필드(name)를 수정 Setter
func (p *Person) SetName(name string) {
	p.name = name
}
// 비공개 필드(name)를 읽는 Getter
func (p *Person) GetName() string {
	return p.name
}

func main() {
	// Person 객체 생성
	person := NewPerson("Alice", 25)

	// 공개 필드 Age에 접근
	fmt.Println("나이:", person.Age) // 나이: 25
	// 비공개 필드 name에 접근하려면 Getter 사용
	fmt.Println("이름:", person.GetName()) // 이름: Alice

	// 비공개 필드(name) 수정
	person.SetName("Bob")
	fmt.Println("변경된 이름:", person.GetName()) // 변경된 이름: Bob
}
```

<br>

### 4. 정리


#### 키워드
Go의 키워드는 총 25개로, 언어의 기본 문법을 구성합니다.

- 제어문  
　if, else: 조건문  
　switch, case, default: 조건 분기  
　for, range: 반복문  
　break, continue: 반복문 제어  
　fallthrough: switch에서 다음 case로 이동  
　return: 함수에서 값을 반환  
- 선언 및 정의   
　var: 변수를 선언  
　const: 상수를 선언  
　type: 사용자 정의 타입 선언  
　struct: 구조체 선언  
　func: 함수 정의  
　interface: 인터페이스 정의  
　package: 패키지 선언  
　mport: 패키지 가져오기  
- 동시성 및 동작  
　go: 고루틴 실행  
　chan: 채널 선언  
　select: 다중 채널 통신  
　defer: 함수 종료 시 실행될 코드 예약  
　panic: 프로그램 중단  
　recover: panic 복구  
- 기타  
　map: 키-값 쌍 데이터 구조  
　make: 슬라이스, 맵, 채널 초기화  
　new: 메모리 할당  
　nil: 기본 초기값 (null 값과 유사) 
<br>

#### 패키지
Go에는 표준 라이브러리가 포함되어 있으며, 주요 패키지는 다음과 같습니다:

- fmt : 포맷된 입출력 처리
- os : 운영 체제와 상호작용
- net/http : HTTP 서버 및 클라이언트
- math : 수학 함수 및 상수
- sync : 동기화 관련 기능
- time : 시간 및 날짜 관련 처리
- io : 입력 및 출력 처리
- encoding/json : JSON 인코딩 및 디코딩

<br>
다음 시간에는 고루틴 (Goroutines)과 채널 (Channels)에 대해 알아보겠습니다.