# 34. Redux 기초를 이루는 3가지 원칙

## 1. Redux란

Redux는 상태 관리 라이브러리로, SPA에서 많은 `state`를 가지게 되었고, 이런 `state`를 트리 구조 중 하위 브랜치에 `state`를 보낼 경우 여러 브랜치가 한 `state`를 사용할 경우, 복잡해지는 경우가 생기기 때문에 Redux는 그 모든 `state`를 저장하고 있는 `store`라고 말할 수 있다.

## 2. 3가지 원칙

### 2.1 신뢰할 수 있는 한가지 소스(Single source of truth)

**애플리케이션의 모든 상태는 하나의 저장소 안에 하나의 객체 트리 구조로 저장된다.**

이를 통해 범용적인 애플리케이션(universal application, 하나의 코드 베이스로 다양한 환경에서 실행 가능한 코드)을 쉽게 만들 수 있습니다. 서버로부터 가져온 상태는 시리얼라이즈(serialized, 직렬화)되거나 수화(hydrated, plain형태로 되어 있는 state를 읽어내는 행위)되어 전달되며, 클라이언트에서 추가적인 코딩 없이도 사용할 수 있다.

또한 하나의 상태 트리만을 가지고 있기 때문에 디버깅에도 용이하며, 이전에는 구현하기 어려웠던 기능인 실행취소/다시실행(undo/redo) 등을 손쉽게 구현할 수 있다.

### 2.2 상태는 무조건 읽기 전용이다.(State is read-only)

**상태는 읽기 전용이다. 그 읽기 전용 상태를 변화하는 유일한 방법은 어떤 일이 벌어나는 지를 기술하는 `action`를 전달하는 방법 뿐이다.**

리액트에서 state를 업데이트 해야 할 때, `setState`를 사용하고, 배열을 업데이트 해야 할 때는 배열 자체에 `push`를 직접하지 않고, `concat` 같은 함수를 사용하여 기존의 배열은 수정하지 않고 새로운 배열을 만들어서 교체하는 방식으로 업데이트한다. 리덕스에서도 마찬가지로 기존의 상태는 건들이지 않고 새로운 상태를 생성하여 업데이트 해주는 방식으로 해주면, 나중에 개발자 도구를 통해서 뒤로 돌릴 수도 있고 다시 앞으로 돌릴 수도 있다.

리덕스에서 불변성을 유지하는 이유는 내부적으로 데이터가 변경되는 것을 감지하기 위하여 shallow equality 검사를 하기 때문인데, 이를 통해서 객체의 변화를 감지할 때 객체의 깊숙한 안쪽까지 비교하는 것이 아닌 겉핥기 식으로 비교를 하여 좋은 성능을 유지할 수 있다.

### 2.3 변화는 순수 함수로 작성되어야 한다.(Changes are made with pure functions)

**액션을 통해서 상태 트리가 어떻게 바뀌는지 묘사하기 위해서는 순수 리듀서를 작성해야 한다.**

리듀서는 그저 이전 상태와 액션을 받아서 다음 상태를 반환하는 순수함수로, 새로운 상태 객체를 생성해서 반환해야 한다.

- 리듀서 함수는 이전 상태와 액션 객체를 파라미터로 받는다.
- 이전의 상태는 절대로 건들이지 않고, 변화를 일으킨 새로운 상태 객체를 만들어 반환한다.
- 똑같은 파라미터로 호출된 리듀서 함수는 언제나 똑같은 결과값을 반환한다.

즉, 동일한 인풋이라면 언제나 동일한 아웃풋이 있어야 한다. 하지만 일부 로직 중에는 실행할 때마다 다른 결과값이 나타나는 경우가 있다. 예를 들면 `new Date()`라든지, 랜덤 숫자 생성 로직이라든지, 네트워크에 요청을 한다든지, 그러한 작업은 결코 순수 함수로는 작성할 수 없기 때문에 리듀서 함수의 바깥에서 처리를 해줘야 한다. 이런 경우에 리덕스 미들웨어를 사용하게 된다.

## 3. 요약

![redux](https://user-images.githubusercontent.com/65386533/114649255-1bbd4a00-9d1b-11eb-83a3-f25e74e6dd8a.png)

- `store`에 저장되어 있는 데이터를 `component`가 가져다가 쓴다.
- `component`에서 어떠한 값의 변동이 발생하면 그 값을 `store`에 업데이트를 하게 되는데, 직접적으로 바로 `update`를 시키는 것이 아니다.
- `component`에서 `dispatch`를 통해 `action`으로 호출한다.
- `action`에 정의되어 있는 내용이 `reducer`에 의해서 `handle`이 되고, 그 `handle`에 따라 `store`의 상태값이 `update`가 일어난다.
- `update`가 되어 있는 상태는 `subscribe`를 통해서 실시간으로 `component`에 반영된다.

---

- 참조

  [https://ko.redux.js.org/understanding/thinking-in-redux/three-principles/](https://ko.redux.js.org/understanding/thinking-in-redux/three-principles/)

  [https://ko.redux.js.org/understanding/thinking-in-redux/glossary](https://ko.redux.js.org/understanding/thinking-in-redux/glossary)

  [https://www.youtube.com/watch?v=wSbjROmXTaY](https://www.youtube.com/watch?v=wSbjROmXTaY)

  [https://velog.io/@hwanieee/Redux](https://velog.io/@hwanieee/Redux)

  [https://yceffort.kr/2020/04/redux-study-2](https://yceffort.kr/2020/04/redux-study-2)

  [https://react.vlpt.us/redux/02-rules.html](https://react.vlpt.us/redux/02-rules.html)
