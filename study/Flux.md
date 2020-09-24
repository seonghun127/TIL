# Flux

- 클라이언트-사이드 웹 어플리케이션을 만들기 위해 사용하는 어플리게이션 아키텍쳐

- 단방향 데이터 흐름을 활용해 뷰 컴포넌트를 구성하는 React를 보완하는 역할

- 핵심적인 세가지 부분으로 구성 : `Dispatcher`, `Stores`, `Views`(React 컴포넌트)

- controller-views - views

   관계로 존재(MVC패턴과 혼동하지 말것!)

  - **controller-views :** store에서 데이터를 가져와 그 데이터를 자식에게 보내는 역할

- 단향향으로 데이터가 흐름

- React view에서 사용자가 상호작용을 할 때, 그 view는 중앙의 dispatcher를 통해 action을 전파

### 구조와 데이터 흐름

- `Action` → `Dispatcher` → `Store` → `View`
- dispatcher, store과 view는 독립적인 노드로 입력과 출력이 완전히 구분
- action은 새로운 데이터를 포함하고 있는 간단한 객체로 type 프로퍼티로 구분

### Dispatcher

- 모든 데이터는 중앙 허브인 dispatcher를 통해 흐름
- dispatcher는 action을 호출해 데이터를 불러오고 store로 전달할 수 있도록 메소드를 제공한다.
- action은 dispatcher에게 `action creator 메소드`를 제공하는데, 대부분의 action은 view에서의 사용자 상호작용에서 발생
- dispatcher는 store를 등록하기 위한 콜백을 실행한 이후에 action을 모든 store로 전달
- 등록된 콜백을 활용해 store는 관리하고 있는 상태 중 어떤 액션이라도 관련이 있다면 전달

**+** dispatcher를 돕는 **action creator**메소드는 이 어플리케이션에서 가능한 모든 변화를 표현하는 유의전 API를 지원하는데 사용된다. (Flux 업데이트 주기의 4번째 부분이라고 생각하면 유용)

### Store

- 어플리케이션의 데이터(상태?)와 비지니스 로직을 가지고 있음

- store는 자신을 dispatcher에 등록하고 callback을 제공

- store의 등록된 callback의 내부에서는 switch문을 사용한 action 타입을 활용해서 action을 해석하고 store 내부 메소드에 적절하게 연결될 수 있는 훅을 제공

  *⇒ 여기서 결과적으로 action은 disaptcher를 통해 store의 상태를 갱신*

- action이 전파되면 이 action에 영향이 있는 모든 view를 갱신

  ⇒ view가 어떤 방식으로 갱신해야 되는지 일일이 작성하지 않고서도

  데이터를 변경할 수 있는 형태에서 편리

- Store는 독립적인 세계를 가지고 있어 직접적인 setter 메소드가 없는 대신 dispatcher에 등록한 콜백을 통해 데이터를 받음

- change 이벤트를 controller-views에게 알려주고 그 결과로 데이터 계층에서의 변화가 일어난다.

### Views

- view는 사용자의 상호작용에 응답하기 위해 **새로운 action을 만들어 시스템에 전파**
- Controller-views는 이 이벤트를 듣고 있다가 이벤트 핸들러가 있는 store에서 데이터를 다시 가져온다
- controller-views는 스스로의 setState() 메소드를 호출하고 컴포넌트 트리에 속해 있는 자식 노드 모두를 다시 랜더링

------

### Action

- action의 생성은 dispatcher로 action을 보낼 때 의미있는 헬퍼 메소드로 포개진다

### 흐름

![](https://haruair.github.io/flux/img/flux-simple-f8-diagram-explained-1300w.png)

- Action creator는 라이브러리에서 제공하는 도움 메소드로 메소드 파라미터에서 action을 생성하고 type 을 설정하거나 dispatcher에게 제공하는 역할
- 모든 action은 store가 dispatcher에 등록해둔 callback을 통해 모든 store에 전송
- action에 대한 응답으로 store가 스스로 갱신을 한 다음에는 자신이 변경되었다고 모두에게 알린다.
- controller-view라고 불리는 특별한 view가 변경 이벤트를 듣고 새로운 데이터를 store에서 가져온 후 모든 트리에 있는 자식 view에게 새로운 데이터를 제공
