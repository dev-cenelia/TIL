배열 메서드 정리
1. 배열 메서드란?

배열을 반복하거나, 변환하거나, 조건으로 추려내거나, 누적하는 등의 작업을 함수 형태로 편하게 제공하는 도구. 콜백 함수를 매개변수로 받아서 배열 요소를 가공한다. → JS는 함수를 값처럼 전달할 수 있다(일급 함수)!!

2. 주요 배열 메서드
forEach()

단순 반복

새로운 배열을 만들지 않음

filter()

조건을 만족하는 요소만 배열로 반환

const passed = arr.filter(item => item.score >= 80);
map()

각 요소를 변환하여 새 배열 생성

const result = arr.map(item => ({ ...item, price: item.price * 1.1 }));

※ 객체 리턴 시 ()`로 감싸야 객체 리터럴로 인식됨

reduce()

누적 계산하는 메서드

arr.reduce((acc, item) => acc + item, 0);

acc(누적값)의 타입은 초기값이 결정한다!!

숫자 누적: 0

객체 누적: {}

some()

조건을 만족하는 요소가 하나라도 있으면 true

arr.some(item => item === "Strawberry");
find()

조건을 만족하는 첫 번째 요소를 반환

arr.find(user => user.age >= 25);
every()

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
4. 핵심 요약

filter: 조건으로 배열 추림

map: 배열 변환

reduce: 누적 처리 (acc 초기값 중요!)

some: 하나라도 조건 충족?

find: 조건 충족 첫 요소

every: 모두 조건 충족?

배열 메서드를 활용하면 데이터 처리 로직을 간결하게, 재사용성 높게 구성할 수 있다!
