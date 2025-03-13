---
title: "[Go] 고루틴(Goroutines)과 채널(Channels)"
excerpt: "고루틴(Goroutines)과 채널(Channels)"

categories:
  - BE
tags:
  - [tag1, tag2]

permalink: /BE/learnGolangGR/

toc: true
toc_sticky: true

date: 2025-01-01
last_modified_at: 2025-01-01
---

본 내용은 [go 공식문서](https://gobyexample.com/goroutines)와 [예제로 배우는 Go 프로그래밍](http://golang.site/go/article/21-Go-%EB%A3%A8%ED%8B%B4-goroutine)을 참고 하였습니다.

### 고루틴(Goroutines)
Goroutine은 Go 런타임에 의해 관리되는 경량 스레드입니다. Go에서 "go" 키워드를 사용하여 함수를 호출하면, 런타임시 새로운 goroutine을 실행합니다. goroutine은 비동기적으로 함수루틴을 실행하므로, 여러 코드를 동시에(Concurrently) 실행하는데 사용됩니다.
{: .notice--primary} 

**동기(Syncronous)**: 작업을 순차적으로 처리하며, 하나의 작업이 끝나야 다음 작업을 시작  
**비동기(Asynchronous)**: 작업을 병렬적으로 처리하며, 한 작업이 끝날 때까지 기다리지 않고 다른 작업을 동시에 진행

```go
package main
 
import (
    "fmt"
    "time"
)
 
func say(s string) {
    for i := 0; i < 3; i++ {
        fmt.Println(s, "***", i)
    }
}
 
func main() {
    // 함수를 동기적으로 실행
    say("Sync")
    // 함수를 비동기적으로 실행
    go say("Async1")
    go say("Async2")
    go say("Async3")
    // 메인 함수가 종료되지 않도록 3초 대기
		// goroutine들이 실행을 완료할 시간을 확보
    time.Sleep(time.Second * 3)
}
```
실행결과 1
```go
Sync *** 0
Sync *** 1
Sync *** 2
Async3 *** 0
Async3 *** 1
Async3 *** 2
Async2 *** 0
Async2 *** 1
Async2 *** 2
Async1 *** 0
Async1 *** 1
Async1 *** 2
```
실행결과 2
```go
Sync *** 0
Sync *** 1
Sync *** 2
Async3 *** 0
Async3 *** 1
Async3 *** 2
Async1 *** 0
Async1 *** 1
Async1 *** 2
Async2 *** 0
Async2 *** 1
Async2 *** 2
```
<br>

### 채널(Channels)
> Go 채널은 그 채널을 통하여 데이타를 주고 받는 통로라 볼 수 있습니다. 채널은 흔히 goroutine들 사이 데이타를 주고 받는데 사용되는데, 상대편이 준비될 때까지 채널에서 대기함으로써 별도의 lock을 걸지 않고 데이타를 동기화하는데 사용된다.

```go
package main
import "fmt"

func main() {
	// 채널 생성: "messages"라는 이름 사용
	messages := make(chan string)
	// func() 익명함수
	// messages 채널에 "ping" 문자열을 전송.
	go func() { messages <- "ping" }() 
	msg := <-messages // 채널에서 메시지를 받아옴
	fmt.Println(msg) //출력 ping
}
```

```go
package main
 
func main() {
    ch := make(chan int, 2)
    // 채널에 송신
    ch <- 1
    ch <- 2
    // 채널을 닫는다
    close(ch)
    // 채널 수신
    println(<-ch)
    println(<-ch)
    if _, success := <-ch; !success {
        println("더이상 데이타 없음.")
    }
}
```

```go
package main
 
func main() {
    ch := make(chan int, 2) //make(chan 데이터타입, 버퍼크기)
    // 채널에 송신
    ch <- 1
    ch <- 2
    // 채널을 닫는다
    close(ch)
    // 방법1
    // 채널이 닫힌 것을 감지할 때까지 계속 수신
    /*
    for {
        if i, success := <-ch; success {
            println(i)
        } else {
            break
        }
    }
    */
    // 방법2
    // 위 표현과 동일한 채널 range 문
    for i := range ch { // range ch 만큼 순회
        println(i) // 1    2
    }
}
```