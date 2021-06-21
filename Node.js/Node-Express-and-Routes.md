# Node.js MVC 프로그래밍 이해, 라우팅 환경 구성

## Node Express 기반 사전 준비 사항

- `npm i -g express-generator` 명령어로 node express web 개발 프레임워크를 전역환경으로 설치한다.
- express-generator 패키지는 전역으로 최초에 한번만 설치한다.

---

1. 터미널 창에서 먼저 만드려고 하는 프로젝트의 위치를 확인한다.
2. `express [프로젝트명] --view=ejs` 를 터미널에 입력하여 프로젝트 생성한다.
3. `cd [프로젝트명]` 명령으로 디렉토리를 이동한다. (package.json 파일과 동일한 경로)
4. `npm i` 명령으로 패키지를 복원하여야 한다. (node_modules 파일 복원)
    - package.json 내에 정의된 dependencies 내 필수 노드 패키지를 노드 외부 저장소에서 다운로드 설치한다.
5. `npm start` 명령어로 프로젝트를 실행한다.
    - 실행완료되면 일반적으로 `http://localhost:3000`으로 접근할 수 있다.

## Node Express 명령어

- `npm i -g express-generator` : express-generator를 설치
- `express [프로젝트 이름] --view=pug` // `express [프로젝트 이름] --view=ejs`  : pug 혹은 ejs라는 뷰엔진 기술로 익스프레스를 활용하여 프로젝트에 필요한 모든 구조가 만들어짐
    - 그 후에는 package.json에 있는 모든 패키지들을 포함한 node_modules 폴더를 만들어야 한다. ( `npm i`를 활용)
    - view engine : 서버상의 view(html)를 조작하는 기술
    - express view engine = pug, ejs
    - pug: 별도의 html을 만드는 별도 pug 문법이 존재
    - ejs : 이미 작성된 html 코드 안에 뷰스크립트 코드로 html 제어 (실무에서는 ejs를 더 많이 씀)
- `npm start` : 프로젝트 실행
- `ctrl + c` : 디버그 모드 종료

## 라우팅 모듈의 기본

1. node express 객체를 참조한다. 노드에서 특정 패키지 또는 프레임워크의 기능을 불러와 사용하고자 할 때 `require`라는 명령어를 사용한다.

    `var express = require('express');`

2. express 객체의 router 메소드를 호출해 라우팅 객체를 생성한다. 라우틱 객체는 사용자 요청 URL에 대한 처리를 담당하는 각종 기능을 제공한다.

    `var router = express.Router();`

3. 상단에 정의한 router 객체를 모듈의 외부로 노출시킨다. 모듈내에 정의된 기능과 속성을 외부에 노출하려면 `module.exports`를 사용한다.

    `module.exports = router;`

4. get 방식으로 라우팅 호출 메소드를 정의한다.

    브라우저에서 데이터를 보낼 때 get 방식으로 전달하면 서버 라우팅 메소드에서도 get 메소드로 받아야 한다. 즉, post 방식이면 post로, put이면 put으로, delete이면 delete로 라우팅 메소드를 정의해야 한다.

    router.get 메소드는 웹 브라우저가 최초로 웹 페이지를 호출할 때 사용하는 방식이다.

    `get메소드('사용자가 호출할 URL 주소', URL 호출 시 실행될 기능(함수) 정의);`

    URL 호출 시 실행될 기능(함수)에는 파라미터로 전달되는 세 가지는: 

    1. 사용자 브라우저로부터 전달되는 각종 정보(req객체 = httprequest 객체)
    2. 함수가 실행후에 브라우저에 전달될 응답 객체(res 객체 = httpresponse 객체)
    3. 다른 동작을 실행하고자 할 때 선택적으로 추가하는 next 객체
5. response 객체의 `render()` 메소드는 뷰파일을 호출하고 view파일의 최종 HTML 결과물을 웹 브라우저로 전달한다.

    ```jsx
    router.get('/', function(req, res, next) {
    	res.render("");
    });
    ```

