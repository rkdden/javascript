# 모던 자바스크립트 Deep Dive
* 자바스크립트를 더 깊게 공부하며 더 활용하자.

## 목차
* [1장 프로그래밍](#1장-프로그래밍)
* [2장 자바스크립트란?](#2장-자바스크립트란?)
* [3장 자바스크립트 개발 환경과 실행 방법](#3장-자바스크립트-개발-환경과-실행-방법)
* [4장 변수](#4장-변수)
* [5장 표현식과 문](#5장-표현식과-문)

## 1장 프로그래밍

### 1-1 프로그래밍이란?
* 컴퓨터에게 실행을 요구하는 일종의 커뮤니케이션이다.
  * 프로그래밍에 앞서 해결해야 할 문제를 명확히 이해한 후 적절한 문제 해결 방안을 정의해야 한다.
  * 이때 문제 해결 능력이 요구 된다.
  * 문제 해결 능력은 복잡함을 단순하게 분해하고 자료를 정리하고 구분해서 순서에 맞게 배열해야한다.

* 결국 프로그래밍이란?
  * 기계가 실행할 수 있을 정도로 정확하고 상세하게 요구사항을 설명하는 작업이며, 그 결과물은 코드이다.

### 1-2 프로그래밍 언어
* 문제해결 방안은 컴퓨터에게 전달되어야 한다.
  * 이때 기계어로 명령을 전달해야 한다.
* 기계어로 명령을 전달하는 대신할 대안은 문법으로 구성된 프로그래밍 언어를 사용해 프로그램을 작성한 후, 기계어로 변환하는 번역기를 이용하는 것이다.
  * 이러한 번역기를 컴파일러 혹은 인터프리터라고 한다.

* 프로그래밍은 프로그래밍 언어를 사용해 컴퓨터에게 실행을 요구하는 커뮤니케이션이다.
* 프로그래밍 언어는 구문과 의미의 조합으로 표현된다.

### 1-3 구문과 의미
* 아래 예제를 보자
```js
const number = 'string';
console.log(number * number); // NaN (Not a Number)
```
* 이 예제는 문법적으로는 문제가 없지만 의미적으로는 옳지 않다.
* 결국 코드는 해결 방안의 구체적 구현물이기 때문에, 문법에 부합하는 것은 물론이고 수행하고 싶은 것을 정확히 수행해야 한다.
* 즉 요구사항이 실현되어야 한다.

* 결국 프로그래밍은 요구사항의 집합을 분석해서 적절한 자료구조와 함수의 집합으로 변환한 후, 그 흐름을 제어하는 것이다.

## 2장 자바스크립트란?

### 2-1 자바스크립트의 탄생
* 1995년 넷스케이프 커뮤니케이션즈는 브라우저에서 동작하는 경량 프로그래밍 언어를 도입하기로 결정한다.
  * 이 때 탄생한 것이 브렌던 아키그가 개발한 자바스크립트이다.
* 자바스크립트는 1996년 3월 넷스케이프 내비게이터2에 탑재되었으며 모카로 명명 되었다.
  * 그해 9월 라이브스크립트로 이름이 바뀌었다가 12월에 드디어 자바스크립트라는 이름을 갖는다.
* 자바스크립트가 탄생한 뒤 얼마 지나지 않아 자바스크립트의 파생 버전인 JScript가 출시되어 자바스크립트는 위기를 맞는다.

### 2-2 자바스크립트의 표준화
* 1996년 8월, 마이크로소프트가 JScript를 IE 3.0에 탑재했다.
  * 이 때 JScript와 자바스크립트가 표준화되지 못하고 적당히 호환되었다.
  * 즉, 넷스케이프와 MS가 자사 브라우저의 점유율을 높이기 위해 자사 브라우저에서만 동작하는 기능을 추가했다.
* 이로 인하여 브라우저에 따라 웹페이지가 정상적으로 동작하지 않는 크로스 브라우징 이슈가 발생했다.
  * 결과적으로 모든 브라우저에서 정상적으로 동작하는 웹페이지를 개발하기가 무척 어려워졌다.
* 이에 자바스크립트의 파편화를 방지하고 표준화된 자바스크립트의 필요성이 대두되었다.
* 1996년 11월 넷스케이프에서 컴퓨터 시스템의 표준을 관리하는 ECMA 인터네셔널에 자바스크립트 표준화를 요청했다.
* 1997년 7월 표준화된 자바스크립트 초판이 나왔고 상표권 문제로 자바스크립트는 ECMAScript로 명명 되었다.

### 2-3 자바스크립트 성장과 역사
* 초창기 자바스크립트는 웹페이지의 보조적인 기능을 수행하기 위해 한정적인 용도로 사용되었다.
* 이 시기의 대부분 로직은 웹 서버에서 실행되었고, 브라우저는 전달받은 HTML과 CSS를 렌더링 하는 수준이었다.

#### 렌더링
```
렌더링이랑 HTML, CSS, 자바스크립트로 작성된 문서를 해석해서 브라우저에 시각적으로 출력하는 것을 말한다.
때로는 서버에서 데이터를 HTML로 변환해서 브라우저에 전달하는 과정인 SSR을 가리키기도 한다.
```

#### 2-3-1 Ajax
* 1999년 자바스크립트를 사용해 서버와 브라우저가 비동기 방식으로 데이터를 교환할 수 있는 통신 기능인 Ajax가 XMLHttpRequest라는 이름으로 등장했다.
* 이전의 웹페이지는 완전한 HTML 코드를 서버로부터 전송받아 웹페이지 전체를 렌더링하는 방식으로 동작했다. 따라서 화면이 전환되면 처음부터 렌더링했다.
* 이러한 방식은 변경할 필요가 없는 것까지 변경해서 성능이 떨어졌다.
* Ajax의 등장으로 서버로부터 필요한 데이터만 받아 변경이 필요한 부분만 렌더링하는 방식이 가능해졌다.

#### 2-3-2 jQuery
* 2006년 jQuery의 등장으로 DOM을 더욱 쉽게 제어할 수 있게 되며, 크로스 브라우징 이슈도 어느정도 해결되었다.
* 사용자 층이 순식간에 늘어났으며, 더 쉬운 jQuery를 더 선호하는 개발자가 양산되기도 했다.

#### 2-3-3 V8 자바스크립트 엔진
* 자바스크립트의 가능성이 확인되고 빠르게 동작하는 자바스크립트 엔진의 필요성이 대두되었다.
* 2008년 구글이 보여준 V8 자바스크립트 엔진은 빠른 성능을 보여주었다.
* V8 엔진의 등장으로 자바스크립트는 데스크탑 애플리케이션과 유사한 사용자 경험(UX)를 제공할 수 있는 웹 애플리케이션 프로그래밍 언어로 정착하게 되었다.
* V8 자바스크립트 엔진을 통한 자바스크립트의 발전으로 과거의 로직들이 대거 클라이언트로 이동했다.
* 이는 현재 프론트엔드 영역이 주목받는 계기로 작동했다.

#### 2-3-4 Node.js
* 2009년 라이언 달이 발표한 Node.js는 구글 V8 엔진으로 빌드된 자바스크립트 런타임 환경이다.
* Node.js는 브라우저의 엔진에서만 동작하던 자바스크립트를 브라우저 이외의 환경에서도 동작할 수 있도록 엔진을 독립시킨 js 실행 환경이다.
* Node.js는 다양한 플랫폼에 적용할 수 있지만, 주로 서버 사이드 애플리케이션 개발에 사용되며 내장 API를 제공한다.
* Node.js는 비동기 I/O를 지원하며 단일 스레드 이벤트 루프 기반으로 동작함으로써 요청 처리 성능이 좋다.
* SPA에 적합하다. 하지만 CPU 사용률이 높은 애플리케이션에는 권장하지 않는다.
* Node.js의 등장으로 js는 브라우저, 서버 사이드 개발에서도 사용할 수 있게 되었다.
* 프론트, 백 모두 사용이 가능하며, 웹 프로그래밍 언어의 표준으로 자리 잡고있다.
* js는 크로스 플랫폼을 위한 가장 중요한 언어로 주목받고 있다.
* js는 세계에서 가장 인기 있는 언어이다.

#### 2-3-5 SPA 프레임워크
* 이전의 개발 방식으로는 복잡해진 개발 과정을 수행하기 어려워졌다.
* CBD 방법론을 기반으로 하는 SPA가 대중화되었다.
* Angular, React, Vue.js, Svelte 등 다양한 프레임워크가 등장했다.

### 2-4 자바스크립트와 ECMAScript
* ECMAScript는 js의 표준 사양인 ECMA-262를 말하며, 프로그래밍 언어의 값, 타입, 객체와 프로퍼티, 함수, 표준 빌트인 객체 등 핵심 문법을 규정한다.
* 각 브라우저 제조사는 ECMAScript 사양을 준수해서 브라우저 내장 js 엔진을 구현한다.
* js는 일반적으로 ECMAScript가 뼈대를 이루며 브라우저가 별도 지원하는 클라이언트 사이드 Web API 등을 아우르는 개념이다.

### 2-5 자바스크립트의 특징
* js는 웹을 구성하는 요소중 하나이며 웹 브라우저에서 동작하는 유일한 프로그래밍 언어이다.
* 셀프에서는 프로토타입 기반 상속을, 스킴에서는 일급함수의 개념을 차용했다.
* 자바스크립트는 개발자가 별도의 컴파일 작업을 수행하지 않는 인터프리터 언어이다.
* 인터프리터는 소스코드를 즉시 실행하고 컴파일러는 빠르게 동작하는 머신 코드를 생성하고 최적화 한다.
* js는 명령형, 함수형, 프로토타입 기반 객체지향 프로그래밍을 지원하는 멀티 패러다임 프로그래밍 언어이다.
* js는 프로토타입 기반의 객체지향 언어이다.

### 2-6 ES6 브라우저 지원 현황
* IE를 제외한 대부분의 모던 브라우저는 ES6를 지원한다.
* Node.js는 v4부터 지원하기 시작했다.
* 브라우저에서 아직 지원하지 않는 최신 기능이나 구형 브라우저 같은 경우 바벨과 같은 트랜스파일러를 사용해 ES6이상의 사양으로 구현한 코드를 ES5 이하로 다운그레이드할 수 있다.

## 3장 자바스크립트 개발 환경과 실행 방법
### 3-1 자바스크립트 실행 환경
* 모든 브라우저와 Node.js는 자바스크립트 엔진을 내장하고 있다.
* 그러므로 브라우저에서 동작하는 코드는 Node에서도 동일하게 동작한다.
* 하지만 둘의 용도는 다르다.
* 그러므로 js 이외의 추가로 제공하는 각각의 기능은 호환되지 않는다.
* 예를 들어, 브라우저는 DOM API를 기본적으로 제공한다. 하지만 Node.js는 DOM API를 제공하지 않는다.
* 반대로 Node에서는 파일 시스템을 기본 제공하지만 브라우저는 이를 지원하지 않는다.
* 브라우저는 클라이언트 사이드 Web API를 지원하고, Node는 js와 Node 고유의 API를 지원한다.
![이미지](./images/03/1.png)

### 3-2 웹 브라우저
* 앞으로의 예제에서는 구글 크롬을 사용한다.

#### 3-2-1 개발자 도구
* 크롬 브라우저가 제공하는 개발자 도구는 강력한 도구이다.
* 개발자 도구는 단축키로 열 수 있다.
```
윈도우: F12 또는 Ctrl + Shift + i
macOS: command + option + i
```
![alt](./images/03/2.PNG)
```
패널              설명
Elements: 로딩된 페이지의 DOM과 CSS를 편집해서 렌더링된 뷰를 확인할 수 있다.
Console: 로딩된 페이지의 에러를 확인하거나 js 소스코드의 console.log 메서드의 실행결과를 볼 수 있다.
Sources: 로딩된 페이지의 js 코드를 디버깅 할 수 있다.
Network: 로딩된 페이지의 관련된 request 정보와 성능을 확인할 수 있다.
Application: 웹 스토리지, 세션, 쿠키를 확인 및 관리할 수 있다.
```
#### 3-2-2 콘솔
* 콘솔은 js 코드에서 에러가 났을때 가장 우선적으로 보아야한다.
* console.log 메서드를 사용하는 경우도 봐야한다.
* 콘솔은 js 코드를 직접 입력해 결과를 확인하는 REPL(Read Eval Print Loop: 입력 수행 출력 반복) 환경으로 사용도 가능하다.
* js 코드 실행중 에러가 발생하면 에러의 내용이 콘솔에 출력된다.

#### 3-2-3 브라우저에서 자바스크립트 실행
* 브라우저는 HTML 파일을 로드하면 script 태그에 포함된 js 코드를 실행한다.
* [예제](./03/03-01.html)
* '+' 또는 '-' 버튼을 클릭하면 에러가 발생한다.
* 개발자 도구 콘솔로 확인이 가능하다.

#### 3-2-4 디버깅
* [](./images/03/3.PNG)
* 에러가 난 곳의 링크를 클릭하면 디버깅할 수 있는 Sources 패널로 이동한다.
* 에러가 발생한 곳의 빨간 밑줄이 표시되며, 에러 정보가 표시된다.
* 에러가 난곳을 찾고 바꿔보자.
  * 13번째 줄의 counter-x를 counter로 바꾸면 된다.

### 3-3 Node.js
* 규모가 큰 웹 어플리케이션을 개발할 때 React, Angular, Lodash같은 프레임워크와 라이브러리를 사용한다.
* 이때 Node.js와 npm이 필요하다.

#### 3-3-1 Node.js와 npm 소개
* Node.js는 크롬 V8 js 엔진으로 빌드된 js 런타임 환경이다.
* **npm**은 js 패키지 매니저다.
  * Node.js에서 사용할 수 있는 모듈들을 패키지화해서 모아둔 저장소 역할과 패키지 설치 및 관리를 위한 CLI를 제공한다.

#### 3-3-2 Node.js 설치
* <https://github.com/12tndbs12/node.js/tree/master/ch01#4-1-%EB%85%B8%EB%93%9C-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0/> 를 참고하자

#### 3-3-3 Node.js REPL
* Node.js가 제공하는 REPL을 사용하면 간단한 js 코드를 실행하고 결과를 확인할 수 있다.
* 터미널에서 아래와 같은 명령어 실행
```
$ node
```
* 프롬프트가 >로 변경되면 js 코드 입력
* js 파일을 실행하려면 node 명령어 뒤에 파일 이름 입력
```
$ node index.js
```
* Ctrl + C 키를 두번 입력하면 종료

### 3-4 비주얼 스튜디오 코드
#### 3-4-1 비주얼 스튜디오 코드 설치
* <https://code.visualstudio.com/> 에 접속 후 설치
#### 3-4-2 내장 터미널
* VS Code 실행후, js 파일을 생성하자.
```js
// index.js
const arr = [1,2,3];

arr.foreach(console.log);
```
* ctrl + ` 키로 터미널을 열 수 있다.
```
> node index.js
```
#### 3-4-3 Code Runner 확장 플러그인
* VS Code 마켓플레이스에서 다양한 확장 플러그인을 다운 가능하다.
* 클라이언트 사이드 Web API 는 Node.js 환경에서 실행할 수 없다.

#### 3-4-4 Live Server 확장 플러그인
* Web API가 포함된 js 코드를 실행하려면 브라우저에서 실행해야 한다.
* 이를 위해 Live Server라는 플러그인을 설치해주자.

## 4장 변수

## 5장 표현식과 문
