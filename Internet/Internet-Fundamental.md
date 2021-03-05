# Internet Fundamental

## 1. 인터넷은 어떻게 작동될까?

### 1.1 인터넷의 정의

인터넷(Internet)은 컴퓨터로 연결하여 TCP/IP(Transmission Control Protocol/Internet Protocol)라는 통신 프로토콜을 이용해 정보를 주고 받는 컴퓨터 네트워크이다.

**💡 여기서 통신 프로토콜이란?**
통신 프로토콜은 통신 규약이라고도 불리우는데, 컴퓨터나 원거리 통신 장비 사이에서 메세지를 주고 받는 양식과 규칙의 체계이다.

### 1.2 작동원리

인터넷은 웹의 핵심적인 기술이다. 인터넷의 가장 기본적인 것은, 컴퓨터들이 서로 통신 가능한 거대한 네트워크라는 것이다.

#### 1.2.1 단순한 네트워크

두 개의 컴퓨터가 통신이 필요할 때, 우리는 다른 컴퓨터와 물리적(ex. 이더넷 케이블)으로 또는 무선(ex. 와이파이, 블루투스 시스템)으로 연결되어야 한다. 모든 현대 컴퓨터들은 이러한 연결 중 하나를 이용하여 연결을 지속할 수 있다.

**💡 여기서 이더넷 케이블이란?**
이더넷 케이블은 LAN(Long Area Network, 근거리 통신망)을 통해 프린터와 컴퓨터를 연결하는 데 사용되는 케이블이다. ****

<img src="https://user-images.githubusercontent.com/65386533/110079462-7da99c00-7dcc-11eb-8b12-ce6be5542b06.png" width="500">

이러한 네트워크는 두 대의 컴퓨터로만 제한되지 않고, 원하는 만큼의 컴퓨터를 연결할 수 있는데, 그러나 이렇게 연결할 수록 매우 복잡해진다. (예를 들어 10대의 컴퓨터를 연결하고자 한다면, 컴퓨터 당 9개의 플러그가 달린 45개의 케이블이 필요하게 됨)

이러한 문제를 해결하기 위해 네트워크의 각 컴퓨터는 라우터라고 하는 특수한 소형 컴퓨터에 연결된다. 이 라우터는 단 하나의 작업만 하는데, 철도역의 신호원처럼 주어진 컴퓨터에서 보낸 메시지가 올바른 대상 컴퓨터에 도착하는지 확인하는 일이다. 컴퓨터 B에게 메세지를 보내려면 컴퓨터 A가 메시지를 라우터로 보내야 하며, 라우터는 메시지를 컴퓨터 B로 전달하고 메시지가 컴퓨터 C로 배달되지 않도록 해야 한다.

<img src="https://user-images.githubusercontent.com/65386533/110079463-7da99c00-7dcc-11eb-8666-c861440645b1.png" width="500">

이 라우터를 시스템에 추가하면 10대의 컴퓨터 네트워크에는 10개의 케이블만 필요하다. 각 컴퓨터마다 단일 플러그와 10개의 플러그가 있는 하나의 라우터가 필요하다.

#### 1.2.2 네트워크 속의 네트워크

<img src="https://user-images.githubusercontent.com/65386533/110079465-7e423280-7dcc-11eb-949a-8be68efa80c1.png" width="500">

만약 수백, 수천, 수십억 대의 컴퓨터를 연결하려고 할 땐 어떻게 할까? 물론 단일 라우터는 그 정도까지 확장할 수는 없지만 생각해보면 다른 컴퓨터와 마찬가지로 라우터도 컴퓨터이다. 그럼, 두 대의 라우터를 연결할 수 있을까? 있다! 컴퓨터를 라우터에 연결하고, 라우터에서 라우터로, 무한히 확장할 수있다. 

<img src="https://user-images.githubusercontent.com/65386533/110079467-7edac900-7dcc-11eb-9d1f-8ab173f1f458.png" width="500">

