# 모던 자바스크립트 Deep Dive

## 목차
* [16장 프로퍼티 어트리뷰트](#16장-프로퍼티-어트리뷰트)
* [17장 생성자 함수에 의한 객체 생성](#17장-생성자-함수에-의한-객체-생성)
* [18장 함수와 일급 객체](#18장-함수와-일급-객체)
* [19장 프로토 타입](#19장-프로토-타입)
* [20장 strict mode](#20장-strict-mode)
* [more](./readme5.md)

## 16장 프로퍼티 어트리뷰트
### 16-1 내부 슬롯과 내부 메서드
* 내부 슬롯과 내부 메서드는 js 엔진 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 의사 프로퍼티와 의사 메서드다.
* 내부 슬롯과 내부 메서드는 js 엔진에서 실제로 동작하지만 외부로 공개된 객체의 프로퍼티는 아니다.
* js 엔진의 내부 로직이며 원칙적으로는 직접 접근하거나 호출할 수 있는 방법을 제공하지 않는다.
* 일부 내부 슬롯과 내부 메서드에 한하여 간접적으로 접근할 수 있는 수단을 제공한다.
### 16-2 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체
* js 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다.
* 프로퍼티의 상태란 프로퍼티의 값, 값의 갱신 가능 여부, 열거 가능 여부, 재정의 가능 여부이다.
* 프로퍼티 어트리뷰트는 js 엔진이 관리하는 내부 상태 값인 내부 슬롯 ```[[value]], [[Writable]], [[Eumerable]], [[Configurable]]```이다.
```js
const person = {
  name: 'Lee',
};

// Object.getOwnPropertyDescriptor를 사용하여 간접적으로 확인 가능
// 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환한다.
console.log(Object.getOwnPropertyDescriptor(person, 'name'));
// { value: 'Lee', writable: true, enumerable: true, configurable: true }
```
* Object.getOwnPropertyDescriptor 메서드는ㄴ 프로퍼티 디스크립터 객체를 반환한다.

### 16-3 데이터 프로퍼티와 접근자 프로퍼티
* 프로퍼티는 데이터 프로퍼티와 접근자 프로퍼티로 구분할 수 있다.
  * 데이터 프로퍼티 : 키와 값으로 구성된 일반적인 프로퍼티이다. 지금까지 살펴본 모드느 프로퍼티는 데이터 프로퍼티이다.
  * 접근자 프로퍼티 : 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 하마수로 구성된 프로퍼티다.

#### 16-3-1 데이터 프로퍼티
* 데이터 프로퍼티는 아래 표와 같은 프로퍼티 어트리 뷰트를 갖는다.
* 이 프로퍼티 어트리뷰트는 js 엔진이 프로퍼티를 생성할 때 기본값으로 자동 정의된다.
![](./images/16/1.png)

#### 16-3-2 접근자 프로퍼티
* 접근자 프로퍼티는 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값으르 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티다.
![](./images/16/2.png)
* 접근자 함수는 getter/setter 함수라고도 부른다.
```js
const person = {
  firstName: "Ungmo",
  lastName: "Lee",

  // fullName은 접근자 함수롤 구성된 접근자 프로퍼티이다.
  // getter
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },

  // setter
  set fullName(name) {
    [this.first, this.second] = name.split(" ");
  },
};

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조.
console.log(person.firstName + ' ' + person.lastName);    // Ungmo Lee

// 접근자 프로퍼티를통한 프로퍼티 값의 저장
// fullName에 값을 저장하면 setter 호출
person.fullName = 'Heegun Lee';
console.log(person);

// 접근자 프로퍼티를 통한 프로퍼티 값의 참조
console.log(person.fullName);

// firstName은 데이터 프로퍼티이다.
// 프로퍼티 어트리뷰트를 갖는다
let descriptor = Object.getOwnPropertyDescriptor(person, 'firstName');
console.log(descriptor);
// { value: 'Ungmo', writable: true, enumerable: true, configurable: true }

// fullName은 데이터 프로퍼티이다.
// 프로퍼티 어트리뷰트를 갖는다
descriptor = Object.getOwnPropertyDescriptor(person, 'fullName');
console.log(descriptor);
// { get: [Function: get fullName], set: [Function: set fullName], enumerable: true, configurable: true }
```
* person 객체의 firstName과 lastName은 일반적인 데이터 프로퍼티이다.
* 메서드 앞에 get, set이 붙은 베서드가 getter/setter 함수이며, 함수의 ㅣ이름 fullName이 접근자 프로퍼티다.
* 접근자 프로퍼티는 자체적으로 값을 가지지 않으며 데이터 프로퍼티를 읽거나 저장하라 때 관여한다.
* 내부 슬롯/메서드 관점
  * fullName으로 프로퍼티 값에 접근하면 내부적으로 ```[[Get]]``` 내부 메서드가 호출되어 아래와 같이 동작한다.
  1. 프로퍼티 키가 유효한지 확인한다. / 프로퍼티 키 "fullName"은 유효한 프로퍼티 키이다.
  2. 프로토타입 체인에서 프로퍼티를 검색한다. / person 객체에 fullName 프로퍼티가 존재한다.
  3. 검색된 fullName 프로퍼티가 데이터 프로퍼티인지 접근자 프로퍼티인지 확인한다. / fullName 프로퍼티는 접근자 프로퍼티다.
  4. 접근자 프로퍼티 fullName의 프로퍼티 어트리뷰트 ```[[Get]]```의 값, getter 함수를 호출하여 결과를 반환한다.

* 접근자 프로퍼티와 데이터 프로퍼티를 구별하는 방법
```js
// 일반 객체의 __proto__는 접근자 프로퍼티이다.
Object.getOwnPropertyDescriptor(Object.prototype, '__proto__');
// { get: [Function: get __proto__], set: [Function: set __proto__], enumerable: false, configurable: true }

// 함수 객체의 prototype은 데이터 프로퍼티이다.
Object.getOwnPropertyDescriptor(function () {}, 'prototype');
// { value: {}, writable: true, enumerable: false, configurable: false }
```

### 16-4 프로퍼티 정의
* 프로퍼티 정의란 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나, 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것을 말한다.
* Object.defineProperty 메서드를 사용하면 프로퍼티의 어트리뷰트를 정의할 수 있다.
```js
const person = {};

// 데이터 프로펕 정의
Object.defineProperty(person, 'firstName', {
  value: 'Ungmo',
  writable: true,
  enumerable: true,
  configurable: true,
});

// 객체를 누락시키면 undefined, false가 기본이다.
Object.defineProperty(person, 'lastName', {
  value: 'Lee',
});

// 에러는 일어나지 않으며 무시된다.
person.lastName = 'Kim';
```
* Object.defineProperty 메서드로 프로퍼티 정의시 디스크립터 객체의 프로퍼티를 일부 생략 할 수있다.
* 프로퍼티 | 기본값
  * value | undefined
  * get | undefined
  * set | undefined
  * writable | false
  * enumerable | false
  * configurable | false
* Object.defineProperties를 사용하면 여러 개의 프로퍼티를 한 번에 정의할 수 있다.

### 16-5 객체 변경 방지
* 객체는 프로퍼티를 추가하거나 삭제할 수 있고, 프로퍼티 값을 갱신할 수 있다.
* js는 객체의 변경을 방지하는 다양한 메서드를 제공하며, 메서드마다 금지하는 강도가 다르다.
<table><thead><tr><th>구분</th><th>메소드</th><th>추가</th><th>삭제</th><th>값 읽기</th><th>값 쓰기</th><th>재정의</th></tr></thead><tbody><tr><td>객체 확장 금지</td><td>Object.preventExtensions</td><td>✕</td><td>O</td><td>O</td><td>O</td><td>O</td></tr><tr><td>객체 밀봉</td><td>Object.seal</td><td>✕</td><td>✕</td><td>O</td><td>O</td><td>✕</td></tr><tr><td>객체 동결</td><td>Object.freeze</td><td>✕</td><td>✕</td><td>O</td><td>✕</td><td>✕</td></tr></tbody></table>

#### 16-5-1 객체 확장 금지
* Object.preventExtensions 메서드는 객체의 확장을 금지한다.
* 프로퍼티 추가가 금지된다.
* Object.isExtensible 메서드로 확인 가능하다.

#### 16-5-2 객체 밀봉
* Object.seal 메서드는 객체를 밀봉한다.
* 밀봉된 객체는 읽기와 쓰기만 가능하다.
* Object.isSealed 메서드로 확인 가능하다.

#### 15-5-3 객체 동결
* Object.freeze 메서드는 객체를 동결한다.
* 동결된 객체는 읽기만 가능하다.
* Object.isFrozen 메서드로 확인가능

#### 15-5-4 불변 객체
* 지금까지 본 변경 방지 메서드들은 얕은 변경 방지로 중첩 객체까지는 영향을 주지는 못한다.
* 읽기 전용의 불변 객체를 구현하려면 객체를 값으로 갖는 모든 프로퍼티에 대해 재귀적으로 Object.freeze 메서드를 호출해야 한다.
```js
function deepFreeze(target) {
  // 객체가 아니거나 동결된 객체는 무시
  if (target && typeof target === 'object' && !Object.isFrozen(target)) {
    Object.freeze(target);
    // 모든 프로퍼티를 순회하며 재귀적으로 동결한다.
    Object.keys(target).forEach(key => deepFreeze(target[key]));
  }
  return target;
}
```

## 17장 생성자 함수에 의한 객체 생성
* 객체는 객체 리터럴 이외에도 다양한 방법으로 생성할 수 있다.
* 이 장에서는 생성자 함수를 사용하여 객체를 생성하는 방식을 알아보자.
### 17-1 Object 생성자 함수
* new 연산자와 함께 Object 생성자를 호출하면 빈 객체를 생성하여 반환한다. 이후 프로퍼티 또는 메서드를 추가할 수 있다.
```js
const person = new Object();

person.name = 'Lee';
person.sayHello = function () {
  console.log('Hi');
}
console.log(person);
person.sayHello();
```
* 생성자 함수란 new 연산자와 함께 호출하여 객체를 생성하는 함수이다.
* 생성자 함수에 의해 생성된 객체를 인스턴스라고 한다.
* Object이외에도 String, Number, Boolean, Function, Array, Date, RegExp, Promise 등의 빌트인 생성자 함수를 제공한다.
* 객체를 생성하는 방법은 객체 리터럴이 더 간편하다.

### 17-2 생성자 함수
#### 17-2-1 객체 리터럴에 의한 객체 생성 방식의 문제점
* 객체 리터럴에 의한 객체 생성 방식은 단 하나의 객체만 생성한다.
* 동일한 프로퍼티를 갖는 객체를 여러 개 생성하는 경우 비효율적이다.

#### 17-2-2 생성자 함수에 의한 객체 생성 방식의 장점
* 생성자 함수에 의한 객체 생성 방식은 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다.
* 생성자 함수를 정의하고 new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다.
* new 연산자와 함께 생성자 함수를 호출하지 않으면 일반 함수로 동작한다.
```js
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  }
}

// 인스턴스의 생성
const circle1 = new Circle(5);

// 일반 함수로 동작
const circle3 = Circle(15);
```

#### 17-2-3 생성자 함수의 인스턴스 생성 과정
* 생성자 함수의 역할은 인스턴스를 생성하는 것과 생성된 인스턴스를 초기화하는 것이다.
* 인스턴스를 생성하는 것은 필수이며, 인스턴스를 초기화하는 것은 옵션이다.
* js 엔진은 암묵적인 처리를 통해 아래와 같은 과정을 거쳐 인스턴스를 생성하고 반환한다.
  * 1. 인스턴스 생성과 this 바인딩
    * 암묵적으로 빈 객체가 생성되며, 이 빈 객체가 바로 생성자 함수가 생성한 인스턴스이다.
    * 인스턴스는 this에 바인딩된다.
  * 2. 인스턴스 초기화
    * 생성자 함수에 있는 코드가 this에 바인딩되어 있는 인스턴스를 초기화한다.
  * 3. 인스턴스 반환
    * 생성자 함수 내부의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
    * this가 아닌 다른 객체를 명시적으로 반환하면 return 문에 명시한 객체가 반환된다.
    * 원시 값을 반환하면 무시되고 암묵적으로 this가 반환된다.
* 생성자 함수 내부에서는 return문을 반드시 생략하자.

#### 17-2-4 내부 메서드 [[call]]과 [[Construct]]
* 함수는 객체이므로 일반 객체와 동일하게 동작할 수 있다.
* 하지만 일반 객체는 호출할 수 없지만 함수는 호출할 수 있다.
* 함수 객체는 내부 슬롯과 내부 메서드를 포함해서 [[call]], [[Construct]] 같은 내부 메서드를 추가로 가지고 있다.
* 함수가 일반 함수로서 호출되면 함수 객체의 내부 메서드 [[call]]이 호출되고 new 연산자와 함께 생성자 함수로서 호출되면 내부 메서드 [[Construct]]가 호출된다.
* 내부 메서드 [[call]]을 갖는 함수 객체를 callable이라고 하며, 내부 메서드 [[Construct]]를 갖는 함수 객체를 constructor, 갖지 않는 함수 객체를 non-constructor라고 부른다.

#### 17-2-5 constructor와 non-constructor의 구분
* js 엔진은 함수 정의를 평가하여 함수 객체를 생성할 때 함수 정의 방식에 따라 구분한다.
> constructor: 함수 선언문, 함수 표현식, 클래스
> non-constructor: 메서드(ES6 메서드 축약 표현), 화살표 함수
* ECMAScript 사양에서 메서드란 ES6의 메서드 축약 표현만을 의미한다.
```js
// 일반 함수 정의: 함수 선언문, 함수 표현식
function foo() {}
const bar = function () {};
// 프로퍼티 x에 할당된 것은 일반 함수 정의이다. 메소드 정의로 인정하지 않는다.
const baz = {
  x: function () {}
};

// 일반 함수로 정의된 함수만이 constructor이다.
new foo(); // OK
new bar(); // OK
new baz.x(); // OK

// 화살표 함수 정의
const arrow = () => {};

new arrow(); // TypeError: arrow is not a constructor

// 메소드 정의: ES6의 메소드 축약 표현만을 메소드 정의로 인정한다.
const obj = {
  x() {}
};

new obj.x(); // TypeError: obj.x is not a constructor
```

#### 17-2-6 new 연산자
* new 연산자와 함께 함수를 호출하면 해당 함수는 생성자 함수로 동작한다.
  * 내부 메서드 [[Construct]]가 호출된다.
* new 연산자와 없이 함수를 호출하면 해당 함수는 일반 함수로 호출된다.
  * 내부 메서드 [[Call]]이 호출된다.
* 따라서 생성자 함수는 일반적으로 파스칼 케이스로 명명하여 일반 함수와 구별한다.

#### 17-2-7 new.target
* ES6에서는 new.target을 지원한다.
* 함수 내부에서 new.target을 사용하면 new 연산자와 함께 생성자 함수로서 호출되었는지 확인할 수 있다.
  * new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target은 함수 자신을 가리킨다.
  * new 연산자와 없이 일반 함수로서 호출되면 함수 내부의 new.target은 undefined이다.
```js
function Circle(radius) {
  // 이 함수가 new 연산자와 함께 호출되지 않았다면 new.target은 undefined
  if (!new.target) {
    return new Circle(radius);
  }
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}
```
> 스코프 세이프 생성자 패턴
> new.target은 ES6에서 도입된 문법이며 IE는 지원하지 않는다.
> new.target을 사용할 수 없는 상황이라면 스코프 세이프 생성자 패턴을 사용할 수 있다.
* 대부분의 빌트인 생성자 함수는 new 연산자와 함께 호출되었는지 확인 후 적절한 값을 반환한다.

## 18장 함수와 일급 객체
### 18-1 일급 객체
* 아래와 같은 조건을 만족하는 객체를 일급 객체라고 한다.
  1. 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
  2. 변수나 자료구조(객체, 배열등)에 저장할 수 있다.
  3. 함수의 매개변수에 전달할 수 있다.
  4. 함수의 반환값으로 사용할 수 있다.
* js의 함수는 아래 예제와 같이 조건을 모두 만족하므로 일급 객체다.
```js
// 런타임에 함수 리터럴이 평가되어 함수 객체가 생성되고 변수에 할당된다.
const increase = function (num) {
  return ++num;
}

const decrease = function (num) {
  return --num;
}

// 2.
const predicates = { increase, decrease };

function makeCounter(predicate) {
  let num = 0;

  return function () {
    num = predicate(num);
    return num;
  };
}
// 3.
const increaser = makeCounter(predicates.increase);
```
* 함수가 일급 객체라는 것은 함수를 객체와 동일하게 사용할 수 있다는 의미이다.
* 함수는 일반 개개체와 같이 함수의 매개변수에 전달할 수 있고, 함수의 반환값으로 사용할 수도 있다.

### 18-2 함수 객체의 프로퍼티
* 함수도 프로퍼티를 가질수 있다.
* 브라우저 콘솔에서 console.dir 메서드로 함수 객체의 내부를 볼 수 있다.
![](./imagaes/17/1.png)
* arguments, caller, length, name, prototype 프로퍼티는 모두 함수 객체의 데이터 프로퍼티이다.
#### 18-2-1 arguments 프로퍼티
* arguments 프로퍼티값은 arguments 객체다.
* arguments 객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체이며, 함수 내부에서 지역 변수처럼 사용된다.
* 함수가 호출되면 인수 개수를 확인하고 이에 따라 함수의 동작을 달리 정의할 필요가 있을 수 있다.
* 이 때 유용하게 사용되는 것이 arguments 객체다.
* arguments객체는 매개변수 개수를 확정할 수 없는 가변 인자 함수를 구현할 때 유용하다.
```js
function sum() {
  let res = 0;
  for (let i = 0; i < arguments.lengths; i++) {
    res += arguments[i];
  }
  return res;
}
```
* arguments 객체는 유사 배열 객체다.

#### 18-2-2 caller 프로퍼티
* caller 프로퍼티는 ECMAScript 사양에 포함되지 않은 비표준 프로퍼티다. 참고로만 알아두자.

#### 18-2-3 length 프로퍼티
* 함수 객체의 length 프로퍼티는 함수를 정의할 떄 선언한 매개변수의 개수를 가리킨다.
```js
function foo() {
  console.log(foo.length);  // 0
}

function bar(x) {
  return x;
}
console.log(bar.length);  // 1

function baz(x, y) {
  return x * y;
}
console.log(baz.length);  // 2
```

#### 18-2-4 name 프로퍼티
* 함수객체의 name 프로퍼티는 함수 이름을 나타낸다.
* name 프로퍼티는 ES5와 ES6에서 동작이 다르다.
  * 익명 함수 표현식의 경우 ES5에서 name 프로퍼티는 빈 문자열을 값으로 갖는다.
  * 하지만 ES6에서는 name 프로퍼티는 함수 객체를 가리키는 식별자를 값으로 갖는다.
```js
// 기명 함수 표현식
var namedFunc = function foo() {};
console.log(namedFunc.name);  // foo
// 익명 함수 표현식
var annoymousFunc = function () {};
```

#### 18-2-5 __proto__ 접근자 프로퍼티
* 모든 객체는 [[Prototype]]이라는 내부 슬롯을 갖는다.
* ```__proto__``` 프로퍼티는 [[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티다.
* 내부 슬롯에는 직접 접근할 수 없고 간접적인 접근 방법을 제공하는 경우에 한하여 접근할 수 있다.
```js
const obj == { a: 1 };
// 객체 리터럴 방식으로 생성한 객체의 프로토타입 객체는 Object.prototype이다.
console.log(obj.__proto__ === Object.prototype);  // true
```

#### 18-2-6 prototype 프로퍼티
* prototype 프로퍼티는 생성자 함수로 호출할 수 있는 함수 객체, constructor만이 소유하는 프로퍼티다.
* 일반 객체와 생성잦 함수로 호출할 수 없는 non-constructor에는 prototype 프로퍼티가 없다.

## 19장 프로토 타입
* js는 명령형, 함수형, 프로토타입 기반 객체지향 프로그래밍을 지원하는 멀티 패러다임 프로그래밍 언어다.
* js는 프로토타입 기반의 객체지향 프로그래밍 언어다.
* js를 이루고 있는 거의 모든것이 객체다. (원시 타입의 값을 제외한 나머지 값들은 모두 객체다)
### 19-1 객체지향 프로그래밍
* 객체지향 프로그래밍은 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임이다.
* 객체지향 프로그래밍은 실체를 인식하는 철학적 사고를 프로그래밍에 접목하려는 시도에서 시작한다.
* 실체는 속성을 가지고 있고, 이를 통해 실체를 인식하거나 구별한다.
* 다양한 속성 중에서 프로그램에 필요한 속성만 간추려 내어 표현하는 것을 추상화라고 한다.
* 속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조를 객체라고 한다.
* 원의 경우 원에는 반지름이라는 속성이 있으며 반지름은 원의 상태를 나타내는 데이터이며 원의 지름, 둘레, 넓이를 구하는 것은 동작이다.
```js
const circle = {
  radius: 5,

  getDiameter() {
    return 2 * this.radius;
  },

  getPerimeter() {
    return 2 * Math.PI * this.radius;
  },

  getArea() {
    return Math.PI * this.radius ** 2;
  },
}
```
* 객체지향 프로그래밍은 개개체의 상태를 나타내는 데이터와 상태 데이터를 조작할 수 있는 동작을 하나의 논리적인 단위로 묶어 생각한다.
* 객체는 상태 데이터와 동작을 하나의 논리적인 단위로 묶는 복합적인 자료구조라고 할 수 있다.
* 이때 객체의 상태 데이터를 프로퍼티, 동작을 메서드라고 부른다.

### 19-2 상속과 프로토타입
* 상속은 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것이다.
```js
function Circle(radius) {
  this.radius = radius;
  this.getArea = function () {
    return Math.PI * this.radius ** 2;
  };
}

const circle1 = new Circle(1);
const circle2 = new Circle(2);
```
* 위 예제에서 getArea 메서드가 중복 생성되므로 하나만 셍성하여 모든 인스턴스가 공유해서 사용하는 것이 바람직하다.
* js는 프로토타입을 기반으로 상속을 구현한다.
```js
function Circle(radius) {
  this.radius = radius;
}

// Circle 생성자 함수가 생성한 모든 인스턴스가 getArea 메서드를 공유해서 사용할 수 있도록 프로토타입에 추가한다.
Circle.prototype.getArea = function () {
  return Math.PI * this.radius ** 2;
}

const circle1 = new Circle(1);
const circle2 = new Circle(2);
```
* Circle 생성자 함수가 생성한 모든 인스턴스는 Circle.prototype의 모든 프로퍼티와 메서드를 상속받는다.
* getArea 메서드는 단 하나만 생성되어 Circle 생성자 함수가 생성하는 모든 인스턴스는 상속받는다.
* 상속은 코드의 재사용이란 관점에서 매우 유용하다.

### 19-3 프로토타입 객체
* 프로토타입 객체란 객체 간 상속을 구현하기 위해 사용된다.
* 프로토타입은 어떤 객체의 상위 개체의 역할을 하는 객체이며, 다른 객체에 공유 프로퍼티를 제공한다.
* 모든 객체는 [[Prototype]]이라는 내부 슬롯을 가진다.
* [[Prototype]] 내부 슬롯에 직접 접근을 할 수 없지만, ```__proto__``` 접근자 프로퍼티를 통해 간접적으로 접근이 가능하다.

#### 19-3-1 ```__proto__``` 접근자 프로퍼티
```js
const person = { name: 'Lee' };
```
* 위 예제를 크롬 브라우저의 콘솔에서 출력
![](./images/19/1.PNG)
* ```__proto__``` 접근자 프로퍼티를 통해 Object.prototype에 접근한 결과이다.
* **```__proto__```는 접근자 프로퍼티다.**
  * 내부 슬롯은 프로퍼티가 아니다.
  * Object.prototyp의 ```__proto__```는 getter/setter를 통해 프로토타입을 취득하거나 할당한다.
```js
const obj = {};
const parent = { x: 1 };

// getter 함수인 get __proto__가 호출되어 obj 객체의 프로토타입을 취득한다.
obj.__proto__;

obj.__proto__ = parent;

console.log(obj.x); // 1
```

* **```__proto```접근자 프로퍼티는 상속을 통해 사용된다.**
  * ```__proto__```접근자 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니라 Object.prototype의 프로퍼티다.
  * 모든 객체는 상속을 통해 ```Object.prototype.__proto__``` 접근자 프로퍼티를 사용할 수 있다.

* **```__proto__```** 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유
  * 접근자 프로퍼티를 사용하는 이유는 상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위해서다.
```js
const parent = {};
const child = {};

child.__proto__ = parent;

parent.__proto__ = child;   // TypeError: ~~~
```
  * 위 예제에서 서로를 프로토타입으로 설정했다.
  * 에러가 나지 않으면 비정상적인 프로토타입 체인이 만들어지기 때문에 ```__proto__``` 접근자 프로퍼티가 에러를 발생시킨다.
  * 프로토타입 체인은 단방향 링크드 리스트로 구현되어야 한다.
  * 위 예시처럼 순환참조하는 프로토타입 체인이 만들어지면 체인 종점이 없어서 무한 루프에 빠진다.
* **```__proto__``` 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장하지 않는다.**
  * ```__proto__```는 현재 대부분의 브라우저에서 지원한다.
  * 모든 객체가 ```__proto__```를 사용할 수 있는 것은 아니다.
  * ```__proto___``` 접근자 프로퍼티 대신 프로토타입의 참조를 취득하고 싶다면 Object.getPrototypeOf 메서드를 사용하며, 교체의 경우에는 Object.setPrototypeOf를 사용하자.
```js
const obj = {};
const parent = { x: 1 };
// 프로토타입 취득
Object.getPrototypeOf(obj);   // obj.__proto__;
// 프로토타입 교체
Object.setPrototypeOf(obj, parent); // obj.__proto__ = parent;

console.log(obj.x);   // 1
```

#### 19-3-2 함수 객체의 prototype 프로퍼티
* 함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.
* 생성자 함수로서 호출할 수 없는 함수, 화살표 함수와 ES6 메서드 축약 표현으로 정의한 메서드는 prototype 프로퍼티를 소요하지 않으며 프로토타입도 생성하지 않는다.
* 모든 객체가 가지고 있는 ```__proto__``` 접근자 프로퍼티와 함수 객체만이 가지고 있는 prototype 프로퍼티는 결국 동일한 프로토타입을 가리킨다.

#### 19-3-3 프로토타입의 constructor 프로퍼티와 생성자 함수
* 모든 프로토타입은 constructor 프로퍼티를 갖는다.
* 이 constructor 프로퍼티는 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다.
* 이 연결은 생성자 함수가 생성될 때, 즉 함수 객체가 생성될 때 이루어진다.
```js
function Person(name) {
  this.name = name;
}
const me = new Person('Lee');

console.log(me.constructor === Person);   // true
```

### 19-4 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입
* 리터럴 표기법에 의해 생성된 객체도 프로토타입이 존재한다.
* 하지만 리터럴 표기법에 의해 생성된 객체는 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수라고 단정할 수 없다.
* 프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재한다.

### 19-5 프로토타입의 생성 시점
* 프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다.

#### 19-5-1 사용자 정의 생성자 함수와 프로토타입 생성 시점
* 생성자 함수로서 호출할 수 있는 함수, 즉 constructor는 함수 정의가 펴가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.
```js
console.log(Person.prototype);

// 생성자 함수
function Person(name) {
  this.name = name;
}
```
* non-constructor는 프로토타입이 생성되지 않는다.

#### 19-5-2 빌트인 생성자 함수와 프로토타입 생성 시점
* 빌트인 생성자 함수도 생성되는 시점에 프로토 타입이 생성된다.
* 생성된 프로토타입은 빌트인 생성자 함수의 prototype 프로퍼티에 바인딩된다.
* 생성자 함수 또는 리터럴 표기법으로 객체를 생성하면 프로토타입은 생성된 객체의 [[Prototype]] 내부 슬롯에 할당된다.

### 19-6 객체 생성 방식과 프로토타입의 결정
* 객체는 아래와 같은 생성 방법이 있다.
  * 객체 리터럴
  * Object 생성자 함수
  * 생성자 함수
  * Object.create 메서드
  * 클래스
* 모두 추상 연산 OrdinaryObjectCreate에 의해 생성된다.
* 프로토타입은 추상 연산 OrdinaryObjectCreate에 전달되는 인수에 의해 결정된다.

#### 19-6-1 객체 리터럴에 의해 생성된 객체의 프로토타입
* js 엔진은 객체를 생성할 때 OrdinaryObjectCreate를 호출한다.
```js
const obj = { x: 1 };
```
* obj 객체는 Object.prototype을 프로토타입으로 가지며 상속받는다.

#### 19-6-2 Object 생성자 함수에 의해 생성된 객체의 프로토타입
* Object 생성자 함수를 호출하면 OrdinaryObjectCreate가 호출된다.
* 이 때 OrdinaryObjectCreate에 전달되는 프로토타입은 Object.prototype이다.

### 19-7 프로토타입 체인
```js
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My Name is ${this.name}`);
};

const me = new Person('Lee');
```
* me 객체는 hasOwnProperty를 호출할 수 있다.
* Object.prototype도 상속받았다는 것을 의미한다.
* JS는 객체의 프로퍼티에 접근하려고 할 댸 해당 객체에 접근하려는 프로퍼티가 없다면 [[Prototype]] 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다.
* 이를 프로토타입 체인이라고 한다. 프로토타입 체인은 js가 객체지향 프로그래밍의 상속을 구현하는 메커니즘이다.
* 프로토타입 체인의 최상위의 객체는 언제나 Object.prototype이다.
  * Object.prototype을 프로토타입 체인의 종점이라고 한다.
* 프로토타입 체인은 상속과 프로퍼티 검색을 위한 메커니즘이다.
* 스코프 체인은 식별자 검색을 위한 메커니즘이다.
* 스코프체인과 프로토타입 체인은 서로 협력하여 식별자와 프로퍼티를 검색하는 데 사용된다.

### 19-8 오버라이딩과 프로퍼티 섀도잉
* 메서드를 다시 쓰는걸 오버라이딩 메서드가 가려지는 현상을 프로퍼티 섀도잉이라고 한다.
* 하위 객체를 통해 프로토타입의 프로퍼티를 변경 또는 삭제는 불가능하다.

### 19-9 프로토타입의 교체
* 프로토타입은 동적으로 변경이 가능하다.

#### 19-9-1 생성자 함수에 의한 프로토타입의 교체
```js
const Person = (function Person() {
  function Person(name) {
    this.name = name;
  }

// 1.
  Person.prototype = {
    sayHello() {
      console.log(`Hi! My name is ${this.name}`);
    }
  };

  return Person;
}());

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My Name is ${this.name}`);
};

const me = new Person('Lee');
```
* 1에서 객체 리터럴을 Person.prototype에 할당했다.

#### 19-9-2 인스턴스에 의한 프로토타입의 교체
* 프로토타입은 인스턴스의 ```__proto__``` 접근자 프로퍼티로도 접근할 수 있다.
* 생성자 함수와 인스턴스에 의한 프로토타입 교체는 미묘한 차이가 있다.
* 프로토타입은 직접 교체하지 않는 것이 좋다.

### 19-10 instanceof 연산자
* instanceof 연산자는 이항 연산자이다.
  * 좌변에 객체를 가리키는 식별자, 우변에 생성자 함수를 가리키는 식별자를 피연산자로 받는다.
  * 우변이 함수가 아니면 에러가 발생한다.
```js
객체 instanceof 생성자 함수
```
* 우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true 아니면 false이다.
```js
function Person(name) {
  this.name = name;
}
const me = new Person('Lee');

console.log(me instanceof Person);  // true
console.log(me instanceof Object);  // true
```
* 생성자 함수의 prototype에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인한다.

### 19-11 직접 상속
#### 19-11-1 Object.create에 의한 직접 상속
* Object.create 메서드는 첫 번쨰 매개변수에 전달한 객체의 프로토타입 체인에 속하는 객체를 생성한다.
* 객체를 생성하면서 직접적으로 상속을 구현하는 것이다.
  * 장점은 아래와 같다.
  * new 연산자가 없어도 객체를 생성할 수 있다.
  * 프로토타입을 지정하면서 객체를 생성할 수 있다.
  * 객체 리터럴에 의해 생성된 객체도 상속받을 수 있다.
#### 19-11-2 객체 리터럴 내부에서 ```__proto__```에 의한 직접 상속
* ES6에서는 객체 리터럴 내부에서 ```__proto__``` 접근자 프로퍼티를 사용하여 직접 상속을 구현할 수 있다.
```js
const myProto = { x: 10 };
const obj = {
  y: 20,
  // 객체를 직접 상속
  // obj -> myProto -> Object.prototype -> null
  __proto__: myProto,
};
```

### 19-12 정적 프로퍼티/메서드
* 정적 프로퍼티/메서드는 생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메서드이다.
```js
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
}

// 정적 프로퍼티
Person.staticProp = 'static prop';

// 정적 메서드
Person.staticMethod = function () {
  console.log('staticMethod');
};

const me = new Person('Lee');
```
* Person 생성자 함수 객체가 소유한 프로퍼티 메서드를 정적 프로퍼티/메서드라고 한다.
* 인스턴스로 참조/호출 불가능하다.

### 19-13 프로퍼티 존재 확인
#### 19-13-1 in 연산자
* in 연산자는 객체 내에 특정 프로퍼티가 존재하는지 여부를 확인한다.
```js
// key: 프로퍼티 키를 나타내는 문자열
// object: 객체로 평가되는 표현식
key in object
```
```js
const person = {
  name: 'Lee',
  address: 'Seoul',
};

console.log('name' in person);  // true
console.log('address' in person); // true
console.log('age' in person); // false
console.log('toString' in person);  // true
```
* in은 확인 대상 객체의 프로퍼티 및 상속받은 모든 프로퍼티를 확인한다.
* in 대신 ES6에서 되입된 Reflect.has를 사용할 수 있다. in과 동일하게 동작한다.

#### 19-13-2 Object.prototype.hasOwnProperty 메서드
* Object.prototype.hasOwnProperty 메서드를 사용해도 객체에 특정 프로퍼티가 존재하는지 확인할 수 있다.
```js
console.log(person.hasOwnProperty('name')); //true
console.log(person.hasOwnProperty('age')); //false
// 상속 받은 프로퍼티인 경우 false
console.log(person.hasOwnProperty('toString')); //false
```

### 19-14 프로퍼티 열거
#### 19-14-1 for ... in 문
* 객체의 모든 프로퍼티를 순회하며 열거하려면 for ... in 문을 사용한다.
```js
for(변수선언문 in 객체) {...}
```
```js
const person = {
  name: 'Lee',
  address: 'Seoul',
};

for (const key in person) {
  console.log(key + ': ' + person[key]);
}

// name: Lee
// address: Seoul
```
* 상속받은 프로퍼티까지 열거한다.
* Object.prototype의 프로퍼티는 열거되지 않는다.
* **for ... in 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 true인 프로퍼티를 순회하며 열거한다.**
* 프로퍼티 열거시 순서를 보장하지 않는다.
  * 대부분의 모던 브라우저는 순서를 보장하며 숫자인 프로퍼티 키에 대해서는 정렬을 실시한다.
* 배열에는 for문이나 for of문 forEach메서드를 사용하자

#### 19-14-2 Object.keys/values/entries 메서드
* 객체 자신의 고유 프로퍼티만 열거하기 위해서는 Object.keys/values/entries 메서드를 사용하는 것을 권장한다.
* Object.keys ㅔㅁ서드는 객체 자신의 열거 가능한 프로퍼티 키를 배열로 반환한다.
```js
const person = {
  name: 'Lee',
  address: 'Seoul',
  __proto__: { age: 20 },
};

console.log(Object.keys(person));  // ["name", "address"]
// ES8
console.log(Object.values(person));  // ["Lee", "Seoul"]
// ES8
console.log(Object.entries(person));  // [["name", "Lee"], ["address", "Seoul]]
Object.entries(person).forEach(([key, value]) => console.log(key,value));
// name Lee
// address Seoul
```

## 20장 strict mode
### 20-1 strict mode란?
* 변수 선언을 하고 참조할 경우 js 엔진은 암묵적으로 전역 객체에 변수 프로퍼티를 동적 생성한다.
* 이를 암묵적 전역이라고 부른다.
* ES5부터 실수를 줄이기 위해 strict mode가 추가되었다.
* strict mode는 문법을 엄격히 적용한다.

### 20-2 strict mode의 적용
* 전역의 선두 또는 함수 몸체의 선두에 'use strict';를 추가한다.
```js
'use strict';

function foo() {
  x = 10;   // 에러
}
foo():
```

### 20-3 전역에 strict mode를 적용하는 것은 피하자
* 전역에 적용한 strict mode는 스크립트 단위로 적용된다.

### 20-4 함수 단위로 strict mode를 적용하는 것도 피하자
* strict mode는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.

### 20-5 strict mode가 발생시키는 에러
* 암묵적 전역
* 변수, 함수, 매개변수의 삭제
* 매개변수 이름의 중복
* with 문의 사용

### 20-6 strict mode 적용에 의한 변화
* 일반 함수의 this
  * strict mode에서 함수를 일반 함수로서 호출하면 this에 undefined가 바인딩된다.
* arguments 객체
  * 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.