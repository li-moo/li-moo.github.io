---
title: "컴파일 타임(Compile Time), 빌드 타임(Build Time), 런타임(Run Time)란??"
excerpt: "컴파일 빌드 런타임 헷갈리는 용어 정리하기"

categories:
  - CS
tags:
  - [tag1, tag2]

permalink: /CS/Compile&Run&Buildtime/

toc: true
toc_sticky: true

date: 2024-12-05
last_modified_at: 2024-12-05
---

## 들어가기
> 런타임에러 라는 말 많이 들어보셨을 겁니다 오늘은 런타임 빌드 컴파일 타임을 알아봅시다.

개발 과정에서 자주 등장하는 용어인 컴파일 타임(Compile Time), 빌드 타임(Build Time), 그리고 런타임(Run Time)은 각각의 역할과 중요성이 다릅니다. 하지만 이 개념들 간의 차이를 명확히 이해하지 못하고 헷갈립니다. 

<br>

### 1. 컴파일 타임 (Compile Time)

컴파일 타임은 코드가 텍스트에서 기계가 이해할 수 있는 명령어 집합으로 변환되는 시점을 말합니다. 대부분의 프로그래밍 언어는 실행 전에 이 과정을 거치며, 이 과정에서 문법 오류(syntax error) 등이 감지됩니다.

컴파일 타임 에러는 주로 문법 오류, 데이터 타입 불일치 등이 있습니다.

```typescript
// typeScript 
let age: number = "25"; // 컴파일 타임 에러: 문자열을 숫자 타입에 할당할 수 없음
```
<br>

### 2. 런타임 (Run Time)

런타임은 코드가 실제로 실행되는 시점을 의미합니다. 컴파일된 프로그램이 실제 환경에서 동작하며, 이 과정에서 발생하는 오류를 런타임 에러라 부릅니다.

배열의 범위를 초과하거나(null 값 참조 시 발생).
```javascript
const numbers = [1, 2, 3];
console.log(numbers[5]); // undefined 출력
```
```javascript
const obj = null;
console.log(obj.name); // TypeError: Cannot read property 'name' of null
```
<br>

### 3. 빌드 타임 (Build Time)

빌드 타임은 컴파일을 포함한 전체 빌드 과정을 의미합니다. 컴파일 외에도 의존성 설치, 실행 파일 생성, 테스트 실행 등이 포함될 수 있습니다. 빌드 타임은 컴파일 타임의 상위 개념입니다.

```
설치된 패키지가 누락되었거나 버전 충돌이 있을 때 발생합니다. 
npm run build 명령어는 의존성 설치, 코드 번들링, 최적화 등 다양한 단계를 포함합니다.
``` 
<br>

#### React와 Svelte의 차이

React  
컴포넌트를 생성할 때 가상 DOM(Virtual DOM)을 사용합니다.  
런타임에 가상 DOM과 실제 DOM을 비교하여 변경 사항을 반영합니다.  
{: .notice--info}  

Svelte  
컴파일 타임에 변경 가능성이 있는 부분을 분석하고, 필요한 JavaScript 코드를 생성합니다.  
가상 DOM 없이 직접 실제 DOM을 업데이트하여 런타임 오버헤드를 줄입니다.  
{: .notice--warning}  

<br>

### 실생활로 쉽게 이해하여 봅시다.

- 컴파일 타임: Word 문서를 작성하는 과정과 비슷합니다. 오타나 문법 오류를 찾는 것이 컴파일 타임에 해당합니다.

- 빌드 타임: 작성한 문서를 PDF로 변환하거나, 필요한 이미지를 포함하여 최종 파일로 만드는 과정입니다.

- 런타임: 완성된 PDF를 열어 실제로 읽거나, 다른 사람에게 공유하는 과정입니다. 이때 발생하는 화면 오류(예: 깨진 이미지)가 런타임 문제입니다

<br>

> 컴파일 타임, 빌드 타임, 런타임은 애플리케이션 개발의 다양한 단계를 설명하는 핵심 개념입니다. 이를 명확히 이해하면, 더 나은 코드 품질과 성능 최적화를 이룰 수 있습니다.