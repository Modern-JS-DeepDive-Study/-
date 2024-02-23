# # chapter 26 : ES6 함수의 추가 기능

## 함수의 구분
ES6 이전까지 JS의 함수는 별다른 구분 없이 사용했음.  
JS 함수는 
- 일반적인 함수로 호출
- new 연산자와 함께 호출하여 인스턴스 생성할 수 있는 생성자 함수로서 호출
- 객체 바인딩되어 메서드로서 호출
- 객체 바인딩되어 메서드로서 호출

용도가 많아 실수 유발 및 성능 면에서도 손해이다.  
이러한 문제를 해결하기 위해 ES6에서는 함수를 사용 먹적에 따라 세 가지 종류로 명확히 구분했다.  
|ES6함수 의 구분 | CONSTRUCTOR | prototype | super | arugments |
|--------|:----------:|:--------:|:----------:|:-------:|
|일반 함수(normal) | O | O | X | O |
|메서드(Method) | X | X | O | O |
|화살표 함수(normal) | X | X | X | X |


일반 함수는 constructor 이지만 ES6의 메서드와 화살표 함수는 non-constructor다. 

## 메서드
ES6 사양에서 메서드는 메서드 축약 표현으로 정의된 함수만을 의미한다.
```js
const obj = {
    x: 1,
    // foo는 메서드다.
    foo() { return this.x; },
    // bar에 바인딩된 함수는 메서드가 아닌 일반 함수다.
    bar: function() { return this.x; }
};

console.log(obj.foo()); // 1
console.log(ojb.foo()); // 1
```
ES6 사양에서 정의한 메서드는 인스턴스를 생성할 수 없는 non-constructor이다. 따라서 생성자 함수로서 호출이 불가하다.  
ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 [[HomeObject]]를 갖는다. super 참조는 내부 슬롯 [[HomeObejct]]를 사용하여 수퍼클래스의 메서드를 참조하므로 내부 슬롯 [[HomeObject]]를 갖는 ES6 메서드는 super 키워드를 사용할 수 있다.

## 화살표 함수
화살표 함수(arrow function)은 function 키워드 대신 화살표(=>, fat arrow)를 사용하여 기존의 함수 정의 방식보다 간략하게 함수를 정의할 수 있다.  
특히 화살표 함수는 콜백 함수 내부에서 this가 전역 객체를 가리키는 문제를 해결하기 위한 대안으로 유용하다.

### 화살표 함수 정의
#### 함수 정의
함수 선언문 x, 함수 표현식만 가능
```js
const multiply = (x, y) => x * y;
multiply(2, 3); // -> 6
```

#### 매개변수 선언
매개변수가 여러 개인 경우 소괄호 ()안에 매개변수를 선언
```js
const arrow = (x, y) => { ... };
// 1개인 경우 소괄호 생략 가능
const arrow = x => { ... };
// 매개변수가 없는 경우 소괄호 () 생략 x
```
#### 함수 몸체 정의
하나의 문으로 구성된다면 함수 몸체를 감싸는 중괄호 {}를 생략할 수 있다.  
함수 몸체를 감싸는 중괄호 {}를 생략한 경우 함수 몸체 내부의 문이 표현식이 아닌 문이라면 에러가 발생한다. 표현식이 아닌 문은 반환할 수 없기 때문  
```js
const arrow = () => const x =1; // SyntaxError: Unexpected toekn 'const'

//위 표현은 다음과 같이 해석됨
const arrow = () => { return const x = 1;};
```