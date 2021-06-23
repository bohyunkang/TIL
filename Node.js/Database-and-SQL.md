# 데이터베이스와 MySQL, SQL

## 주요 DB 객체 소개

1. Tables : 데이터가 실제 저장되는 객체
2. Views : 가상 테이블
3. StoredProcedures(SP) : DB에서 프로그래밍 방식으로 Data를 처리할 때
4. Functions(SQL) : DB에서 사용하는 재사용 가능한 SQL 기능 함수

### Table

- ERD(Entity-Relationship Diagram) : 테이블간의 관계를 그림으로 정리해놓은 것.(여기서 Entity는 Table을 의미함)
- Column(열)과 Row(행)로 구성되어 있다.
- Column : 관리하고자 하는 정보의 속성
    - 속성(property) 또는 어트리뷰트(attribute)라고도 한다.
    - ex) 사용자 아이디, 이름, 전화번호, 메일주소, 가입일시, 가입상태 등..
- Row : 관리되는 속성들의 집합
    - 한 명의 사용자와 관련된 속성 값들의 집합
    - ex) bbo, 강보현, 010-0000-0000, abcd1234@abc.com, 2021-06-23, 1
    - 한 명의 사용자 정보는 설계된 속성들의
- 컬럼은 데이터 관리항목이며, 로우는 각 항목으로 구성된 실제 데이터를 말한다.
- 데이터 무결성 : RDBMS는 각종 제약(Constraints) 조건들을 이용해 데이터 무결성을 유지한다.
    - 대표적인 RDBMS의 제약 사항으로는,
        1. 데이터 타입 지정: 입력되는 데이터가 지정된 형식이 아니면 등록이 되지 않게 한다.
        2. 데이터 최대 길이: 지정된 최대 길이 이상을 저장하려고 하면 에러를 발생시켜 등록되지 않게 한다.
        3. NULL 옵션: NOT NULL(=NN, 반드시 데이터가 입력되어야 함), NULL(데이터가 입력되지 않아도 됨).
- DB Table의 컬럼 값으로서의 Null이란?
    - Null과 빈 문자열은 다르다.
    - Null: 값이 들어온 것도 아니고 안 들어온 것도 아닌 데이터가 아예 들어오지 않은 초기 상태
    - 빈 문자열: 빈 문자열도 실제 데이터는 존재하는 것이다. Null 상태가 아니며, 빈 공백 데이터가 들어가 있는 상태를 말한다.

#### Table Column Type

1. Column 주요 타입
    - INT : 정수형
    - CHAR(10) : 문자열 고정길이
        - CHAR(1) = 1bit = 1 또는 0을 입력할 수 있음
    - VARCHAR(10) : 문자열 가변길이
        - VARCHAR(1) = 4bit = 알파벳 하나를 입력할 수 있음

        💡고정길이 VS 가변길이?

        선언할 때 CHAR(10)로 고정길이 선언을 하게 되면 사용자가 입력한 데이터가 1개이든 10개이든 10개의 자리를 다 차지하고, 선언할 때 VARCHAR(10)로 가변길이를 선언하게 되면 사용자가 입력한 데이터가 1개만 사용하게 되면 안 쓰게 되는 9개의 공간을 반환해주는 가변성이 있다.

        그렇기 때문에 사용자가 입력한 데이터의 길이가 정해져 있는 경우는 CHAR를 사용하면 좋고, 사용자가 입력한 데이터의 길이를 모르는 경우 VARCHAR를 사용하는 게 좋다.

        💡NCHAR? NVARCHAR?

        유니코드 form으로 정확하게 length를 저장하고 싶은 경우 사용한다. 예를 들어 VARCHAR(10)를 하게 되면, 영어 10개를 저장할 수 있지만, 한글은 영어의 2배로 자리를 차지하기 때문에, 동일하게 10개를 입력하고 싶다면 NVARCHAR로 설정하면 좋다. N은 유니코드를 의미하기 때문에, 영어가 아닌 다른 나라 언어들도 사용하게 된다면 NCHAR나 NVARCHAR를 사용하는 것이 좋다.

    - TEXT : 긴 문자열  1000자리 이상 문자열 생성
        - 유니코드가 들어가면 NTEXT를 사용해야 한다.
    - TINYINT : -127~ 128 까지 정수형
    - DATETIME : 날짜시간
    - TIME : 시간형
2. Column 주요 키워드
    - NULL & NOT NULL 의 차이
    - AUTO_INCREMENT : 자동 숫자 증가 데이터 생성
    - UNSIGNED : 숫자 자료형인 경우 음수 무시 0~ 해당 자릿수까지
    - ZEROFILL : 숫자의 자릿수 고정, INT(4) 1이 입력된경우 0001
        - 대부분의 숫자형 데이타 타입은 옵션으로 ZEROFILL이란 것을 가지게 되는데, 화면에 숫자를 표시할 때 숫자 왼쪽에 0을 붙이라는 뜻입니다. 즉 INT(3) ZEROFILL이라고 정의된 필드에서 저장된 값 3을 불러온다면 "003"과 같이 표시됩니다.
