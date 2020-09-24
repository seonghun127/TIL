

# 06 | Interaction With Server



## Client

- 서버로부터 데이터의 응답을 받아서 보여줌

  _+)  __Interface ?__ 사물 간 또는 사물과 인간 간의 의사소통이 가능하도록 만들어진, 물리적, 가상적 매개체(접점)을 의미_

### Browser

- 우리가 작성한 코드(Html, css, js)가 컴퓨터가 알아들을 수 있게해줌.
- 브라우저에 내부 엔진이 존재함(ex. v8)



## Server

- 자원을 제공(serve)하는 주체!
- 클라이언트가 리소스(자원) 요청을 보내면 서버는 DB에서 해당 자원을 꺼내와 클라이언트에게 응답함

## API

- 프로그래밍 되어있는 애플리케이션과 의사소통 가능한 매개체

- 클라이언트가 서버가 어떻게 구성되어있는지 모르고,  DB에 어떤 리소스가 있는지 모른다면, 클라이언트는 그 리소스를 활용할 수 없다..

  → 따라서, 서버는 클라이언트에게 DB를 잘 활용할 수 있도록 인터페이스를 제공해야함

  ⇒ 그게 바로 API(서버의 자원을 잘 가져다 쓸 수 있게 만들어 놓은 interface, *메뉴판 느낌 !* )



## HTTP

- 클라이언트와 서버가 통신할 때 사용하는 규약

작동방식

- 항상 요청과 응답으로 이루어짐
- 요청을 무시하면 오류가 남. (있으면 있다고, 없으면 없다고, 잘못됐다면 잘못됐다고 꼭 응답을 해줘야함)

구성

- Header : 어디서 보내는 요청인지(origin), content-type, 어떤 클라이언트를 이용해 보냈는지(user-agent) 등의 정보를 담고 있음
- Body

속성

- stateless : http의 각 요청은 모두 **독립적**임. 앞에 요청과 이어져있지 않음! 그 부분을 보완하기 위해 인증이란걸 함. conextless 기억! 문맥이 없음
- connectionless : 한번의 요청엔, 한번의 응답. 응답을 두 번 보낼 수 없음

메소드

- GET : 서버에 자원 요청(read) - 상태변경은 절대로 GET 으로 하지 않는다!(CSRF 위험)

- POST : 서버에 자원 생성(create)

- PUT : 서버의 자원을 수정(update)

- DELETE: 서버의 자원 제거

  

## Request

메세지 구조

- 요청 라인 : GET / HTTP/1.1
  - 요청 메소드 : GET, POST, PUT, DELETE
  - 요청 URL
  - HTTP 버전
- 요청 헤더 : 키-값 방식으로 들어감
  - Accept : 클라이언트가 받을 수 있는 컨텐츠
  - Cookie : 쿠키
  - Content-Type : 메세지 바디 종류
  - Content-Length : 메세지 바디 길이
  - If-Modified-Since : 특정 날짜 이후에 변경됐을 때만
- 요청 바디(엔티티)

Content-Type과 Body

참조문서 : https://www.w3.org/Protocols/rfc1341/4_Content-Type.html

1. form 형태 : URLEncoded 방식

   - application/x-www-form-urlencoded
   - 메세지 바디 : 쿼리 문자열

2. json 형태

   - application/json

3. 멀티파트 형태 : 이진파일 넘길 때 사용, 하나의 메세지 바디에 파트를 나눠서 작성

   - boundary는 파트 구분자

   - multipart/form-data: boundary=frontier

     

## response

메세지 구조

- 응답 라인
  - 버전
  - 상태 코드
  - 상태 메세지 : HTTP/1.1 200 OK
- 응답 헤더
  - Content-Type : 바디 데이터의 타입
  - Content-Length : 바디 데이터 크기
  - Set-Cookie : 쿠키 설정
  - ETag : 엔티티 태그
- 응답 바디 : HTML, JSON, Octet Stream 등이 있다.

상태 코드

- 1xx : 정보

- 2xx : 성공

  - 200 : OK. 요청 성공
  - 201 : Created. 생성 요청 성공
  - 202 : Accepted. 요청 수락(처리 보장X)
  - 204 : 성공했으나 돌려줄게 없음

- 3xx : 리다이렉션

  - 300 : Multiple choices. 여러 리소스에 대한 요청 결과 목록
  - 301,302,303 : Redirect. 리소스 위치가 변경된 상태
  - 304 : Not Modified. 리소스가 수정되지 않았음

- 4xx : 클라이언트 오류

  - 400 : Bad Request. 요청 오류
  - 401 : Unauthorized. 권한없음
  - 403 : Forbidden. 요청 거부
  - 404 : Not Found. 리소스가 없는 상태

- 5xx : 서버 오류

  - 500 : Internal Server Error. 서버가 요청을 처리 못함
  - 501 : Not Implemented. 서버가 지원하지 않는 요청
  - 503 : Service Unavailable. 과부하 등으로 당장 서비스가 불가능한 상태

  

  참고 > https://sjh836.tistory.com/81 https://goddaehee.tistory.com/169



## Ajax(Asynchronous JS and XML)

- 페이지 전환(깜빡임) 없이 필요한 부분(페이지의 일부)만 새로운 정보를 받아올 수 있음

- 서버와 자유롭게 통신할 수 있고**(XMLHttpRequest(XHR)의 등장**),

  페이지 깜빡임 없이 seamless하게 작동하는(**JS와 DOM이용**),

  **단순한 web page가 아닌 Web App의 등장**



## fetch()

- 웹프라우저에게 첫번째 인자값을 서버로부터 요청

(ex. url일 경우, 해당 url을 요청, 파일일 경우 해당 파일 실행

- fetch()를 불러들이는 경우, 취득할 리소스를 반드시 인수로 지정
- Promise객체를 반환
- 리퀘스트가 성공하든 실패하든 해당 리퀘스트 통신에 대한 Response객체가 취득

#### **then()**

- 서버가 응답을 하는데에 시간이 걸릴경우, 응답이 끝나면 then() 실행
- 비동기적으로 실행됨
- fetch API가 then()의 콜백함수를 실행시킬 때, 콜백함수의 첫번째 인자로 response의 객체를 줌

→ rsponse객체기 때문에 JSON.stringfy()를 해주는 것임!(우리가 쓰기 좋게!)

