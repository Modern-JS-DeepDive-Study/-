# chapter 22 : this
## this 키워드
자신이 속한 객체를 가리키는 식별자를 참조해야 자신이 속한 객체의 프로퍼티 참조 가능    
this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수. this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드 참조 가능   
**this가 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 의해 동적 결정**

#### this 바인딩
this(키워드로 분류되지만 식별자 역할)와 this가 가리킬 객체를 바인딩하는 것

- 객체 리터럴 메서드 내부에서의 this는 메서드를 호출한 객체를 가리킴
- 생성자 함수 내부의 this는 생성자 함수가 생설할 인스턴스
- 자바나 C++ 같은 클래스 기반 언어에서 this는 언제나 클래스가 생성하는 인스턴스를 가리킴
- JS에서의 this는 함수 호출되는 방식에 따라 this에 바인딩될 값, this 바인딩이 동적 결정된다.
- strict mode(엄격 모드) 역시 this 바인딩에 영향을 준다. -> strict mode 일반 함수 내부 this는 undefined

## 함수 호출 방식과 this 바인딩
- 렉시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프 결정
- this 바인딩은 함수 호출 시점에 결정

함수 호출 방식은 다음과 같다.
- 1. 일반 함수 호출
2. 메서드 호출
3. 생성자 함수 호출
4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출

### 일반 함수 호출
this에는 전역 객체 바인딩 ( 중첩 함수, 콜백 함수 포함)  
화살표 함수 사용으로 this 바인딩 일치시키는 방법도 있음 (화살표 함수는 상위 스코프에 바인딩)

### 메서드 호출
소유한 객체가 아닌 메서드를 호출한 객체에 바인딩
```js
const person = {
    name: 'Lee',
    getName() {
        return this.name;
    }
}

console.log(person.getName()); /// Lee

const anotherPerson = {
    name: 'Kim'
};

anotherPerson.getName = person.getName;

console.log(anotherPerson.getName()); // Kim

const getName = person.getName;

console.log(getName()); // ''
```

### 생성자 함수 호출
객체를 생성하는 함수. 생성자 함수 내부의 this에는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩됨

### Function.prototype.apply/call/bind 메서드에 의한 간접 호출
- apply, call, bind 메서드는 function.prototype의 메서드  
- apply, call 메서드는 this로 사용할 객체와 인수 리스트를 인수로 전달받아 함수 호출
ex)
```js
Function.prototype.apply(thisArg[, argsArray])

Function.prototype.call(thisArg[, arg1[, arg2[, ...]]])
```

ex)
```js
function getThisBinding() {
    return this;
}

const thisArg = { a: 1 };

console.log(getThisBinding());

console.log(getThisBinding.apply(thisArg)); // {a : 1}
console.log(getThisBinding.call(thisArg)); // {a : 1}
```
- apply, call 둘다 기본적으로 함수 호출 (함수에 인수 전달하지는 않음) 호출 방식만 다를 뿐 동일 동작
- apply : 호출할 함수의 인수를 배열로 묶어 전달
- call : 호출할 함수의 인수를 쉼표로 구분한 리스트 형식 전달
- bind : 함수 호출 x, 첫 번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성 후 반환
- bind는 메서드의 this와 메서드 내부 중첩 함수 or 콜백 함수 this 불일치 문제 해결 때 사용

## Questions
- this란 무엇인가요?
- this 바인딩에 대해 설명해보세요