3. 주요 Key
    - PRIMARY KEY : 유일키
        - 중복되서는 않되는 키
        - 로우 간의 데이터를 구분하기 위해서 설정함
        - 모든 테이블에는 1개 이상의 PK Column이 존재해야 한다.
    - UNIQUE KEY : 유니크 키
    - Foreign Key : 참조키
        - 외부 테이블의 키를 참조하는 경우를 말한다.
    - INDEX KEY : 인덱스 키
        - 데이터를 찾을 때마다 스캔을 하지 않고, 좌표를 찍어 빠르게 데이터를 찾을 수 있다.
        - NON CLUSTERED INDEX KEY
        - CLUSTERED INDEX


## MySQL 사용법

1. Workbench 클라이언트 툴로 MySQL(RDBMS)에 접속한다.
2. 데이터베이스(Schema)를 생성한다. (Schemas 탬에서 우클릭 > Create Schema 클릭)

    1) 시스템 데이터베이스 : RDBMS 시스템을 관리하기 위한 시스템 DB (🚫삭제하면 안됨!🚫)

    2) 사용자 데이터베이스 : 사용자가 만든 DB

3. Create Schema
    - Schema Name(DB Name) : 영문
    - Charset : 저장할 데이터의 문자 형태에 맞게 지정을 하면 된다. (한글이라면 UTF-8 선택이 기본)
        - Charset 지정을 제대로 해주지 않으면
    - Collation : 저장할 데이터 문자의 정렬 방식 지정 (UTF-8-UNICODE-CI : 유니코드 문자에 대한 정렬)
        - Collation 지정을 따로 해주지 않으면, 유니코드에 대한 오름차순, 내림차순 적용이 되지 않는다.
4. 지정을 다 하고 Apply를 누르면 아래와 같은 DB 생성 SQL 구문이 생성된다.

    ```sql
    CREATE SCHEMA `new_schema` DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci ;
    ```

5. DB가 생성된다.


## SQL

- Structured Query Language의 약자로 '구조화된 질의 언어'라는 뜻
- 데이터베이스 대상 데이터 처리를 위한 언어

### CRUD SQL 명령어

- CREATE : INSERT INTO 스키마명.테이블명 (컬럼1, 컬럼2) VALUES (값1, 값2);
- READ : SELECT * FROM 스키마명.테이블명;
- UPDATE : UPDATE 스키마명.테이블명 SET 컬럼명 = "값";
- DELETE : DELETE 스키마명.테이블명;

🌟 데이터베이스에 특정 작업을 하고자 하면 SQL 쿼리 구문을 DB에 전달하고 해당 SQL 쿼리 구문이 DB에서 실행되어 결과값이 반환된다.

### 간단한 사용법

```sql
# MySQL 인라인 주석 처리 예시

/*
영역을 지정해서 영역 주석처리 예시
*/

# DATABASE 사용 지정하기
# use 스키마명
use new_schema;

# users 테이블 조회하기
select * from users;

# 새로운 DB(schema) 생성하기
# create database 스키마명
create database test_db;

# 지정한 DB 사용하기
use test_db;

# 테이블 생성하기
use new_schema;

CREATE TABLE `new_schema`.`members`(
`memberid`	VARCHAR(50)	NOT NULL,
`membername` NVARCHAR(100) NOT NULL,
`gender` CHAR(1) NOT NULL,
`birthday` VARCHAR(8) NULL,
`entrydated` DATETIME NOT NULL,
PRIMARY KEY(`memberid`)
);

# 테이블 구조 변경하기
ALTER TABLE `new_schema`.`members`
CHANGE COLUMN `gender` `gender` VARCHAR(1) NULL;

# 기존 테이블 신규 컬럼 추가하기
ALTER TABLE `new_schema`.`members`
ADD COLUMN `age` INT NOT NULL;

# 테이블 컬럼 구조 보기
DESC `new_schema`.`users`;

# 테이블 삭제하기
DROP TABLE `new_schema`.`members`;

# 테이블에 데이터 입력하기
SELECT * FROM users;

INSERT INTO users(memberid, pwd, membername, email, telephone, birthday, entrydate, entrystatecode)
Values
('wowthing', '12222111', '아웅산', '3331ddd@naver.com', '010-4566-5678', '20000901', sysdate(), 1);

# 기존 데이터 수정하기
# 해당 테이블의 지정한 컬럼의 모든 로우 값을 일괄 변경한다
UPDATE users SET telephone = '010-3456-7890', birthday = '000000', entrydate=sysdate();

# Save mode가 걸려있을 때는 WHERE 조건을 넣어줘야 UPDATE나 DELETE가 가능하다
UPDATE users SET telephone='010-3456-7890', birthday='000000', entrydate=sysdate() WHERE memberid='bbo';
SELECT * FROM users WHERE memberid='bbo';

# 테이블 데이터 삭제하기
DELETE FROM users WHERE memberid='wowthing';
SELECT * FROM users;

# 데이터 조건별 조회
SELECT * FROM users WHERE telephone='010-4444-5678';

SELECT * FROM users WHERE 
telephone='010-4444-5678' and birthday='19590801';
SELECT * FROM users WHERE 
telephone='010-4444-5678' or membername='ohhappy';

# 데이터 목록 정렬해서 가져오기
# 오름차순(Ascending) 컬럼 정렬 조회: 1,2,3,4, a,b,c,d, 가,나,다,라 ...
SELECT * FROM users
WHERE entrystatecode=1
ORDER BY memberid asc;

# 내림차순(Descending) 컬럼 정렬 조회: 4,3,2,1, z,y,x, 하,파,차 ...
SELECT * FROM users
WHERE entrystatecode=1
ORDER BY memberid desc;
```
