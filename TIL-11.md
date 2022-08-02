# 🦄TIL-11🦄

## 1. REST API
> REpresentational State Transfer 의 약자로 2000년도에 Roy Fielding 박사 논문에서 최초로 소개되었다. 로이 필딩은 HTTP 주요 저자 중 한 사람으로 웹의 장점을 최대한 활용할 수 있는 아키텍처로써 REST를 발표했다고 한다.

## 2. HTTP Method
> HTTP Method는 GET / POST / PUT / DELETE가 대표적이며 CRUD에서 주로 사용한다. CRUD는 Create / Read / Update / Delete를 묶어서 부르는 말이다.
- GET : 조회 (Read)
- POST : 생성 (Create)
- PUT : 수정 (Update)
- DELETE : 삭제 (Delete)

## 3. REST 특징
- Uniform Interface
  - URI로 지정한 리소스에 대한 조작을 통일되고 한정적인 인터페이스로 수행한다.
- Stateless
  - 무상태성, 즉 작업을 위한 상태 정보를 따로 저장하고 관리하지 않는다.
  - 세션이나 쿠키 정보를 별도로 저장하고 관리하지 않으므로 서버는 들어오는 요청만을 처리한다.
- Cacheable
  - 캐시 사용을 통해 응답시간이 빨라지고 서버의 자원 이용률을 향상시킬 수 있다.
- Self-descriptiveness
  - REST API 메시지만 보고도 이를 쉽게 이해할 수 있도록 자체 표현 구조로 되어 있다.
- Client-Server
  - 자원 있는 쪽이 server, 자원을 요청하는 쪽이 client가 된다.
  - 각각의 역할이 확실히 구분되어 의존성이 줄어든다.
- Layered System
  - REST 서버는 다중 계층으로 구성될 수 있으며 프록시나 게이트웨이처럼 네트워크 기반의 중간매체를 사용할 수 있게 한다

## 4.REST API 설계 목표
REST API 설계 시 가장 중요한 2가지 항목은 아래와 같다.
> URI는 정보의 자원을 표현해야 한다.
> 자원에 대한 행위는 HTTP Method로 표현한다.

## 5. REST API 설계 규칙
### Rule 1. 슬래시 구분자(/)는 계층 관계를 나타내는 데 사용한다.
`/blog/sangminlog`
### Rule 2. URI 마지막 문자로 슬래시 구분자(/)를 사용하지 않는다.
`/blog/sangminlog/`  (X)
`/blog/sangminlog`   (O)
### Rule 3. 가독성을 높이기 위해서는 _(언더바)가 아닌 -(하이픈)을 사용한다.
`/blog/sangminlog_logo`  (X)
`/blog/sangminlog-logo`  (O)
### Rule 4. URI 경로에는 소문자가 적절하다.
`/Blog/Sangminlog`    (X)
`/blog/sanmginlog`    (O)
### Rule 5. 파일 확장자는 URI에 포함하지 않는다.
> accept header를 사용한다.



`http://example.com/blog/sangminlog/logo.jpg`  (X)
> GET /blog/sangminlog/logo
`HTTP/1.1 Host: example.com Accept: image/jpg` (O)
### Rule 6. 기본적으로 명사를 사용하지만, 컨트롤 자원을 의미하는 경우 동사를 허용한다.
`/blog/sangminlog/duplicating`  (X)



`/blog/sangminlog/duplicate `   (O)
### Rule 7. 리소스 간 연관 관계가 있는 경우 아래와 같이 처리한다.
> /리소스명/리소스ID/관계가 있는 다른 리소스명

`/users/{userid}/devices`   (일반적으로 소유 'has'의 관계를 표현할 때)
