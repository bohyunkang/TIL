# CORS 이슈

> 개발을 하다보면 아래 사진과 같은 이슈를 맞닥뜨리게 되는데.. 이를 CORS 이슈라고 한다!

<img src="https://user-images.githubusercontent.com/65386533/122945754-c1440800-d3b3-11eb-9319-79cb8ded23ad.PNG" alt="cors-1" width="100%" /> ▲ 로컬에 있는 파일을 불러왔을 때
<img src="https://user-images.githubusercontent.com/65386533/122945762-c1dc9e80-d3b3-11eb-9139-e65bc0fe7738.PNG" alt="cors-2" width="100%" /> ▲ 서버에 연결된 파일을 불러왔을 때

## CORS란?
- CORS는 Cross-Origin-Resource-Sharing의 약자
- 도메인이 다른 웹 브라우저에서 API 요청하게 되면 API 데이터 제공자가 기본적으로 데이터를 제공하지 않는데, 이때 발생하는 이슈이다.
- 즉, 서로 다른 도메인 간의 데이터를 공유하게 되면 발생하는 문제

## 해결방법
- 서버에서 클라이언트로 응답을 보낼 때, 응답 헤더에 Access-Control-Allow-Origin이라는 헤더를 넣어주어 서버 도메인과 다른 클라이언트 도메인의 요청을 허락하게 한다.

### 💡 Node.js에서의 CORS 해결 방법
1. API를 제공하는 프로젝트 내에 `npm i cors` 명령어로 CORS 패키지를 설치한다. 
2. 프로젝트 내 app.js 파일에 CORS 패키지를 적용한다.

**[모든 도메인을 허용하고자 할 경우]**

📂 app.js

```jsx
// cors 노드 패키지를 참조한다.
const cors = require('cors');

// 노드 어플리케이션에 cors 기능 적용
app.use(cors());

```

**[특정 도메인만을 허용하고자 할 경우]**

📂 app.js

```jsx
// cors 노드 패키지를 참조한다.
const cors = require('cors');

// 특정 도메인 주소만 CORS 허용하기
const options = {
	origin: "http://localhost:5000",
	credentials: true,
	optionsSuccessStatus: 200,
};
app.use(cors(options));
```
