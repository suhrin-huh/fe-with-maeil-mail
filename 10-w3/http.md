# HTTP란 무엇인가요?

## HTTP(Hypertext Transfer Protocol)
HTTP(Hypertext Transfer Protocol)이란 웹에서 클라이언트(브라우저)와 서버가 데이터를 주고받을 때 사용하는 표준 통신 규약(Protocol)이다. 웹 페이지 열기, API 요청 보내기, 이미지 다운로드 등 웹에서 이루어지는 거의 모든 데이터 교환은 HTTP를 기반으로 이루어진다.

## 주요 특징
- 요청-응답(Request-Response) 방식 : 클리아언트가 서버에 요청(Request)을 보내고 서버가 응답(Response)을 반환하는 구조이다.

- 비연결성(Stateless) : HTTP는 상태를 기억하지 않기때문에, 한 요청이 끝나면 서버는 클라이언트의 상태를 유지하지 않는다. 필요하다면 ㅌ쿠키, 세션, 토큰 등을 이용하여 상태를 관리한다.

- 신뢰성 있는 데이터 전송 : HTTP는 TCP 프로토콜 위에서 동작하기 때문에 신뢰성 있는 데이터 전송 방식을 사용한다.

- 다양한 데이터 전송 가능 : HTML, JSON, XML, 이미지, 영상 등 다양한 포맷을 실어 보낼 수 있다.

- 고정된 요청/응답의 구조 : 요청과 응답은 URL, HTTP 메서드, 상태 코드, 헤더, 바디 등 정해진 구조를 가진다.

## HTTPS(Hypertext Transfer Protocol Secure)
HTTP 통신에 TLS/SSL 암호화계를 추가한 보안 버전이다.
데이터가 암호화를 통한 도청 방지, 서버의 정체성 인증을 통한 피싱 방지, 데이터 변조 방지 등의 장점으로 인해 로그인, 결제 등 개인정보가 오가는 서비스에서는 필수적으로 HTTPS를 사용한다.

## RESTful API

RESTful API는 HTTP를 활용하여 REST(Representational State Transfer) 설계 원칙을 따르는 API를 말한다.

### REST vs RESTful vs REST API

세 용어는 웹 API 설계 방식을 이야기할 때 자주 등장하지만, 의미가 다르다.

1. REST(Representational State Transfer)
REST는 웹 시스템을 효율적으로 설계하기 위한 원칙으로 다음과 같은 아키텍처 제약 조건을 의미한다.

- 클라이언트-서버 구조 : 클라이언트와 서버 간의 역할을 명확히 분리한다.
- 무상태성(Stateless) : 서버는 클라이언트의 상태를 저장하지 않으며, 각 요청은 독립적으로 처리한다.
- 캐시 가능(Cacheable) : 서버의 응답 시간을 개선하기 위해 리소스 캐싱을 지원한다.
- 계층화 구조(Layered System)
- 일관된 인터페이스(Uniform Interface) : 고유한 URI로 리소스를 식별하고 일관된 인터페이스를 통해 클라이언트와 서버가 간단하고 예측 가능하게 통신할 수 있게한다.


2. REST API
REST 원칙을 기반으로 만든 API를 의미한다.
REST의 제약을 완벽하게 지키지 않아도 REST 스타일로 만든 API면 REST API라고 부르는 경우가 많다.

`GET /users` : 사용자 목록 조회
`POST /users` : 사용자 생성
`DELETE .users/1` : 특정 사용자 삭제

3. RESTful API
REST API 중에서도 REST의 제약 조건을 보다 엄격하고 일관성 있게 지킨 API를 말한다.

`GET /users?role=admin` => 리소스 중심 + 필터링 기능

