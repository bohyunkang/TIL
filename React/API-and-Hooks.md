# 25. API 연동의 기본 & useReducer로 요청 상태 관리하기

## 1. 비동기 작업의 이해

웹 앱을 만들다 보면 처리할 때 시간이 걸리는 작업들이 있는데, 클라이언트 쪽에서 서버 쪽 데이터가 필요할 때는 Ajax를 사용해 서버의 API를 호출하여 데이터를 수신한다. 이렇게 서버의 API를 사용해야 할 때는 네트워크 송수신 과정에서 시간이 걸리기 때문에 작업이 즉시 처리되는 게 아니라 응답을 받을 때까지 기다렸다가 처리하는데, 이 과정에서 해당 작업을 비동기적으로 처리한다.

💡 **동기? 비동기?란 무엇일까!?**
동기란, 말 그대로 동시에 일어난다는 뜻. 요청과 그 결과가 동시에 일어난다는 약속으로 바로 요청을 하면 시간이 얼마가 걸리던지 요청한 자리에서 결과가 주어진다.
비동기란, 동시에 일어나지 않는다를 의미. 요청과 결과가 동시에 일어나지 않는다.

만약 작업을 동기적으로 처리한다면, 요청 결과가 나올 때까지 다른 작업을 할 수 없는데, 비동기적으로 처리를 하면 웹 앱이 멈추지 않기 때문에 여러 가지 요청을 할 수 있고, 기다리는 과정에서 다른 함수도 호출할 수 있다.

### 1.1 CallBack 함수

자바스크립트에서 비동기 작업을 할 때, 가장 흔히 사용하는 방법이 CallBack 함수이다. 콜백 함수란 이름 그대로 나중에 호출되는 함수를 말하는데, 콜백함수라고 해서 그 자체로 특별한 선언이나 문법적 특징을 가지고 있지는 않다. **어떤 이벤트가 발생했거나 특정 시점에 도달했을 때 시스템에서 호출하는 함수를 말한다.** 즉 콜백 함수는 콜백 함수라는 유니크한 문법적 특징을 가지고 있는 것이 아니라, 호출방식에 의한 구분이다. 예를 들어, `setTimeOut`이나 `EventHandler`가 대표적인 예이다.

근데, 이 콜백 함수에는 치명적인 단점이 있는데,

![callback_hell](https://user-images.githubusercontent.com/65386533/113867740-e0d28800-97e9-11eb-902b-8c6fcc5d86cd.png)

계속 쓰다가는 이렇게 콜백 지옥같은 코드가 생길 수 있다는 점이다. 이러한 콜백 지옥이 생기는 경우는 아래와 같다.

- 하나의 비동기 요청이 완료된 뒤, 완료로 인해 얻어진 값을 사용해 다음 비동기 요청이 이루어짐.
- 여러 번의 비동기 호출이 이루어지는데 각 처리는 비동기로 이루어지나, 각 비동기 호출간의 실행순서는 동기적이었으면 함.

그렇기 때문에 웬만하면 지양해야 하는 방식이다.

### 1.2 Promise

콜백 지옥 같은 코드가 형성되지 않게 하는 방안으로 `Promise`가 ES6에 도입이 되었다.

```jsx
new Promise(function (resolve, reject) {
  // ...
});
```

`Promise` 객체를 생성할 때 인자로 해당 요청에 대한 콜백함수를 넘겨준다. 콜백함수에는 2개의 함수가 인자로 전달되는데, 첫번째는 정상수행 후 실행될 `resolve`이고, 두번째는 실패 후 실행될 `reject`이다.

비동기 요청이 정상종료 되었는지 여부에 따라 `resolve`와 `reject`함수를 적절하게 실행함으로 인해 흐름을 제어할 수 있게 됩니다.

![promise](https://user-images.githubusercontent.com/65386533/113867747-e203b500-97e9-11eb-81f5-8cfb2a2f175c.PNG)

### 1.3 async/await

`async/await`는 Promise를 더욱 쉽게 사용할 수 있도록 해 주는 ES2017(ES8) 문법이다. 이 문법을 사용하려면 함수의 앞 부분에 `async` 키워드를 추가해야 하는데, 그러면 항상 프라미스를 반환한다. 심지어 프라미스가 아닌 값을 반환하더라도 *이행 상태의 프라미스(resolved promise)*로 값을 감싸서 이행된 프라미스가 반환되게 한다. 또한 `await`는 프라미스가 처리될 때까지 기다리게 하고, 결과는 그 이후에 반환된다.(await = '기다리다'라는 뜻을 가짐) `promise.then` 보다 좀 더 세련되고 가독성이 좋으며 쓰기도 쉽다.

## 2. axios로 API 호출하여 데이터 연동하기

API를 호출하기 위해서는 `axios`라는 라이브러리를 사용해야 하는데, `axios`를 사용해서 `GET`, `PUT`, `POST`, `DELETE` 등의 메서드로 API 요청을 할 수 있다.

- `GET` : 데이터 조회
- `PUT` : 데이터 등록
- `POST` : 데이터 수정
- `DELETE` : 데이터 제거

### 2.1 사용법

```jsx
import axios from "axios";

axios.get("/users/1");
```

get이 위치한 자리에는 메서드 이름을 소문자로 넣어준다. 예를 들어 새로운 데이터를 등록하고자 할 때는 `axios.post()`를 사용하면 된다. 그리고 파라미터에는 API의 주소를 넣는다.

## 3. useState와 useEffect로 데이터 로딩하기

`useState`를 사용하여 요청 상태를 관리하고, `useEffect`를 사용하여 컴포넌트가 렌더링되는 시점에 요청을 시작하는 작업을 해보는데, 요청에 대한 상태를 관리할 때에는 다음과 같이 총 3가지 상태를 관리해 주어야 한다.

1. 요청의 결과
2. 로딩 상태
3. 에러

하지만 여기서 주의할 점은 `useEffect`에 등록하는 함수에, 첫번째 파라미터로 등록하는 함수에는 `async`를 붙이면 안된다는 것이다. `useEffect`에서 반환해야 하는 값은 뒷정리 함수이기 때문이다. 따라서 `useEffect` 내부에서 `async/await`를 사용하고자 한다면, 함수 내부에 `async` 키워드가 붙은 또 다른 함수를 만들어 사용해 줘야 한다.

## 4. useReducer로 요청 상태 관리하기

`useReducer`로 구현했을 때의 장점은 `useState`의 `setState` 함수를 여러 번 사용하지 않아도 된다는 점과, 리듀서로 로직을 분리했으니 다른 곳에서도 쉽게 재사용할 수 있다는 점이다.

물론 `useState`로 구현을 해도 무방하다.

---

- 참조

  [https://react.vlpt.us/integrate-api/01-basic.html](https://react.vlpt.us/integrate-api/01-basic.html)

  [https://react.vlpt.us/integrate-api/02-useReducer.html?q=](https://react.vlpt.us/integrate-api/02-useReducer.html?q=)

  [https://private.tistory.com/24](https://private.tistory.com/24)

  [https://bravenamme.github.io/2019/10/29/javascript-promise/](https://bravenamme.github.io/2019/10/29/javascript-promise/)

  [https://joshua1988.github.io/web-development/javascript/promise-for-beginners/#promise가-뭔가요](https://joshua1988.github.io/web-development/javascript/promise-for-beginners/#promise%EA%B0%80-%EB%AD%94%EA%B0%80%EC%9A%94)
