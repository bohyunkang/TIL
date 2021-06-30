# 회원 정보 수정, 삭제 라우팅 주요 개념

## 1. 호출 URL 주소와 라우팅 메소드의 관계

- **라우팅**이란, 사용자가 URL을 통해 해당 앱에 리소스를 요청하는 행위 또는 리소스를 찾아가는 과정을 말한다.
- 라우팅의 첫 출발은 사용자가 URL을 호출(어플리케이션의 특정 리소스를 호출)하는 것이다.
- URL 호출 방식을 결정하는 측은 사용자(웹 브라우저/URL)이다.(아래에 대표적인 4가지 메소드)

  - GET 방식으로 서버 주소 호출하기

    ⇒ url 주소로 최초 호출하는 모든 경우는 모두 GET 방식이다.

    ⇒ 또는 `<form>` 태그 내 또는 ajax 호출 시 type이 명시적으로 GET으로 지정된 경우

  - POST 방식으로 서버 주소 호출하기

    ⇒ 대표적으로 신규 데이터를 등록하거나 신규가 아니더라도 브라우저에서 데이터를 서버로 전송하는 경우 사용한다.

  - PUT 방식으로 서버 주소 호출하기

    ⇒ 기존 데이터를 수정 목적으로 브라우저에서 데이터를 서버로 전송할 때 사용(POST로 대체 가능함!)

  - DELETE 방식으로 서버 주소 호출하기

    ⇒ 기존 데이터를 삭제 목적으로 서버 기능을 호출하는 경우 사용(GET으로 대체 가능함!)

---

🌟 클라이언트(브라우저)에서 URL 주소(웹페이지, 데이터 호출 OPEN API)를 호출하는 방식(메소드)에 따라 해당 호출 세부 라우팅 주소와 호출방식이 일치하는 라우팅 메소드가 호출된다.

---

💡 Q1. `http://www.naver.com/member/list` 를 호출했다. 이 주소는 서버에 어떤 노드 라우팅 메소드가 호출될까?

→ member.js 라우터 파일 내에 list라는 라우팅 주소를 GET 방식으로 제공하는 라우팅 메소드를 호출한다.(app.js 내 memberRouter의 기본 주소가 naver.com/member로 설정이 되어 있다) `router.get("/list", function(req, res) {});`

💡 Q2. 아래 소스에서 `form` 구문 내 사용자가 입력한 데이터는 어떻게 호출될까?

```html
<form method="post" action="/member/regist" >
	이름: <input type="text" value="BO" />
	<input type="submit">전송</input>
</form>
```

→ member.js 라우터 파일 내에 regist라는 라우팅 메소드를 찾는데 post로 정의된 라우팅 메소드가 실행된다. `router.post("/regist", function(req, res) {});`

## 2. URL 주소 기반 데이터 전달 방식의 차이

브라우저에서 데이터를 서버로 전달하는 방식에는 get, post, put, delete, patch 등 대부분이 가능하지만, URL 주소(GET 방식)에 문자열로 데이터를 전송하는 방식은 아래와 같다.

- QueryString 방식: http://www.naver.com/produce?pid=1&name=computer

  ⇒ 중요하지 않은 정보를 전송할 때 사용한다.

  └ NODE에서 해당값 추출하기: `const productId = req.body.pid;`

- Parameter 방식: http://www.naver.com/produce/1

  ⇒ 라우팅 메소드의 주소란에 정의된 와일드카드 :pid 변수명을 정의하고 해당 변수명을 req.params.변수명으로 추출한다.

  └ NODE에서 해당값 추출하기: `const productId = req.params.pid;`

### 3. 동기 방식과 비동기 방식의 차이

- 동기 방식 코딩 예시

  ```jsx
  router.get("", function(req, res) {
  	Member.create().then(처리 결과는 여기서 처리).catch();
  }
  ```

  해당 동기 처리 기능이 완료되어야 다음 단계 코드가 실행된다. 실행 결과를 바로 반환받는 코드 사용은 불가능함.

- 비동기 방식 코딩 예시

  ```jsx
  router.get("", async (req, res) => {
  	await Member.create();
  	const result = await Member.update();
  	await Member.destroy();
  }
  ```

  반드시 라우팅 메소드의 반환함수를 async 화살표 함수로 변경해야 한다.

  DB 처리 부분에 await으로 시작해서 데이터 처리가 이루어져야 한다.

  실행결과를 바로 반환받을 수도 있다.

  여러 개의 DB 명령을 연속으로 동시에 실행할 수 있다.
