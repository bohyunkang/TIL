# useMemo를 사용하여 연산한 값 재사용하기

## 1. useMemo

`useMemo`는 메모이제이션 훅으로, 계산량이 많은 함수의 반환값을 재활용하는 용도로 사용된다. `useMemo`에서 **Memo**는 "memoized"를 의미하는데, 이는, **이전에 계산한 값을 재사용한다**는 의미를 가지고 있다.

💡 **메모이제이션(Memoization)**
메모이제이션은 컴퓨터 프로그램이 동일한 계산을 반복해야 할 때, 이전에 계산한 값을 메모리에 저장함으로써 동일한 계산의 반복 수행을 제거하여 프로그램 실행 속도를 빠르게 하는 기술을 말한다.
메모이제이션 훅으로는 `useMemo`와 `useCallback`이 있다.

## 2. 사용 방법

```jsx
function MyComponent({ a, b }) {
  const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
  return <div>{memoizedValue}</div>;
}
```

1. `useMemo` 훅의 첫 번째 매개변수로 함수를 입력한다. `useMemo` 훅은 이 함수가 반환한 값을 기억한다.
2. `useMemo` 훅의 두 번째 매개변수로 입력된 배열의 값이 변경되지 않으면, 이전에 반환한 값을 재사용한다.
3. 만약 배열의 값이 변경됐으면 첫 번째 매개변수로 입력된 함수를 실행하고 그 반환 값을 기억한다.

## 3. 사용 예시

리스트에 숫자를 추가하면 추가된 숫자들의 평균을 보여주는 함수형 컴포넌트를 작성한다.

- 📂 Average.js

  ```jsx
  import React, { useState } from "react";

  const getAverage = (numbers) => {
    console.log("평균값 계산 중...");
    if (numbers.length === 0) return 0;
    const sum = numbers.reduce((a, b) => a + b);
    return sum / numbers.length;
  };

  const Average = () => {
    const [list, setList] = useState([]);
    const [number, setNumber] = useState("");

    const onChange = (e) => {
      setNumber(e.target.value);
    };
    const onInsert = (e) => {
      const nextList = list.concat(parseInt(number));
      setList(nextList);
      setNumber("");
    };

    return (
      <div>
        <input value={number} onChange={onChange} />
        <button onClick={onInsert}>등록</button>
        <ul>
          {list.map((value, index) => (
            <li key={index}>{value}</li>
          ))}
        </ul>
        <div>
          <b>평균값: </b> {getAverage(list)}
        </div>
      </div>
    );
  };

  export default Average;
  ```

- 📂 App.js

  ```jsx
  import React from "react";
  import Average from "./Average";

  const App = () => {
    return <Average />;
  };

  export default App;
  ```

위 코드를 입력한 브라우저에서 숫자들을 등록하게 되면, 리스트들의 평균값을 잘 구해준다. 하지만 콘솔을 열어보면, 숫자를 등록할 때뿐만 아니라 `input` 내용이 수정될 때도 `getAverage` 함수가 호출되는 것을 확인할 수 있다. 숫자들이 등록됐을 때만 평균값을 구하면 되기 때문에 이런 경우에 `useMemo` 훅을 사용하여 작업을 최적화하면 된다.

렌더링하는 과정에서 특정 값이 바뀌었을 때만 연산을 실행하고, 값이 바뀌지 않았다면 연산했던 결과를 다시 사용하는 것이다.

- 📂 Average.js

  ```jsx
  import React, { useState, useMemo } from "react";

  const getAverage = (numbers) => {
    console.log("평균값 계산 중...");
    if (numbers.length === 0) return 0;
    const sum = numbers.reduce((a, b) => a + b);
    return sum / numbers.length;
  };

  const Average = () => {
    const [list, setList] = useState([]);
    const [number, setNumber] = useState("");

    const onChange = (e) => {
      setNumber(e.target.value);
    };
    const onInsert = (e) => {
      const nextList = list.concat(parseInt(number));
      setList(nextList);
      setNumber("");
    };

    const avg = useMemo(() => getAverage(list), [list]);

    return (
      <div>
        <input value={number} onChange={onChange} />
        <button onClick={onInsert}>등록</button>
        <ul>
          {list.map((value, index) => (
            <li key={index}>{value}</li>
          ))}
        </ul>
        <div>
          <b>평균값: </b> {avg}
        </div>
      </div>
    );
  };

  export default Average;
  ```

이제 list 배열의 내용이 바뀔 때만 `getAverage` 함수가 호출된다.

## 4. 주의 사항

`useMemo`는 성능 최적화를 위해 사용할 수는 있지만 의미상으로 보장이 있다고 생각하지는 말아야 한다. 가까운 미래에 React에서는 이전 메모이제이션된 값들의 일부를 "잊어버리고" 다음 렌더링 시에 그것들을 재계산하는 방식을 채택할 수도 있다. 그렇기 때문에 `useMemo`를 사용하지 않고도 동작할 수 있는 코드를 작성하고 그것을 추가하여 성능을 최적화하는 것을 권장한다.

또한 일반적으로 소프트웨어의 성능 최적화에는 그에 상응하는 대가가 따르기 마련이다. 따라서 **성능 최적화를 할 때는 얻을 수 있는 실제 성능 이점이 지불하는 대가에 비해서 미미하지 않은지에 대해 반드시 따져보고 사용해야 한다.** 예를 들어, `useMemo` 훅을 남용하면, 컴포넌트의 복잡도가 올라가기 때문에 코드를 읽기도 어려워지고 유지보수성도 떨어지게 된다. 또한 `useMemo`가 적용된 레퍼런스는 재활용을 위해서 가비지 컬렉션에서 제외되기 때문에 메모리를 더 쓰게 된다.

💡 **가비지 컬렉션(Garbage Collection)**
가비지 컬렉션은 말 그대로 '쓰레기 수집'이라는 뜻으로, 프로그램이 동적으로 할당했던 메모리 영역 중에서 필요없게 된 영역을 해제하는 메모리 관리 기법 중 하나이다. 필요없게 된 영역이란, 어떤 변수도 가리키지 않게 된 영역을 의미한다.

---

- 참조

  [https://react.vlpt.us/basic/17-useMemo.html](https://react.vlpt.us/basic/17-useMemo.html)

  [https://ko.reactjs.org/docs/hooks-reference.html#usememo](https://ko.reactjs.org/docs/hooks-reference.html#usememo)

  [https://ko.wikipedia.org/wiki/메모이제이션](https://ko.wikipedia.org/wiki/%EB%A9%94%EB%AA%A8%EC%9D%B4%EC%A0%9C%EC%9D%B4%EC%85%98)

  [https://www.daleseo.com/react-hooks-use-memo/](https://www.daleseo.com/react-hooks-use-memo/)

  [https://ko.wikipedia.org/wiki/쓰레기*수집*(컴퓨터\_과학)](<https://ko.wikipedia.org/wiki/%EC%93%B0%EB%A0%88%EA%B8%B0_%EC%88%98%EC%A7%91_(%EC%BB%B4%ED%93%A8%ED%84%B0_%EA%B3%BC%ED%95%99)>)

  <실전 리액트 프로그래밍> 이재승 저.

  <리액트를 다루는 기술> 김민준 저.
