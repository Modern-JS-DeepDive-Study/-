# chapter46 : 제너레이터와 async/await

## 제너레이터란?
ES6에서 도입된 제너레이터는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수다. 제너리에터와 일반 함수의 차이는 다음과 같다.
- 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.
- 제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있다.
- 제너레이터 함술르 호출하면 제너레이터 객체를 반환한다.

## 제너레이터 함수의 정의
제너레이터 함수는 function* 키워드로 선언한다. 그리고 하나 이상의 yield 표현식을 포함한다. 이것을 제외하면 일반 함수를 정의하는 방법과 같다.
```js
// 제너레이터 함수 선언문
function* genDecFun() {
    yield 1;
}
// 제너레이터 함수 표현식
const genExpFunc = function* {
    yield 1;
};

// 제너레이터 메서드
const obj = {
    * genObjMethod() {
        yield 1;
    }
}

// 제너레이터 클래스 메서드
class MyClass {
    * genClsMethod() {
        yield 1;
    }
}
```
에스터리스크(*)의 위치는 function 키워드와 함수 이름 사이라면 어디든지 상관없다. 하지만 일관성 유지를 위해 fucntion 키워드 바로 뒤에 붙이는 것을 권장한다.  
제너레이터 화살표 함수 정의 x 이며 new 연산자와 함께 생성자 함수로 호출 할 수 없다.

## 제너레이터 객체
**제너레이터 함수를 호출하면 일반 함수처럼 함수 코드 블록을 실행하는 것이 아니라 제너레이터 객체를 생성해 반환한다. 제너레이터 함수가 반환한 제너레이터 객체는 이터러블이면서 동시에 이터레이터다.**  
다시 말해, 제너레이터 객체는 Symbol.iterator 메서드를 상속받는 이터러블이면서 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환하는 next 메서드를 소유하는 이터레이터다. 
```js
// 제너레이터 함수
function* genFunc() {
    yield 1;
    yield 2;
    yield 3;
}

//제너레이터 함수를 호출하면 제너레이터 객체를 반환
const generator = genFunc();
```
제너레이터 객체는 next 메서드를 갖는 이터레이터지만 이터레이터에는 없는 return, throw 메서드를 갖는다. 제너레이터 객체의 세 개의 메서들르 호출하면 다음과 같이 동작한다.
- next 메서드를 호출하면 제너레이터 함수의 yield 표현식까지 코드 블록을 실행하고 yield된 값을 value 프로퍼티 값으로, false를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.

## 제너레이터의 일시 중지와 재개
- 일반 함수는 호출 이후 제어구너을 함수가 독점하지만 제너레이터는 함수 호출자에게 제어권을 양도(yield)하여 필요한 시점에 함수 실행을 재개할 수 있다.  
- **yield 키워드는 제너레이터 함수의 실행을 일시 중지시키거나 yield 키워드 뒤에 오는 표현식의 평가 결과를 제너레이터 함수 호출자에게 반환한다.**
- **제너레이터 객체의 next 메서드를 호출하면 yield 표현식까지 실행되고 일시 중지(suspend)된다. 이때 함수의 제어권이 호출자로 양도된다.**
- 이 때 제너레이터 객체의 next 메서드는 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다.
- next 메서드가 반환한 이터레이러 리절트 객체의 value 프로퍼티에는 yield 표현식에서 yield된 값이 할당되고 done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었음을 나타내는 true 가 할당된다.
- 이터레이터 next 메서드와 달리 제너레이터 객체의 next 메서드에는 인수를 전달할 수 있다.
- 제너레이터 객체의 next 메서드에 전달한 인수는 제너레이터 함수의 yield 표현식을 할당받는 변수에 할당된다.
- 제너레이터를 활용하면 비동기 처리를 동기 처리처럼 구현할 수 있다.

