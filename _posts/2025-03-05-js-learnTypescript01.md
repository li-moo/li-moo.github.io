---
title: "[TS] 타입스크립트 알아보기"
excerpt: "TypeScript의 대해 알아봅시다."

categories:
  - FE
tags:
  - [tag1, tag2]

permalink: /FE/learnTS01/

toc: true
toc_sticky: true

date: 2025-03-05
last_modified_at: 2025-03-06
---

TypeScript는 JavaScript를 기반으로 하면서도 정적 타입 검사 기능을 추가하여 개발자에게 더욱 안전한 코딩 환경을 제공합니다. 하지만 TypeScript를 제대로 이해하려면, JavaScript와의 관계를 먼저 파악하는 것이 중요합니다. [TypeScript 공식문서](https://www.typescriptlang.org/ko/)
{: .notice--primary} 


## JavaScript
JavaScript는 원래 웹 브라우저에서 동작하는 간단한 스크립팅 언어로 시작되었습니다. 점점 브라우저 엔진이 최적화되고 다양한 API가 추가되면서 JavaScript는 웹 개발의 중심이 되었고, 현재는 Node.js를 통해 서버 개발까지 가능해졌습니다. 하지만 JavaScript는 초기에 빠른 개발을 목표로 설계된 만큼, 몇 가지 문제점이 존재합니다.
<br>
### 문제점
1. 예상치 못한 동작
```js
if ("" == 0) {
  console.log("참입니다!"); // == 연산자가 자동으로 타입 변환
}
```
2. 오타로 인한 오류 감지 어려움
```js
const obj = { width: 10, height: 15 };
const area = obj.width * obj.heigth; // NaN (obj.height 오타 때문에 발생)
```
<br>

## TypeScript 등장배경
TypeScript는 JavaScript의 문제점을 해결하기 위해 등장한 언어입니다. 프로그램을 실행하기 전에 코드의 오류를 미리 찾아줍니다. 이것이 정적 타입 검사(Static Type Checking)입니다.
{: .notice--primary} 

### 정적 타입 검사(Static Type Checking)
```ts
const obj = { width: 10, height: 15 };
const area = obj.width * obj.heigth; 
// 오류: 'heigth' 속성이 존재하지 않음
```

### TypeScript와 JavaScript의 차이점

1. 구문(Syntax)
```ts
let message: string = "Hello, TypeScript!";
```
message 변수에 문자열만 할당할 수 있게 타입을 지정합니다.

2. 정적 타입 시스템
```js
console.log(4 / []); // JavaScript에서는 NaN 출력
```
```ts
console.log(4 / []); 
// TypeScript 오류: 숫자와 배열을 나눌 수 없음
```
TypeScript는 잘못된 연산을 사전에 방지해 런타임 오류를 줄일 수 있습니다.

### TypeScript의 런타임 동작
TypeScript는 JavaScript의 런타임 동작을 변경하지 않습니다. -> TypeScript로 작성한 코드는 결국 JavaScript로 변환됩니다.

TypeScript 컴파일러는 타입 검사만 수행하고, 최종 결과물에서는 타입 정보가 제거됩니다.

---

## TypeScript의 기본 타입 정리  

### 원시 타입 (Primitive Types) 
`Number, String, Boolean, null, undefined` 
JavaScript와 동일한 기본 데이터 타입을 사용하며, TypeScript에서는 타입을 명시적으로 선언할 수 있습니다.  

```ts
// number 타입
const num1: number = 100;
const num2 = 100; // 타입 추론: TypeScript가 자동으로 number로 추론
```
<br>

### 특수 타입 - any, unknown, void, never  

#### any
모든 값을 허용 (가능하면 사용을 지양해야 함)
```ts
let value: any = 100;
value = 'Hello'; 
value = true; 
value = { key: 'value' }; 

console.log(value); // { key: 'value' }
```
<br>

#### unknown
any처럼 모든 값을 할당 가능, 사용하기 전에 타입 검사 必
```ts
let data: unknown = 'TypeScript';
// console.log(data.toUpperCase()); 오류: unknown 타입은 바로 조작 x

if (typeof data === 'string') {
  console.log(data.toUpperCase()); // 타입 검사 후 정상 동작
}
```
<br>

#### void
값이 없는 상태를 의미 (주로 함수의 반환 타입) 
```ts
function printLog(msg: string): void {
  console.log(msg);
}

const returnValue = printLog('print'); // "print"
console.log('return value : ', returnValue); // "return value : ",  undefined 
```
`void`는 함수가 값을 반환하지 않음
<br>

#### never
절대 값이 할당될 수 없는 상태 (예외 처리, 무한 루프 등)

1️⃣ 항상 예외를 던지는 함수
```ts
function throwError(): never {
  throw new Error('err');
  console.log('err'); // 실행되지 않음
}
```
2️⃣ 무한 루프 함수
```ts
function infiniteLoop(): never {
  while (true) {
    // 무한 반복
  }
}
```
3️⃣ never 타입의 값 제한 
```ts
function validCheck(arg: never) {
  // 이 함수는 never 타입 외의 값을 받을 수 없음
}
```
<br>

### 객체 타입 - object, {}, array, tuple, enum

#### object 타입  
```ts
const obj: object = { name: 'john' };
const obj2: {} = { name: 'john' };
```
💡 **주의**  
- `obj.name`처럼 접근하려면 정확한 타입을 지정해야 합니다.  

```ts
const obj3: { name: string } = { name: 'john' };
console.log(obj3.name); // ✅ 정상 실행
```
object는 객체 타입을 의미하지만, 원시 타입 (number, string, boolean 등)은 허용하지 않아.

#### {} (빈 객체 타입)
```ts
const obj1: {} = { name: "John" }; 
const obj2: {} = 123; // 사실상 any와 유사
```

#### 배열(Array) 타입 선언  
배열의 요소는 항상 같은 타입이여야 합니다.
```ts
const arr: Array<number> = [0, 1, 2];
const arr2: number[] = [0, 1, 2];
```
💡 두 방식 모두 동일하게 숫자 배열을 의미합니다. 

#### 튜플 (Tuple): 고정된 길이를 가진 배열
```ts
const tupl: [number, string, string, number] = [0, '1', '2', 3];
```
💡 **설명**  
- 첫 번째 요소는 `number`,  
- 두 번째, 세 번째 요소는 `string`,  
- 네 번째 요소는 `number`만 가능.  

튜플은 인덱스마다 타입이 다를 때 유용하게 사용됩니다.  

#### 열거형 (Enum)  
열거형(enum)은 특정 값들의 집합을 정의할 때 사용됩니다.  
```ts
enum Color {
  Red,
  Green,
  Blue,
}

function selectColor(color: Color) {
  switch (color) {
    case Color.Red:
      console.log('Red selected');
      break;
    case Color.Green:
      console.log('Green selected');
      break;
    case Color.Blue:
      console.log('Blue selected');
      break;
    default:
      validCheck(color); // 절대 실행되지 않음 (never 타입 활용)
  }
}

selectColor(Color.Red); // "Red selected"
selectColor(2); // "Blue selected"
```
💡 설명 
- `Color.Red`는 `0`, `Color.Green`은 `1`, `Color.Blue`는 `2`로 자동 할당됩니다.  
- `selectColor(2)`를 호출하면 `Color.Blue`로 인식됩니다.  

#### 문자열 값을 가진 Enum  
```ts
enum Type {
  User = 'user',
  Admin = 'admin',
}

console.log('type enum :', Type.User, Type.Admin); // "type enum :", "user", "admin"
```
문자열 enum은 숫자 enum과 달리 자동 증가 값이 없음. 명확한 값이 필요할 때 유용합니다.  
<br> 

### 제네릭 (Generics)
제네릭을 사용하면 타입을 동적으로 설정할 수 있습니다. -> 타입을 변수처럼
```ts
function identity<T>(arg: T): T {
  return arg;
}

let output1 = identity<string>("Hello");  // T = string
let output2 = identity<number>(123);      // T = number
```

## 🔹 정리  

| 타입 | 설명 |
|------|------------------------------------------------|
| number, string, boolean | 기본 원시 타입 |
| any | 모든 타입을 허용 (최대한 사용 지양) |
| unknown | `any`와 비슷하지만, 타입을 확정하기 전에는 연산 불가 |
| void | 반환 값이 없는 함수에 사용 |
| never | 절대 값이 할당될 수 없는 상태 (예외 발생, 무한 루프) |
| Array<T> 또는 T[] | 배열 타입 |
| tuple | 고정된 길이와 타입을 가진 배열 |
| enum | 특정 값들의 집합을 정의 |
| object | 객체 타입 (타입을 명시적으로 정의해야 속성 접근 가능) |
| 제너릭(Generic) | 타입을 변수처럼 사용할 수 있도록 정의 |