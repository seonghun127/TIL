# Asynchronous & Promise

## synchronous

- 현재 작업의 응답이 발생함과 동시에 다음 작업을 요청(순서를 가지고 진행)

- 자바스크립트는 원래 동기적이다.

→ 자바스크립트는 호이스팅 된 이후 부터 우리가 작성한 순서에 맞춰서 하나씩 동기적으로 코드블럭이 실행된다.

*Q. hoisting?* ⇒ `var`혹은 `function 선언` 이 자동적으로 제일 위로 올라감.



## Asynchronous

- 일이 끝날때까지 기다리지 않고, 동시에 일을 처리하는 것 (non-blocking)



## Callback

- 원래 **비동기**라함은 **예상할 수 없다**는 것과 같은 의미(각각의 Task가 걸리는 시간이 다르기 때문에)

- 콜백은 순서가 보장되지 않는 비동기를 조종할 수 있는 수단

- **Callback**함수를 통해 비동기적으로 그 일이 끝나면 다음일을 **순차적으로 처리할 수 있게** 해줌

  *비동기야, 그 일 (얼마나 걸릴지 모르겠지만)다 마치면  내가 콜백으로 넘긴거 호출해줘 !*

- 근데 callback이 너무 반복되면, 가독성이 굉장히 떨어짐(Callback Hell) → **Promise**사용



## Promise

- callback chain을 핸들링할 수 있음
- 비동기 작업이 맞이할 미래의 완료(`reslove`) 또는 실패(`reject`)와 그 결과 값을 나타냄
- promise의 핵심은 내가 아직 모르는 value와 함께 작업할 수 있게 해줌.
- Promise를 사용하면 평평한 indent를 유지할 수 있음

### promise 생성자

- `new Promise()` : promise자체가 class, `new`키워드를 통해 instance생성

  → promise를 지원하지 않는 함수를 감쌀 때 사용

- 매개변수 : executor(Promise생성자에 전달되는 콜백)

  ```jsx
  const myFirstPromise = new Promise((resolve, reject) => {
    //   resolve(someValue)        // fulfilled
    // or
    //   reject("failure reason")  // rejected
  });
  ```

  - `resolve()` : 다음 행동으로 넘어감(성공했기 때문에!) = > `then()`
  - `reject()` : 에러를 핸들링함 ⇒ `catch()`

### promise의 세가지 상태

- *`pending` : 프로미스가 대기중인 상태로 이행되지도, 거부되지도 않은 초기 상태*

- *`fulfilled` : 연산이 성공적으로 완료됨( `resolve()`호출)*

- *`rejected` : 연산이 실패함( `reject()`호출)*

  ⇒ pending 상태의 Promise는 fulfilled 또는 rejected 상태로 바뀔 수 있지만, fullfilled 또는 rejected 상태는 settled 된 것으로, 다른 상태로 바뀔 수 없다.

### promise 프로토타입 메서드

- `then()` : 이행된 값(fulfilled value)을 받을 수 있다. promise가 끝난 이후의 명령어를 전달하기 위해 사용.
- `catch()`: 거절된 이유(rejected value)를 받을 수 있다.



## Async & Await

- promise hell을 보완하기 위해서 나온 ES6 문법
- `async`: 함수에 `async`라고 표시를 해줘야만 `await`을 쓸 수 있음
- `await` : 비동기 함수들을 동기적으로 사용할 수 있음