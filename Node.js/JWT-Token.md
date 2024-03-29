# JWT(Json Web Token)

### 1. Open API(RESTful Backend Technology) 사용 허가
  - 무료로 제공(100% 개방)하기에는 사용하는 리소스(서버, 네트워크, 운영비용) 비용이 과다 발생할 수도 있다.
  - 적대적 행위를 하는 해커집단에 의해 쉽게 해킹되어 손실을 보거나 악용될 소지가 100%
  - 그래서 어떤 오픈 API 서비스도(사용자 인증=허가된+확인된 사용자에게만 서비스 제공) 인증 기반 API 서비스를 제공한다.
  - 대한민국 공공데이터 포털도 인증 후 발급된 인증코드 기반 API 호출 및 사용이 가능하다.
### 2. 인증(Authentication)과 권한(Authorization) 적용

  #### 2.1 인증

  - 사용자를 인식하는 과정: 회원가입과 로그인 과정을 걸쳐 인증을 실시한다.
  - 인증된 사용자에게는 인증키(토큰)를 제공한다.
  - 인증된 사용자는 인증키(토큰)를 가지고 제공된 서비스를 이용한다.

  #### 2.2 권한

  - 서비스 또는 시스템에서 제공되는 기능(메뉴)를 인증된 사용자마다 구분해서 사용권한을 부여한다.
  - 버튼, 정보 제공에 대한 권한도 조정이 가능하다.
### 3. JWT 토큰 사용하는 경우의 수
  - 사용자 인증 수단: 백엔드에서 발급한 JWT  사용만 하는 경우(백엔드 서비스 호출 시 발급받은 JWT 토큰을 전달만 하는 것)
  - 시스템과 시스템 간 정보 공유 수단: 발급받은 토큰을 오픈해서 해당 토큰 내에 JSON 값을 사용하는 경우
  
  ⇒ 토큰을 만들 때 인증키를 기반으로 토큰을 만들고 토큰 내의 정보를 추출할 때도 만들 때 사용한 인증키 값을 이용해서 추출한다.

### JWT 토큰 주의 사항
  - 진짜로 중요한 정보(개인, 금융, 의료 등)는 JWT 토큰에 담지 않는다.
