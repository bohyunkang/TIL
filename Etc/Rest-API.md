# REST API란

웹 어플리케이션을 만들려면 데이터베이스에 정보를 입력하고 읽어올 수 있어야 한다. 그런데 웹 브라우저에서 데이터베이스에 직접 접속하여 데이터를 변경한다면 보안상 문제가 될 수 있다. 그래서 REST API를 만들어서 사용한다.

![rest-api](https://user-images.githubusercontent.com/65386533/119801311-8cf63c80-bf18-11eb-814c-c4f3c76f710d.png)

▲ REST API의 역할

클라이언트가 서버에 자신이 데이터를 조회 · 생성 · 삭제 · 업데이트하겠다고 요청하면, 서버는 필요한 로직에 따라 데이터베이스에 접근하여 작업을 처리한다.

### HTTP 메소드

REST API는 요청 종류에 따라 다른 HTTP 메서드를 사용한다. HTTP 메서드는 여러 종류가 있으며 주로 사용하는 메서드는 다음과 같다.

- GET: 데이터를 조회할 때 사용한다. (= 받겠다)
- POST: 데이터를 등록할 때 사용한다. 인증 작업을 거칠 때 사용하기도 한다. (= 보내겠다)
- DELETE: 데이터를 지울 때 사용한다. (= 지정한 서버의 파일을 삭제하겠다)
- PUT: 데이터를 새 정보 통째로 교체할 때 사용한다. (= 넣겠다)
- PATCH: 데이터의 특정 필드를 수정할 때 사용한다. (= 부분적으로 넣겠다)

![http-method](https://user-images.githubusercontent.com/65386533/119801300-8a93e280-bf18-11eb-80e3-c1db6518d9ae.png)

> 💡 **PUT과 PATCH의 차이**

> **PUT**: 리소스의 모든 것을 업데이트한다.

> **PATCH**: 리소스의 일부를 업데이트한다.

> 즉, DB의 특정 칼럼만 수정하고자 한다면 PATCH를 사용하고 그게 아니라 전부 다 수정되어야 하는 사항이라면 PUT을 사용하면 된다.

## Rest란

HTTP 통신에서 어떤 자원에 대한 **CRUD 요청을 Resource와 Method로 표현**하여 특정한 형태로 전달하는 방식

- "Representational State Transfer"의 약자
- 어떤 자원에 대해 CRUD(Create, Read, Update, Delete) 연산을 수행하기 위해 URI(resource)로 요청을 보내는 것

## RESTful API란

- 이러한 REST 기반의 API를 웹으로 구현한 것이 RESTful API인데, 예를 들어 우리는 게시글을 작성하기 위해 http://localhost:3000/api/place 라는 URI에 POST 방식을 사용하여 JSON 형태의 데이터를 전달할 수 있다. 위와 같이 CRUD 연산에 대한 요청을 할 때, 요청을 위한 Resoure(자원, URI)와 이에 대한 Method(행위, POST) 그리고 Representation of Resource(자원의 형태, JSON)을 사용하면 표현이 명확해지므로 이를 REST라 하고, 이러한 규칙을 지켜서 설계된 API를 REST API 또는 RESTful한 API라고 한다.

### RESTful API의 구성 요소

- Resource 서버는 Unique한 ID를 가지는 Resource를 가지고 있으며, 클라이언트는 이러한 Resource에 요청을 보낸다. 이러한 Resource는 URI에 해당한다.
- Method 서버에 요청을 보내기 위한 방식으로 GET, POST, PUT, PATCH, DELETE가 있다. CRUD 연산 중에서 처리를 위한 연산에 맞는 Method를 사용하여 서버에 요청을 보내야 한다.
- Representation of Resource 클라이언트와 서버가 데이터를 주고 받는 형태로 JSON, XML, TEXT, RSS 등이 있다. 최근에는 key-value를 활용하는 JSON을 주로 사용한다.

#### 💡 POSTMAN 활용하기

- POSTMAN이란, 프론트엔드/백엔드 구분되어 개발되는 개발 및 서비스 환경 아래 백엔드에서 제공해주는 각종 REST API를 손쉽게 테스트할 수 있는 환경을 제공하는 서비스를 말한다.
- 프론트엔드(클라이언트)에서 데이터를 저장, 수정, 삭제 등 모든 형태의 데이터 호출 및 반환값 확인을 할 수 있다.

---

- 참조

  [https://meetup.toast.com/posts/92](https://meetup.toast.com/posts/92)

  [https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html](https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html)

  [https://develop-log-sj.tistory.com/6](https://develop-log-sj.tistory.com/6)

  <리액트를 다루는 기술> 김민준 저.
