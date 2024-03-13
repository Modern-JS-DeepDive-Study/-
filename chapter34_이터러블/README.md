# chapter 34 : 이터러블

## 이터레이션 프로토콜

ES6에서 도입된 이터레이션 프로토콜은 순회 가능한 데이터 컬렉션(자료구조)을 만들기 위해 ECMAScript 사양에 정의하여 미리 약속한 규칙이다.

ES6 이전의 순회 가능한 데이터 컬렉션, 즉 배열, 문자열, 유사 배열 객체, DOM 컬렉션 등은 통일된 규약없이 각자 나름의 구조를 가지고 for 문, for ...in 문, forEach 메서드 등 다양한 방법으로 순회할 수 있었다.  
ES6에서는 순회 가능한 데이터 컬렉션을 이터레이션 프로토콜을 준수하는 이터러블로 통일하여 for...of 문, 스프레드 문법, 배열 디스트럭처링 할당의 대상으로 사용할 수 있도록 일원화했다.

- 이터러블 프로토콜 : Well-known Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다. 이러한 규약을 이터러블 프로토콜이라 하며, 이터러블 프로토콜을 준수한 객체를 이터러블이라 한다. **이터러블은 for...of 문으로 순회할 수 있으며 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.**
- 이터레이터 프로토콜 : 이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다. 이터레이터는 next 메서드를 소유하며 next 메서드를 호출하면 이터러블을 순회하며 **value와 done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환**한다. 이러한 규약을 이터레이터 프로토콜이라 하며, **이터레이터 프로토콜을 준수한 객체를 이터레이터라 한다.** 이터레이터는 이터러블의 요소를 탐색하기 위한 포인터 역할을 한다.

### 이터러블

이터러블 프로토콜을 준수한 객체를 이터러블이라 한다. 즉, 이터러블은 Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체를 말함.
Symbol.iterator 메서드를 직접구현하지 않거나 상속받지 않은 일반 객체는 for...of 문으로 순회할 수 없으며 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 없다.  
TC39 프로세스의 stage 4(Finished) 단계에서 스프레드 프로퍼티 제안은 일반 객체에 스프레드 문법의 사용을 허용한다.

### 이터레이터

- 이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환. **이터러블의 Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 갖는다.**
- next 메서드는 이터러블의 각 요소를 순회하기 위한 포인터의 역할을 함.
- 즉 next 메서드를 호출하면 이터러블을 순차적으로 한 단계씩 순회하며 순회 결과를 나타내는 이터레이터 리절트 객체를 반환

```js
const array = [1, 2, 3];

const iterator = aray[Symbol.iterator]();

// next 메서드를 호출하면 이터러블을 순회하며 순회 결과를 나타내는 이터레이터 리절트 객체를 반환
// 이터레이터 리절트 객체는 value와 done 프로퍼티를 갖는 객체
console.log(iterator.next()); // {value: 1, done: false}
console.log(iterator.next()); // {value: 2, done: false}
console.log(iterator.next()); // {value: 3, done: false}
console.log(iterator.next()); // {value: undefined, done: true}
```

done 프로퍼티는 이터러블의 순회 완료 여부를 나타냄

## 빌트인 이터러블

js는 이터레이션 프로토콜을 준수한 객체인 빌트인 이터러블을 제공  
다음의 표준 빌트인 객체들은 빌트인 이터러블이다.
| 빌트인 이터러블 | Symbol.iterator 메서드 |
|:-------:|:--------:|
| Array | Array.prototype[Symbol.iterator] |
| Map | Map.prototype[Symbol.iterator] |
| Set | Set.prototype[Symbol.iterator] |
| TypedArray | TypedArray.prototype[Symbol.iterator] |
| arguments| arguments[Symbol.iterator] |
| DOM 컬렉션 | NodeList.prototype[Symbol.iterator] |
| DOM 컬렉션 | HTMLCollection.prototype[Symbol.iterator] |

## for ... of 문

