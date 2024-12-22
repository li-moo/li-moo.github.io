---
title: "[JS] .filter() "
excerpt: ".filter()로 검색 기능 만들기"

categories:
  - FE
tags:
  - [tag1, tag2]

permalink: /FE/js]Filter/

toc: true
toc_sticky: true

date: 2023-10-28
last_modified_at: 2023-10-28
---

## filter() 요소 추출
JavaScript에서 filter() 함수는 배열에서 특정 조건에 맞는 요소들만 추출해 새로운 배열을 반환하는 메서드입니다. 
{: .notice--primary}

<br>

### 1.  filter() 문법
```javascript
const numbers = [1, 2, 3, 4, 5];
const filteredNumbers = numbers.filter(number => number > 2);

console.log(filteredNumbers); // 결과 [3, 4, 5]
```
`filter(요소 => 조건식)`

<br>

### 2. 예제
```javascript
import React, { useState } from "react";

const Cart = () => {
  // 장바구니 데이터 예
  const cartData = [
    { name: "Apple", product_code: 123 },
    { name: "Banana", product_code: 456 },
    { name: "Orange", product_code: 789 },
    { name: "Grapes", product_code: 101 },
  ];

  const [keyword, setKeyword] = useState("");
  const handleSearch = (value) => { 
    setKeyword(value);
  };

  const filteredProducts = cartData.filter((item) =>
    item.name.toLowerCase().includes(keyword.toLowerCase()) ||
    item.product_code.toString().toLowerCase().includes(keyword.toLowerCase())
  );
  // item.name.toLowerCase():  상품의 이름에서 문자열을 모두 소문자로 변환
  // .includes(): 이 메서드는 문자열이 특정 텍스트를 포함하는지 확인 (true or false 반환)
  // item.product_code.toString().toLowerCase(): 문자열로 변환 toString(), 


  return (
    <div>
      <h1>장바구니 검색</h1>
      
      {/* 검색창 */}
      <input
        type="text"
        placeholder="상품을 검색하세요"
        value={keyword}
        onChange={(e) => handleSearch(e.target.value)} // 검색어 업데이트
      />

      <ul>
        {/* 필터링된 상품 목록 출력 */}
        {filteredProducts.length > 0 ? (
          filteredProducts.map((product) => (
            <li key={product.product_code}>
              {product.name} (상품 코드: {product.product_code})
            </li>
          ))
        ) : (
          <li>검색 결과가 없습니다.</li>
        )}
      </ul>
    </div>
  );
};

export default Cart;
```
이 코드는 두 가지 검색 기준(name과 product_code)으로 사용자 입력(keyword)한 검색어를 기반으로 필터링합니다.