이러한 네트워크는 우리가 인터넷이라고 부르는 것에 매우 가깝지만, 우리가 더 알아야 할 것들이 있다. 네트워크는 구축됐지만, 집과 다른 지역 사이에, 아주 먼 곳에 케이블을 연결할 수는 없다. 이러한 문제는 어떻게 처리해야 할까? 예를 들어 전력 및 전화와 같이 이미 집에 연결된 케이블이 있다. 전화기 기반의 시설은 이미 세계 어느 곳과도 연결되어 있으므로 우리가 필요로 하는 완벽한 배선이라고 할 수 있다.

<img src="https://user-images.githubusercontent.com/65386533/110079468-7edac900-7dcc-11eb-8a22-a08e32369a02.png" width="500">

따라서 우리의 네트워크를 전화 시설과 연결하기 위해 '모뎀'이라는 특수 장비가 필요하다. 이 모뎀은 우리 네트워크의 정보를 전화 시설에서 처리할 수 있는 정보로 바꾼다.

<img src="https://user-images.githubusercontent.com/65386533/110079470-7f735f80-7dcc-11eb-9eaa-907b536a4c92.png" width="500">

그래서 네트워크는 전화 시설에 연결되고, 다음 단계는 우리의 네트워크에서 도달하려는 네트워크로 메세지를 보내는 것이다. 그렇게 하기 위해서는 네트워크를 인터넷 서비스 제공 업체(Internet Service Provider, ISP)에 연결한다. ISP는 모두 함께 연결되는 몇몇 특수한 라우터를 관리하고, 다른 ISP의 라우터에도 액세스할 수 있는 회사이다. 따라서 우리 네트워크의 메세지는 ISP 네트워크의 네트워크를 통해 대상 네트워크로 전달된다. 인터넷은 이러한 전체 네트워크의 인프라로 구성된다.

**💡 여기서 인프라란?**
기반 시설, 또는 기간 시설, 또는 인프라스트럭처는 경제 활동의 기반을 형성하는 기초적인 시설과 시스템을 말하며, 도로나 하천, 항만, 공항 등과 같이 경제 활동에 밀접한 사회 자본을 말한다. 이를 흔히 인프라라고 부른다.

## 2. HTTP란 무엇일까?

### 2.1 HTTP의 정의

HTTP는 HyperText Transfer Protocol의 약자로, 한국어로는 '초본문 전송 규약' 혹은 '하이퍼 본문 전송 규약'이라고 한다.

인터넷에서 웹 서버와 사용자 컴퓨터에 설치된 웹 브라우저 사이에 문서를 전송하기 위한 통신 규약으로, 주로 HTML 문서를 주고 받는 데에 사용된다.

HTTP로 되어 있는 웹 페이지를 보기 위한 웹 브라우저로는 마이크로소프트의 인터넷익스플로러, 모질라 재단의 파이어폭스, 구글의 크롬 등이 있다.

### 2.2 HTTP와 HTTPS

#### 2.2.1 HTTP 통신의 문제점

HTTP 서버는 기본 포트인 80번 포트에서 서비스 대기 중이며, 클라이언트(웹 브라우저)가 TCP 80 사용해 연결하면 서버는 요청에 응답하면서 자료(정보)를 전송한다.

HTTP는 정보를 '텍스트'로 주고 받는데, 단순 텍스트를 주고 받기 때문에 네트워크에서 전송 신호를 인터셉트하는 경우 원하지 않는 데이터 유출이 발생할 수 있다.

#### 2.2.2 HTTPS의 등장

HTTPS는 HTTP에 S(Secure Socket)을 추가한 것이다. 기본 구조나 사용 목적은 HTTP와 거의 동일하나, 데이터를 주고 받는 과정에 '보안' 요소가 추가된다는 것이 가장 큰 차이점이다. 쉽게 말해 HTTPS를 사용하면 서버와 클라이언트 사이의 모든 통신이 암호화된다는 것. 전자 상거래에서 널리 쓰인다.

#### 2.2.3 HTTPS의 암호화 과정

HTTPS 사용 시 암호화, 복호화 관련 내용을 간단하게 정리하면 다음과 같다.

암호화와 복호화를 시킬 수 있는 서로 다른 키 2개가 존재할 때 이 두 개의 키는 서로 암호화와 복호화를 할 수 있다는 룰을 가지고 있다.

