# 브라우저 객체 모델(BOM)

브라우저 객체 모델(BOM: Browser Object Model)은 웹 브라우저가 가지고 있는 모든 객체를 의미한다.

아래 그림과 같이 최상위 객체는 window이고 그 아래로 frames, history, location, navigator, screen, document 객체가 있다.

![BOM](https://user-images.githubusercontent.com/65386533/111587987-4bedf780-8806-11eb-918f-6f87bf5d5c74.png)

우리가 지금까지 많이 사용하였던 document 객체도 사실은 window 객체의 자식 객체이다.

- **window** : 메인 브라우저 윈도우
- **window.frames** : 브라우저 윈도우를 차지하고 있는 프레임들
- **window.history** : 사용자가 방문한 URL 저장
- **window.location** : 현재 URL
- **window.navigator** : 브라우저에 대한 정보(버전 번호와 같은 정보들)
- **window.screen** : 사용자 화면
- **window.document** : 메인 브라우저에 표시된 HTML 문서

---

- 참조

  <HTML5+CSS3+JavaScript로 배우는 웹 프로그래밍 기초> 천인국 저.

  [https://www.splessons.com/lesson/javascript-bom/](https://www.splessons.com/lesson/javascript-bom/)
