# chapter 48 : 모듈

## 모듈의 일반적 의미
- 모듈이란 애플리케이션을 구성하는 개별적 요소로서 재사용 가능한 코드 조각을 말한다. 일반적으로 모듈은 기능을 기준으로 파일 단위로 분리한다.   
- 이 때 모듈이 성립하려면 모듈은 자신만의 파일 스코프(모듈 스코프)를 가질 수 있어야 한다.  
- 모듈의 모든 자산은 캡슐화되어 다른 모듈에서 접근할 수 없다. 즉 개별적 존재로서 애플리케이션과 분리되어 존재한다.
- 하지만 쓰지 못하면 의미가 없으므로, **모듈은 공개가 필요한 자산에 한정하여 명시적으로 선택적 공개가 가능하다. 이를 export 라 한다.**
- 공개된 모듈의 자산은 다른 모듈에서 재사용할 수 있다. 모듈 자산을 사용하는 모듈을 모듈 사용자라 한다.
- **모듈 사용자는 모듈이 공개한 자산 중 일부 또는 전체를 선택해 자신의 스코프 내로 불러들여 재사용할 수 있다. 이를 import라 한다.**

## JS와 모듈
자바스크릅티를 클라이언트 사이드, 즉 브라우저 환경에 국한하지 않고 범용적으로 사용하려는 움직임이 생기면서 모듈 시스템은 반드시 해결해야 하는 핵심 과제가 되었다. 이런 상황에서 제안된 것이 CommonJS와 AMD다.  
JS 런타임 환경인 Node.js는 모듈 시스템의 사실상 표준인 CommonJS를 채택했고 독자적인 진화를 거쳐, 현재는 CommonJS 사양과 100% 동일하지는 않지만 기본적으로 CommonJS 사양을 따르고 있다.  
즉 Node.js는 ECMAScript 표준 사양은 아니지만 모듈 시스템을 지원한다. 따라서 Node.js 환경에서는 파일별로 독립적인 파일 스코프(모듈 스코프)를 갖는다.

## ES6 모듈(ESM)
- IE를 제외한 브라우저에서 ES6 모듈을 사용할 수 있다.  
- script 태그에 type="module" 어트리뷰트를 추가하면 로드된 js 파일은 모듈로서 동작한다.  
- ESM을 명확히 하기 위해 ESM의 파일 확장자는 mjs를 사용할 것을 권장한다.
```js
<script type="module" src="app.mjs"></script>
```

### 모듈 스코프
**ESM은 파일 자체의 독자적인 모듈 스코프를 제공한다. 따라서 모듈 내에서 var 키워드로 선언한 변수는 더는 전역 변수가 아니며 window 객체의 프로퍼티도 아니다.**

```js
// foo.mjs
// x 변수는 전역 변수가 아니며 window 객체의 프로퍼티도 아니다.
var x = 'foo';
console.log(x); // foo
console.log(window.x); // undeifined
```

### export 키워드
export 키워드는 선언문 앞에 사용한다. 이로써 변수, 함수, 클래스 등 모든 식별자를 export할 수 있다.

### import 키워드
다른 모듈에서 공개한 식벼자를 자신의 모듈 스코프 내부로 로드하려면 import 키워드를 사용한다.
```js
// app.mjs
// 같은 폴더 내의 lib.mjs 모듈이 export한 식별자 이름으로 import한다.
// ESM의 경우 파일 확장자를 생략할 수 없다.
// alias 가능
import { pi as PI, square, Person } from './lib.mjs';

console.log(PI);
```

```html
<!DOCTYPE html>
<html>
    <body>
        <script type="module" src="app.mjs"></script>
    </body>
</html>
```

위 예제의 app.mjs는 애플리케이션의 진입점이므로 반드시 script 태그로 로드해야 한다. 하지만 lib.mjs는 app.mjs의 import 문에 의해 로드되는 의존성이다. 따라서 lib.mjs는 script 태그로 로드하지 않아도 된다.   

1개만 export 할 때는 default를 사용할 수 있고, 이름 없이 하나의 값을 export한다. 이 경우 var, let, const 키워드는 사용할 수 없다.  
default 키워드와 함께 export한 모듈은 {} 없이 임의의 이름으로 import한다.

## Questions
- 모듈에 대해 설명해보시오.
- 모듈 스코프에 대해 설명해보시오. 