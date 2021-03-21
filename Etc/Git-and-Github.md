# Git Github

## 1. Git

### 1.1 Git이 필요한 이유

- 끊임없는 수정본의 문제점
1. 변경 사항 파악 어려움 : 무엇을 바꿨는지?
2. 되돌리기 어려움 : CTRL+Z의 한계, 삭제 파일 추적 불가
3. 용량 차지 : N개가 있다면 N배
4. 협업이 어려움 : 누가 언제 무엇을 건드렸나?

### 1.2 Git의 기본 idea

- VCS(Version Control System, 버전 관리 시스템) : 파일의 변경 사항을 저장하고, 원하는 시점의 버전을 다시 꺼내올 수 있는 시스템
- Snapshot : 특정 시점에서 파일의 상태 (현재 상태의 모든 정보)
- Delta : 파일 이전 상태와 비교한 변경 사항
- DVCS(분산 버전 관리 시스템), 저장소의 파일 시스템 전체를 스냅샷으로 취급하며 변경하지 않은 파일은 새로 저장하지 않고 링크만 저장하는 방식.

![git](https://user-images.githubusercontent.com/65386533/110748415-04ee8800-8283-11eb-8243-ef6b4f1b9930.png)

### 1.3 Git Flow

#### 1.3.1 Git 사용 선언

**선언과 저장소(Initialization & Repository)**

- 흔히 **Repo(레포)**라고 부름
- 사용자가 변경한 모든 내용을 추적하는 공간(하나의 **디렉토리**로 볼 수 있음)
- 현재 상태, 변경 시점, 변경한 사용자, 설명 텍스트 등 저장

**Git은 이제 Local에서 가능한 상태**

- 모든 것은 local에서 저장 및 버전 관리 가능 (서버가 죽어도 상관없음)

- 원격 서버에는 나중에 올릴 수 있음. **즉, Wi-Fi 없는 환경에서도 작업 가능**

**Git은 데이터를 추가만 할 수 있다.**

파일 삭제 == 삭제 기록 추가 ⇒ -1 == +(-1)

데이터베이스에 **저장한 순간부터는 삭제까지 추적**

**Git은 파일을 추적하지 않는다.**

대신 파일의 내용 단위로 추적하고, **각 문자와 줄을 추적한다.**

빈 디렉토리는 추적하지 않는다.

💡 **어떤 파일이 상태에 따라 계속 바뀌고, 딱히 저장할 필요가 없다면!?**
저장하지 말고, 굳이 관리하고 싶지 않은 파일은 따로 처리하자.
Untracked ↔ Tracked

#### 1.3.2 파일 상태

Git이 추적하는 파일은 총 **3가지 상태**로 구분된다.

1. Unmodified : 이전 버전과 비교하여 **수정된 부분이 없는** 상태.
2. Modified : 이전 버전과 비교하여 **수정된 부분이 있는** 상태.
3. Staged : 저장(커밋)을 위해 준비된 상태 (스테이징, Staging)

💡 **여기서 커밋(Commit)이란?**
 데이터베이스를 비롯한 개발 환경에서 변경 사항을 확정 짓는 일.
주로 지금까지 개인 작업한 내용을 공동의 작업환경 또는 저장소에 공유할 때 쓰인다.

#### 1.3.3 변경 사항 선택

스테이징(Staging)을 하면 커밋하고 싶은 파일 선택

커밋(Commit)을 하면 새로운 버전으로 업로드

💡 **Q. 이때, 스테이징은 업로드를 2번 하는 과정으로 보이는데, 왜 굳이 한번 더 과정을 거치는걸까?**

커밋 전, 스테이징이 필요한 이유

1) 여러 작업 중, 일부분만 커밋해야 할 경우

작업을 하다가 특정한 수정 사항을 업로드해야 하는 상황에서 본인은 그 기능 뿐만 아니라 다른 부분도 수정 작업 중 ⇒ 이때 바로 커밋을 하면 현재 작업 중인 파일을 다 내리고, 에러를 수정하고, 내린 파일들을 다시 가져와서 작업해야 함. 하지만 스테이징을 거치면 개발한 것들만 올리고, 현재 작업 중인 것은 계속 컴퓨터에서 작업하면 됨!

2) 커밋 전 상태를 수정 또는 체크할 경우

커밋을 할 때는 수정 사항에 메시지를 남기는데, 메세지에 오타를 내거나, 파일을 잘못 커밋하는 경우는 충분히 발생할 수 있다. 서버에서 직접 수정하는 방법은 위험하기때문에 스테이징을 통해서, 업로드하고 싶은 파일과 하고 싶지 않은 파일을 나눠 커밋한다. 즉, 업로드하고 싶은 파일과 하고 싶지 않은 파일을 커밋하기 전에 관리해줄 수 있는 공간이 Stage인 것이다.

