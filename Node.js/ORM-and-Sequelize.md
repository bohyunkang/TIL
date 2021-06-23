# ORM과 Sequelize 프레임워크

## ORM이란? 

- Object Relational Mapping의 약자
- ORM은 객체지향 프로그램 언어를 이용해 DB 프로그래밍을 할 때 RDBMS의 데이터베이스 구조 및 데이터를 관리하기 위한 SQL을 잘 모르더라도 프로그래밍적인 방법을 이용하기 쉽게 데이터 모델 객체 기반으로 DB 프로그래밍을 할 수 있는 환경을 제공한다.

### 모델

- 백엔드 개발 언어로 데이터의 구조를 표현한다.
- 데이터의 구조를 표현한 모델의 객체(인스턴드화=메모리에 데이터 구조를 올리는 방법) 모델에 데이터를 담을 수 있다.
- 데이터가 담긴 모델 객체를 레이어 간 데이터를 이동시킬 수 있다.

### 모델의 종류

1. Data 모델 : 데이터 베이스의 테이블과 mapping되는 백엔드 측 모델 클래스를 말한다.
    - 테이블 구조와 동일한 데이터 구조를 백엔드 개발 언어로 클래스(객체)로 정의한다.
    - 데이터 모델에는 데이터를 담을 수 있다.
2. Entity 모델 : 비지니스 관점에서 정보의 구조를 표현한 모델을 말한다.
    - ex) age 속성 추가, 테이블에는 없는 속성임
3. DTO(Data Transfer Object) 모델 : 데이터의 일괄 저장 및 이동을 위해 추가로 만드는 모델을 말한다.
    - 목적은 많은 데이터를 한번에 빠르게 전달할 수 있게 전송 전용 모델 클래스를 만든다.
    - 예를 들어 Data모델에 `userModel`과 `productModel`이 있다고 쳤을 때, DTO 모델로는 `userProductModel`을 만들어 그 안에 `userModel`과 `productModel`의 속성을 모두 넣어놓고 한번에 전송을 한다.
4. View 모델 : 화면전용 모델을 말한다.
    - 예를 들어, `userViewModel`을 만들어, 그 속에는 `userModel`이라는 데이터 모델과 age 등 추가적인 속성을 더 넣어준다.

### ORM 프레임워크 Sequelize 환경 구성하기

```bash
// ORM 프레임워크인 sequelize 패키지와 MySQL 지원 패키지인 MySQL2를 동시 설치
npm install sequelize mysql2

// sequelize 명령어 사용을 위한 sequelize-cli 패키지 전역으로 설치
npm install –global sequelize-cli

// 현재 Node Express 프로젝트에 ORM 구현을 위한 추가 프로젝트 템플릿(예제소스)를 추가
sequelize init
```

![sequelize](https://user-images.githubusercontent.com/65386533/123097140-ca43e080-d46a-11eb-9031-b6f53e277b47.PNG)

1. `sequelize init`을 하고 나면 사진과 같은 폴더가 추가 생성된다.

    📁 config : DB 연결 정보 환경 설정 파일

    📁 models : 테이블과 mapping되는 모델 정의 파일 보관 및 모델 기반 프로그래밍 처리 모듈 제공

    📂 index.js

    📁 migrations

    📁 seeders

2. 📁models > index.js를 수정한다. (자동 생성 코드에는 에러가 있으므로)

    ```js
    // Node 프레임워크에서 제공하는 폴더 경로/파일 관리 객체 참조
    const path = require("path");

    // Sequelize ORM 프레임워크 패키지 참조
    const Sequelize = require("sequelize");

    // DB 연결정보가 있는 config파일에서 development항목의 DB정보를 조회한다.
    // .env = 개발모드 환경설정 파일로, 전역적으로 사용해야 하는 공통 정보를 저장해놓는 파일을 말한다.
    const env = process.env.NODE_ENV || 'development';

    // 관련정보 수정
    const config = require(path.join(__dirname,'..','config','config.json'))[env];

    // DB관리 객체 생성
    const db= {};

    // DB연결정보로 시퀄라이즈 ORM 객체 생성
    const sequelize = new Sequelize(config.database,config.username,config.password,config);

    // DB 처리 객체에 시퀄라이즈 정보 맵핑처리
    // 이후 DB객체를 통해 데이터 관리가능해짐
    db.sequelize = sequelize;
    db.Sequelize = Sequelize;
    db.Config = config;

    // DB관리객체 모듈 출력
    module.exports = db;
    ```

3. `npm install dotenv` env 패키지 설치 후 프로젝트 내에 .env 파일을 만들어 `NODE_ENV=development` 를 적는다.
4. 📁 config > config.json에 development 객체 수정

    ```json
    "development": {
    	"username": "mySQL유저이름",
    	"password": "비밀번호",
    	"database": "DB이름",
    	"host": "127.0.0.1",
    	"dialect": "mysql"
    }
    ```

5. `app.js` 파일에 시퀄라이즈 모델 모듈을 추가하고, MySQL과 연동처리를 해준다.

    ```js
    // .env 설정 기능 참조 적용
    require("dotenv").config();

    // 시퀄라이즈 ORM 객체 참조하기
    var sequelize = require("./models/index").sequelize;

    // 시퀄라이즈 ORM 객체를 이용해 지정한 MySQL 연결 동기화하기
    sequelize.sync();
    ```

6. MySQL 연동이 완료된다.