예를 들어 A키로 암호화 하면 반드시 B키로 복호화할 수 있고, B키로 암호화하면 반드시 A키로 복호화할 수 있다는 것. 그 중에서 하나의 키는 모두에게 공개키로 공개키 저장소에 등록해 사용하고, 남은 하나의 키를 개인키로 이용해 통신한다. 클라이언트는 공개키를 얻어 데이터를 암호화하여 전송하고, 서버는 개인키를 이용해 보호화한다. 만약 반대로 서버가 정보를 제공하고 있는 경우에는 개인키로 암호화한 정보를 전송하기 때문에 공개키가 있는 사용자는 누구나 정보를 알 수 있다.

#### 2.2.4 HTTPS 사용 시 장점

<img src="https://user-images.githubusercontent.com/65386533/110079458-7d110580-7dcc-11eb-97f9-ef5bd84c71da.jpg" width="400">

HTTP와 달리 HTTPS는 '보안' 기능이 추가된 만큼 HTTP보다 처리 속도가 더 느릴 수 밖에 없고, 관련 웹 서버의 사양(스펙)도 중요한 부분이 된다. 물론 현재는 서버와 네트워크 상태가 우수하면서 HTTP와 HTTPS의 차이가 체감하기 어려울 만큼 좁혀졌고, HTTPS를 사용하는 웹 사이트가 많아졌다.

또한 네이버, 다음, 구글 등 IT 기업에서 검색 엔진 최적화 관련 내용을 웹사이트에 적용하고 있고, SEO(Search Engine Optimization) 즉, 검색 엔진 최적화를 위해서도 HTTP 대신 HTTPS 보안 접속을 적용해야 한다. 동일한 키워드의 페이지(사이트)가 있다고 할 때, 사용자가 키워드 검색 시 상위 노출되는 기준 중 하나가 '보안' 요소이며, 때문에 HTTP 사이트보다 HTTPS 사이트가 우선 검색될 수 있다는 것이다.

### 2.3 HTTP 통신과 Socket 통신

네트워크를 통해 서버로부터 데이터를 가져오기 위한 통신 방식은 크게 HTTP 통신과 Socket 통신 2가지가 있다.

#### 2.3.1 HTTP 통신

HTTP 통신은 클라이언트의 요청이 있을 때만  서버가 응답하여 처리를 한 후에 연결을 끊는 방식이다.

**💡 여기서 클라이언트(Client)란?**
클라이언트란 네트워크를 이용하여 서버 시스템에 연결된 PC나 스마트폰 등 사용자 측을 말한다.

**💡 여기서 서버(Server)란?**
웹 사이트는 동영상, 음악, 사진 파일과 같이 하드 디스크에 저장된 파일묶음과 같다고 생각할 수 있는데, 웹 사이트가 앞서 말한 파일들과의 다른 점은 HTML이라는 프로그래밍 코드가 들어있다는 점이다.
다른 파일처럼 HTML도 하드 디스크 어딘가에 저장해야 하는데, 인터넷에선 서버라는 특별하고 강력한 컴퓨터를 사용한다. 데이터 저장하고 제공(serve)하는 일을 하므로 서버(server)라고 부른다.

<img src="https://user-images.githubusercontent.com/65386533/110079454-7bdfd880-7dcc-11eb-9d04-f813c2ee0675.png" width="500">

이러한 연결 방식은 클라이언트가 요청을 보내는 경우에만 서버가 응답하는 단방향적 통신으로, 서버가 클라이언트로 요청을 보낼 수는 없다.

HTTP 통신은 실시간 연결이 아닌, 필요한 경우에만 서버로 접근하는 콘텐츠 위주의 데이터를 사용할 때 용이하다.(ex. 블로그 포스트, 인터넷 뉴스 등)

#### 2.3.2 Socket 통신

Socket 통신은 HTTP 통신과 달리 서버와 클라이언트가 특정 포트를 통해 연결을 성립하고 있어 실시간으로 양방향 통신을 하는 방식이다.

**💡 여기서 포트(Port)란?**
포트란 컴퓨터의 주변 장치와 연결하기 위한 연결단을 말한다.
네트워크에서의 포트는 컴퓨터의 물리적 포트(랜선)에서 데이터가 통해오는 것과 같이 각 프로토콜의 데이터가 통하는 논리적인 통로를 말한다.

