---
title: "[React] React Hooks 알아보기"
excerpt: "useState useEffect"

categories:
  - FE
tags:
  - [tag1, tag2]

permalink: /FE/ReactHooks/

toc: true
toc_sticky: true

date: 2023-01-31
last_modified_at: 2023-01-31
---

## ⭐React Hooks 란?
Hook은 React 버전 16.8부터 React요소로 새로 추가된 기능입니다.  
기존 Class 바탕의 코드를 작성할 필요 없이 상태 값과 여러 React의 가능을 사용할 수 있습니다. [React 공식문서](https://ko.react.dev/learn)
{: .notice--primary}  

### 1. useState
useState는 마치 주머니 같습니다. 주머니 안에 여러 물건을 넣고 뺄 수 있듯이, useState는 **상태(state)**를 담는 역할을 합니다. 그리고 주머니에서 물건을 꺼낼 때마다 그 물건이 무엇인지 확인할 수 있듯이, useState는 현재 상태의 값을 확인할 수 있게 해줍니다.

`const [count, setCount] = useState(초기값);`

- state(count): 상태값을 저장합니다.  
- setState(setCount) : 상태값을 변경할 때 사용합니다.  
- 초기값: 상태의 초깃값을 설정합니다.  

<br>
### 2. useEffect
useEffect는 외부 시스템과 컴포넌트를 동기화하는 React Hook입니다.  
리액트 컴포넌트가 렌더링 될 때마다 특정 작업을 실행할 수 있습니다.

```javascript
useEffect(() => {
  console.log("useEffect called");
}, []);
``` 

- Mount: 초기화 작업을 처리할 수 있습니다.
- Update: 상태 변경에 따른 동작을 처리할 수 있습니다.
- UnMount: 컴포넌트 제거 전 정리 작업을 할 수 있습니다


<br>

## ⭐React Hooks 활용

<br>

### 1. useState [+1] Btn
```javascript
// App.js
// 버튼 클릭으로 +1 된 값 출력하기
import { useState } from 'react'; //useState 훅을 가져 옴

function App() {

// time이라는 변수 setTime은 time을 업데이트하는 변수
// useState의 초기값은 '1'
  const[time, setTime] = useState(1);

  const handleClick = () => {
    setTime(time + 1);
  };

  console.log('업데이트!!');

  return (
    <div>
      <span>현재 시각: {time}시</span>
      // 버튼을 클릭할때 마다(onClick) handle함수가 실행돼 time이 +1이 된다
      <button onClick={handleClick}>Update</button>
    </div>
  );
}

export default App;

// 새로고침을 누르면 초기화된다.
// 브라우저를 새로고침하면 애플리케이션의 모든 메모리 초기화
```
useState를 사용하여 버튼 클릭 시 숫자를 증가시키는 예제입니다.   
초기값은 1이며, 버튼 클릭 시 setTime 함수가 실행되어 time 값이 1씩 증가합니다.   
변경된 값은 화면에 즉시 반영됩니다. 브라우저를 새로고침하면 상태가 초기화됩니다.  

<br>

### 2. useEffect 사용법
```javascript
// App.js
import { useEffect, useState } from 'react';
import React from 'react';

function App() {
  const [count, setCount ] = useState(1);
  const [name, setName] = useState('');

  const handleCountUpdate = () => {
    setCount(count + 1);
  };

  const handleInputChange = (e) => {
    setName(e.target.value);
  }

  // 랜더링 마다 매번 실행됨 - 랜더링 이후
  // useEffect( () => {
  //   console.log('⚙️랜더링');
  // });

  // count가 변화할 때 마다 실행
  //   useEffect( () => {
  //     console.log('🚨count가 변화');
  // }, [count]);

  // count와 name 중 하나가 변화할 때 마다 실행
  //   useEffect(() => {
  //   console.log('🎯 상태 업데이트: count와 name');
  // }, [count, name]);

  // 특정 조건에서만 실행
  // useEffect(() => {
  //   if (name.length < 3) {
  //     console.log('⚠️ 이름이 너무 짧습니다.');
  //   }
  // }, [name]);

  // count가 10이 넘으면 '카운터 초과'
  // useEffect(() => {
  //   if (count > 10) {
  //     setName('카운트 초과');
  //   }
  // }, [count]);

  // 랜더링 되고 한번 만 실행
  useEffect( () => {
    console.log('🚀마운팅');
  }, [])

  return ( 
    <div>
      <button onClick={handleCountUpdate}>Update</button>
      <span>count: {count}</span>
      <input type='text' value={name} onChange={handleInputChange} />
      <span>name: {name}</span>
    </div>
  );
}

export default App;
```
useEffect를 사용하여 React 컴포넌트의 상태 변화(data 변화)나 특정 조건에서 실행할 동작을 정의하는 예제입니다.

<br>

### 3. hooks로 백엔드 서버와 통신
```javascript
import React, { useEffect, useState } from 'react';

function FetchMember({ memberNumber }) {
  const [memberData, setMemberData] = useState(null);

  // API 엔드포인트를 변수로 설정
  const MEMBER_API_URL = 'http://15.164.140.169:4000/member';

  const fetchMemberData = () => {
    fetch(`${MEMBER_API_URL}?member_number=${memberNumber}`) // 변수 활용
      .then((res) => res.json())
      .then((data) => {
        console.log('Member Data:', data); // 받아온 데이터 확인
        setMemberData(data);
      })
      .catch((error) => {
        console.error('Error fetching member data:', error); // 오류 발생 시 에러 출력
        setMemberData(null);
      });
  };

  // memberNumber 값이 변경될 때마다 실행
  useEffect(() => {
    if (memberNumber) {
      fetchMemberData();
    }
  }, [memberNumber]);

  return (
    <div>
      {memberData ? (
        <>
          <p>id: {memberData.id}</p>
          <p>name: {memberData.name}</p>
          <p>car number: {memberData.car_number}</p>
        </>
      ) : (
        <p>memberData 가 없습니다.</p>
      )}
    </div>
  );
}

export default FetchMember;
// memberNumber로 API 호출하여 회원 데이터를 가져오고, 
// 데이터를 화면에 표시하거나 없으면 "memberData 가 없습니다." 메시지를 출력합니다.  
```
**useState**를 사용해 API 데이터를 저장하고 관리하고, **useEffect**로 memberNumber 변경 시 데이터를 fetch하여 상태를 업데이트합니다.