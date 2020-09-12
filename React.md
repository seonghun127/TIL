# React

- 사용자와 인터랙션 많이 일어나는 웹페이지는 관리해야할 DOM이 많아짐

  ⇒  복잡한 DOM 관리와 상태값 관리를 최소화하고,

  기능 개발에 집중하기 위해 만들어진 프론트엔드 라이브러리

- `Component`: 하나의 의미를 가진 독립적인 단위 모듈 (사용자 정의 HTML 태그 느낌)

### React Concept

1. Data Flow는 단방향이다.

   - 상위 컴포넌트에서 하위 컴포넌트로 데이터(**props**)가 흐름
   - 직접적으로 하위 컴포넌트에서 상위 컴포넌트에 데이터를 줄 수 없다.(간접적으론 가능)

2. `Props`

   - 상위 컴포넌트가 하위 컴포넌트에게 내려주는 데이터
   - 하위 컴포넌트는 `props`를 사용만 할 수 있고, 변경은 불가능 하다.(Read-Only)

   ```jsx
   function Parent(){
   	return (
   		<child name="Tom" /> // 상위컴포넌트에서 태그안에 속성을 주는 것처럼 props를 넘겨
   	)
   }
   ```

   ```jsx
   function child(){
   	return (
   		<h1>hello,{props.name}</h1> // 하위컴포넌트에서 props 쓰는 방법
   	)
   } 
   ```

3. `state`

- 컴포넌트가 갖고 있는 상태

- 객체의 형태로 컴포넌트 내에서 보관하고 관리

  🚨 **주의**

  - `state`를 사용하려면, 반드시 **class Component**로 작성되어야 한다.

  - 값을 변경하려면, 반드시 `setState()`를 사용해야 한다.

    → 만약 `setState()`를 사용하지 않는 다면?

    *: Life cycle을  타지 않기 때문에 life cycle method들을 사용 ❌*

- `state`값을 변경하기 위해 `setState()`를 사용한 후, `render()`가 실행됨

4. Life cycle

- 컴포넌트가 브라우저에 보여질 때, 업데이트 될 때, 사라질 때, 각 단계 전/후로 특정 메서드가 호출됨

  1. 생성(작성된 컴포넌트가 호출될 때)

     `constructor()` → `render()` → `componentDidMount()`

  2. 업데이트(state값 변경 됐을 때)

  `state`값 변경 → `render()` → `componentDidUpdate()`

- Life cycle 메서드를 실행하기 위해서는 반드시 **class Component**여야 한다. (함수형 컴포넌트는 ❌)



### F**unctional Component VS Class Component**

```jsx
function App() {
  return (
    <div className='App'>
      <GroceryList arr={[`cucumbers`, `kale`, `tomato`]} />
    </div>
  );
}

function GroceryList(props) {
  return (
    <div>
      <ul>
        <li>{props.arr[0]}</li>
        <li>{props.arr[1]}</li>
        <li>{props.arr[2]}</li>
      </ul>
    </div>
  );
}
```

```jsx
function App() {
  return (
    <div className='App'>
      <GroceryList arr={[`cucumbers`, `kale`, `tomato`]} />
    </div>
  );
}
class GroceryList extends React.Component {  
constructor(props) {
    super(props);
  }
  render() {
    return (
      <ul>
        {this.props.arr.map((item) => (
          <GroceryListItem list={item} />
        ))}
      </ul>
    );
  }
}

class GroceryListItem extends React.Component {
  constructor(props) {
    super(props);
  }
  render() {
    return <li>{this.props.list}</li>;
  }
}
```



### JSX

- 리액트 컴포넌트를 화면에 보여주기 위해 사용하는 javascript의 확장 문법
- JSX로 코드를 작성하면 babel이 원래 코드로 바꿔줌

**규칙**

1. 반드시 하나의 엘리먼트로 감싸야 한다.

2. 자바스크립트 코드를 작성할 땐 `{ }` 안에 작성한다.

3. `{ }`안이라 해도, if문을 사용할 수 없기 때문에 삼항연산자 혹은 IIFE 사용

4. 엘리먼트의 클래스 이름을 적용할 때, `className`이라고 써야 한다.

   (ES6의 `class`와 헷갈리지 않기 위함)

   ⇒ `<div className='app-container'>Hello</div>`