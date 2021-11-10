# 투두리스트 MVC 패턴으로 만들어보기 - 1(JavaScript)

## 계기

지난 9개월동안 리액트를 사용하여 개발을 했다. 그런데 바닐라 자바스크립트로만 무언가를 만들어보는데 익숙해지기 전에 리액트를 사용하는데 더 익숙해져버린 것 같다는 생각이 들었다.

프론트엔드 개발자에게 자바스크립트에 대한 이해도와 활용 능력이 굉장히 중요한데.. 그래서 이 기회에 SPA 프레임워크나 라이브러리를 사용하지 않고 자바스크립트만을 이용해서 연습을 했다.

무작정 만들어본 것은 아니고 인프런에서 김정환님의 [만들고 비교하며 학습하는 리액트](https://www.inflearn.com/course/%EB%A7%8C%EB%93%A4%EB%A9%B4%EC%84%9C-%ED%95%99%EC%8A%B5%ED%95%98%EB%8A%94-%EB%A6%AC%EC%95%A1%ED%8A%B8) 를 수강한 후에 만들어 본 것이다.

리액트를 사용할 때의 장점을 비교하기 위해, 강의 앞쪽에 순수 자바스크립트로만 기능을 구현하는 내용이 있는데, 내가 원하던 것이라고 생각해서 수강했다.

투두리스트는 프로그래밍을 처음배우고 많은 사람이 만드는 것이지만, 현재 거창한 사이드 프로젝트를 하려는 목적으로 진행한 것이 아니다. 자바스크립트로 mvc 패턴 방식의 개발에 익숙해지기 위해 연습하는 것이기에 투두리스트도 괜찮겠다고 판단했다.

또 자바스크립트를 학습하는 다른 분들에게도 도움이 될 것 같아서 블로그에도 남기기로 결정했다.

**참고 :**

- 제가 하는 방식이 정석도 아니고 틀린 부분도 있을 수 있기 때문에, 지적이나 피드백은 감사히 받겠습니다.
- 정환님께서 강의에서 사용하신 코드와 똑같거나 비슷한 부분이 있습니다. 학습 용도로 소스코드를 사용하는 것은 가능하다고 하셨기에 제 프로젝트에도 적용했습니다.

## MVC 패턴이란 무엇일까?

먼저 MVC 패턴이 무엇인지 간단하게 짚고 넘어가야겠다. MVC 패턴은 개발할 때 많이 쓰이는 디자인 패턴이다. MVC는 각각의 줄임말인데, 아래와 같다

- **Model :** 데이터를 관리한다. 데이터를 내부적으로 저장하고 추가, 삭제 등의 기능을 수행한다.
- **View :** 화면 상의 눈에 보이는 부분을 관리한다. html, css 등
- **Controller :** 사용자의 요청을 처리한다. 필요한 데이터를 모델에게 요청하고 뷰를 업데이트하여 사용자가 알 수 있도록 한다.

## HTML & CSS

먼저 html과 css를 작성하자.

**index.html**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="style.css" />
    <script src="/src/main.js" type="module" defer></script>
    <title>Todo List</title>
  </head>
  <body>
    <header class="header">
      <h1>TODO LIST</h1>
    </header>
    <form id="todo-form">
      <input
        type="text"
        name="add-todo"
        autofocus
        placeholder="Add your todo things :D"
        autocomplete="off"
      />
    </form>
    <main class="container">
      <ul id="todo-list"></ul>
    </main>
    <footer class="footer"></footer>
  </body>
</html>
```

**style.css**

```css
:root {
  --max-width: 500px;
}

button,
ul,
li {
  padding: 0;
}

button {
  cursor: pointer;
}

input {
  height: 46px;
  width: 300px;
  padding-left: 8px;
  font-size: 16px;
}

header {
  text-align: center;
}

#todo-form {
  width: var(--max-width);
  margin: auto;
  display: flex;
  justify-content: center;
}

.container {
  width: var(--max-width);
  margin: auto;
  display: flex;
  justify-content: center;
  text-align: center;
}

#todo-list {
  width: 100%;
}

.todo {
  display: flex;
  align-items: center;
  height: 50px;
}

#todo-check {
  width: 30px;
  height: 30px;
  background-color: #fff;
  border-radius: 50%;
  border: 2px solid pink;
  margin-right: 16px;
}

#todo-check {
  color: green;
}

#todo-remove {
  width: 26px;
  height: 26px;
  border-radius: 50%;
  border: 1px solid black;
  background-color: #fff;
  margin-left: auto;
}

.empty-message {
  width: 100%;
  height: 200px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  color: indianred;
}
```

아주 **예쁜** 투두리스트를 만드는 것이 목적이 아니기 때문에, 위와 같이 간단하게 작성했다. 이것을 보고 투두리스트를 진행하는 분이 계시다면, 이런 흉측한 투두리스트 보다는 본인만의 **아름다운** 투두리스트를 만들어보는 것도 좋을 것 같다.

혹시나 해서 말하자면 css 파일 상단에 :root 관련된 부분은 css에서 변수를 선언하는 것이다. 파일 중간에 선언한 변수를 사용하는 것을 확인할 수 있을 것이다.

## View

우리의 투두리스트에서는 몇 가지의 뷰가 필요하다. 나는 아래와 같이 구성했다.

1. TodoFormView
2. TodoListView

`TodoFormView`는 할 일을 입력하는 form 태그와 관련된 View다. `TodoListView 는 이름 그대로 우리가 추가한 투두 목록을 보여주는 View다.

