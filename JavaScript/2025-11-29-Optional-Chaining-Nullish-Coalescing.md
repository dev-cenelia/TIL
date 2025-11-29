Optional Chaining(?.) & Nullish Coalescing(??) 정리
1. Optional Chaining(?.) 이란?

객체나 배열처럼 중첩된 값에 안전하게 접근하기 위한 문법
중간 단계가 null 또는 undefined여도 에러 없이 undefined를 반환하고 끝낸다.

const city = user?.address?.city;


위 코드는 아래와 비슷한 의미:

const city =
  user &&
  user.address &&
  user.address.city;

1-1. 기본 사용
const user = {
  name: "Tom",
  profile: {
    contact: {
      email: "tom@example.com",
    },
  },
};

const email = user?.profile?.contact?.email;
// "tom@example.com"


중간에 하나라도 null 또는 undefined면 전체 결과는 undefined.

1-2. 없는 값에 접근할 때 에러 방지
const user = {};

console.log(user.address?.city);
// undefined (에러 안 남)


기존 방식:

console.log(user.address.city);
// Cannot read properties of undefined 에러

1-3. 배열 접근과 함께 사용
const orders = [
  { id: 1, user: { name: "Tom" } },
  { id: 2, user: { name: "Sara" } },
  {
    id: 3,
    user: {
      name: "Jake",
      profile: null,
    },
  },
];

const thirdData = orders[2];
const thirdNickname = thirdData?.user?.profile?.nickname;

console.log(thirdNickname); // undefined


thirdData.user는 있음

thirdData.user.profile은 null

?. 덕분에 에러 없이 undefined만 반환

1-4. 경로 전체를 “위에서부터 차례대로 체크한다”

접근 경로가:

response.data.posts[2].author.profile.contact.email


이라면 optional chaining으로는:

const email = response
  ?.data
  ?.posts?.[2]
  ?.author
  ?.profile
  ?.contact
  ?.email;


이 흐름은:

response가 있는지 확인

response.data가 있는지 확인

response.data.posts가 있는지 확인

posts[2]가 있는지 확인

author가 있는지 확인

profile이 있는지 확인

contact가 있는지 확인

email 접근

중간에 하나라도 null/undefined면 전체 결과는 undefined.

내가 헷갈렸던 부분:
“실질적으로 내가 불러오려는 데이터 상단의 요소들은 다 체크해야 하는 거네?”
→ 맞다. optional chaining은 그 경로를 한 단계씩 안전하게 타고 들어가는 문법이라고 보면 된다.

2. Nullish Coalescing(??) 이란?

한 줄로 정리:

값이 null 또는 undefined일 때만 오른쪽 기본값을 쓰는 연산자

형식:

const result = 값 ?? 기본값;


동작:

값이 null 또는 undefined면 → 기본값 사용

그 외(0, "", false, NaN 등)는 → 그대로 사용

2-1. 기본 예제
const nickname = null;
const name = "";
const displayName = nickname ?? name ?? "Guest";

// nickname → null → 다음
// name → "" (null/undefined 아님) → 그대로 사용
console.log(displayName); // ""


내가 썼어야 하는 정답:

const displayName = nickname ?? name ?? "Guest";


실수한 코드:

const displayName = name ?? nickname ?? "Guest";
// 순서가 name → nickname 이라 nickname은 보지도 못하고 name에서 끝남

3. || 와 ?? 의 차이
3-1. || (논리 OR)

|| 는 “falsy면 오른쪽으로 넘어감”.

0 || 10        // 10
"" || "hello"  // "hello"
false || true  // true


0, "", false 도 “없음”으로 취급

3-2. ?? (Nullish OR 느낌)

?? 는 “null 또는 undefined일 때만” 오른쪽으로 넘어감.

0 ?? 10        // 0
"" ?? "hello"  // ""
false ?? true  // false
null ?? "hi"   // "hi"
undefined ?? 5 // 5


정리:

|| : falsy(0, "", false, null, undefined, NaN 등)이면 오른쪽

?? : 오직 null, undefined일 때만 오른쪽

4. 0과 ""은 왜 “유효한 값”인가?

Nullish에서 “유효한 값”이라는 말은:

null / undefined가 아닌 모든 값

이라는 뜻이다.

0 → 유효한 값

"" → 유효한 값

false → 유효한 값

NaN → 유효한 값

[], {} → 유효한 값

이 값들은 “비어 보이거나 falsy”해도
Nullish 기준에서는 데이터가 있는 상태로 취급된다.

4-1. stock 예시
const product = {
  details: {
    stock: 0,
  },
};

const finalStock = product.details?.stock ?? 0;
console.log(finalStock); // 0


