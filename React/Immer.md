# immer를 사용한 더 쉬운 불변성 관리

## 1. 불변성의 중요성

리액트 컴포넌트에서 상태를 업데이트할 때 불변성을 지키는 것은 매우 중요한데, **불변성을 지킨다**라는 의미는 **기존의 값을 직접 수정하지 않으면서 새로운 값을 만들어 내는 것**을 말한다. 만약 불변성이 지켜지지 않으면 객체 내부의 값이 새로워져도 바뀐 것을 감지하지 못하기 때문에 `React.memo`에서 서로 비교하여 최적화하는 것도 불가능해진다.

추가로 전개 연산자(... 문법, Spread Operator)를 사용하여 객체나 배열 내부의 값을 복사할 때는 얕은 복사(shallow copy)를 하게 된다.

아래 예시 코드를 살펴보면,

```jsx
const todos = [
  { id: 1, checked: true },
  { id: 2, checked: true },
];
const nextTodos = [...todos];

nextTodos[0].checked = false;
console.log(todos[0] === nextTodos[0]); // 아직까지는 똑같은 객체를 가리키기 때문에 true를 출력함.

nextTodos[0] = {
  ...nextTodos[0],
  checked: false,
};
console.log(todos[0] === nextTodos[0]); // 새로운 객체를 할당해 주었기에 false
```

즉, 내부의 값이 완전히 새로 복사되는 것이 아니라 가장 바깥쪽에 있는 값만 복사된다. 그래서 내부의 값이 객체 혹은 배열이라면 내부의 값 또한 따로 복사를 해 줘야 한다. 만약 객체 안에 있는 객체라면 불변성을 지키면서 새 값을 할당해야 하므로, 배열 혹은 객체의 구조가 정말 복잡해진다면 불면성을 유지하면서 업데이트를 하는 것도 까다로워진다. 간혹 실제 프로젝트에서 복잡한 상태를 다루 때가 있는데, 그럴 때마다 전개 연산자를 여러 번 사용하는 것은 꽤 번거롭고, 가독성도 좋지 않다.

이러한 상황에 `immer`라는 라이브러리를 사용하면, 구조가 복잡한 객체도 매우 쉽고 짧은 코드로 불변성을 유지하면서 업데이트를 해 줄 수 있다.

## 2. immer 사용법

### 2.1 immer 설치

`$ yarn add immer`

우선 위 명령어로 작업할 디렉토리에 `immer`를 설치해 준다.

### 2.2 immer 불러오기

`import produce from 'immer';`

코드의 상단에서 `immer` 를 불러온다. 보통 `produce` 라는 이름으로 불러온다.

### 2.3 함수 이용

`produce` 함수를 사용 할 때에는 첫번째 파라미터에는 수정하고 싶은 상태, 두번째 파라미터에는 어떻게 수정할지를 정의하는 콜백 함수를 넣어준다. 두번째 파라미터에 넣는 함수에서는 불변성에 대해서 신경쓰지 않고 그냥 업데이트 해주면 `immer`가 다 알아서 해준다.

```jsx
import produce from "immer";

const originalState = [
  {
    todo: "리액트 배우기",
    done: true,
  },
  {
    todo: "immer 사용해보기",
    done: false,
  },
];

const nextState = produce(originalState, (draftState) => {
  draftState.push({ todo: "공부내용 정리하기" }); //
  draftState[1].done = true;
});

console.log(nextState);
{
  /* 출력 결과
  [
    {
      todo: "리액트 배우기",
      done: true
    },
    {
      todo: "immer 사용해보기",
      done: true
    },
    {
      todo: "공부내용 정리하기"
    }
  ]
*/
}
```

## 3. immer와 함수형 업데이트

`useState`를 사용하면 함수형 업데이트를 할 수 있는데, 아래 예시 코드를 살펴보면,

```jsx
const [todo, setTodo] = useState({
  text: "Hello",
  done: false,
});

const onClick = useCallback(() => {
  setTodo((todo) => ({
    ...todo,
    done: !todo.done,
  }));
}, []);
```

이렇게 `setTodo` 함수에 업데이트를 해주는 함수를 넣음으로써, 만약 `useCallback`을 사용하는 경우 두번째 파라미터인 `deps` 배열에 `todo`를 넣지 않아도 된다. 이렇게 함수형 업데이트를 하는 경우에, `immer`를 사용하면 상황에 따라 더 편하게 코드를 작성 할 수 있다.

만약에 `produce` 함수에 두개의 파라미터를 넣게 된다면, 첫번째 파라미터에 넣은 상태의 불변성을 유지하면서 새로운 상태를 만들어주지만, 만약에 첫번째 파라미터를 생략하고 바로 업데이트 함수를 넣어주게 된다면, 반환 값은 새로운 상태가 아닌 상태를 업데이트 해주는 함수가 된다.

```jsx
const [todo, setTodo] = useState({
  text: "Hello",
  done: false,
});

const onClick = useCallback(() => {
  setTodo(
    produce((draft) => {
      draft.done = !draft.done;
    })
  );
}, []);
```

결국 `produce`가 반환하는것이 업데이트 함수가 되기 때문에 `useState`의 업데이트 함수를 사용 할 때 위와 같이 구현 할 수 있게 된다.

## 5. 정리

`immer`를 사용하여 컴포넌트를 작성할 때는 객체 안에 있는 값을 직접 수정, 배열에 직접적인 변화를 일으키는 `push`, `splice` 함수를 사용할 수 있다.

하지만 `immer`를 사용한다고 해서 무조건 코드가 간결해지는 것은 아니며 성능적으로는 `immer`를 사용하지 않은 코드가 조금 더 빠르기 때문에, 굳이 `immer`를 사용하지 않아도 되는 경우에는 권장하지 않는다. 불변성을 유지하는 코드가 복잡할 때만 사용하기를 권장한다.

---

- 참조

  [https://react.vlpt.us/basic/23-immer.html](https://react.vlpt.us/basic/23-immer.html)

  [https://immerjs.github.io/immer/produce](https://immerjs.github.io/immer/produce)

  [https://xiubindev.tistory.com/106](https://xiubindev.tistory.com/106)

  <리액트를 다루는 기술> 김민준 저.