이터러블을 순회하면서 이터러블의 요소를 변수에 할당  
for ... of 문은 for ... in 문의 형식과 매우 유사  
for... in : 프로퍼티 어트리뷰트 [[Enumerable]] 값이 true 인 프로퍼티를 순회하며 열거 키가 심벌은 열거 x
for... of : 내부적으로 이터레이터의 next 메서드를 호출하여 이터러블을 순회하며 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티 값을 for...of 문의 변수에 할당.

## 이터러블과 유사 배열 객체

- length, for 문 순회, 배열처럼 인덱스로 프로퍼티 값에 접근가능
- 유사 배열 객체는 이터럽르이 아닌 일반 객체 => Symbol.iterator 메서드가 없기 때문에 for ... of문으로 순회 불가
- arguments, NodeList, HTMLCollection은 유사 배열 객체이면서 이터러블
- 모든 유사 배열 객체가 이터러블인 것은 아니지만 Array.from을 이용해 유사 배열 객체 or 이터러블을 인수로 전달받아 배열로 변환하여 반환할 수 있다.

## 이터레이션 프로토콜의 필요성

이터레이션 프로토콜은 데이터 소비자와 데이터 공급자를 연결하는 인터페이스 역할을 함

## 사용자 정의 이터러블

### 사용자 정의 이터러블 구현

피보나치 수열을 구현한 사용자 정의 이터러블 ex)

```js
const fibonacci = {
  [Symbol.iterator]() {
    let [pre, cur] = [0, 1];
    const max = 10; //수열 최대값

    // Symbol.iterator 메서드는 next 메서드를 소유한 이터레이터를 반환해야 하고
    // next  메서드는 이터레이터 리절트 객체를 반환해야 한다.

    return {
      next() {
        [pre, cur] = [cur, pre + cur];
        // 이터레이터 리절트 객체 반환
        return { value: cur, done: cur >= max };
      },
    };
  },
};

// 이터러블인 객체를 순회할 때마다 next 메서드가 호출
for (const num of fibonacci) {
  console.log(num); // 1 2 3 5 8
}
```

### 이터러블을 생성하는 함수

```js
const fibonacci = function (max) {
  let [pre, cur] = [0, 1];

  // Symbol.iterator 메서드는 next 메서드를 소유한 이터레이터를 반환

  return {
    [Symbol.iterator]() {
      return {
        next() {
          [pre, cur] = [cur, pre + cur];
          // 이터레이터 리절트 객체 반환
          return { value: cur, done: cur >= max };
        },
      };
    },
  };
};

// 수열 최대값을 인수로 전달하면서 호출
for (const num of fibonacci(10)) {
  console.log(num); // 1 2 3 5 8
}
```

### 무한 이터러블과 지연 평가

무한 수열 구현 ex)

```js
const fibonacciFunc = function () {
  let [pre, cur] = [0, 1];

  return {
    [Symbol.iterator]() {
      return this;
    },
    next() {
      [pre, cur] = [cur, pre + cur];
      return { value: cur };
    },
  };
};

for ( const num of fibonacciFunc()) {
    if (num > 10000) break;
    console.log(num);
}

// 배열 디스트럭처링 할당을 통해 무한 이러터블에서 3개의 요소만 취득한다.
const [f1, f2, f3] = fibonacciFunc();
console.log(f1, f2, f3); // 1 2 3
```
위 예제의 이터러블은 **지연 평가(lazy evaluation)** 를 통해 데이터를 생성  
이처럼 지연 평가를 사용하면 불필요한 데이터를 미리 생성하지 않고 필요한 데이터를 필요한 순간에 생성하므로 빠른 실행 속도를 기대할 수 있고 불필요한 메모리를 소비하지 않으며 무한도 표현할 수 있다는 장점이 있다.

## Questions
- 지연 평가에 대해 설명해보시요
- 이터러블과 이터레이터의 차이에 대해 설명해보시오.
