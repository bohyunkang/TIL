# 입문자를 위한 웹팩- 원데이클래스를 듣고 정리한 내용

## 강의 내용
- 웹팩이란: 흩어져 있는 파일들을 압축시키는 도구(번들러), 즉 코드를 압축시켜주는 역할을 한다.
- 모든 프로젝트 파일들은 src 폴더에서 진행된다.
- 복수의 변수를 export하고 싶을 땐 ```export { btnChange }; ``` 와 같이 중괄호를 사용해야하나,
  단수는 ```export default btnChange``` 라고 해도 됨. (import 작성 방식도, 단수이냐 복수이냐로 좌우된다.=똑같아야함)
- 캐시 무효화 : 브라우저가 캐시를 저장해서 변경사항이 있어도 바로 반영되지 않는 경우를 방지하기 위해 하는 작업
- 자바스크립트 공부하듯이 깊게 팔 정도의 공부는 아니며, 공식 문서를 보고 커스터마이징하는 식으로 작업하는 것을 추천한다.

### 추가로 더 알아볼 내용
- Parcel : 번들러 중 하나, 사용 방법을 알고 있으면 좋음
- Next.js 사용 중. 서버사이드 렌더링 방식. 일반 리액트와는 조금 다름.
- Atomic design : 컴포넌트 관리하는 기법 중 하나
- Ant design: less로 디자인 되어있는 템플릿.(react framework 디자인 UI)
- styled component