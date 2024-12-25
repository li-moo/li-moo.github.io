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
