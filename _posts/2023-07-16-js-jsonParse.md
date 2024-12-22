---
title: "[React] JavaScript에서 JSON 문자열로 받은 데이터 파싱하기 "
excerpt: "JSON.parse(), SSE로 데이터를 받았는데 JSON일 때"

categories:
  - FE
tags:
  - [tag1, tag2]

permalink: /FE/jsonParse/

toc: true
toc_sticky: true

date: 2023-07-16
last_modified_at: 2023-07-16
---

## JSON 데이터 파싱
서버에서 보내는 데이터가 JSON 형식이라면, 우리는 이를 JavaScript 객체로 파싱해야 할 필요가 있습니다. 이 글에서는 JavaScript에서 문자열로 받은 JSON 데이터를 파싱하는 방법을 설명하겠습니다.

<br>

### 1. JSON 이란?
```json
{
  "name": "John",
  "age": 30,
  "address": {
    "street": "123 Main St",
    "city": "New York",
    "zipcode": "10001"
  },
  "city": "New York"
}
```
JSON은 "name/value" 쌍을 기반으로 한 객체와 값들의 순서화된 리스트(배열) 구조로, 사람이 읽고 쓰기 쉬운 데이터 교환 형식이다.
[JSON 공식문서](https://www.json.org/json-ko.html)
{: .notice--primary}  

<br>

### 2. SSE (Server-Sent Events)와 데이터 수신
웹 애플리케이션에서는 SSE (Server-Sent Events) 기술을 사용하여 서버에서 클라이언트로 실시간 데이터를 전송받을 수 있습니다. EventSource는 서버로부터 지속적으로 데이터를 받아오는 방법 중 하나로, 페이지가 새로 고침 없이 실시간 업데이트를 받을 수 있게 해줍니다.

<br>

### 3. JSON.parse()으로 JSON 객체로 변환하기
```javascript
  const fetchLowItemSSE = () => {  // SSE 연결을 설정하고 데이터를 받아오는 함수
  const LowItem_url = `${process.env.REACT_APP_BE_API}/notifications/low-inventory/${loginData.store_id}`;  
  // API URL을 환경 변수와 로그인된 사용자 ID로 동적으로 설정
  const eventSource = new EventSource(LowItem_url, {  // EventSource 객체를 사용하여 SSE 연결
    headers: {
      Accept: 'text/event-stream',  // 서버로부터 text/event-stream 형식의 데이터를 받겠다는 요청
    },
  });

  eventSource.onopen = function (event) {  // SSE 연결이 성공적으로 열렸을 때 실행되는 함수
    if (eventSource.readyState === EventSource.OPEN) {  // 연결 성공
      console.log('연결 성공'); 
    } else {
      console.log('연결 실패');  // 연결에 실패한 경우 '연결 실패' 출력
    }
  };

  eventSource.onmessage = (e) => {  // 서버에서 메시지가 수신되었을 때 실행되는 함수
    const firstData = JSON.parse(e.data)[1].data;  
    // JSON.parse(e.data) 서버에서 보낸 메시지 문자열을 JSON 객체로 변환하는 작업을 수행
    // [1]: 결과에서 두 번째 요소를 선택
    // .data;: 선택된 객체(두 번째 요소)의 속성
    const secondData = JSON.parse(firstData); //JSON.parse()를 한 번 더 호출하여 객체로 변환
    const messageLow = secondData.message;  // 메시지 추출
    const productsLow = secondData.products;  // 제품 목록 추출

    setCartListData(messageLow);  // 메시지를 저장
    setMessagesLowItem((prev) => [...prev, messageLow]);  // 이전 메시지 목록에 새로운 메시지를 추가
    setCartListLowProductData(productsLow);  // 제품 목록을 저장

    console.log("productsLow", productsLow); 
  };

  eventSource.onerror = (e) => {  // 오류가 발생했을 때 실행되는 함수
    eventSource.close();  // 오류가 발생하면 SSE 연결을 종료

    if (e.error) {  // 오류가 있을 경우
      console.log('에러가 발생했습니다.');  // 에러 발생 메시지 출력
      console.log(e);  // 에러 객체를 콘솔에 출력
    }
    if (e.target.readyState === EventSource.CLOSED) {  // 연결이 종료되었을 때 실행되는 코드
    }
    return () => {
      eventSource.close();  // SSE 연결 종료
    };
  };
};

```
이렇게 해서 JSON.parse()으로 JSON 객체로 변환 할 수 있습니다.

<br>

위 코드에서 `.data`가 이해 안간다면

```javascript
const user = {
  "name": "John",
  "age": 30,
  "address": {
    "street": "123 Main St",
    "city": "New York",
    "zipcode": "10001"
  },
  "city": "New York"
};

const address = user.address;
console.log(address);
```
결과  
```json
{
  "street": "123 Main St",
  "city": "New York",
  "zipcode": "10001"
}
```