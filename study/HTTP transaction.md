# HTTP 트랜잭션 해부

[HTTP 트랜잭션 해부](https://nodejs.org/ko/docs/guides/anatomy-of-an-http-transaction/)

### Stream

- 메모리 버퍼와 대역폭을 절약할 수 있는 이벤트 기반의 I/O 인터페이스를 제공

### EventEmitter

- 이벤트를 내보내는 모든 객체는 EventEmitter 클래스의 인스턴스
- 이 객체는 하나 이상의 함수를 이벤트로 사용할 수 있도록 이름을 넣어 추가하는 eventEmiter.on() 함수를 사용할 수 있다.

  [[Node.js] 이벤트(Event) 이해하기](https://stickie.tistory.com/66)

---

### 서버 생성

- `createServer` : 웹 서버 객체를 만듦

```jsx
const http = require('http');

//모든 node 웹 서버 애플리케이션은 웹 서버 객체를 만들어야 함!
const server = http.createServer((request, response) => {
  // 여기서 작업이 진행됩니다!
});
```

→ 이 서버로 오는 HTTP 요청마다 `createServer`에 전달된 함수가 한 번씩 호출됨

- `creatServer`가 반환하는 서버 객체는 _EventEmitter_ 이다.

```jsx
// 이렇게도 사용가능! 위에 코드는 축약문법
const server = http.createServer();
server.on('request', (request, response) => {
  // 여기서 작업이 진행됩니다!
});
```

- `listen`: 서버가 사용하고자 하는 포트 번호를 받아서 서버를 시작함.

```jsx
const PORT = 5000;
const HOST = 'localhost';

server.listen(PORT, HOST);
```

[Node.js v14.12.0 Documentation](https://nodejs.org/api/net.html#net_server_listen)

- `request`

  - 요청을 처리하기 위해선, **메서드와 URL**을 확인한 후, 적절한 작업을 실행하게 됨
  - `request` 객체에 유용한 프로퍼티를 활용해서 메서드와 url을 확인 할 수 있다.

  ```jsx
  const { method, url } = request;
  ```

  - `request` 객체는 *IncomingMessage*의 인스턴스이다.

    - _IncomingMessage?_

      IncomingMessage 개체는 http.Server 또는 http.ClientRequest에 의해 생성되고 각각 'request'및 'response'이벤트에 대한 첫 번째 인수로 전달됩니다. 응답 상태, 헤더 및 데이터에 액세스하는 데 사용할 수 있습니다.

  - **header**또한 request에 전용 객체가 있다.

  ```jsx
  const { headers } = request;
  const userAgent = headers['user-agent'];
  ```

  → 모든 헤더는 **소문자로만 표현**된다.

  - **body** : post나 put요청일 경우 request의 body는 중요! (바디 데이터를 받는 것은 좀 복잡)

  ```jsx
  let body = [];
  request
    .on('data', (chunk) => {
      body.push(chunk);
    })
    .on('end', () => {
      body = Buffer.concat(body).toString();
      // 여기서 `body`에 전체 요청 바디가 문자열로 담겨있습니다.
    });
  ```

  → `data`이벤트를 통해 chunk를 받아서 배열에 담아 `end`이벤트에서 문자열로 합침

  (request객체가 ReadableStream 인터페이스를 구현하고 있으므로, `data`와 `end`이벤트를 사용 )

  - `request` 정리

  ```jsx
  const http = require('http');

  http
    .createServer((request, response) => {
      const { headers, method, url } = request;
      let body = [];
      request
        .on('error', (err) => {
          console.error(err);
        })
        .on('data', (chunk) => {
          body.push(chunk);
        })
        .on('end', () => {
          body = Buffer.concat(body).toString();
          // 여기서 헤더, 메서드, url, 바디를 가지게 되었고
          // 이 요청에 응답하는 데 필요한 어떤 일이라도 할 수 있게 되었습니다.
        });
    })
    .listen(8080); // 이 서버를 활성화하고 8080 포트로 받습니다.
  ```

- `response`

  - 클라이언트에 데이터 응답을 하기 위해 여러 메서드가 있음
  - **HTTP 상태코드** (디폴트는 200)

  ```jsx
  //statusCode로 상태코드 설정
  response.statusCode = 404;
  ```

  - **헤더** 설정 메서드: **setHeader, writeHead**

  ```jsx
  //1. 암묵적인 헤더
  response.setHeader('Content-Type', 'application/json');
  response.setHeader('X-Powered-By', 'bacon');
  ------------------// 2. 명시적인 헤더
  response.writeHead(200, {
    'Content-Type': 'application/json',
    'X-Powered-By': 'bacon',
  });
  ```

  - **바디** 전송: **write, end**

  ```jsx
  response.write('<html>');
  response.write('<body>');
  response.write('<h1>Hello, World!</h1>');
  response.write('</body>');
  response.write('</html>');
  response.end();
  ------------------------------response.end(
    '<html><body><h1>Hello, World!</h1></body></html>'
  );
  ```

  📢️ 바디에 데이터 청크를 작성하기전에 상태코드와 헤더를 먼저 작성해야 함!!
