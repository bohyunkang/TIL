# URL 쿼리스트링 키값 추출 함수

- url에서 query string 키의 값을 추출해서 반환한다.

```js
function urlParam(name) {
  // 인자로 넘겨진 매개변수가 results를 return 한다.
  var results = new RegExp("[\?&]" + name + "=([^&#]*)").exec(
  window.location.href
  );
  if (results == null) {
  	return null;
  } else {
  	return results[1] || 0;
  }
}

// 인자로 받고자 하는 key를 입력하면 된다.
urlParam(원하는키);
```