stock = 0 은 “재고 없음(0개)”이라는 의미 있는 정보

Nullish는 0을 “없음”으로 취급하지 않으므로 기본값으로 덮어쓰지 않는다.

내가 헷갈렸던 부분:

stock은 0이지만 0도 유효한 값으로 사용해야 함
이걸 Nullish에서 어떤 의미로 받아들여야 하는지 헷갈렸다.

→ 의미:
0은 null/undefined가 아니기 때문에,
Nullish에서는 “이미 값이 있는 상태”로 인정한다.
그래서 ?? 뒤 기본값으로 바꾸지 않는다.

4-2. 빈 문자열("") 예시
const user = {
  name: "",
  nickname: null,
};

const { name, nickname } = user;
const displayName = nickname ?? name ?? "Guest";

console.log(displayName); // ""


nickname: null → 다음으로 넘어감

name: "" → null/undefined가 아님 → 유효한 값 → 여기서 끝

내가 이해한 포인트:
“빈 문자열도 null이나 undefined가 아닌 ‘빈 값’일 뿐,
Nullish 입장에서는 유효한 값이다.”

5. ?? 는 AND(&&)가 아니라 “특수한 OR” 느낌

내가 중간에 헷갈린 부분:

이건 그리고(AND) 같은 건가? && 도 들어가는 건가?

정리:

&& 는 “둘 다 true여야 한다”는 조건용 연산자

?? 는 “왼쪽이 null/undefined면 오른쪽으로 넘어가는 값 선택용 OR”

const name = nickname ?? "Guest";


의 뜻은:

nickname이 있으면 그걸 쓰고,
없으면 Guest를 쓴다.

즉, Nullish는 “조건식”이라기보단
**조건을 기반으로 “값을 선택하는 OR”**이라고 이해하면 편함.

예:

const padding = props.padding ?? 16;
// padding 값이 null/undefined면 16, 아니면 그대로 사용

6. Optional Chaining + Nullish 조합

실무에서 가장 많이 쓰는 패턴:

const city = user?.address?.city ?? "Unknown";


동작 흐름:

user?.address?.city 에 안전하게 접근

중간에 하나라도 null/undefined면 전체 결과는 undefined

undefined ?? "Unknown" → "Unknown"

예제:

const product = {
  title: "Wireless Mouse",
  price: 25000,
  discount: null,
  details: {
    stock: 0,
    shipping: {
      method: "택배",
      fee: undefined,
    },
  },
};

// 1) discount: null → 기본값 0
const finalDiscount = product.discount ?? 0; // 0

// 2) stock: 0 → 유효한 값, 기본값으로 덮어쓰지 않음
const finalStock = product.details?.stock ?? 0; // 0

// 3) fee: undefined → 기본값 3000 사용
const shippingFee = product?.details?.shipping?.fee ?? 3000; // 3000

7. 이번에 내가 특히 헷갈렸던 포인트 정리

Optional Chaining에서
“실질적으로 내가 불러오려는 데이터 상단의 요소들은 다 체크해야 하는 거네?”
→ 맞다. ?.는 그 경로를 한 단계씩 안전하게 타고 내려가는 역할.

Nullish에서
“stock은 0이지만 0도 유효한 값으로 사용해야 함”
“빈 문자열도 null/undefined는 아닌데, 이건 어떤 의미?”
→ Nullish 기준에서 0, "", false 등은 ‘값이 있는 상태’
→ 기본값으로 덮어쓰지 않는다.
→ 오직 null/undefined만 “데이터 없음”으로 보고 기본값으로 대체.

“?? 이건 결국 ‘또는(OR)’ 느낌 아닌가? &&는 아닌 거지?”
→ 그렇다. ??는 null/undefined일 때 다음 값으로 넘어가는 “특수한 OR”
→ 조건 검사(AND)가 아니라, 값 선택용 OR.

숫자 데이터에 "0"(문자열)을 기본값으로 쓰면 왜 애매한가?
→ 타입이 달라져서 연산 시 문제 생김
→ 숫자 데이터에는 숫자 기본값, 문자열 데이터에는 문자열 기본값을 쓰는 게 안전.

8. 정리용 공식

Optional Chaining(?.)
→ “중간에 뚫려 있어도 에러 내지 말고 undefined로 끝내라.”

Nullish Coalescing(??)
→ “null/undefined일 때만 오른쪽 기본값을 써라.”

0, "", false
→ falsy지만 nullish가 아니다.
→ 값이 “존재한다”고 보고 그대로 유지.

자주 쓰는 패턴

const value = obj?.deep?.path ?? defaultValue;
