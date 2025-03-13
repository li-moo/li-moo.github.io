---
title: "[Go] go local server"
excerpt: "go server"

categories:
  - BE
tags:
  - [tag1, tag2]

permalink: /BE/learnGolangGR/

toc: true
toc_sticky: true

date: 2025-01-05
last_modified_at: 2025-01-05
---

> Go 소스 코드는 텍스트 형식으로 작성됩니다. 이 코드는 Go 컴파일러(go build 명령어 등)를 통해 바이너리 파일로 컴파일됩니다. 컴파일러는 Go 소스 코드를 읽고, 이를 CPU가 이해할 수 있는 머신 코드로 변환합니다.
<br>

### 로컬로 실행해보기
```go
package main

import (
	"fmt"
	"net/http"
)

func main() {
	http.HandleFunc("/", func(writer http.ResponseWriter, request *http.Request) {
		fmt.Fprint(writer, "Hello Go")
	})

	http.ListenAndServe(":3000", nil)
}
```