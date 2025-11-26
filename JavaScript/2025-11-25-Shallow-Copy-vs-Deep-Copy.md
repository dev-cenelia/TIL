# 얕은 복사 vs 깊은 복사 정리



## 1. 복사란?
- 자바스크립트에서 **참조 타입(객체, 배열)** 을 다룰 때 중요한 개념
- 변수에 값을 직접 저장하는 것이 아니라 **메모리 주소를 참조**
- 복사 방식에 따라
  - **주소만 복사(얕은 복사)**
  - **내부 값까지 완전 복사(깊은 복사)**
  로 나뉜다



## 2. 얕은 복사(Shallow Copy)
- **겉(1 depth)만 복사**
- 내부에 **참조 타입이 있으면 주소 공유 → 원본과 연결**

### 주요 방법
```js
const obj2 = { ...obj1 };         // Spread 문법
const arr2 = [...arr1];           // Spread 문법
const obj2 = Object.assign({}, obj1);
const arr2 = arr1.slice();
문제점 예시
js
코드 복사
const user1 = {
  name: "Kim",
  scores: { math: 80 },
};

const user2 = { ...user1 };
user2.scores.math = 100;

console.log(user1.scores.math); // 100 (원본도 변경)
→ 내부 객체는 같은 주소를 바라봄

3. 깊은 복사(Deep Copy)
내부까지 전부 새로 생성

원본과 완전히 독립된 값

주요 방법
js
코드 복사
const newObj = JSON.parse(JSON.stringify(obj));
=> 가장 쉬운 방법이지만,

함수, undefined, Symbol, Date, RegExp 등 유실 또는 변환됨

제대로 된 깊은 복사: 구조적 복제
js
코드 복사
const copy = structuredClone(obj);
장점

Date, Array, Object 등 대부분 타입 안전하게 복사

원본과 완전 분리

주의

함수(Function)는 복사 불가

4. 비교 요약
구분	주소 공유 여부	성능	실무 사용도	예시
얕은 복사	내부 참조 공유	빠름	매우 높음	Spread, assign
깊은 복사	완전 분리	느릴 수 있음	상황 따라 사용	structuredClone

→ 중첩 객체가 없다면 얕은 복사로 충분
→ 중첩 객체가 있다면 깊은 복사 필요

5. 실무 팁
상태 관리(Recoil, Redux) 에서 특히 중요

원본 변경 방지 → 사이드 이펙트 감소

JSON 방식 대신 structuredClone 우선 고려

얕은 복사 후 내부만 선택적으로 깊게 복사하는 경우도 많음

js
코드 복사
const user2 = {
  ...user1,
  scores: { ...user1.scores },
};
6. 핵심 요약
자바스크립트 객체/배열은 참조 타입

얕은 복사 = 주소만 복사 → 내부 객체는 공유

깊은 복사 = 내부까지 전부 새 메모리 할당

중첩 구조 유무에 따라 선택해야 함
