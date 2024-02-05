# chapter 20 : strict mode 
## strict mode란?
ES5 부터 strict mode 도입  
자바스크립트 엔진 최적화 작업에 문제 일으킬 코드에 명시적 에러 발생 (ESLint 같은 린트 도구도 유사 효과 가능)

## strict mode의 적용
전역의 선두 또는 함수 몸체의 선두에 'use strict' 사용 시 strict 모드 적용된다.

## 전역 strict mode 적용은 피하자.
외부 서드파티 라이브러리 경우 라이브러리가 non-strict mode인 경우도 있기 때문에 전역에 strict mode 적용은 바람직하지 않다.

## 함수 단위 strict mode도 피하자.
즉시 실행 함수로 감싼 스크립트 단위 적용이 바람직하다.

## strict mode시 발생시키는 에러

### 암묵적 전역
선언 x 변수 참조시 ReferenceError 발생

### 변수, 함수, 매개변수의 삭제
delete 연산자로 변수, 함수, 매개변수 삭제시 SyntaxError 발생

### 매개변수 이름의 중복
중복 매개변수 시 SyntaxError 발생

### with 문의 사용
with 문 사용시 SyntaxError 발생

## strict mode 적용에 의한 변화
### 일반 함수의 this
strict mode에서 함수를 일반 함수로 호출 시 this에 undefined가 바인딩 된다. 이 때 에러 발생은 안한다.

### arguments 객체
strict mode에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.

## Questions 
일반 모드와 strict 모드의 차이에 대해서 말해보시오.