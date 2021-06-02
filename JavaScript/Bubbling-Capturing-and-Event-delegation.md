# 버블링과 캡쳐링, 그리고 이벤트 위임

## 1. 버블링(Bubbling)과 캡쳐링(Capturing)

```jsx
<div onclick="alert('div에 할당한 핸들러!')">
  <em>
    <code>EM</code>을 클릭했는데도 <code>DIV</code>에 할당한 핸들러가
    동작합니다.
  </em>
</div>
```

핸들러는 `<div>`에 할당되어 있지만, `<em>`이나 `<code>`같은 중첩 태그를 클릭해도 동작한다.

분명 `<em>`을 클릭했는데 왜 `<div>`에 할당한 핸들러가 동작하는 것일까? 자세히 알아보자.

### 1.1 버블링

이벤트 버블링은 특정 화면 요소에서 이벤트가 발생했을 때 해당 이벤트가 더 상위의 화면 요소들로 전달되어 가는 특성을 의미한다.

<img width="457" alt="event-bubbling" src="https://user-images.githubusercontent.com/65386533/116398705-10156b80-a863-11eb-8102-f2d06f106d89.png">

```html
<body>
  <div class="one">
    <div class="two">
      <div class="three"></div>
    </div>
  </div>
</body>
```

```jsx
var divs = document.querySelectorAll('div');
divs.forEach(function (div) {
  div.addEventListener('click', logEvent);
});

function logEvent(event) {
  console.log(event.currentTarget.className);
}
```

이러한 코드가 있다고 가정을 하자. 세 개의 div 태그에 모두 클릭 이벤트를 등록하고 클릭하면 logEvent 함수가 호출된다. 여기서 최하위 태그인 `<div class="three"></div>`를 클릭하면 콘솔에는 이렇게 출력됩니다.

<img width="249" alt="event-bubbling-log" src="https://user-images.githubusercontent.com/65386533/116398713-11df2f00-a863-11eb-82bc-a0e3f019fe15.png">

분명 하나의 div만 클릭했음에도 콘솔에는 3개의 이벤트가 발생된 거로 보인다. 그 이유는 브라우저가 이벤트를 감지하는 방식 때문이다.

브라우저는 특정 화면 요소에서 이벤트가 발생했을 때 그 이벤트를 최상위에 있는 화면 요소까지 이벤트를 전파시킨다. 따라서, 클래스 명 three → two → one 순서로 div 태그에 등록된 이벤트들이 실행되는 것이다. 이와 같이 **하위에서 상위 요소로 이벤트를 전파하는 방식을 이벤트 버블링**이라고 한다.

### 1.2 캡쳐링

이벤트 캡쳐링은 버블링과는 반대 방향으로 진행되는 이벤트 전파 방식이다.

<img width="459" alt="event-capturing" src="https://user-images.githubusercontent.com/65386533/116398714-1277c580-a863-11eb-8e8c-058afaaef78a.png">

```html
<body>
  <div class="one">
    <div class="two">
      <div class="three"></div>
    </div>
  </div>
</body>
```

```jsx
var divs = document.querySelectorAll('div');
divs.forEach(function (div) {
  div.addEventListener('click', logEvent, {
    capture: true, // default 값은 false입니다.
  });
});

function logEvent(event) {
  console.log(event.currentTarget.className);
}
```

아까와 똑같은 코드에 `addEventListener()` API에서 옵션 객체에 `capture: true`를 설정해준다. 그러면 해당 이벤트를 감지하기 위해 이벤트 버블링과는 반대 방향으로 탐색한다.

결과는 버블링과 동일하게 `<div class="three"></div>`를 클릭해도 아래와 같은 결과가 나타난다.

<img width="137" alt="event-capture-log" src="https://user-images.githubusercontent.com/65386533/116398718-1277c580-a863-11eb-96ff-b445dc3b0bf1.png">

## 2. 이벤트 위임(Event Delegation)

이벤트 위임은 **하위 요소에 각각 이벤트를 붙이지 않고 상위 요소에서 하위 요소의 이벤트들을 제어하는 방식**을 말한다.

