JavaScript Class 정리
1. 클래스(Class)란?

객체를 반복적으로 생성하기 위한 템플릿(설계도).
상품, 유저처럼 공통 구조를 가진 데이터를 일관된 형태로 생성할 때 사용한다.
ES6 이후 도입된 문법으로, 이전의 생성자 함수 방식보다 더 읽기 쉽고 직관적이다.

2. 클래스 구성 요소
2-1. constructor()

new 키워드로 객체를 생성할 때 가장 먼저 실행되는 초기 설정 함수.
객체가 가질 기본 재료(초기값)를 세팅하는 영역이다.

class Product {
  constructor(name, price) {
    this.name = name;
    this.price = price;
  }
}

2-2. 메서드(Method)

클래스 내부에 정의하며, 생성된 객체가 사용할 기능을 의미한다.
가격 계산, 옵션 처리, 조건 검사 등 객체의 행동을 담당한다.

class Product {
  getDiscountedPrice(rate) {
    return this.price * (1 - rate);
  }
}

2-3. new 키워드

클래스(설계도)에 재료를 넣어서 실제 객체를 생성하는 과정.
클래스 자체는 설계도이며, new를 통해 실제 인스턴스를 만들어 사용한다.

const p1 = new Product("신발", 50000);

3. 클래스 직관적 비유

class = 설계도(틀)

constructor = 초기 재료 설정

new = 실제로 객체를 찍어내는 과정

메서드 = 그 객체가 가진 기능(행동)

이 구조로 이해하면 가장 쉽다.

4. 클래스가 필요한 이유

여러 객체를 동일한 구조로 생성해야 할 때 일관성 확보

초기값, 상태, 기능을 하나의 묶음으로 관리 가능

전역 함수 남발이나 중복 코드 방지

규모가 커질수록 데이터 구조가 명확해져 유지보수 용이

5. 프로토타입 최소 개념

클래스 내부 메서드는 prototype에 저장되며 모든 인스턴스가 공유한다.
덕분에 객체마다 같은 함수를 복사하지 않기 때문에 메모리 사용도 효율적이다.

class A {
  sayHi() {}
}

const a1 = new A();
const a2 = new A();

a1.sayHi === a2.sayHi; // true


6. React와의 관계

React에는 예전 Class Component 시절이 있었지만
현재는 Hooks 기반 함수형 컴포넌트가 표준이기 때문에
클래스를 직접 사용할 일은 거의 없다.

단, JavaScript 기본기 측면에서는 필수 개념이다.
