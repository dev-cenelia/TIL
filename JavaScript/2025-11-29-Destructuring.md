구조 분해 할당(Destructuring) 정리
1. 구조 분해 할당이란?

배열/객체 같은 구조화된 데이터를
한 번에 여러 변수로 나눠 담는 문법

기존 방식

const user = { name: "Tom", age: 20 };
const name = user.name;
const age = user.age;


구조 분해 방식

const user = { name: "Tom", age: 20 };
const { name, age } = user;


→ 코드 줄 수가 줄고, 의미가 더 명확해짐
→ React에서 props 받을 때 거의 필수로 사용

2. 배열 구조 분해
2-1. 기본 사용
const arr = [1, 2, 3];

const [a, b, c] = arr;
// a = 1, b = 2, c = 3

2-2. 건너뛰기
const numbers = [10, 20, 30, 40];

const [first, , third] = numbers;
// first = 10, third = 30

2-3. 기본값 설정
const [x = 0, y = 0] = [];
// x = 0, y = 0

const [a = 1, b = 2] = [10];
// a = 10, b = 2

2-4. rest와 함께 사용
const arr = [1, 2, 3, 4, 5];

const [first, ...rest] = arr;
// first = 1
// rest = [2, 3, 4, 5]

const [first, second, ...rest2] = arr;
// first = 1
// second = 2
// rest2 = [3, 4, 5]


배열 구조 분해 안에서 ...rest는
“앞에서 쓰고 남은 요소들을 배열로 모은 것”

3. 객체 구조 분해
3-1. 기본
const user = { name: "Tom", age: 20 };

const { name, age } = user;
// name = "Tom", age = 20

3-2. 변수 이름 바꾸기(alias)
const user = { name: "Tom", age: 20 };

const { name: userName, age: userAge } = user;
// userName = "Tom", userAge = 20


해석: user.name 값을 userName이라는 변수에 담겠다.

3-3. 기본값과 함께 사용
const user = { name: "Ana" };

const {
  name,
  age = 0,
  nickname: nick = "guest"
} = user;

// age = 0
// nick = "guest"

3-4. 객체에서 rest 사용
const user = {
  id: 99,
  name: "Hana",
  age: 30,
  country: "Korea",
};

const { id, ...info } = user;

// id = 99
// info = { name: "Hana", age: 30, country: "Korea" }


...info에는 id를 제외한 나머지 key들이 객체로 모인다.

4. 중첩 구조 분해
4-1. 중첩 객체
const user = {
  name: "Lia",
  address: {
    city: "Seoul",
    zip: 12345
  }
};

const {
  address: { city, zip }
} = user;

console.log(city, zip); // "Seoul", 12345


여기서 address라는 변수는 따로 생기지 않는다.

console.log(address); // ReferenceError

4-2. 배열 + 객체 + 중첩
const products = [
  {
    id: 1,
    info: { title: "Keyboard", price: 30000 }
  },
  {
    id: 2,
    info: { title: "Monitor", price: 120000 }
  },
];

const [, { info: { price: monitorPrice } }] = products;

console.log(monitorPrice); // 120000


두 번째 요소 선택 후, info.price를 monitorPrice라는 변수에 담는 패턴

5. 함수 매개변수에서 구조 분해

실무/React에서 가장 자주 쓰는 패턴.

5-1. 객체 매개변수 구조 분해
function printUser({ name, age }) {
  console.log(name, age);
}

printUser({ name: "Tom", age: 20 });

5-2. 기본값 + alias까지 함께
function createUser({
  name,
  age = 0,
  role: userRole = "normal",
}) {
  console.log(name, age, userRole);
}

createUser({ name: "Amy", role: "admin" });
// name = "Amy", age = 0, userRole = "admin"


React에서 props 받을 때 이 패턴이 그대로 사용된다.

6. rest / spread와 구조 분해 관계 정리
6-1. spread(펼치기)

역할: 배열/객체를 개별 요소로 “펼치는” 것

예시

const arr = [1, 2, 3];
const newArr = [...arr, 4, 5];
// [1, 2, 3, 4, 5]

const user = { name: "Tom", age: 20 };
const userWithCity = { ...user, city: "Seoul" };
// { name: "Tom", age: 20, city: "Seoul" }

6-2. rest(모으기)

역할: 구조 분해 안에서 “남은 값들을 모아서” 받는 것

배열

const [first, second, ...rest] = [1, 2, 3, 4, 5];
// rest = [3, 4, 5]


객체

const { id, ...info } = { id: 1, name: "Tom", age: 20 };
// info = { name: "Tom", age: 20 }

6-3. 핵심 정리

spread: 값을 펼쳐서 넣는다.

rest: 남은 값을 모아서 새 변수에 담는다.

rest는 항상 “구조 분해 문맥 안에서만” 사용된다.
(혼자 단독으로 쓰지 않음)

7. 실무에서 선호되는 패턴 vs 비추천 패턴
7-1. 실무에서 자주 쓰는 패턴

중첩 객체 구조 분해

const response = {
  status: 200,
  data: {
    user: { id: 5, name: "Aiden", age: 25 }
  },
};

const {
  data: {
    user: { id, name }
  }
} = response;


index로 꺼내고 → 구조 분해

const payments = [
  { id: 1, details: { method: "card", amount: 5000 } },
  { id: 2, details: { method: "bank", amount: 12000 } },
  { id: 3, details: { method: "cash", amount: 7000 } },
  { id: 4, details: { method: "kakao", amount: 30000 } },
];

const fourthData = payments[3];
const {
  details: { amount: fourthAmount },
} = fourthData;

console.log(fourthAmount); // 30000


함수 매개변수에서 props 구조 분해

function UserCard({ name, age, address: { city } }) {
  console.log(name, age, city);
}

7-2. 실무에서 조심해서 쓰는 패턴
const [,, { user: { nickname } }] = orders;


문법적으로는 맞지만:

가독성이 떨어짐

“배열에서 세 번째 요소”에 강하게 의존

데이터 정렬이 바뀌면 쉽게 깨짐

→ 학습용으로는 좋지만, 실무에서는
index 변수로 꺼내서 구조 분해하는 방식이 더 선호됨.

const thirdOrder = orders[2];
const { user: { nickname: thirdUserNickname } } = thirdOrder;

8. 오늘 헷갈렸던 부분 정리
8-1. ...rest와 구조 분해 관계

...rest는 구조 분해 문법과 완전히 연결된 기능

구조 분해 없이 단독으로 쓰지 않는다.

예시

const [first, second, ...rest] = [1, 2, 3, 4, 5];
// rest = [3, 4, 5]

8-2. 점(.)은 구조 분해에서 사용하지 않는다

틀린 예시:

// 이렇게는 못 씀
const { product.productName } = data;


올바른 예시:

const {
  product: { productName, price },
} = data;

8-3. alias를 써야 “변수명 바꾸기”가 된다
const { price: monitorPrice } = product;
// price 값을 monitorPrice 변수에 담는 것

9. 구조 분해 체크리스트

 배열 구조 분해 기본 ([a, b] = arr)

 배열에서 rest 사용 ([a, ...rest])

 객체 구조 분해 기본 ({ name, age } = user)

 객체에서 alias 사용 ({ name: userName })

 객체에서 rest 사용 ({ id, ...info })

 중첩 구조 분해 (객체 안의 객체, 배열 안의 객체)

 함수 매개변수에서 구조 분해 (function fn({ name }) {})

 index로 꺼내고 → 구조 분해 (실무 스타일)
