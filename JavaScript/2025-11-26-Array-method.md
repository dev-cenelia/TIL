배열 메서드 정리
1. 배열 메서드란?

배열을 반복 / 변환 / 조건 필터 / 누적하는 작업을 편하게 제공하는 도구

콜백 함수를 매개변수로 받아 배열 요소를 가공

자바스크립트는 함수를 값으로 전달할 수 있는 일급 함수 언어!

2. 주요 배열 메서드
✔ forEach()

단순 반복

새로운 배열을 만들지 않음

arr.forEach(item => console.log(item));

✔ filter()

조건을 만족하는 요소만 모아 새 배열로 반환

const passed = arr.filter(item => item.score >= 80);

✔ map()

각 요소를 변환하여 새 배열 생성

객체 리턴 시 ()로 감싸야 객체 리터럴로 인식됨

const result = arr.map(item => ({
  ...item,
  price: item.price * 1.1,
}));

✔ reduce()

배열을 누적해 하나의 값으로 만드는 메서드

acc(누적값)의 타입은 초기값이 결정

arr.reduce((acc, item) => acc + item, 0);


초기값 예시

숫자 누적: 0

객체 누적: {}

✔ some()

조건을 하나라도 만족하면 true

arr.some(item => item === "Strawberry");

✔ find()

조건을 만족하는 첫 번째 요소 반환

arr.find(user => user.age >= 25);

✔ every()

모든 요소가 조건을 만족하면 true

arr.every(score => score >= 60);

3. filter + map + reduce 조합 (실무 핵심)
const items = [
  { name: "Keyboard", price: 30000 },
  { name: "Mouse", price: 15000 },
  { name: "Monitor", price: 200000 },
  { name: "USB", price: 10000 },
];

const filtered = items.filter(item => item.price >= 20000);

const discounted = filtered.map(item => ({
  ...item,
  price: item.price * 0.9,
}));

const total = discounted.reduce((acc, item) => acc + item.price, 0);

console.log(total); // 198000


데이터 필터 → 변환 → 누적 흐름이 실무에서 매우 자주 쓰임

4. 핵심 요약

filter: 조건으로 배열 추림

map: 배열 변환

reduce: 누적 처리 (초기값 중요!)

some: 하나라도 충족?

find: 첫 요소 반환

every: 모두 충족?

→ 배열 메서드 활용하면 코드 간결 + 재사용성 증가 + 유지보수 편함
