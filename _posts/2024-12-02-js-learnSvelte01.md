---
title: "[JS] 스벨트 기본"
excerpt: "svelte 알아보기, React와 차이점"

categories:
  - FE
tags:
  - [tag1, tag2]

permalink: /FE/learnSvelte01/

toc: true
toc_sticky: true

date: 2024-12-02
last_modified_at: 2024-12-02
---

[svelte](https://svelte.dev/playground/hello-world) << url를 svelte 통해 코드를 실행해보세요!⚙️
{: .notice--warning}  

<br>

#### Q. 아래는 Svelte로 구현한 숫자 증가/감소 버튼입니다. React와 무엇이 다른지 생각해봅시다.

```svelte
<script>
	let count = 0;

	function increase() {
		count += 1;
	}
	  function decrease() {
    count -= 1;
  }
</script>

<div>
	<p>Count: {count}</p>
	<button on:click={increase}>+</button>
	<button on:click={decrease}>-</button>
</div>
```

Svelte는 JavaScript 변수만 수정해도 화면이 자동으로 업데이트되지만,   
`const [count, setCount] = useState(0); `   
React는 상태를 변경하려면 setState 함수를 호출해야 화면이 업데이트됩니다.
[컴파일 타임]()

<br>

#### Q. props 전달 방법 차이점이 무엇일까요?
```react
function Parent() {
  const [count, setCount] = useState(0);

  return <Child count={count} />;
}

function Child({ count }) {
  return <div>Count: {count}</div>;
}
```

```svelte
<!-- Parent.svelte -->
<script>
  let count = 0;
</script>

<Child {count} />
<!-- Parent.svelte -->

<!-- Child.svelte -->
<script>
  export let count;
</script>

<div>Count: {count}</div>
<!-- Child.svelte -->
```
<br>

#### Q.svelte 라우팅은 어떻게 할까요?

먼저 svelte-spa-router 다운받아 줍니다.
```bash
npm i svelte-spa-router`
```
.js 파일에 라우팅을 해줍니다.
```javascript
import Home from './Home.svelte';
import About from './About.svelte';
import Movie from './Movie.svelte';
import NotFound from './NotFound.svelte';

const routes = {
  '/': Home,
  '/movie/:id': Movie, // 파라미터가 있는 페이지
  '/about': About,
  '*': NotFound,
};

export default routes;
```
<br>

#### Q. react .map 문법과 svelte

```react
const menus = [
  { id: 1, href: "/home", name: "Home" },
  { id: 2, href: "/about", name: "About" },
  { id: 3, href: "/contact", name: "Contact" }
];

function App() {
  return (
    <ul>
      {menus.map(({ id, href, name }) => (
        <li key={id}>
          <a href={href}>{name}</a>
        </li>
      ))}
    </ul>
  );
}
```

```svelte
<script>
  let menus = [
    { id: 1, href: "/home", name: "Home" },
    { id: 2, href: "/about", name: "About" },
    { id: 3, href: "/contact", name: "Contact" }
  ];
</script>

<ul>
  {#each menus as { id, href, name } (id)}
    <li><a href={href}>{name}</a></li>
  {/each}
</ul>
```

##### 1. 반복으로 데이터 표시 `{#each 데이터 as 요소}`
```svelte
<script>
  let items = ["Apple", "Banana", "Cherry"];
</script>

<ul>
  {#each items as item}
    <li>{item}</li>
  {/each}
</ul>
```
##### 2. 인덱스 활용 `{#each 데이터 as 요소, 인덱스}`
```svelte
<script>
  let items = ["Apple", "Banana", "Cherry"];
</script>

<ul>
  {#each items as item, index}
    <li>{index + 1}: {item}</li>
  {/each}
</ul>
```
##### 3. 고유 식별자 `{#each 데이터 as 요소 (key)} `
```svelte
<script>
  let users = [
    { id: 1, name: "Alice" },
    { id: 2, name: "Bob" },
    { id: 3, name: "Charlie" }
  ];
</script>

<ul>
  {#each users as user (user.id)}
    <li>{user.name}</li>
  {/each}
</ul>
``` 

#### Q. react에서 조건부 렌더링과 svelte 무엇이 다를까요?

```jsx
{imageLoading ? <Loader scale="0.5" absolute={true} /> : null} && 연산자
```
```jsx
{imageLoading && <Loader scale="0.5" absolute={true} />}
```
##### 4. 빈 데이터 처리 `{:else}`
```svelte
<script>
  let items = [];
</script>

<ul>
  {#each items as item}
    <li>{item}</li>
  {:else}
    <li>No items found.</li>
  {/each}
</ul>
```

#### svelte Else-if 문법(Else-if blocks)
```svelte
{#if 조건}
  <!-- 조건이 true일 때 -->
{:else if 다른조건}
  <!-- 첫 번째 조건이 false이고, 다른조건이 true일 때 -->
{:else}
  <!-- 모든 조건이 false일 때 -->
{/if}
```
아래는 svelte 공식 문서의 예시이다.
```svelte
<script>
	let count = $state(0);

	function increment() {
		count += 1;
	}
</script>

<button onclick={increment}>
	Clicked {count}
	{count === 1 ? 'time' : 'times'}
</button>

{#if count > 10}
	<p>{count} is greater than 10</p>
{:else}
	<p>{count} is between 0 and 10</p>
{/if}
```
버튼 클릭 횟수가 0~10사이이면 `{count} is between 0 and 10`  
10을 초과하면 `{count} is greater than 10`