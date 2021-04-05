# styled-components

![sc1](https://user-images.githubusercontent.com/65386533/113510144-488f9580-9594-11eb-975c-4bd98036662d.png)

## 1. styled-components란?

`styled-components`란 자바스크립트 파일 내에서 CSS를 사용할 수 있게 해주는 대표적인 CSS-in-JS 라이브러리이다. 기존 돔을 만드는 방식인 css, scss 파일을 밖에 두고, 태그나 `id`, `class` 이름으로 가져와 쓰지 않고, 동일한 컴포넌트에서 컴포넌트 이름을 쓰듯 스타일을 지정하는 방식이다. `styled-components`는 현존하는 CSS-in-JS 관련 리액트 라이브러리 중에서 가장 인기 있는 라이브러리입니다. 이에 대한 대안으로는 emotion과 styled-jsx가 있다.

💡 **CSS-in-JS?**
CSS-in-JS는 외부의 파일에 CSS를 정의하는 대신에 JavaScript와 결합하는 패턴을 의미한다.

## 2. 사용 시 장점

- 자바스크립트 파일 하나에 스타일까지 작성할 수 있기 때문에 `.css` 또는 `.scss` 확장자를 가진 스타일 파일을 따로 만들지 않아도 된다는 이점이 있다.
- 일반 `classNames`를 사용하는 CSS/Sass를 비교했을 때, 가장 큰 장점은 props 값으로 전달해 주는 값을 쉽게 스타일에 적용할 수 있다는 것이다.

## 3. 문법

### 3.1 Tagged 템플릿 리터럴

스타일을 작성할 때, `(백틱)를 사용하여 만든 문자열에 스타일 정보를 넣어 준다. 이 문법을 Tagged 템플릿 리터럴이라고 한다. 일반 템플릿 리터럴과 다른 점은 템플릿 안에 자바스크립트 객체나 함수를 전달할 때 온전히 추출할 수 있다는 점이다.

```jsx
`hello ${{ foo: "bar" }} ${() => "world!"}!`;
// 출력 결과 "hello [object Object] () => 'world'!"
```

위 코드와 같이 템플릿에 객체를 넣거나 함수를 넣으면 형태를 잃어버리게 되는데, Tagged 템플릿 리터럴을 사용하면 템플릿 사이 사이에 들어가는 객체나 함수의 원본 값을 그대로 추출할 수 있다.

### 3.2 스타일링된 엘리먼트 만들기

styled-components를 사용하여 스타일링된 엘리먼트를 만들 때는 컴포넌트 파일 상단에 `styled`를 불러오고, `styled.태그명`을 사용하여 구현한다.

```jsx
import styled from "styled-components";

const Title = styled.h1`
  font-size: 2rem;
`;
```

또한, 사용해야 할 태그명이 유동적이거나 특정 컴포넌트 자체에 스타일링을 하고 싶을 땐 아래와 같이 사용한다.

```jsx
// 태그의 타입을 styled 함수의 인자로 전달
const Content = styled("p")`
  background: blue;
`;
// 아예 컴포넌트 형식의 값을 넣어줌
const StyledContent = styled(Content)`
  color: white;
`;
```

### 3.3 스타일에서 props 조회하기

```jsx
const Container = styled.div`
  /* props로 넣어준 값을 직접 전달해 줄 수 있음 */
  background: ${(props) => props.color || "blue"};
  padding: 1rem;
  display: flex;
`;
```

코드를 보면 background 값에 `props`를 조회해서 `props.color`의 값을 사용하게 한다. 그리고 `color`의 값이 주어지지 않았을 때는 `blue`를 기본 색상으로 설정했다. 이렇게 만들어진 코드는 JSX에서 사용될 때 `<Container color="yellow">(...)</Container>`와 같이 color에 props를 넣어줄 수 있다.

### 3.4 props에 따른 조건부 스타일링

기존 CSS는 조건부 스타일링이 필요할 때 className을 사용하여 조건부 스타일링 했는데, styled-components에서는 조건부 스타일링을 간단하게 props로도 처리할 수 있다.

```jsx
${props =>
	props.inverted &&
	css`
		background: none;
		border: 1px solid black;
		color: white;
`};
```

이렇게 만들어진 코드는 JSX에서 사용될 때

```jsx
<Button>안녕하세요</Button>
<Button inverted={true}>테두리만 강조</Button>
```

와 같이 서로 다른 스타일을 적용할 수 있다.

하지만 이와 같이 props를 참조하는 경우에는 반드시 CSS로 감싸 주어서 Tagged 템플릿 리터럴을 사용해 주어야 한다. 감싸주지 않을 경우에는 Tagged 템플릿 리터럴로 적용되지 않아 해당 내용이 문자열로 취급되기 때문이다. 그렇기 때문에 조건부 스타일링을 할 때 넣는 여러 줄의 코드에서 props를 참조하지 않는다면 굳이 CSS를 불러와서 사용하지 않아도 된다.

---

## 💡 그렇다면 어떤 라이브러리를 사용해야 하며 어떻게 사용해야 할까?

### **질문**

의견이 필요합니다. 여러분은 어떻게 리액트 스타일을 쓰시나요?

컴포넌트 각각에 CSS를 적용하나요?

하나의 큰 CSS 파일을 쓰시나요?

SASS로 관리하시나요?

CSS 모듈로 관리하나요?

아니면 Styled-component를 쓰나요?

### **답변**

- 컴포넌트 각각에 CSS를 쓰는 것이 가장 일반적이긴 합니다.
- 하나의 큰 CSS 파일을 만드는 것은 가독성이 떨어지고, 관리하기도 어렵고, 분명히 확장성에도 한계가 있습니다.
- SASS는 좋은 방법이지만 SASS를 새로 익히는 시간이 들어갑니다. 다시 한번 말하지만 하나의 큰 파일에 쓰는 것은 쓸모 없습니다._(SASS를 사용한다면 모듈을 잘 분리하라는 뜻 같네요— 역자 주)_
- Styled-component는 그저 혼란스럽습니다. 인라인 CSS스타일에 비해 나쁠 것은 없지만, 필요한 것보다 더 많은 코드를 쓰게 될 수 있습니다. 이것이 어떻게 컴포넌트 각각 CSS를 사용하는 것과 다른지 모르겠습니다.
- CSS Modules 는 쓸데없이 복잡합니다._(className을 사용해 모듈처럼 적용하는 방법을 말하는 것 같습니다. [동명의 라이브러리](https://github.com/css-modules/css-modules)도 있네요 — 역자 주)_

저는 가독성을 최우선으로 하고 가능한 boilerplate를 피하기 위해 몇 가지를 섞어 씁니다.

웹페이지의 스타일들을 block으로 나눠 CSS파일을 적용합니다.

예시)

`table.css`: 테이블 요소를 쓰는 컴포넌트에 적용

`layout.css`: 바깥쪽 레이아웃 (header와 footer 등)에 적용

`basic.css`: 공통적으로 적용되는 스타일 a tag, fonts, buttons, p tag, color 등.

`specific.css`: 특별히 스타일이 많이 들어가는 요소들을 위한 스타일은 따로 빼내서 작성합니다. *(애니메이션이 많이 들어가는 요소, 굉장히 화려하지만 한 두번만 사용되는 버튼 등이 있을 것 같네요 — 역자 주)*

마지막으로 필요한 만큼만 약간의 sass와 boilerplate를 더합니다. 저는 이것을 “modular css”라고 부릅니다.

참조 : [리액트에서 스타일을 어떻게 적용하는 것이 좋을까요? (페이스북 ReactJs 그룹)](https://minoo.medium.com/%EB%A6%AC%EC%95%A1%ED%8A%B8%EC%97%90%EC%84%9C-%EC%8A%A4%ED%83%80%EC%9D%BC%EC%9D%84-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%A0%81%EC%9A%A9%ED%95%98%EB%8A%94-%EA%B2%83%EC%9D%B4-%EC%A2%8B%EC%9D%84%EA%B9%8C%EC%9A%94-eae661f49e18)

---

- 참조

  [https://ko.reactjs.org/docs/faq-styling.html#gatsby-focus-wrapper](https://ko.reactjs.org/docs/faq-styling.html#gatsby-focus-wrapper)

  [https://analogcoding.tistory.com/181](https://analogcoding.tistory.com/181)

  <리액트를 다루는 기술> 김민준 저.
