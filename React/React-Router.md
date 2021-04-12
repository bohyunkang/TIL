# 리액트 라우터 기본적인 사용법

## 1. SPA란?

SPA란 Single Page Application의 약자로, 단일 페이지 어플리케이션을 의미하는데, 말 그대로 페이지가 1개인 어플리케이션으로 현재 웹 개발의 트렌드이다.

기존 웹 서비스는 요청 시마다 서버로부터 리소스들과 데이터를 해석하고 화면에 렌더링하는 방식인 반면 SPA는 브라우저에 최초에 한번 페이지 전체를 로드하고, 이후부터는 특정 부분만 Ajax를 통해 데이터를 바인딩하는 방식이다.

단일 페이지 어플리케이션이라고 해서 정말로 한 페이지만 있는 것은 아니기 때문에 다른 주소가 되면 다른 화면을 보여주는 작업이 필요하다. 이를 라우팅이라고 한다.

## 2. 라우터 설치 및 적용

react-router는, 써드파티 라이브러리로서, 비록 공식은 아니지만 가장 많이 사용되고 있는 라이브러리이다. 이 라이브러리는 클라이언트 사이드에서 이뤄지는 라우팅을 간단하게 해준다.

일단 작업하고 있는 폴더에 `yarn add react-router-dom` 명령어로 라우터를 설치하고, index.js 파일에 `BrowserRouter`라는 컴포넌트를 활용해 구현한다.

- 📂 src > index.js

  ```jsx
  import React from 'react';
  import ReactDOM from 'react-dom';
  import { BrowserRouter } from 'react-router-dom'; // * BrowserRouter 불러오기
  import App from './App';

  // * App 을 BrowserRouter 로 감싸기
  ReactDOM.render(
    <BrowserRouter>
      <App />
    </BrowserRouter>,
    document.getElementById('root')
  );
  ```

## 3. Route 특정 주소에 컴포넌트 연결하기

- 📂 src > App.js

  ```jsx
  import React from 'react';
  import { Route } from 'react-router-dom';
  import About from './About';
  import Home from './Home';

  const App = () => {
    return (
      <div>
        <Route path='/' component={Home} />
        <Route path='/about' component={About} />
      </div>
    );
  };

  export default App;
  ```

  그런데 이때, About 페이지에서는 Home 화면까지 렌더링이 된다. 이는 `/about`의 경로가 `/`도 포함하기 때문인데, 이때는 아래와 같이 Home 라우터에 exact라는 props를 넣어주면 된다.

  `<Route path="/" component={Home} exact={true} />`

  이렇게 하면 경로가 완전히 똑같을 때에만 컴포넌트를 보여주게 된다.

## 4. Link: 누르면 다른 주소로 이동시키기

Link 컴포넌트는 클릭하면 다른 주소로 이동시키는 컴포넌트이다. 만약 리액트에서 기존의 `<a>`태그를 사용하여 링크를 걸게 되면, 페이지를 이동하면서 페이지를 아예 새로 불러오게 되기 때문이다. 이렇게 되면 우리 리액트 앱이 지닌 상태들도 초기화되고, 렌더링된 컴포넌트도 모두 사라지게 되어 새로 렌더링을 하기 때문이다. 그렇기 때문에 `<a>` 대신 Link 컴포넌트를 사용해야 한다.

### 4.1 사용 방법

```jsx
<Link to='이동하고자 하는 다른 주소'></Link>
```

사용방법은 간단하게 Link의 to라는 props에 이동하고자 하는 주소를 적으면 된다.

## 5. Route 하나에 여러 개의 path 설정하기

Route 하나에 여러 개의 path를 지정하는 것은 최신 리액트 라우터 v5부터 적용된 기능으로, 이전에는 여러 개의 path에 같은 컴포넌트를 보여주고 싶은 경우 아래와 같이 해야만 했다.

- Before

  ```jsx
  import React from 'react';
  import { Route } from 'react-router-dom';
  import About from './About';
  import Home from './Home';

  const App = () => {
    return (
      <div>
        <Route path='/' component={Home} exact={true} />
        <Route path='/about' component={About} />
        <Route path='/info' component={About} />
      </div>
    );
  };

  export default App;
  ```

- 리액트 라우터 v5 적용 후

  ```jsx
  import React from 'react';
  import { Route } from 'react-router-dom';
  import About from './About';
  import Home from './Home';

  const App = () => {
    return (
      <div>
        <Route path='/' component={Home} exact={true} />
        <Route path={['/about', '/info']} component={About} />
      </div>
    );
  };

  export default App;
  ```

  Route를 두 번 사용하는 대신, path props를 배열로 설정하여서 여러 경로에서 같은 컴포넌트를 출력하게 한다.

---

- 참조

  [https://react.vlpt.us/react-router/](https://react.vlpt.us/react-router/)

  [https://linked2ev.github.io/devlog/2018/08/01/WEB-What-is-SPA/](https://linked2ev.github.io/devlog/2018/08/01/WEB-What-is-SPA/)

  <리액트를 다루는 기술> 김민준 저.
