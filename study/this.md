### What happens when the engine **runs code**?

1. Global memory 생성
2. Global execution context 생성

### What happens if I **call the funtion**?

1. Local memory 생성(lexical scope에 의존)
2. Local execution context 생성

#### **Execution context**

- 어떤 함수가 호출되면, excution context가 만들어진다(함수 단위로 생김!)

  1. call stack에 push (call stack은 개발자 도구에서 확인 가능)  
     → call stack에 4 개가 담겨 있다?  
     ⇒ 실행 컨텍스트가 4개 존재한다는 의미

  2. 함수를 벗어나면, call stack에서 pop!

- 함수 scope별로 생성
- 담긴 내용
  - scope내 변수 및 함수(Global, Local)
  - 전달 인자(**_arguments_** keword이용하면 확인 가능)
  - 호출된 근원(_arguments.callee.caller_)
  - **this**

### **this**

- 모든 함수 scope내에서 자동으로 설정되는 특수한 식별자
- execution context의 구성 요소 중 하나로, 함수가 실행되는 동안 이용할 수 있다.

## 5 patterns of Binding 'this'

### 1. Global : window

### 2. Function 호출: window (전역은 부모가 window기 때문에!)

```jsx
var name = 'global variable'; // var name -> 전역 변수

console.log(this.name); // global에서 this는 window
// window.name === this.name

function foo() {
  console.log(this.name); // === window.name과 같은 결과임
}
foo(); // function invocation -> this는 window
```

- 함수의 scope가 여러 개 겹쳐있어도(outer함수에 inner함수가 있어도) 함수 호출 시 this는 window임

### 3. Method 호출: 부모 object(직계 부모만 가져옴)

- mehod는 객체에 담긴 함수를 뜻함

```jsx
let obj = {
  foo: function () {
    console.log(this);
  },
};

obj.foo(); // method 호출임!
```

```jsx
var counter = {
  val: 0,
  increment: function () {
    this.val += 1; //여기서 this는 부모 객체를 가리킴 -> this === counter
  },
};

counter.increment(); // method 호출
console.log(counter.val); // 0 + 1  -> 1 출력
counter['increment'](); // method 호출
console.log(counter.val); // 1 + 1 -> 2 출력
```

```jsx
var obj = {
	fn: funtion(a,b){
		return this;
	}
};

var obj2 = {
	method: obj.fn
};

//실행되는 시점에서의 부모가 this임!
console.log( obj2.method() === obj2);  // true
console.log( obj.fn() === obj);        // true
```

### 4. Construction mode(new 연산자로 생성된 function영역의 this) : 새로 생성된 객체(인스턴스)

```jsx
function Car(brand) {
  this.brand = brand; // this는 avante를 가리킴
}
let avante = new Car('hyundai');
```

### 5. .call() or .apply() 호출: call, apply의 첫번째 인자로 명시된 객체

- **call(): 값을 ,로 구분해서 넘겨줌**

```jsx
function identify() {
  return this.name.toUpperCase();
}

function speak() {
  var greeting = "Hello, I'm " + identify.call(this);
  return greeting;
}

var me = { name: 'kyle' };
var you = { name: 'Tom' };

identify.call(me);
identify.call(you);
speak.call(me); // Hello, I'm KYLE
speak.call(you); // Hello, I'm TOM
```

- **apply() : 값을 배열 형태로 넘겨줌**

```jsx
var add = function (x, y) {
  this.val = x + y;
};
var obj = {
  val: 0,
};

add.apply(obj, [2, 8]);
console.log(obj.val); // 10

add.call(obj, 2, 8);
console.log(obj.val); // 10
```
