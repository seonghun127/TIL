![자바스크립트 실행환경](https://miro.medium.com/max/700/1*4lHHyfEhVB0LnQ3HlhSs8g.png)

출처: https://blog.sessionstack.com/how-does-javascript-actually-work-part-1-b0bacc073cf  
참고영상: https://www.youtube.com/watch?v=8aGhZQkoFbQ

#### 자바스크립트

- 싱글 스레드, non-blocking, 비동기, 동시성을 가진 언어

#### V8엔진

- 대표적인 자바스크립트 엔진으로, Chrome과 Node.js에서 사용

  1. `Memory Heap`: 메로리 할당이 일어나는 곳
  2. `Call Stack`: 함수가 실행될 때, 해당 함수가 쌓이고 리턴이 되면 pop!

#### Web APIs

- DOM, Ajax, setTimeout과 같이 브라우저에서 제공하는 API
  (자바스크립트 엔진안에 포함되어 있지 않음)
- `Call Stack`에 Web API가 호출되면 `Web APIs` 영역에서 실행됨
- Web API의 작동이 완료되면, 콜백을 `Callback Queue`에 집어 넣음

#### Callback Queue

- 비동기적으로 실행된 콜백함수를 보관

#### Event Loop

- `Call Stack`과 `Callback Queue`를 주시하면서, `Call Stack`이 비어있으면 `Callback Queue`의 첫번째 콜백을 스택에 쌓아 효과적으로 실행 할 수 있게 한다.

=> 자바스크립트가 싱글 스레드이지만, Event Loop에 기반한 동시선 모델이라고 하는 이유!