6. 라우터 파일을 만들면 반드시 app.js 파일 내에 라우팅 파일을 등록해줘야 한다. 등록 후, 기본 라우팅 주소를 정의한다.

    ```jsx
    // 게시글 관리 라우팅 파일을 참조한다.
    var articleRouter = require("./routes/article.js");
    .
    .
    .
    // 게시글 라우터의 기본 url 경로를 설정한다.
    // http://localhost:3000/article/~~~
    app.use("/article", articleRouter);
    ```

## 웹 브라우저와 웹 서버

http란, 웹 브라우저와 웹 서버가 통신할 때 지켜야 하는 약속을 의미한다. 

- HTTP Request 방식 (웹 브라우저에서 웹 서버로 데이터를 전송하는 방식)
    1. GET :  get 메소드 또는 querystring 방식으로 호출
    2. POST : 사용자 폼 등록
    3. PUT/PATCH : 사용자 폼 수정
    4. DELETE : 사용자 데이터 삭제


## MVC 패턴

- UI 디자인 패턴 중 하나
    - 가장 중요한 건 컨트롤러(C)
    - Node.js에서는 컨트롤러를 routes라 한다.
    - 라우팅이란? 사용자가 url을 입력하면 그 주소에 맞게 서버를 딱 찍어주는 것을 의미한다.
    - 모든 언어에서 같이 사용하는 패턴! (=공용어)
- 일반적으로 MVC 패턴으로 개발하는 경우
    - http://도메인주소/컨트롤러이름/액션메소드이름
    - http://도메인주소/라우팅파일이름/라우팅메소드이름
        - ex. `http://localhost:3000/article/list`


## 레이아웃(마스터) 페이지 개념 이해하기

💡 레이아웃(마스터) 페이지란? 모든 페이지에서 계속 공통적으로 나오는 컴포넌트를 말한다.

ex. 헤더, 사이드바, 푸터

그 부분을 물리적으로 하나의 페이지로 별도 관리를 하게 되면 유지 보수를 하기 훨씬 편하다.

💡 컨텐츠 페이지란? 공통된 페이지가 아닌 내용이 계속 바뀌어야 하는 페이지를 말한다.

**Work Flow**

1. 레이아웃 페이지 만들기
    - view폴더 안에 layout.ejs 파일을 생성 후 레이아웃 페이지 콘텐츠를 html로 작성
    - layout.ejs 파일 내 아래 코드를 추가

        ```html
        <!-- 실제 페이지 콘텐츠 영역 -->
        <!-- body 영역에 실제 컨텐츠 HTML이 추가되어 표현된다. -->
        		<%- body %>
        <!-- 콘텐츠 페이지에서 구현한 자바스크립트 소스가 아래 영역에서 노출된다. -->
        		<%- script%>
        ```

2. 컨텐츠 페이지 구성하기
3. 최종 브라우저로 전달할 때는 레이아웃 페이지와 콘텐츠 페이지가 병합되어 전달되게 하기

## 레이아웃 페이지 제공 노드 패키지 설치하기

`npm i express-ejs-layouts`


## nodemon 패키지 설치하기

nodemon은 작업 시 프로그램 소스를 반영 후 npm start와 ctrl + c 중지하는 행위를 자동화해 준다.

주로 개발모드에서만 사용하며 전역으로 설치하는 게 좋다.

`npm i -g nodemon`

설치 후에는 package.json 파일 내 어플리케이션 시작 명령어 옵션을 아래와 같이 수정해야 한다.

**before:** `"start": "node ./bin/www"`

**after:** `"start": "nodemon ./bin/www"`

🚫 주의! 개발 시에는 유용하나 서버 배포 시 다시 원래 상태인 `"start": "node ./bin/www"`로 바꿔줘야 한다.


## 노드 패키지 설치 명령어

1. `npm init` : package.json 파일 생성
2. `npm i` : package.json 파일 내 dependency 목록에 정의된 각종 패키지를 프로젝트 폴더 내 node_modules 내 설치 또는 복원
3. `npm install [패키지명] // npm i [패키지명]` : 프로젝트 폴더 내 node_modules 내부에 설치
4. `npm i -g [패키지명]` : 데스크탑의 전역 공간에 노드 패키지 설치 사용
💡 package-lock.json : 패키지의 의존관계(종속성)에 대한 정보는 package.json이 아닌 package-lock.json에 저장됨