이 프로젝트의 규모는 작지만 더 규모가 큰 프로젝트라면 View가 더 많이 만들어질 것이다. 그래서 그 View들의 공통된 부분을 Class로 정의하도록 하자.

**src/views/View.js**

```javascript
export default class View {
  constructor(element) {
    if (!element) throw Error("No element");

    this.element = element;
    this.originDisplay = element.style.display || "";
  }

  hide() {
    this.element.display = "none";
    return this;
  }

  show() {
    this.element.display = this.originDisplay;
    return this;
  }

  on(eventName, eventHandler) {
    this.element.addEventListener(eventName, eventHandler);
  }

  emit(eventName, detail) {
    const event = new CustomEvent(eventName, { detail });
    this.element.dispatchEvent(event);
    return this;
  }
}
```

위에서 언급한 `TodoFormView`와 `TodoListView`는 이 View를 상속할 것이다.
각 메서드가 무슨 기능을 하는지 하나씩 뜯어보자.

### constructor

```javascript
constructor(element) {
    if (!element) throw Error("No element");

    this.element = element;
    this.originDisplay = element.style.display || "";
  }
```

`contructor`는 생성자 함수다. 객체를 생성할 때 호출되며 객체의 변수를 초기화할 수 있다.
예를 들어 `form`을 관리하는 엘리먼트가 있다고 하자. 그러면 우리는 이 form 앨리먼트를 View를 생성하면서 매개변수로 넘겨줄 수 있다. 아래 코드를 확인하자.

```javascript
const formElement = document.querySelector("#todo-form");
const formView = new View(formElement);
```

이렇게 하면 우리가 방금 만든 `formView`는 내부적으로 formElement를 element 변수로 저장한다. 그리고 form이 가지고 있는 display(block, flex, inline-block 등)가 `this.originDisplay`에 할당된다.

하지만 이렇게 View를 생성할 때는 무조건 매개변수로 앨리먼트를 넘겨주어야 한다. 매개변수 없이 그냥 `new View()` 를 한다면, 우리가 작성한 코드에 의해 **"No Element"** 에러가 발생한다.

### hide

```javascript
hide() {
    this.element.display = "none";
    return this;
  }
```

`hide()` 메서드의 목적은 우리가 만든 뷰를 보여주지 않을 때 사용한다. 예를 들면, 아직 우리가 등록한 할 일이 아무것도 없다고 해보자. 그러면 우리는 **"할 일을 추가해주세요."** 와 같은 메세지를 보여줄 수 있다.

하지만 이 메세지는 우리가 할 일을 하나라도 추가하는 순간 사라져야 한다. 그럴 때 사용하는 메서드가 `hide()` 이고, 나중에 다시 보여주어야 할 필요가 있을 때, display 값을 "none"이 아닌 원래의 것으로 되돌려야 한다. 그래서 생성자 함수에서 `originDisplay`를 저장하도록 한 것이다.

### show

```javascript
show() {
    this.element.display = this.originDisplay;
    return this;
  }
```

앨리먼트의 display를 처음에 저장했던 originDisplay로 되돌려주는 역할을 한다.

### on

```javascript
on(eventName, eventHandler) {
    this.element.addEventListener(eventName, eventHandler);
  }
```

on() 메서드는 내부적으로 받은 DOM 앨리먼트에 이벤트를 등록하는 역할을 수행한다.
첫 번째 매개변수로 이벤트의 이름, 그리고 두 번째 매개변수로 이벤트를 핸들링할 콜백함수를 받는다.
아래 코드를 참고하면 이해가 쉬울 것이다.

```javascript
const button = document.querySelector("#button");
button.addEventListener("click", () => {
  console.log("You clicked button!!");
});
```

우리는 위의 코드를 아래와 같이 사용할 수 있는 것이다.

```javascript
const button = document.querySelector("#button");
const buttonView = new View(button);

buttonView.on("click", handleClick);

function handleClick(event) {
  // ...
}
```

### emit

```javascript
emit(eventName, detail) {
    const event = new CustomEvent(eventName, { detail });
    this.element.dispatchEvent(event);
    return this;
  }
```

마지막으로 `emit()` 메서드다. 이 녀석도 on과 같이 첫번째 매개변수로 이벤트의 이름을 받는다. 두번째 인자로는 detail을 받는데, 이는 우리가 사용할 커스텀 이벤트와 관련이 있다.

기존에 존재하는 이벤트가 아닌 우리가 이벤트를 새로 만든다면, `detail`이라는 프로퍼티에 우리가 원하는 값을 같이 전달할 수 있다. MDN의 [Custom Event](https://developer.mozilla.org/ko/docs/Web/API/CustomEvent/CustomEvent)를 참고하자.

지금 당장 어떤 값을 전달할지는 모르겠지만, 무언가를 전달해야할 가능성이 높을 것이다.
그리고 `dispatchEvent()` 를 통해 우리가 만든 커스텀 이벤트를 **발생**시킨다. 이벤트를 등록하는 것이 아니라 **발생**시키는 것이다.

이제 필요한 View도 만들고 기능도 하나씩 구현해보자.