#### 1.3.4 커밋 완료

커밋을 하면 이상한 40자리 숫자 + 알파벳 조합이 생긴다.

이는 내용을 주소로 활용(Content-addressable Key-Value Storage)이다. 상태를 찾기 위해서는 Key가 필요하고, 버전의 주소, 내용(파일 구조) 등을 Hash 값으로 만들고 상태를 나타낸다. 여기서는 SHA1 해시 값을 사용하여 40자리로 표현한다.

💡 **여기서 Hash란?**
임의의 데이터를 고정된 크기의 데이터로 바꾸는 과정을 말한다.

*️⃣ 소프트웨어웨어 버전은 다음과 같이 개발된다.

![software-version](https://user-images.githubusercontent.com/65386533/110748416-05871e80-8283-11eb-84fd-e8cad46b9424.PNG)

[A].[B].[C]와 같이 보통 .으로 구분되는 3개의 숫자로 표현된다.

- [A] : Major : 이전 버전과 호환이 안되는 아예 새로운 상태
- [B] : Minor : 기능 추가, 변경
- [C] : Patch : 미미한 내부 에러 수정

## 2. Github

### ✅ README.md 작성 팁

1. 프로젝트 내용 (이미지, 로고)
2. 설치 방법
3. 코드 예제
4. 개발 환경 설정 방법
5. 기여 방법
6. 로그 변경
7. 크레딧
8. 라이센스
9. 연락처

### ✅ 저장이 싫다면? .gitignore

추적을 무시하고 싶다면 정규 표현식을 맞춰서 .gitignore 파일을 작성하면 된다.

### 2.1 명령어

- `git init`

    Git 초기화를 의미. 로컬에서 진행함.

- `git add [file]`

    [file]을 스테이지로 올림. 폴더나 전체도 가능함.

- `git commit -m "커밋 메세지"`

    간단한 설명과 함께 commit

- `git log`

    이전 commit 기록 살펴보기

- `git remote add origin [url]`

    origin이라는 이름으로 [url]과 연결. 꼭 origin이 아니어도 원하는 이름으로 연결해도 된다.

- `git push origin master`

    원격 저장소 master branch에 업데이트

- `git clone [url]`

    원격 저장소에서 다운로드

- `git branch [name]`

    [name] branch 만들기. 기능을 병렬적으로 개발하고, 완성되기 전까지 기능별로 따로 관리.

- `git checkout [name]`

    [name] branch로 이동하기.

- `git merge [name]`

    [name] branch를 현재 branch로 합친다.

    합친 후에는 **push**를 해야 함.

- `git rebase master`

    base를 master로 re-base한다. 기준점을 바꿔서 깔끔하게 바꿀 수 있음.

    *단, 원격에 올린 커밋은 rebase하지 말 것*

- `git branch -d [name]`

    완료된 branch를 지우기. 완료했거나 필요가 없어진 branch는 정리한다.

- `git fetch`

    원격 저장소와 동기화. 원격에서 기록을 가져와 동기화한다. 원격 저장소의 내용만 알 수 있음.

- `git pull`

    원격 저장소와 동기화하고 merge하기. 원격에서 기록을 가져옴과 동시에 합치는 작업까지 수행한다.

- `git reset [option] [branch]`

    branch 이후 기록을 없애는 법. 커밋으로 프로젝트가 망하면, 원하는 커밋으로 reset하는 것.

    하지만 이 방법은 좋은 방법은 아니다.

    다수가 협업을 하는 상황에서 나 혼자만 reset을 해버리게 되면 동료들과 나와의 버전은 안 맞게 되기 때문이다.

- `git revert [branch]`

    수정한 기록도 남기는 법. 협업을 하는데, 커밋 로그를 함부로 지우면 서로의 버전에 문제가 생길 수 있으니, revert로 수정하는 기록 조차도 남기는 방법

- `git stash`

    현재 작업하고 있는 작업물을 따로 저장하기. 아직 커밋하기엔 부족한 상황인데, 빠르게 branch를 바꿔야 하는 상황이라면 stash를 사용해서 작업물을 따로 저장하자.

- `Fork`

    명령어는 따로 없음. 사이트에서 할 수 있음.

    다른 사람의 프로젝트에 기여를 하고 싶을 때, fork를 뜬 후, 수정을 하고, pull request를 보낸다.

---

- 참조

    [https://www.youtube.com/watch?v=YQat_D1C-ps](https://www.youtube.com/watch?v=YQat_D1C-ps)

    [https://stibee.com/api/v1.0/emails/share/LBqA6Yf5oJNhryPpeBHL1ATFgY1dsw==](https://stibee.com/api/v1.0/emails/share/LBqA6Yf5oJNhryPpeBHL1ATFgY1dsw==)