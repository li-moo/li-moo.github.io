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

### 고루틴(Goroutines)
Goroutine은 Go 런타임에 의해 관리되는 경량 스레드입니다. 여러 개의 OS 스레드에 다중화되어 실행됩니다. 만약 하나의 Goroutine이 I/O 대기와 같은 블로킹 상태에 있으면, 다른 Goroutine은 계속 실행됩니다. 이러한 설계는 스레드 생성과 관리의 복잡성을 숨깁니다.
{: .notice--primary} 

### 채널(Channels)

```go
package main

import (
	"fmt"
	"time"
)

// 고루틴을 통해 제곱을 계산하고 결과를 채널로 전송
func square(num int, result chan int) {
	// 숫자의 제곱을 계산
	time.Sleep(time.Second) // 잠시 대기 (예시를 위한 시간 지연)
	result <- num * num     // 채널에 결과 전송
}

func main() {
	// 결과를 받을 채널 생성
	result := make(chan int)

	// 고루틴을 실행
	go square(5, result)
	go square(10, result)

	// 고루틴의 결과를 기다림
	result1 := <-result // 첫 번째 고루틴의 결과
	result2 := <-result // 두 번째 고루틴의 결과

	// 결과 출력
	fmt.Println("5의 제곱:", result1)
	fmt.Println("10의 제곱:", result2)
}
```