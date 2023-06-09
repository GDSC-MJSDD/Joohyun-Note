## 23.1 소스코드의 타입

---

<aside>

💡 실행 컨텍스트란, **자바스크립트의 동작 원리**를 담고 있는 핵심 개념이다.

</aside>

ECMAScript는 소스코드를 4가지의 타입으로 구분한다.

- **전역 코드(global code)**
  - 전역에 존재하는 소스코드
  - 전역 객체와 연결하기 위해 전역 실행 컨텍스트가 생성
- **함수 코드(function code)**
  - 함수 내부에 존재하는 소스코드
  - 스코프 체인의 일원으로 연결하기 위해 함수 실행 컨텍스트가 생성
- **eval 코드(eval code)**
  - eval 함수에 인수로 전달되어 실행되는 소스코드
  - 자신만의 독자적인 스코프를 생성하기 위해 실행 컨텍스트 생성
- **모듈 코드(module code)**
  - 모듈 내부에 존재하는 소스코드
  - 모듈별로 독자적인 실행 컨텍스트가 생성

<br>

소스코드를 구분하는 이유는?

> 💡 소스코드의 타입에 따라 실행 컨텍스트를 생성하는 과정과 내용이 다르기 때문!

<br>

## 23.2 소스코드의 평가와 실행

---

<aside>

💡 자바스크립트는 코드를 ‘**소스코드의 평가**’, ‘**소스코드의 실행’**으로 나누어 처리한다.

</aside>

다음 코드를 통해 실행 과정에 대해 살펴보면,

```jsx
var x;
x = 1;
```

- 소스코드의 평가
  - 변수 선언문인 `var x;` 를 먼저 실행
  - 변수 식별자인 `x` 는 실행 컨텍스트가 관리하는 스코프에 등록되고 undefined로 초기화
- 소스코드의 실행
  - 변수 선언문 `var x;` 는 이미 소스코드 평가 단계에서 실행이 완료
  - 변수 할당문인 `x = 1;` 만 실행
  - `x` 가 선언된 변수인지부터 확인해야 한다. → 실행 컨텍스트에 등록되어 있는지 확인
  - 등록이 잘 되어 있다면 값을 할당하고 결과값을 다시 실행 컨텍스트에 등록

<br>

## 23.3 실행 컨텍스트의 역할

---

<aside>

💡 실행 컨텍스트는 코드 실행을 위해 **스코프, 식별자, 코드 실행 순서를 모두 관리**한다.

</aside>

실행 컨텍스트는,

1. 소스코드를 실행하는데 **필요한 환경을 제공**하고 **코드 실행 결과를 관리**하는 영역
2. 자바스크립트의 모든 코드는 실행 컨텍스트를 통해 실행되고 관리된다.
3. **렉시컬 환경**에서
   1. 식별자와 스코프를 관리
4. **실행 컨텍스트 스택**으로
   1. 코드의 실행 순서를 관리

<br>

## 23.4 실행 컨텍스트 스택

---

<aside>

💡 실행 컨텍스트 스택은 **코드의 실행 순서를 관리**하며 **스택(stack) 자료구조로 관리**한다.

</aside>

다음 코드를 살펴보자

```jsx
const x = 1;

function foo() {
	const y = 2;

	function bar{
		const z = 3;
		consol.log(x + y + z);
	}
	bar();
}

foo();

// output
6
```

이 코드를 실행했을 때 생성되는 실행 컨텍스트에는

1. **전역 실행 컨텍스트**
   - 전역 변수 x와 전역 함수 foo()가 등록
2. **foo() 함수 실행 컨텍스트**
   - foo() 함수 내부의 지역 변수 y와 중첩 함수 bar()가 등록
3. **bar() 함수 실행 컨텍스트**
   - bar() 함수 내부의 지역 변수 z가 등록

의 순서대로 추가(push)되고, 삭제(pop)된다.

<br>

> 실행 컨텍스트 스택의 최상위에 존재하는 실행 컨텍스트는 **항상 현재 실행 중인 코드의 실행 컨텍스트**다.

<br>

## 23.5 렉시컬 환경

---

<aside>

💡 렉시컬 환경은 스코프와 식별자를 관리, 기록하는 자료구조로 **실행 컨텍스트를 구성하는 컴포넌트**다.

</aside>

렉시컬 환경은 두 개의 컴포넌트로 구성된다.

1. **환경 레코드**
   - 스코프에 포함된 식별자를 등록
   - 식별자에 바인딩된 값을 관리하는 저장소
2. **외부 렉시컬 환경에 대한 참조**
   - 상위 스코프를 가리킨다.
   - 상위 스코프란, 현재 실행 컨텍스트를 포함하는 상위 코드의 렉시컬 환경!
   - 단방향 링크드 리스트인 스코프 체인을 구현
