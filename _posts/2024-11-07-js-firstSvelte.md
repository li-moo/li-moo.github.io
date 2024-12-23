---
title: "[JS] 스벨트 시작해보기 "
excerpt: "스벨트 알아보면서 설치까지 할 수 있다."

categories:
  - FE
tags:
  - [tag1, tag2]

permalink: /FE/firstSvelte/

toc: true
toc_sticky: true

date: 2024-11-07
last_modified_at: 2024-11-07
---

## 1. Svelte 란?
Svelte는 모듈에서 다루는 다른 프레임워크와는 다른 웹앱 구축 방식을 제공합니다. React와 Vue와 같은 프레임 워크는 앱이 실행되는 동안 사용자의 브라우저에서 대부분의 작업을 수행하지만 Svelte는 해당 작업을 앱을 빌드하는 컴파일 단계로 전환하여 고도로 최적화된 바닐라 JavaScript를 생성합니다. - mdn web docs &ensp; [Svelte 공식문서](https://svelte.dev/playground/hello-world)
{: .notice--primary}  
<br>

### 1. Svelte 기본 구조
```javascript
<script>
  // JavaScript(script작성)
  let name = "Svelte";
  
  function greet() {
    alert(`Hello, ${name}!`);
  }
</script>

<style>
  /* CSS(스타일) */
  h1 {
    color:rgb(60, 116, 206);
  }

  button {
    font-size: 1.2rem;
    background-color:rgb(171, 117, 189);
    color: white;
    border: none;
    border-radius: 4px;
    padding: 0.5rem 1rem;
    cursor: pointer;
  }

  button:hover {
    background-color:rgb(131, 83, 149);
  }
</style>

<!-- HTML 영역(UI) -->
<h1>Welcome to {name}!</h1>
<button on:click={greet}>Click Me</button>
```

## 2. Svelte 설치하기

### 1. Node.js 설치
스벨트를 사용하려면 Node.js가 필요합니다. [node.js 다운로드](https://nodejs.org/ko)  
설치 확인은 터미널에서 `node -v` 로 버전이 뜨는지 확인해주세요

<br>

### 2. 프로젝트 생성
터미널에서 다음 명령어를 실행해 새로운 스벨트 프로젝트를 생성합니다:
```bash
npm create vite@latest my-svelte-app -- --template svelte
```
`my-svelte-app`는 프로젝트 이름이여서 원하는 이름으로 변경 해주세요.
<br>

### 3. 의존성 설치
생성된 프로젝트 폴더로 이동해 의존성을 설치
```bash
cd my-svelte-app // npm으로 다운로드한 앱 폴더로 이동  
npm install // package.json에 정의된 의존성들을 설치
```
<br>

### 4. 개발 서버 실행
```bash
npm run dev
```
개발 서버를 실행하면 터미널에 표시된 주소(예) http://localhost:5173)를 브라우저에서 열어 Svelte가 정상적으로 작동하는지 확인해보세요.
