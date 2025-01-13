---
title: "[Go] go server"
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

### 로컬로 실행
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