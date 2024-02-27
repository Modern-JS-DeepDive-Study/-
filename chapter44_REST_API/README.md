# chapter44 : REST API
REST(REpresentational State Transfer)는 HTTP/1.0과 1.1의 스펙 작성에 참여했고 아파치 HTTP 서버 프로젝트의 공동 설립자인 로이 필딩의 2000년 논문에서 처음 소개되었다.   
REST의 기본 원칙을 성실히 지킨 서비스 디자인을 "RESTful"이라고 표현한다.

즉, REST는 HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처고, REST API는 REST를 기반으로 서비스 API를 구현한 것을 의미한다.

## REST API의 구성
REST API는 자원, 행위, 표현의 3가지 요소로 구성된다. REST는 자체 표현 구조로 구성되어 REST API만으로 HTTP 요청의 내용을 이해할 수 있다.
| 구성 요소 | 내용 | 표현 방법|
|:-----:|:-------:|:---------:| 
| 자원 | 자원 | URI(엔드포인트) |
| 행위 | 자원에 대한 행위 | HTTP 요청 메서드 |
| 표현 | 자원에 대한 행위의 구체적 내용 | 페이로드 |

## REST API 설계 원칙
REST에서 가장 중요한 기본적인 원칙은 두 가지다. **URI는 리소스를 표현**하는 데 **행위에 대한 정의는 HTTP 요청 메서드**를 통해 하는 것이 RESTful API를 설계하는 중심 규칙이다.
### 1. URI는 리소스를 표현해야 한다.
리소스 식별 이름은 동사보다는 명사 사용
```js
# bad
GET /getTodos/1
Get /todos/show/1

# good
GET /todos/1
```
### 2. 리소스에 대한 행위는 HTTP 요청 메서드로 표현한다.
앞에서 말한 주로 5가지 요청 메서드(겟, 포스트, 풋, 패치, 딜리트 등)을 사용하여 CRUD를 구현  
리소스에 대한 행위는 HTTP 요청 메서드를 통해 표현하며 URI에 표현하지 않음. 리소스를 취득하는 경우에는 GET, 삭제하는 경우에는 DELETE. 리소스에 대한 행위를 명확히 표현
```js
# bad
GET /todos/delete/1

# good
DELETE /todos/1
```

## Questions
- REST API에 대해 설명해보시오.
- POST, PUT, PATCH의 차이를 설명해보시오.
