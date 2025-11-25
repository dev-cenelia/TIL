## 1. Promise 개념 정리
Promise란?

비동기 작업의 상태를 표현하는 객체

작업의 완료/실패 여부를 추적하는 "상태 머신"

실제 비동기 작업을 수행하는 것이 아니라, 결과를 담는 그릇

Promise의 3가지 상태

pending: 대기 중

fulfilled: 성공(resolve)

rejected: 실패(reject)

Promise가 필요한 이유

콜백 지옥(callback hell) 해결

비동기 처리 흐름을 가독성 있게 만들기 위해

에러 핸들링 통합(then/catch)

## 2. then 체이닝
then의 동작

then은 성공값(fulfilled) 을 받아 다음 then으로 전달

return된 값은 다음 then(v)으로 들어감

Promise.resolve(1)
  .then(v => v + 1)   // 2
  .then(v => v + 10)  // 12
  .then(v => console.log(v)); // 12

then 체이닝 특징

각 then은 새로운 Promise를 반환

따라서 무한히 이어붙일 수 있음

## 3. 에러 처리 (catch)
throw/reject 발생 시

체인 중 하나라도 실패하면 → 즉시 catch로 점프

실패한 이후의 then들은 실행되지 않음

.then(v => { throw v })
.catch(err => console.log(err));

중요한 특징

catch가 실행되면 Promise 상태가 다시 fulfilled로 회복

그래서 catch 뒤에 then이 정상적으로 이어짐

.catch(err => {
  return err + 5;
})
.then(v => console.log(v)); // catch에서 return한 값

## 4. 런타임 에러도 catch로 감
예시
.then(v => v.abc.def) // TypeError → catch로 이동

JSON.parse 에러도 catch로 감

JSON 문법에 맞지 않으면 자동으로 SyntaxError 발생

Promise 체인에서는 catch가 처리

## 5. async / await
async

함수에 붙이면 자동으로 Promise를 반환하는 함수가 됨

async function fn() {
  return 10; // 실제로는 Promise.resolve(10)
}

await

Promise가 결과를 반환할 때까지 기다리는 문법

async 함수 안에서만 사용 가능

코드 흐름이 동기처럼 읽힘

async function load() {
  const res = await fetch(url);
}

await의 동작 원리

await 아래의 코드를 마이크로태스크 큐로 넘기고

즉시 async 함수 밖으로 제어권 반환

따라서 순서가 섞이는 이유는 이벤트 루프 때문임

## 6. try/catch (async 에러 처리)

비동기 코드를 동기처럼 에러 핸들링 가능

try {
  const res = await fetch(url);
  const data = await res.json();
} catch (err) {
  console.log("에러:", err);
}

## 7. fetch / AJAX / axios 차이 정리
AJAX (개념)

비동기 통신 전체를 말하는 “개념”

fetch

브라우저 내장 HTTP 요청 API

Promise 기반

axios

HTTP 요청 라이브러리

React 전용이 아니라 어디서나 사용 가능

async/await

비동기 문법 (Promise를 편하게 쓰는 도구)

✔ 핵심 정리

fetch/axios → 실제 비동기 요청 담당
Promise → 비동기 결과를 표현
async/await → Promise를 읽기 쉽게 만든 문법
AJAX → 비동기 통신이라는 개념 자체
