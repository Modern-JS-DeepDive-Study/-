# chapter 16 : 프로퍼티 어트리뷰트

## 내부 슬롯과 내부 메서드  
내부 슬롯 : 의사 프로퍼티(pseudo property)  
내부 메서드 : 의사 메서드(pseudo method)  
ECMAScript 사양에 등장하는 이중 대괄로```[[...]]```로 감싼 이름들이 내부 슬롯과 내부 메서드다.    
내부 슬롯과 내부 메서드는 js 엔진 내부 로직으로 접근이나 호출 방법을 제공하지 않으나, 일부에 대해서는 간접적으로 제공한다.  
```[[Prototype]]```이라는 내부 슬롯은 __proto__를 통해 간접적 접근이 가능하다.

## 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체
JS는 프로퍼티 생성시 프로퍼티 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의  
**프로퍼티의 상태** : 프로퍼티의 값(value), 값의 갱신 간응 여부(writable), 열거 가능 여부(enumerable), 재정의 가능 여부(configurable)  
프로퍼티 어트리뷰트 : 내부 상태 값(meta-property)인  
```내부 슬롯 : [[Value]], [[Writable]], [[Enumerable]], [[Configurable]]```  
- Object.getOwnPropertyDescriptor(객체 참조, 프로퍼티 키(문자열) ) : 프로퍼티 어트리뷰트 간접확인 가능, PropertyDescriptor 객체 반환. 문제 시 undefined 반환

## 데이터 프로퍼티와 접근자 프로퍼티
데이터 프로퍼티, 접근자 프로퍼티가 있다.
### 데이터 프로퍼티
키와 값으로 구성된 일반적인 프로퍼티. 지금까지 본 건 모두 데이터 프로퍼티   
- [[Value]] : value - 프로퍼티 값으로 초기화, 프로퍼티 키 통해 프로퍼티 값 접근시 반환
- [[Writable]] : writeable - true 초기화
- [[Enumerable]] : enumerable - true 초기화
- [[Configurable]] : configurable - true 초기화
### 접근자 프로퍼티
자체적 값 없이 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수(accessor function)로 구성된 프로퍼티
- [[Get]] : get : 값 읽을 때 호출되는 접근자 함수
- [[Set]] : set : 값 저장시 호출되는 접근자 함수
- [[Enumerable]] : 데이터 프로퍼티와 같음
- [[Configurable]] : 데이터 프로퍼티와 같음