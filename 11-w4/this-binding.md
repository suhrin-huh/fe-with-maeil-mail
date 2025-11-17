# 상황에 따라 this 바인딩이 어떻게 이루어지는지 설명해주세요.

## 호출 방식에 따른 `this` 바인딩
자바스크립트에서는 `this`는 함수가 호출되는 방식에 따라 값이 달라진다.

### 전역 호출
전역에서 함수가 호출되면, `this`는 **전역 객체**를 참조한다. 브라우저 환경에서는 `window 객체`를, Node.js 환경에서는 `global 객체`를 가리킨다.

```js
function globalFunc() {
  console.log(this);
}

golbalFunc(); // 브라우저: window, Node.js: global
```

### 메서드 호출
객체의 메서드로 호출된 함수에서는 `this`는 **해당 객체**를 참조한다.

```js
const obj = {
  name: "홍길동",
  greet: function  () {
    console.log(`안녕, 나는 ${this.name}이야.`)
  };
}
obj.greet(); // "Alice"
```

### 생성자 함수와 클래스
생성자 함수나 클래스에서 this는 새로 생성되는 객체, 인스턴스를 참조한다.

```js
function Person(name) {
  this.name = name;
}

const person = new Person("Alice");
console.log(person.name) // Alice
```


### 명시적 바인딩
`call()`, `apply()`, `bind()` 메서드를 사용하면 this를 명시적으로 설정할 수 있다. 


4. 명시적 바인딩
call(), apply(), bind() 메서드를 사용하면 this를 명시적으로 설정할 수 있습니다.

function greet() {
  console.log(this.name);
}
const user = { name: "Alice" };
greet.call(user); // "Alice"
5. 화살표 함수
화살표 함수는 상위 스코프의 this를 상속받습니다. 자체적인 this를 가지지 않으므로, 사용 위치에 따라 this가 결정됩니다.

const obj = {
  name: "Alice",
  greet: () => console.log(this.name),
};
obj.greet(); // undefined (전역 `this`)
6. DOM 이벤트 핸들러
DOM 요소의 이벤트 핸들러에서 this는 기본적으로 이벤트를 발생시킨 요소를 참조합니다. 하지만 화살표 함수를 사용하면 상위 스코프의 this를 참조합니다.

button.addEventListener("click", function () {
  console.log(this); // 클릭된 button 요소
});
지금까지 설명드린 것과 같이 this는 함수 호출 방식에 따라 값이 달라집니다. 따라서 상황에 따른 동작을 이해하고 적절한 방식을 사용해야 합니다. 특히, 화살표 함수와 명시적 바인딩은 this를 제어하는 데 유용합니다.


## 공식문서 참고
> Stacking context is a **three-dimensional conceptualization of HTML elements** along an imaginary z-axis relative to the user, who is assumed to be facing the viewport or the webpage. The stacking context determines how elements are layered on top of one another along the z-axis (think of it as the "depth" dimension on your screen). Stacking context determines the visual order of how overlapping content is rendered.

`Stacking context`는 사용자가 웹페이지를 바라보고 있다고 가정했을 때, HTML 요소들을 가상의 z축(깊이) 상에서 3차원적으로 개념화한 것입니다. `Stacking context`는 요소들이 화면에서 겹칠 때 어떤 순서로 쌓여 보이는지를 결정합니다.

### 참고자료 
- [쌓임 맥락에 대해 설명해주세요](https://www.maeil-mail.kr/)