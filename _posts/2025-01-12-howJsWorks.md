---
title: "[JS] 자바스크립트 어떻게 동작할까?"
excerpt: "engine, the runtime, and the call stack"

categories:
  - FE
tags:
  - [tag1, tag2]

permalink: /FE/howJsWorks/

toc: true
toc_sticky: true

date: 2025-01-12
last_modified_at: 2025-01-12
---

JavaScript 가장 효율적이고 일반적으로 사용되는 스크립트 언어 중 하나입니다.
클라이언트 측 스크립트 언어는 클라이언트 측(사용자 기기)에서, 웹 브라우저 내에서 실행된다는 것을 의미합니다. 
현재 대부분의 최신 웹 브라우저는 JavaScript를 지원하며, 
자체적인 JavaScript 엔진을 가지고 있습니다.
<br>

### JavaScript 엔진
JavaScript 엔진은 JavaScript 코드를 해석하고 실행하는 프로그램입니다. 유명한 엔진으로는 다음과 같은 것들이 있습니다:
- V8: Google **Chrome**과 Node.js를 지원하는 Google의 엔진
- SpiderMonkey: Mozilla **Firefox**를 위한 엔진
- JavaScriptCore: **Safari** 브라우저를 위한 엔진
- Chakra: **Internet Explorer**를 위한 엔진
<br>

### 엔진의 주요 구성 요소 
- 콜 스택(Call Stack): 코드가 실행되는 곳
- 메모리 힙(Memory Heap): 객체가 저장되는 비구조적 메모리 풀
<br>

### Call Stack vs Heap
#### **Call Stack (for Function Execution)**
- Function Calls: 함수 호출과 실행 컨텍스트를 관리합니다.
- Last In, First Out (LIFO): 가장 최근에 호출된 함수부터 처리됩니다.
- Context Management: 현재 실행 중인 함수의 컨텍스트를 추적합니다.
- Single Threaded: 단일 스레드에서 순차적으로 실행됩니다.

#### Heap (for Function Execution)
- Memory Allocation: 런타임 동안 동적으로 메모리를 할당합니다.
- Objects and References: 객체와 변수를 저장하며, Call Stack에서 참조됩니다.
- Garbage Collection: 더 이상 사용되지 않는 메모리를 JavaScript 엔진이 자동으로 회수합니다.
<br>

### JavaScript 실행 과정
1. 파싱: 코드를 읽고 "추상 구문 트리(AST)"라는 데이터 구조로 변환합니다.
2. 컴파일: AST를 바탕으로 중간 표현(IR)으로 변환한 뒤, 최종적으로 기계 코드로 변환합니다. (JIT(Just-In-Time) 컴파일러)
3. 실행: 변환된 기계 코드를 실행합니다.
<br>

### JavaScript 엔진의 주요 동작
#### 1. 파싱 (Parsing)
  코드를 읽어서 문법을 검사하고, **추상 구문 트리(AST)**를 생성합니다.
  AST는 코드의 구조를 트리 형태로 표현한 데이터 구조입니다.
#### 2. 컴파일 (Compilation)
  AST를 바탕으로 중간 표현(IR)으로 변환한 뒤(인터프리터), 최종적으로 기계 코드로 변환(컴파일러)합니다.
  현대 JavaScript 엔진에서는 JIT(Just-In-Time) 컴파일러가 이 작업을 수행합니다.
#### 3. 실행 (Execution)
  변환된 기계 코드를 프로세서에서 실행합니다.
<br>

### 비동기: Callback Queue와 Microtask Queue

JavaScript의 비동기 동작은 *Callback Queue**와 **Microtask Queue**를 통해 이루어집니다. 
- Callback Queue: 콜백 함수들이 대기하며, Call Stack이 비워진 뒤 실행됩니다. 
`setTimeout`, `setInterval`과 같은 타이머 콜백이 이곳에 들어갑니다.
- Microtask Queue: Promises와 `MutationObserver` 같은 미시 작업(microtasks)이 처리되며, 
Callback Queue보다 우선순위가 높습니다.

```javascript
console.log("Start");

setTimeout(() => {
    console.log("Timeout");
}, 0);

Promise.resolve().then(() => {
    console.log("Promise");
});

console.log("End");
```
실행결과
```
Start
End
Promise
Timeout
```
1. console.log("Start") 실행 ➡️ Start가 출력됩니다. (동기 코드)
2. setTimeout 등록 ➡️ 타이머 콜백은 Callback Queue에 등록되며, Call Stack이 비어 있을 때 실행됩니다.
3. Promise.resolve().then() 등록 ➡️ .then()의 콜백은 Microtask Queue에 추가됩니다.
4. console.log("End") 실행 ➡️ End가 출력됩니다. (동기 코드)
5. Microtask Queue 실행 ➡️ Microtask Queue에서 Promise 콜백이 실행되어 Promise가 출력됩니다.
6. Callback Queue 실행 ➡️ Callback Queue에서 타이머 콜백이 실행되어 Timeout이 출력됩니다.

> JS의 이러한 내부 동작 방식을 이해하면 더 나은 코드를 작성할 수 있습니다.

[출처1](https://medium.com/sessionstack-blog/how-does-javascript-actually-work-part-1-b0bacc073cf)
[출처2](https://coralogix.com/blog/how-js-works-behind-the-scenes-the-engine/)
[출처3](https://www.geeksforgeeks.org/how-javascript-works/)