<img src="https://user-images.githubusercontent.com/65386533/110079471-7f735f80-7dcc-11eb-9dcf-5af413f54324.png" width="500">

클라이언트만 요청을 보낼 수 있는 HTTP 통신과 달리 Socket 통신은 서버 역시 클라이언트로 요청을 보낼 수 있으며, 계속 연결을 유지하는 연결지향형 통신이기 때문에 실시간 통신이 필요한 경우에 자주 사용된다.(ex. 실시간 동영상 스트리밍, 실시간 채팅 등 즉각적으로 정보를 주고 받는 경우)

#### 2.3.3 각 통신의 특징

<img src="https://user-images.githubusercontent.com/65386533/110079461-7d110580-7dcc-11eb-8cd3-d41aaeaa8205.png" width="700">

## 3. 도메인과 DNS

### 3.1 도메인의 정의

도메인(Domain)이란, 인터넷에 연결된 컴퓨터를 사람이 기억하고 쉽게 입력할 수 있도록 문자(영문, 한글 등)으로 만든 인터넷 주소이다. 인터넷에서 IP(Internet Protocol) 주소를 사람이 기억하기 쉽게 만들어진 것이다.

**여기서 TCP와 IP란?**
인터넷을 하다보면 IP(Internet Protocol)라는 단어를 많이 접했을 것이다. IP에는 항상 TCP(Transmission Control Protocol)라는 단어도 따라오는데, 이 둘에 대해 알아보자.
뜻을 찾아보면, IP는 패킷 통신 방식의 인터넷 프로토콜이며, TCP는 전송 제어 프로토콜이라고 한다.
오늘날 인터넷 통신의 대부분은 패킷 통신을 기본으로 하고 있는데, IP와 TCP는 이러한 패킷 통신을 위한 인터넷 규약이다. IP는 데이터 조각들을 최대한 빨리 목적지로 보내는 역할을 하는데, 그 과정에서 데이터 조각들의 순서가 뒤바뀌거나 일부가 누락이 되면 TCP가 도착한 조각을 점검하여 줄을 세우고 망가졌거나 빠진 조각을 다시 요청하는 것이다. 이 두 방식의 조합을 통하여 인터넷 데이터 통신을 하게 되는데 이를 TCP/IP라고 부른다.

**여기서 패킷 통신이란?**
어플리케이션에서 보내는 데이터를 목적지까지 전송하기 위해서는 각각의 프로토콜에서 정의한 제어 정보(IP 주소, 포트 번호, 오류체크번호)가 필요하다. 제어 정보는 앞쪽에 붙는 헤더(Header)와 뒤쪽에 붙는 (Trailer)로 나뉘는데, 이렇게 제어 정보가 결합된 형태로 실제 데이터가 전송되는데 이를 '패킷(Packet)'이라고 부른다. (패킷 = 제어 정보 + 데이터)
이 패킷이 수신 측에 도착하면 이더넷/IP/TCP 계층을 지나면서 생성된 헤더 또는 트레일러가 제거되고, 데이터를 어플리케이션이 받게 된다.

### 3.2 DNS

인터넷 브라우저에 도메인 이름(ex. www.naver.com)을 입력하면 DNS는 해당 도메인에 대한 정보를 찾는 작업을 수행한다. 도메인은 우리가 웹사이트에 접속하는 방법을 기억할 수 있는 친근한 방법이지만, 이 친근한 이름 뒤에서 컴퓨터들은 숫자를 사용하여 서로 통신한다. 이러한 숫자는 인터넷 프로토콜, 즉 IP 주소(ex.125.209.222.141)를 형성하고, 이는 도메인 이름 아래에서 웹사이트의 거리 주소 역할을 한다.

즉, DNS는 사람들의 연락처가 담긴 거대한 전화번호부라고 생각할 수 있다.

## 4. 호스팅은 무엇일까?

### 4.1 호스팅의 정의

호스팅(Hosting)이란, 정보의 집약체인 서버의 전체 혹은 일부를 이용할 수 있도록 임대해주는 서비스를 말한다.

