# 1. 호이스팅(Hoisting)

## 1.1 호이스팅이란?

자바스크립트 및 액션스크립트 코드를 인터프리터가 로드할 때, 변수의 정의가 그 범위에 따라 선언과 할당으로 분리되어 변수의 선언을 항상 컨텍스트 내의 최상위로 끌어올리는 것을 의미한다. 이는 오로지 변수에만 해당되는 것은 아니고 함수도 가능하며, 자바스크립트에서 함수의 호출을 첫 줄에서 하고 마지막 줄에 함수를 정의해도 문제없이 작동되도록 하는 유용한 특성이다. 한마디로, 호이스팅이란 변수가 끌어올려지는 현상을 말한다.

## 1.2 호이스팅의 대상

- `var` 변수 선언과 함수선언문에서만 호이스팅이 발생한다.
  - `var` 변수와 함수의 선언만 호이스팅되며, 할당은 끌어 올려지지 않는다.
  - `let`, `const` 변수 선언과 함수 표현식에서는 호이스팅이 발생하지 않는다.

## 1.3 호이스팅의 범위

1. 전역 범위(global scope)
   - 전역 범위에서는 스크립트의 단위에서 최상단으로 끌어 올려진다.
2. 함수 범위(function scope)
   - 함수 범위에서는 해당 함수의 최상단으로 끌어 올려진다.

## 1.4 변수 호이스팅(var, let, const)

아래 두 예제를 살펴보자.

```jsx
function sayHi() {
  phrase = 'Hello';

  alert(phrase);

  var phrase;
}
sayHi();
```

```jsx
function sayHi() {
  var phrase;

  phrase = 'Hello';

  alert(phrase);
}
sayHi();
```

위의 두 예제는 동일하게 동작한다. `var phrase;`가 다른 행에 위치해 있어도, 호이스팅으로 인해 둘은 동일하게 동작한다. **하지만 선언은 호이스팅되지만 할당은 호이스팅 되지 않는다.**

```jsx
function sayHi() {
  alert(phrase);

  var phrase = 'Hello';
}

sayHi();
```

`var phrase = "Hello";` 행에서는 두 가지 일이 일어난다.

1. 변수 선언(`var`)
2. 변수에 값을 할당(`=`)

변수 선언은 함수 실행이 시작될 때 처리되지만(호이스팅), 할당은 호이스팅 되지 않기 때문에 할당 관련 코드에서 처리된다. 따라서 위의 코드는 아래 코드처럼 동작하게 된다.

```jsx
function sayHi() {
  var phrase; // 선언은 함수 시작 시 처리됩니다.

  alert(phrase); // undefined

  phrase = 'Hello'; // 할당은 실행 흐름이 해당 코드에 도달했을 때 처리됩니다.
}

sayHi();
```

이와 같이 모든 `var` 선언은 함수 시작 시에 처리가 되므로, `var`로 선언한 변수는 어디서든 참조할 수 있다. 하지만 변수에 무언가를 할당하기 전까지는 값이 `undefined`이다.

하지만 `let`과 `const`에서는 호이스팅이 일어나지 않는다. 정확하게는 호이스팅되지 않는 것이 아니라 스코프에 진입할 때 변수가 만들어지고 TDZ(Temporal Dead Zone)가 생성되지만, 코드 실행이 변수가 실제 있는 위치에 도달할 때까지 액세스를 할 수 없는 것이다.

💡 **TDZ(Temporal Dead Zone)란?**
변수 선언 이전에 변수를 참조하는 영역을 말한다. 해당 영역에서 선언 이전에 참조한 변수는 참조 에러(ReferenceError)가 발생한다. 즉, TDZ에 영향을 받는 변수는 선언 이전에 참조하는 것을 금지하고 있다.

자세하게 알기 위해서는 **변수의 3단계 생성과정**에 대해서 생각을 해봐야 한다.

1. **선언 단계** : 변수를 실행 컨텍스트의 변수 객체에 등록한다.
2. **초기화 단계** : 실행 컨텍스트에 등록된 변수 객체에 대한 메모리를 할당한다. 이 단계에서 변수는 undefined로 초기화된다.
3. **할당 단계** : `undefined`로 초기화된 변수에 값을 할당한다.

이 생성과정에서 `var` 키워드로 변수를 만들 경우, 선언 단계와 초기화 단계가 동시에 이뤄진다. 하지만 `let`, `const` 키워드는 선언 단계와 초기화 단계가 분리되어 진행된다.

- [테스트 코드](http://pythontutor.com/visualize.html#mode=edit)

## 1.5 함수 호이스팅(함수 선언문, 함수 표현식)

### 1.5.1 함수 선언문과 함수 표현식의 차이

함수 선언문(Function Declaration)은 아래와 같이 일반 프로그래밍 언어에서의 함수 선언과 비슷한 형식으로 생겼다.

```jsx
function 함수명() {
	구현 로직;
}
```

반면에 함수 표현식(Function Expression)은 아래와 같이 변수값에 함수 표현을 담아 놓은 형식으로 생겼다.

```jsx
var test1 = function () {
  // (익명) 함수표현식
  return '익명 함수표현식';
};

var test2 = function test2() {
  // 기명 함수표현식
  return '기명 함수표현식';
};

// 익명 함수 표현식과 기명 함수 표현식의 차이는 함수에 식별자가 있느냐 없느냐이다.
```

### 1.5.2 함수 선언문에서의 호이스팅

함수 선언문은 코드를 구현한 위치와 관계없이 자바스크립트의 특징인 호이스팅에 따라 브라우저가 자바스크립트를 해석할 때 맨 위로 끌어올려진다.

함수 표현식은 함수 선언문과 달리 선언과 호출 순서에 따라서 정상적으로 함수가 실행되지 않을 수 있다.

---

- 참고

  [https://ko.javascript.info/var](https://ko.javascript.info/var)

  [https://namu.wiki/w/호이스팅](https://namu.wiki/w/%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85)

  [https://gmlwjd9405.github.io/2019/04/22/javascript-hoisting.html](https://gmlwjd9405.github.io/2019/04/22/javascript-hoisting.html)

  [https://velog.io/@stampid/Hoisting호이스팅이란](https://velog.io/@stampid/Hoisting%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85%EC%9D%B4%EB%9E%80)

  [https://medium.com/korbit-engineering/let과-const는-호이스팅-될까-72fcf2fac365](https://medium.com/korbit-engineering/let%EA%B3%BC-const%EB%8A%94-%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85-%EB%90%A0%EA%B9%8C-72fcf2fac365)

  [https://coffeeandcakeandnewjeong.tistory.com/25](https://coffeeandcakeandnewjeong.tistory.com/25)

  [https://ko.javascript.info/recursion](https://ko.javascript.info/recursion)
