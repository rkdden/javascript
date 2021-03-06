# 모던 자바스크립트 Deep Dive

## 목차
* [21장 빌트인 객체]()
* [22장 this]()
* [23장 실행 컨텍스트]()
* [24장 클로저]()
* [25장 클래스]()
* [more](./readme5.md)

# 21장 빌트인 객체
## 21-1 자바스크립트 객체의 분류
* 표준 빌트인 객체
  * ECMAScript 사양에 정의된 객체
* 호스트 객체
  * ECMAScript 사양에 정의되어 있지 않지만 js 실행 환경에서 추가로 제공하는 객체
* 사용자 정의 객체
  * 사용자가 직접 정의한 객체

## 21-2 표준 빌트인 객체
* js는 Object, String, Number 등등 40여개의 표준 빌트인 객체를 제공한다.
* Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체다.
* 표준 빌트인 객체는 인스턴스 없이도 호출 가능한 빌트인 정적 메서드를 제공한다.

## 21-3 원시값과 래퍼 객체
* 원시값인 문자열, 숫자, 불리언 값의 경우 이들 원시값에 대해 마치 객체처럼 마침표 표기법으로 접근하면 js 엔진이 일시적으로 원시값을 연관된 객체로 변환해 준다.
* 문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체를 래퍼 객체라고 한다.
* 문자열, 숫자, 불리언, 심벌 이외의 원시값인 null과 undefined는 래퍼 객체를 생성하지 않는다.

## 21-4 전역 객체
* 전역 객체는 코드가 실행되기 이전 단계에 js 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체이다.
* 어떤 객체에도 속하지 않는 최상위 객체다.
* 브라우저 환경에서는 window이며 Node.js에서는 global이다.
> globalThis
> * ES11에서 도입되었으며, 브라우저 환경과 Node.js 환경에서 전역 객체를 가리키던 식별자를 통일한 식별자이다.
* 전역 객체는 모든 빌트인 객체의 최상위 객체다.
* 전역 객체의 특징
  * 전역 객체는 개발자가 의도적으로 생성할 수 없다.
  * 전역 객체의 프로퍼티를 참조할 때 wiwndow(또는 global)를 생략할 수 있다.
  * 전역 객체는 모든 표준 빌트인 객체를 프로퍼티로 가지고 있다.
  * js 실행 환경에 따라 추가적으로 프로퍼티와 메서드를 갖는다.
  * var 키워드로 선언한 전역 변수와 선언하지 않은 변수에 값을 할당한 암묵적 전역, 그리고 전역 함수는 전역 객체의 프로퍼티가 된다.
  * let이나 const로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.
  * 브라우저 환경의 모든 js코드는 window를 공유한다.

### 21-4-1 빌트인 전역 프로퍼티
* 빌트인 전역 프로퍼티는 전역 객체의 프로퍼티를 의미한다.
* **Infinity**: 무한대를 나타내는 숫자값
* **NaN**: 숫자가 아님을 나타내는 숫자값
* **undefined**: 원시 타입 undefined를 값으로 갖는다.

### 21-4-2 빌트인 전역 함수
* 빌트인 전역 함수는 전역 객체의 메서드이다.
* **eval**: eval 함수는 문자열을 인수로 받는다. 전달받은 코드가 표현식이면 코드를 런타입에 평가해서 값을 생성하며, 문잉라면 런타임에 실행한다.
```js
// 표현식인 문
eval('1 + 2;'); // -> 3
// 표현식이 아닌 문
eval('var x = 5;'); // -> undefined

console.log(x); // 5
```
  * eval 함수는 기존의 스코프롤 런타임에 동적으로 수정한다.
  * eval 함수의 사용을 금지하자.

* **isFinite**: 전달받은 인수가 정상적인 유한수인지 검사한다. 유한수이면 true, 무한수이면 false를 반환한다.
* **isNaN**: 전달받은 인수가 NaN인지 검사하여 결과를 불리언으로 반환한다. 숫자면 false, 숫자가 아니면 true를 반환한다.
* **parseFloat**: 전달받은 문자열 인수를 실수로 해석하여 반환한다.
* **parseInt**: 문자열 인수를 정수로 해석하여 반환한다. 두번째 인수로 진법을 정의할 수 있다.
* **encodeURI/decodeURI**
![](./images/21/URL.png)
  * encodeURI는 완전한 URI를 문자열로 전달받아 인코딩한다.
  * decodeURI는 인코딩된 URI를 인수로 전달받아 디코딩한다.
* **encodeURIComponent/decodeURIcomponent**
  * encodeURIComponent는 URI 구성 요소를 인수로 전달받아 인코딩한다.
  * decodeURIComponent는 URI 구성 요소를 인수로 전달받아 디코딩한다.

### 21-4-3 암묵적 전역
* 선언되지 않은 객체는 js엔진에 의해 전역 객체의 프로퍼티가 되어 전역 변수처럼 동작한다.
* 이러한 현상을 암묵적 전역이라고 한다.

# 22장 this
## 22-1 this 키워드
* 메서드가 자신이 속한 객체의 프로퍼티를 참조하려면 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.
* this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다. this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.
* this가 가리키는 값, this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.
```js
// 객체 리터럴
const circle = {
  radius: 5,
  getDiameter() {
    return 2 * this.radius;
  }
}
```

## 22-2 함수 호출 방식과 this 바인딩
* this 바인딩은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.
* 함수의 호출 방식
  * 일반 함수 호출
  * 메서드 호출
  * 생성자 함수 호출
  * 간접 호출

### 22-2-1 일반 함수 호출
* 기본적으로 this에 전역 객체가 바인딩 된다.
* 일반 함수로 호출된 모든 함수 내부의 this에는 전역 객체가 바인딩된다.




# 23장 실행 컨텍스트
# 24장 클로저
# 25장 클래스