## 제너레이터의 활용
### 이터러블의 구현
제너레이터 함수를 사용하면 이터레이션 프로토콜을 준수해 이터러블을 생성하는 방식보다 간단히 이터러블을 구현할 수 있다.
```js
// ex)
// 무한 이터러블 생성 함수
const infiniteFibonacci = ( function () {
    let [pre, cur] = [0, 1];

    return {
        [Symbol.iterator]() { reteurn this; },
        next() {
            [pre, cur] = [cur, pre + cur]; 

            return { value : cur}
        }
    };
}());

// infiniteFibonacci는 무한 이터러블이다.
for (const num of infiniteFibonacci) {
    if (num > 10000) break;
    console.log(num);
}
```
```js
// 제너레이터 사용 ex)
// 무한 이터러블 생성 제너레이터 함수
const infiniteFibonacci = (function* () {
    let [pre, cur] = [0, 1];

    while(true) {
        [pre, cur] = [cur, pre + cur];
        yield cur;
    }
}());

for (const num of infiniteFibonacci) {
    if (num > 10000) break;
    console.log(num);
}
```

### 비동기 처리
이런 특성을 활용하면 프로미스를 사용한 비동기 처리를 동기처럼 구현할 수 있다.

```js
const fetch = require('node-fetch');

const async = generatorFunc => {
    const generator = generatorFunc();
    
    const onResolved = arg => {
        const result = generator.next(arg);

        return result.done
            ? result.value : result.value.then(res => onResolved(res));
    };
    return onResolved;
};

(async(function* fetchTodo() {
    const url = 'https://~';
    
    const response = yield fetch(url);
    const todo = uield response.json();
    console.log(tood);
    // {userId : 1, id : 1, title : 'delectus aut autem', completed: false}
})());
```

혹시 제너레이터 실행기가 필요하다면 직접 구현하는 것보다 co 라이브러리를 사용하도록 하자.

## async/await
제너레이터보다 간단하고 가독성 좋게 비동기 처리를 동기 처리처럼 동작하도록 구현할 수 있는 async/awiat가 도입되었다.  
이를 이용하면 프로미스의 후속 처리 메서드 없이 마치 동기 처리처럼 프로미스가 처리 결과를 반환하도록 구현할 수 있다. 

### async 함수
await 함수는 반드시 async 함수 내부에서 사용해야 한다. async 함수는 async 키워드를 사용해 정의하며 언제나 프로미스를 반환한다. 

### await 키워드
- await 키워드는 프로미스가 settled 상태가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환한다. 
- await 키워드는 반드시 프로미스 앞에서 사용해야 함  
- 비동기 처리가 완료될 때까지 순차적으로 기다릴 필요가 없음 -> 1개의 await + Promise.all등을 사용
- 비동기 처리가 완료될 때까지 순차적으로 기다려야 함 -> 여러 개의 await으로 순차적 처리

### 에러 처리
- 비동기 함수의 콜백 함수를 호출한 것은 비동기 함수가 아니기 때문에 try...catch 문을 사용해 에러 캐치를 할 수 없다.
- async/await 에러 처리는 try...catch 사용가능. 프로미스 반환 비동기 함수는 명시적으로 호출할 수 있기 때문
- **async 함수 내에서 catch 문을 사용해서 에러 처리를 하지 않으면 async 함수는 발생한 에러를 reject하는 프로미스를 반환한다. 따라서 async 함수를 호출하고 Promise.prototype.catch 후속 처리 메서드를 사용해 에러를 캐치할 수도 있다.
```js
const fetch = require('node-fetch');

const foo = async () => {
    const wrongUrl = "https://~";

    const response = await fetch(wrongUrl);
    const data = await response.json();
    return data;
};

foo().then(console.log).catch(console.error); // TypeError: Failed to fetch
```

## Questions
- 제너레이터의 특징에 대해 설명해보시오.
- async/await의 특징에 대해 설명해보시오.
- async/await을 통한 비동기 처리와 fetch를 통한 비동기 처리에 대해 설명해보시오.