서버를 관리하기 위해서는 24시간 내내 안정적으로 전기를 공급해야 하고, 빠르고 안정적인 인터넷 회선을 사용해야 하며, 철저한 보안 시스템을 갖추고 있어야 한다. 따라서 개인이 서버를 관리하기보다 전문 업체의 호스팅 서비스를 사용하는 것이 일반적이다.

### 4.2 호스팅의 종류

#### 4.2.1 웹 호스팅

웹 호스팅은 여러 고객이 하나의 서버를 함께 사용하는 형태다. 하나의 서버를 나눠서 사용하기 때문에 저렴하게 이용할 수 있고, 호스팅 업체의 통합 관리를 받기에 편리하다. 하지만 트래픽 양이 증가하는 경우 서버가 다운되는 등의 제약이 있다.

#### 4.2.2 서버 호스팅
서버 호스팅은 고객이 단독 서버를 사용하는 형태이다. 넓은 하드웨어 공간을 사용할 수 있고, 서버 운영/관리에 대한 직접적인 권한을 가질 수 있다. 또한, 빠른 데이터 전송 속도도 보장한다. 하지만 단독으로 서버를 사용하는 만큼 비용이 높은 편이다. 대기업이나 대형 포탈 혹은 대형 오픈마켓과 같이 많은 데이터를 사용하는 기업들이 사용하기 좋다.

#### 4.2.3 클라우드 서버

서버 호스팅을 가상화한 것으로, 가상 서버를 단독으로 사용할 수 있는 형태다. 고객이 필요할 때마다 서버 자원을 늘리거나 축소하여 유연하게 서비스를 이용할 수 있다. 하지만 하나의 가상 서버에 문제가 생기면 연결된 다른 가상 서버에도 문제가 생길 수 있다는 단점이 있다.

---

- 참조

    [https://ko.wikipedia.org/wiki/인터넷](https://ko.wikipedia.org/wiki/%EC%9D%B8%ED%84%B0%EB%84%B7)

    [https://ko.wikipedia.org/wiki/통신_프로토콜](https://ko.wikipedia.org/wiki/%ED%86%B5%EC%8B%A0_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)

    [https://tutorial.djangogirls.org/ko/how_the_internet_works/](https://tutorial.djangogirls.org/ko/how_the_internet_works/)

    [https://developer.mozilla.org/ko/docs/Learn/Common_questions/How_does_the_Internet_work](https://developer.mozilla.org/ko/docs/Learn/Common_questions/How_does_the_Internet_work)

    [https://ij.manual.canon/ij/webmanual/WebPortal/APN/apn_connection_lan.html?lng=ko](https://ij.manual.canon/ij/webmanual/WebPortal/APN/apn_connection_lan.html?lng=ko)

    [https://ko.wikipedia.org/wiki/기반_시설](https://ko.wikipedia.org/wiki/%EA%B8%B0%EB%B0%98_%EC%8B%9C%EC%84%A4)

    [https://ko.wikipedia.org/wiki/HTTP](https://ko.wikipedia.org/wiki/HTTP)

    [http://naver.me/FGmSnUNI](http://naver.me/FGmSnUNI)

    [https://tutorial.djangogirls.org/ko/how_the_internet_works/](https://tutorial.djangogirls.org/ko/how_the_internet_works/)

    [http://wiki.hash.kr/index.php/클라이언트](http://wiki.hash.kr/index.php/%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8)

    [https://namu.wiki/w/포트](https://namu.wiki/w/%ED%8F%AC%ED%8A%B8)

    [https://brunch.co.kr/@wangho/6](https://brunch.co.kr/@wangho/6)

    [https://dongwook8467.tistory.com/66](https://dongwook8467.tistory.com/66)

    [https://aws.amazon.com/ko/route53/what-is-dns/](https://aws.amazon.com/ko/route53/what-is-dns/)

    [https://kr.godaddy.com/help/dns-665](https://kr.godaddy.com/help/dns-665)

    [http://blog.wishket.com/호스팅이란-무엇일까-그린클라이언트/](http://blog.wishket.com/%ED%98%B8%EC%8A%A4%ED%8C%85%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C-%EA%B7%B8%EB%A6%B0%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8/)