```html
<h1>오늘의 할 일</h1>
<ul class="itemList">
  <li>
    <input type="checkbox" id="item1" />
    <label for="item1">이벤트 버블링 학습</label>
  </li>
  <li>
    <input type="checkbox" id="item2" />
    <label for="item2">이벤트 캡쳐 학습</label>
  </li>
</ul>
```

```jsx
var inputs = document.querySelectorAll('input');
inputs.forEach(function (input) {
  input.addEventListener('click', function (event) {
    alert('clicked');
  });
});
```

위 코드는 To do List를 간단한 리스트 형식으로 나타낸 코드인데,

![event-delegation-1](https://user-images.githubusercontent.com/65386533/116398720-13105c00-a863-11eb-9cc5-6a69718aef8d.gif)

자바스크립트 `querySelectorAll()`을 이용해 화면에 존재하는 모든 인풋 박스 요소를 가져온 다음, 각 인풋 박스의 요소에 클릭 이벤트 리스너를 추가한다. 화면을 실행시키고 각 리스트 아이템의 인풋 박스(체크 박스)를 클릭하면 위와 같이 경고창이 표시된다.

근데 만약 여기서 할 일이 더 생겨 리스트 아이템을 더 추가하게 되면

```jsx
// ...

// 새 리스트 아이템을 추가하는 코드
var itemList = document.querySelector('.itemList');

var li = document.createElement('li');
var input = document.createElement('input');
var label = document.createElement('label');
var labelText = document.createTextNode('이벤트 위임 학습');

input.setAttribute('type', 'checkbox');
input.setAttribute('id', 'item3');
label.setAttribute('for', 'item3');
label.appendChild(labelText);
li.appendChild(input);
li.appendChild(label);
itemList.appendChild(li);
```

![event-delegation-2](https://user-images.githubusercontent.com/65386533/116398723-13a8f280-a863-11eb-8830-b9f855fb2bb8.gif)

기존의 리스트 아이템은 문제 없이 동작하지만 새로 추가된 리스트 아이템에는 클릭 이벤트 리스너가 동작하지 않는다. 그 이유는 인풋 박스에 클릭 이벤트 리스너를 추가하는 시점에서 리스트 아이템은 두 개이고, 새롭게 추가된 리스트 아이템에는 클릭 이벤트 리스너가 등록되지 않았기 때문이다. 이런 식으로 매번 새롭게 추가된 리스트 아이템까지 클릭 이벤트 리스너를 일일이 달아주는 것은 쉬운 일이 아니고, 깔끔한 코드가 아니다.

리스트 아이템이 많아지면 많아질수록 이벤트 리스너 작업 자체가 굉장히 번거로워진다. 이러한 번거로운 작업을 해결할 수 있는 방법이 바로 이벤트 위임이다.

```jsx
// var inputs = document.querySelectorAll('input');
// inputs.forEach(function(input) {
// 	input.addEventListener('click', function() {
// 		alert('clicked');
// 	});
// });

var itemList = document.querySelector('.itemList');
itemList.addEventListener('click', function (event) {
  alert('clicked');
});

// 새 리스트 아이템을 추가하는 코드
// ...
```

화면의 모든 인풋 박스에 일일이 이벤트 리스너를 추가하는 대신 인풋 박스의 상위 요소인 `<ul>` 태그 `.itemList`에 이벤트 리스너를 달아놓고 하위에서 발생한 클릭 이벤트를 감지한다. 이 부분이 앞에서 배웠던 이벤트 버블링이다.

![event-delegation-3](https://user-images.githubusercontent.com/65386533/116398725-14418900-a863-11eb-93b7-01bee0d703ae.gif)

새로 추가된 리스트 아이템에서 클릭 이벤트가 정상적으로 동작한다. 이렇게 함으로써 리스트 아이템이 새로 추가될 때마다 클릭 이벤트를 달지 않아도 된다.

---

- 참고

  [https://ko.javascript.info/bubbling-and-capturing](https://ko.javascript.info/bubbling-and-capturing)

  [https://ko.javascript.info/event-delegation](https://ko.javascript.info/event-delegation)

  [https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/#이벤트-버블링---event-bubbling](https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/#%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%B2%84%EB%B8%94%EB%A7%81---event-bubbling)
