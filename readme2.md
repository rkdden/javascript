# 모던 자바스크립트 Deep Dive

## 목차
* [6장 데이터 타입](#6장-데이터-타입)
* [7장 연산자](#7장-연산자)
* [8장 제어문](#8장-제어문)
* [9장 타입 변환과 단축 평가](#9장-타입-변환과-단축-평가)
* [10장 객체 리터럴](#10장-객체-리터럴)
* [more](./readme3.md)

## 6장 데이터 타입
* 데이터 타입은 값의 종류를 말한다.
* js의 모든 값은 데이터 타입을 갖는다.
* js(es6)는 7개의 데이터 타입을 제공한다.
  * **원시 타입**과 **객체 타입**으로 분류할 수 있다.
> 원시타입
* 숫자 타입( number ): 숫자. 정수와 실수 구분 없이 하나의 숫자 타입만 존재
* 문자열 타입( string ): 문자열
* 불리언 타입( boolean ): 논리적 참(true)과 거짓(false)
* undefined 타입: var 키워드로 선언된 변수에 암묵적으로 할당되는 값
* null 타입: 값이 없다는 것을 의도적으로 명시할 때 사용하는 값
* 심벌 타입 ( symbol ): ES6에서 추가된 7번째 타입

> 객체 타입
* 객체, 함수, 배열 등

### 6-1 숫자타입
* js에서는 하나의 숫자 타입만 존재한다.
* 모든 수를 실수로 처리한다.
```js
// 모두 숫자 타입
var integer = 10;   // 정수
var double = 10.12; // 실수
var negative = -10; // 음의 정수
```
* 숫자 타입은 추가저거으로 세가지 특별한 값이 있다.
  * Infinity: 양의 무한대
  * -Infinity: 음의 무한대
  * NaN: 산술 연산 불가 (Not a Number)

### 6-2 문자열 타입
* 텍스트 데이터를 나타내는 데 사용한다.
* 문자열을 ( '' ), ( "" ), ( `` )으로 텍스트를 감싼다.

### 6-3 템플릿 리터럴
* ES6부터 새로 도입되었다.
* 멀티라인 문자열, 표현식 삽입, 태그드 템플릿 등 편리한 문자열 처리 기능을 제공한다.
* ``(백틱)을 사용해 표현한다.
```js
var template = `Template literal`;
```
#### 6-3-1 멀티라인 문자열
* 일반 문자열 내에서 줄바꿈등의 공백을 표현하려면 이스케이프 시퀀스를 사용해야 한다.
* 템플릿 리터럴을 사용하면 이스케이프 시퀀스 없이도 줄바꾸무이 허용되고, 모든 공백도 그대로 적용된다.
```js
var template = `<ul>
  <li><a href="#">Home</a></li>
</ul>`;
console.log(template);
-------------------
// 출력 결과
<ul>
  <li><a href="#">Home</a></li>
</ul>
```
#### 6-3-2 표현식 삽입
* 문자열은 문자열 연산자 + 를 사용해 연결할 수 있다.
```js
var first = 'ABC';
var last = 'Lee';
// ES5에서 사용
console.log('My name is' + first + ' ' + last + '.');
```
* 템플릿 리터럴에서는 표현식 삽입으로 해결
```js
var first = 'ABC';
var last = 'Lee';
console.log(`My name is ${first} ${last}.`);
```
* 표현식 삽입은 반드시 템플릿 리터럴 내에서 사용해야 한다.

### 6-4 불리언 타입
* 불리언 타입은 참, 거짓을 나타태는 true와 false 뿐이다.

### 6-5 undefined 타입
* undefined 타입의 값은 undefined가 유일하다.
* undefined는 개발자가 의도적으로 할당하기 위한 값이 아닌 js 엔진이 변수를 초기화 할 때 사용하는 값이다.

### 6-6 null 타입
* null 타입의 값은 null이 유일하다.
* 변수의 값이 없다는 것을 의도적으로 명시할 때 사용한다.
```js
var foo = 'Lee';
// 이전 참조를 제거
foo = null;
```

### 6-7 심벌 타입
* ES6에서 추가된 7번째 타입이며, 변경 불가능한 원시 타입의 값이다.
* 다른 값과 중복 되지 않으며, 객체의 프로퍼티 키를 만들기 위해 사용한다.
* 심벌은 Symbol 함수를 호출해서 생성한다.
```js
// 심벌 값 생성
var key = Symbol('key');
console.log(typeof key);  // symbol

// 객체 생성
var obj = {};

obj[key] = 'value';
console.log(obj[key]);  // value
```

### 6-8 객체 타입
* 11장에서 자세히
* js를 이루고 있는 거의 모든 것이 객체이다.

### 6-9 데이터 타입의 필요성
#### 6-9-1 데이터 타입에 의하나 메모리 공간의 확보와 참조
* 메모리에 값을 저장하려면 메모리 공간의 크기를 정해야 한다.
  * 낭비와 손실없이 하기 위해
* js 엔진은 값의 종류에 따라 정해진 크기의 메모리 공간을 확보한다.
* js 엔진은 변수 타입에 따라 읽을 메모리 단위를 정하고 읽는다.

#### 6-9-2 데이터 타입에 의한 값의 해석
* 메모리에서 읽어들인 값을 데이터 타입에 따라 해석한다.
* 데이터 타입이 필요한 이유
  * 값을 저장할 때 확보해야 하는 메모리 공간의 크기를 결정하기 위해서
  * 값을 참조할 때 한 번에 읽어 들여야 할 메모리 공간의 크기를 결정하기 위해
  * 메모리에서 읽어 들인 2진수를 어떻게 해석할지 결정하기 위해

### 6-10 동적 타이핑
#### 6-10-1 동적 타입 언어와 정적 타입 언어
* C나 자바는 정적 타입 언어이다.
```C
// c 변수에는 1바이트 정수 타입의 값만 할당 가능
char c;
// num 변수에는 4바이트 정수 타입의 값만 할당 가능
int num;
```
* 정적 타입 언어는 컴파일 시점에 타입체크를 수행한다.
  * 통과시 실행
  * 실패시 에러
* js는 변수 선언시 타입을 선언하지 않는다.
  * 어떤 값이라도 자유롭게 할당 가능하다.
  * typeof 연산자로 변수의 데이터 타입을 알 수 있다.
* js의 변수는 선언이 아닌 할당에 의해 타입이 결정(타입 추론)된다.
  * 또한 재할당에 의해 변수의 타입은 언제든지 동적으로 변할 수 있다.
  * 이러한 특징을 동적 타이핑이라하며, 이러한 언어들을 동적 타입 언어라고 한다.

#### 6-10-2 동적 타입 언어와 변수
* 이러한 언어는 편리하다. 하지만 위험하다.
* 복잡한 프로그램에서는 변화하는 변수 값을 추적하기 어려울 수 있다.
* 동적 타입 언어는 유연성은 높지만 신뢰성이 떨어진다.
* 변수 사용시 주의사항
  * 변수는 꼭 필요한 경우에만 사용한다.
  * 변수의 유효 범위는 최대한 좁게 만들어 변수의 부작용을 억제한다.
  * 전역 변수는 최대한 사용하지 않는다.
  * 변수보다는 상수를 사용해 값의 변경을 억제하자. const 키워드 사용
  * 변수 이름은 변수의 목적이나 의미를 알 수 있게 네이밍하자.
* 가독성이 좋은 코드가 좋은 코드다.

## 7장 연산자
* 연산자는 표현식을 대상으로 산술, 할당, 비교, 논리, 타입, 지수 연산 등을 수행해서 하나의 값을 만든다.
* 연상의 대상을 피연산자라고 한다.
* 피연산자가 "값"이라면 연산자는 피연산자를 연산해서 새로운 값을 만드는 동사의 역할이다.

### 7-1 산술 연산자
* 산술 연산자는 피연산자를 대상으로 수학적 계산을 수행해 새로운 숫자 값을 만든다. 불가능하다면 NaN을 반환한다.
* 피연산자의 개수에 따라 이항 산술 연산자와 단항 산술 연산자로 구분한다.
* 부수 효과가 없다.

#### 7-1-1 이항 산술 연산자
* 이항 산술 연산자는 2개의 피연산자를 산술 연산하여 숫자 값을 만든다.
* 어떤 산술 연산을 해도 새로운 값을 만든다.
> 이항 산술 연산자
```
+ : 덧셈
- : 뺄셈
* : 곱셈
/ : 나눗셈
```

#### 7-1-2 단항 산술 연산자
* 단항 산술 연산자는 1개의 피연산자를 산술 연산하여 숫자값을 만든다.
> 단항 산술 연산자
```
++ : 증가
-- : 감소
+ : 효과가 없다.
- : 양수를 음수로, 음수를 양수로 반환
```
* 증가 / 감소 연산자는 부수 효과가 있다.
  * 피연산자 앞에 위치한 전위 증가/감소 연산자는 먼저 값을 증가/감소시킨 후, 연산을 수행한다.
  * 피연산자 뒤에 위치한 전위 증가/감소 연산자는 먼저 연산을 수행한 후, 값을 증가/감소시킨다.

#### 7-1-3 문자열 연결 연산자
* ' + ' 연산자는 피연산자 중 하나 이상이 문자열인 경우 문자열 연결 연산자로 동작한다.
```js
// 문자열 연결 연산자
'1' + 2; // -> '12'
1 + '2'; // ->12
```
* 개발자의 의도와 상관없이 js 엔진이 타입이 자동 변환된다.
* 이를 암묵적 타입 변환 또는 타입 강제 변환이라고 한다.

### 7-2 할당 연산자
* 할당 연산자는 우항에 있는 피연산자의 평가 결과를 변수에 할당한다.
> 할당 연산자
```
=
+=
-=
*=
/=
%=
```
```js
var x;
x = 10;
        // x = 10;
x += 5; // x = x + 5;
        // x = 15;
x -= 5; // x = x - 5;
        // x = 10;
x *= 5; // x = x * 5;
        // x = 50;
x /= 5; // x = x / 5;
        // x = 10;
x %= 5; // x = x % 5;
        // x = 0;
var str = 'my name is';
str += 'Lee';
        // str = 'my name is Lee';
```
* 할당문은 값으로 평가되는 표현식인 문으로 할당된 값으로 평가된다.
```js
var a, b, c;
// 연쇄할당. 오른쪽에서 왼쪽으로 진행
// 1. c = 0;
// 2. b = 0;
// 3. a = 0;
a = b = c = 0;
```

### 7-3 비교 연산자
* 비교 연산자는 피연산자를 비교후 boolean 결과로 반환한다.
#### 7-3-1 동등/일치 비교 연산자
* 동등/일치 비교 연산자는 좌항과 우항의 피연산자가 같은 값으로 평가되는지 비교해서 불리언 값을 반환한다.
> 비교 연산자
```
== : 동등 비교, 단순 값이 같은지 비교
=== : 일치 비교, 값과 타입이 같은지 비교
!= : 부동등 비교, 단순 값만 다른지 비교
!== : 불일치 비교, 값과 타입이 다른지 비교
```
* 동등 비교 연산자는 비교시 암묵적 타입 변환을 통해 타입을 일치시킨다.
```js
5 == 5; // true
// 암묵적 타입 변환
5 == '5'; // true
```
* 동등 비교 연산자는 예측하기 어려운 결과를 만드니 ===를 사용하자.
* NaN만 주의하자
  * NaN 같은 경우 isNaN을 사용하자.
```js
NaN === NaN;  // false
isNaN(NaN);   // true
```

#### 7-3-2 대소 관계 비교 연산자
* 피연산자의 크기를 비교하여 boolean 값을 반환한다.
> 대소 관계 비교 연산자
```
> : 왼쪽이 더 크다.
< : 왼쪽이 더 작다.
>= : 왼쪽보다 크거나 같다.
<= : 오른쪽보다 크거나 같다.
```

### 7-4 삼항 조건 연산자
* 삼항 조건 연산자는 조건식의 평가 결과에 따라 반환값을 결정한다.
> 삼항 조건 연산자
```
조건식 ? 조건식이 true시 반환값 : 조건식이 false시 반환값

var result = score >= 60 ? 'pass' : 'fail';
```
* 평가 결과가 boolean 값이 아니면 boolean 값으로 암묵적 타입 변환된다.
```js
var x = 2;
// x % 2 는 0이므로 0은 false로 변환
var result = x % 2 ? '홀수' : '짝수';
```
* if else 문으로도 사용 가능하지만 if else문은 값처럼 사용할 수 없다.

### 7-5 논리 연산자
* 논리 연산자는 우항과 좌항의 피연산자를 논리 연산한다.
> 논리 연산자
```
|| : 논리합(OR)
&& : 논리곱(AND)
! : 부정(NOT)
```
* 반환값은 불리언 값이다.

### 7-6 쉼표 연산자
* 왼쪽 피연산자부터 차례대로 피연산자를 평가하고 마지막 피연산자의 평가가 끝나면 마지막 평가 결과를 반환한다.
```js
var x, y, z;
x = 1, y = 2, z = 3; // 3
```

### 7-7 그룹 연산자
* 소괄호로 피연산자를 감싸는 그룹 연산자는 가장 먼저 평가하며, 우선순위 조절이 가능하다.
```js
10 * 2 + 3; // 23
10 * ( 2 + 3 ) // 50
```

### 7-8 typeof 연산자
* typeof는 데이터 타입을 문자열로 반환한다.
* 'string', 'number', 'boolean', 'undefined', 'symbol', 'object', 'function' 중 하나를 반환한다.
* null은 object를 반환한다. -> 버그
  * ===를 사용해서 극복하자.

### 7-9 지수 연산자
* ES7부터 도입 되었다. 좌항의 피연산자를 밑으로, 우항의 피연산자를 지수로 거듭 제곱해서 숫자로 반환한다.
```js
2 ** 2;  // 4
2 ** 2.5; // 5.6568....
2 ** 0 // 1
2 ** -2;  // 0.25
```
* 기존에는 Math.pow를 이용했다.

### 7-10 그 외의 연산자
* 다른 장에서 자세하게 알아보자.
* ?, ??, delete, new, instanceof, in 등이 있다.
### 7-11 연산자의 부수 효과
* 일부 연산자는 다른코드에 영향을 주는 부수 효과가 있다.
* =, ++, --, delete 이다.
```js
var x;

x = 1;
        // x = 1;
x++;
        // x = 2;

var o = { a: 1 };

// delete는 객체의 프로퍼티를 삭제한다.
delete o.a;
        // o = {};
```

### 7-12 연산자 우선순위
* 연산자가 실행되는 순서
> 연산자 우선순위
```
1     ()
2     new(매개변수 존재), [](프로퍼티 접근), ()(함수호출), ?.(옵셔널 체이닝)
3     new(매개변수 미존재)
4     x++, x--
5     !x, +x, -x, ++x, --x, typeof, delete
6     **
7     *, /, %
8     +, -
9     <, <=, >, >=, in, instanceof
10    ==, !=, ===, !==
11    ??
12    &&
13    ||
14    ? ... | ...
15    할당 연산자
16    ,
```
* 모두 기억하기 힘들다.
  * ()를 사용해서 우선순위를 명시적으로 제어하자.

### 7-13 연산자 결합 순서
* 어느 쪽 분터 평가를 수행할 것인지를 나타내는 순서.

## 8장 제어문
* 제어문은 코드 블록을 실행하거나 반복 실행시 사용한다.

### 8-1 블록문
* 블록문은 0개 이상의 문을 중괄호로 묶은 것이다.
* 코드 블록 또는 블록이라고 부른다.
* 블록문은 끝에 세미콜론을 붙이지 않는다.
```js
// 블록문
{
  var foo = 10;
}

// 제어문
var x = 1;
if (x < 10) {
  x++;
}

// 함수 선언문
function sum(a, b) {
  return a + b;
}
```

### 8-2 조건문
* 조건문은 주어진 조건식의 평과 결과에 따라 블록문의 실행을 결정한다.
* if else 문과 switch문을 제공한다.
#### 8-2-1 if else 문
* if는 조건식이 참이면 실행 else는 조건식이 거짓이면 실행된다.
* 조건식을 추가하고 싶으면 else if 문을 사용한다.
* if와 else는 한번만 사용 가능하며, else if문 은 여러번 사용 가능하다.
#### 8-2-2 switch 문
* switch문은 표현식을 평가해서 일치하는 표현식을 갖는 case문을 실행한다.
* 일치하는 case문이 없으면 default로 이동한다.
* break문을 사용해서 코드 블록에서 탈출해야한다.

### 8-3 반복문
* 반복문은 조건식의 평가 결과가 참인 경우 코드 블록을 실행한다.
* 이후 조건식을 재평가해서 참인경우 다시 실행한다. 조건문이 거짓일 때까지 반복된다.
* for문, while문, do while문을 제공한다.

#### 8-3-1 for문
* for문은 조건식이 거짓으로 평가될 때까지 코드 블록을 반복 실행한다.
```js
for(변수 선언문 또는 할당문; 조건식; 증감식) {
  조건문이 참인 경우 반복 실행될 문;
}

// 예제
for (let i = 0; i < 2; i++) {
  console.log(i);
}

// 무한루프
for (;;) { ... }
```
* for문안에 for문 사용이 가능하다.

#### 8-3-2 while 문
* while은 조건식이 참이면 반복 실행한다.
* for문은 반복횟수가 명확할때, while문은 불명확할 때 주로 사용한다.
* 무한루프도 가능하다.
* break문으로 탈출 가능

#### 8-3-3 do while문
* do while은 코드 블록을 먼저 실행하고 조건식을 평가한다.
```js
// 예제
var count = 0;
do {
  console.log(count);
  count++;
} while (count < 3);
```

### 8-4 break문
* break문은 레이블문, 반복문, switch문을 탈출한다.
* 레이블문이란 식별자가 붙은 문
```js
foo: console.log('foo');
```

### 8-5 continue문
* continue문은 반복문의 코드 블록 실행을 중단하고 반복문의 증감식으로 실행 흐름을 이동시킨다.

## 9장 타입 변환과 단축 평가
### 9-1 타입 변환이란?
* js의 모든 값은 타입이 있다.
* 값의 타입은 다른 타입으로 변환이 가능하다.
* 개발자가 의도적으로 값의 타입을 변환하는 것을 **명시적 타입 변환** 또는 **타입 캐스팅**이라고 한다.
```js
var x = 10;
// 명시적 타입 변환
var str = x.toString();
        // typeof str = string, str = '10'
```
* js 엔진에 의해 암묵적으로 타입이 자동 변환되는 것을 **암묵적 타입 변환** 또는 **타입 강제 변환**이라고 한다.
```js
var x = 10;
// 암묵적 타입 변환
var str = x + '';
        // typeof str = string, str = '10'
```
* 명시적 타입 변환과 암묵적 타입 변환이 기존 원시 값을 변경하는 것은 아니다.

### 9-2 암묵적 타입 변환
* js 엔진은 코드의 문맥을 고려해 암묵적으로 데이터 타입을 강제 변환할 때가 있다.
* 암묵적 타입 변환이 일어나면 문자열, 숫자, 불리언과 같은 원시 타입중 하나로 타입을 자동 변환한다.


### 9-3 명시적 타입 변환
* 명시적으로 타입을 변경하는 방법은 다양하다.
* 표준 빌트인 생성자 함수를 new 없이 호출하는 방법과 빌트인 메서드를 사용하느느 방법, 암묵적 타입 변환을 이용하는 방법이 있다.

#### 9-3-1 문자열 타입으로 변환
* 문자열 타입으로 변환하는 방법
  * String 생성자 함수를 new 연산자 없이 호출하는 방법
  * Object.prototype.toString 메서드를 이용하는 방법
  * 문자열 연결 연산자를 이용하는 방법

#### 9-3-2 숫자 타입으로 변환
* 숫자 타입으로 변환하는 방법
  * Number 생성자 함수를 new 연산자 없이 호출하는 방법
  * parseInt, parseFloat 함수를 사용하는 방법
  * ' + ' 단항 산술 연산자를 이용하는 방법
  * ' * ' 단항 산술 연산자를 이용하는 방법

#### 9-3-3 불리언 타입으로 변환
* 불리언 타입으로 변환하는 방법
  * Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
  * ! 부정 논리 연산자를 두 번 사용하는 방법

### 9-4 단축 평가
#### 9-4-1 논리 연산자를 사용한 단축 평가
* || 또는 && 연산자 표현식의 평가 결과는 boolean 값이 아닐 수도 있다.
* 논리곱 연산자는 논리 연산의 결과를 결정하는 두 번째 피연산자를 반환
* 논리합 연산자는 논리 연산의 결과를 결정한 첫 번째 피연산자를 반환
```js
'Cat' && 'Dog' // "Dog"
'Cat' || 'Dog' // "Cat"
```
* 논리곱과 논리합은 논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그대로 반환한다. 이를 단축 평가라고 한다.
* 단축 평가는 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것을 말한다.
```
단축 평가 표현식
true || anything    -> ture
false || anything   -> anything
true && anything    -> anything
false && anything   -> false
```
* 단축 평가는 아래와 같은 상황에서 유용하게 사용된다.
  * 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때
  ```js
  var elem = null;
  var value = elem.value; // 에러
  -------------------------------
  var elem = null;
  var value = elem && elem.value; // null
  ```
  * 함수 매개변수에 기본값을 설정할 때
  ```js
  function getStringLength(str) {
    str = str || '';
    return str.length;
  }
  getStringLength(); // 0
  getStringLength('hi'); // 2
  // -------------------------------
  // es6의 매개변수 기본값 설정
  function getStringLength(str = '') {
    return str.length;
  }
  getStringLength(); // 0
  getStringLength('hi'); // 2
  ```

#### 9-4-2 옵셔널 체이닝 연산자
* ES11에서 도입되었다.
* ?. 으로 사용하며 좌항의 피연산자가 null 또는 undefined이면 undefined를 반환하고, 아니면 우항의 참조를 이어간다.
```js
var elem = null;
var value = elem?.value;
        // value = undefined
```

#### 9-4-3 null 병합 연산자
* ES11에서 도입되었다.
* ?? 로 사용하며 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고, 아니면 좌항의 피연산자를 반환한다.
* 변수에 기본값을 설정할 때 유용하다.
```js
var foo = null ?? 'default string';
        // foo = "default string";
```

## 10장 객체 리터럴
### 10-1 객체란?
* js는 객체기반의 프로그래밍 언어이며, 거의 모든것이 객체이다.
* 원시 값을 제외한 나머지 값(함수, 배열, 정규 표현식등)은 모두 객체다.
* 원시 타입은 단 하나의 값만 나타내지만 객체 타입은 다양한 타입의 값을 하나의 단위로 구성한 복합적인 자료구조다.
* 원시값은 변경 불가능한 값, 객체는 변경 가능한 값이다.
![](./images/10/1.png)
* js에서 사용할 수 있는 모든 값은 프로퍼티 값이 될 수 있다.
* 프로퍼티 값이 함수일 경우 메서드라고 부른다.
* 프로퍼티와 메서드
  * 프로퍼티: 객체의 상태를 나타내는 값
  * 메서드: 프로퍼티를 참조하고 조작할 수 있는 동작

### 10-2 객체 리터럴에 의한 객체 생성
* 다양한 객체 생성 방법을 지원한다.
  * 객체 리터럴
  * object 생성자 함수
  * 생성자 함수
  * Object.create 메서드
  * 클래스(ES6)
* 객체 리터럴은 중괄호 내에 0개 이상의 프로퍼티를 정의한다.
```js
var person = {
  name: 'Lee',
  sayHello: function () {
    console.log(`Hello! My name is ${this.name}`);
  }
};

console.log(typeof person);   // object
console.log(person);          // { name: 'Lee', sayHello: [Function: sayHello] }s
```
* 중괄호 안에 프로퍼티를 정의하지 않으면 빈 객체가 생성된다.

### 10-3 프로퍼티
* 객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성된다.
  * 프로퍼티 키: 빈 문자열을 포함하는 모든 문자열 또는 심벌 값
  * 프로퍼티 값: js에서 사용할 수 있는 모든 값
* 프로퍼티를 나열할 때 쉼표로 구분한다.
* 식별자 네이밍 규칙을 따르지 않아도 되며 따르지 않는다면 반드시 이름에 따옴표를 써야한다.
  * 웬만하면 따르자.
* 이미 존재하는 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티가 덮어쓴다.

### 10-4 메서드
* 프로퍼티 값이 함수일 경우 일반 하마수와 구분하기 위해 메서드라고 부른다.
```js
var circle = {
  radius: 5,  // 프로퍼티

  // 원의 지름
  getDiameter: function () {  // 메서드
    return 2 * this.radius; // this는 circle을 가리킨다.
  }
};
```

### 10-5 프로퍼티 접근
* 프로퍼티 접근 방법은 두가지이다.
  * 마침표 프로퍼티 접근 연산자(.)를 사용하는 마침표 표기법
  * 대괄호 프로퍼티 접근 연산자([])를 사용하는 대괄호 표기법
* 프로퍼티 키가 식별자 네이밍 규칙을 준수하면 두개 다 사용가능하다.
```js
var person = {
  name: 'Lee',
};
// 마침표 표기법
console.log(person.name); // Lee
// 대괄호 표기법
console.log(person['name']);  // Lee
```
* 대괄호 표기법 사용시 내부에 지정하는 프로퍼티 키는 반드시 따옴표로 감싼 문자열 이어야 한다.
* 객체에 존재하지 않는 프로퍼티에 접근하면 undefined를 반환한다.

### 10-6 프로퍼티 값 갱신
* 이미 존재하는 프로퍼티에 값을 할당하면 프로퍼티 값이 갱신된다.

### 10-7 프로퍼티 동적 생성
* 존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 생성되어 추가된다.
```js
var person = {
  name: 'Lee',
};
person.age = 20;

console.log(person);  // {name: 'Lee', age: 20}
```

### 10-8 프로퍼티 삭제
* delete 연산자는 객체의 프로퍼티를 삭제한다.
```js
var person = {
  name: 'Lee',
};
person.age = 20;

delete person.age;

// 무시된다.
delete person.address;
```

### 10-9 ES6에서 추가된 객체 리터럴의 확장 기능
* ES6에서 더욱 간편하고 표현력 있는 객체 리터럴의 확장 기능을 제공한다.
#### 10-9-1 프로퍼티 축약 표현
* ES6에서 프로퍼티 값으로 변수를 사용하는 경우 변수 이름과 프로퍼티 키가 동일한 이름이면 프로퍼티 키를 생략 가능
* 프로퍼티 키는 변수 이름으로 자동 생성
```js
let x = 1, y = 2;

// 프로퍼티 축약 표현
const obj = {
  x,
  y,
};

console.log(obj); // {x: 1, y: 2}
```

#### 10-9-2 계산된 프로퍼티 이름
* 문자열 값으로 평가되는 표현식을 사용해 프로퍼티 키를 동적으로 생성 가능하다.
* 프로퍼티 키로 사용할 표현식을 대괄호로 묶어야 하며, 이를 계산된 프로퍼티 이름이라고 한다.
```js
// ES5
var prefix = 'prop';
var i = 0;
var obj = {};

// 계산된 프로퍼티 이름으로 프로퍼티 키 동적 생성
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;

console.log(obj); // { prop-1: 1, prop-2: 2, prop-3: 3 }
```
```js
// ES6
const prefix = 'prop';
let i = 0;

const obj = {
  [`${prefix}-${++i}`] = i,
  [`${prefix}-${++i}`] = i,
  [`${prefix}-${++i}`] = i,
};

console.log(obj); // { prop-1: 1, prop-2: 2, prop-3: 3 }
```

#### 10-9-3 메서드 축약 표현
* ES6에서는 메서드 정의시 function 키워드를 생략한 축약 표현을 사용 가능하다.
```js
const obj = {
  name: 'Lee',
  // 메서드 축약 표현
  sayHi() {
    console.log('Hi!' + this.name);
  }
};
```

