# OAuth & Social Login

## OAuth

- 사용자의 패스워드 없이, 외부 서비스에서도 **인증**을 가능하게 하고 그 서비스의 **API를 이용하게 해주는 것(권한**)

  ⇒  이미 사용자 정보를 갖고 있는 웹 서비스(구글, 깃헙) 에서 사용자의 인증을 대신해주고, 접근 권한에 대한 토큰을 발급한 후, 이를 이용해 내 서버에 인증을 가능하게 함

*__Authentication(인증) ?__*

- 사용자가 서버에 본인임을 증명하여 확인하는 과정

  ex) Id와 password를 이용하여  로그인

***Authorization(권한) ?***

- 인증을 성공적으로 마친 후, 특정 자원에 대한 접근 권한이 있는지 확인하는 과정

## OAuth 2.0

- OAuth 1.0의 단점을 보완하여 만들어짐
- `Resource owner`: 사용자.
- `Client`: Resource Server 에서 제공하는 자원을 사용하는 애플리케이션 (예, 페이스북의 뉴스를 모아서 보여주는 앱)
- `Resource server(API server)`: 자원을 호스팅하는 서버, (예, 페이스북 사진 비디오 등)
- `Authorization Server`: 사용자의 동의를 받아서 권한을 부여하는 서버, 일반적으로 Resource Server 와 같은 URL 하위에 있는 경우가 많음.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/deb02d38-9f85-407b-bc25-17bc2e601fe2/img_guide2.jpg](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/deb02d38-9f85-407b-bc25-17bc2e601fe2/img_guide2.jpg)

https://developers.payco.com/guide/development/start

### Acess Token

- 권한 요청을 정상적으로 마치면, 클라이언트에서 Access Token이 발급 됨
- 보호된 리소스에 접근할 때 권한 확인용으로 임의의 문자열 형태
- 발급 받은 Access Token은 서비스에 자체적으로 저장, 관리함