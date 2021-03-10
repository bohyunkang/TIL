# JPEG PNG GIF

## JPEG

1. 실사 사진과 같이 복잡하고 많은 색상으로 이루어진 이미지에 적합하다.
2. 1600만 개의 색상을 사용할 수 있으며 손실 압축 방식을 사용한다. 즉 압축 과정에서 약간의 데이터는 영구히 사라진다. 
3. 투명 배경을 지원하지 않는다.
4. 파일 크기는 작고 애니메이션은 지원하지 않는다.

## PNG

1. 클립 아트와 같이 적은 수의 색상을 가진 이미지에 적합하다.
2. PNG-8, PNG-24, PNG-32 등의 세부 종류가 있으며 얼마나 많은 색상을 지원하느냐에 따라서 나누어진다.
3. PNG 또한 압축을 하지만 손실이 발생하지 않는 무손실 압축 방식을 사용한다.
4. 투명 배경도 지원한다.
5. 같은 품질의 이미지의 경우에 JPEG보다는 크기가 크다.

## GIF

1. PNG처럼 로고나 클립 아트 형태의 이미지에 적합하다.
2. 256개의 색상만을 지원한다.
3. PNG와 같이 무손실 압축 기법을 사용한다.
4. 투명 배경도 지원한다.
5. 애니메이션도 지원한다.

### 💡 이미지의 크기가 큰 경우 처리 방법

최근에는 스마트폰에서도 고화질의 카메라를 사용할 수 있다. 이러한 카메라로 사진을 찍으면 이미지가 상당히 커지 수 있는데, 예를 들어 3000X3000 정도의 이미지도 가능하다.

물론 <img> 태그의 width와 height 속성을 이용해 화면에서 크기를 줄일 수 있지만 이미지 원본은 그대로 두고, 화면에서 표시할 때만 크기를 작게 결정하는 방법은 문제점이 있다.

우리가 웹 페이지에서 이미지의 크기를 지정하면 이미지의 크기 변경은 클라이언트 컴퓨터의 브라우저에서 이루어진다. 따라서 먼저 크기가 큰 원본 이미지가 서버로부터 다운로드되어야 한다.

이미지의 크기가 크면 페이지 적재 속도도 느려지고 특히 모바일 장치에서는 더욱 더 문제가 된다.

따라서 가장 좋은 방법은 이미지를 서버에 올리기 전에 포토샵과 같은 사진 어플리케이션을 사용해 이미지의 크기를 적절하게 줄여서 올리는 것이다.

포토샵의 "Save for Web" 메뉴를 사용하면 좋다.

일반적으로 웹에서 이미지의 크기는 800X800보다 작아야 한다.

---

- 참조

    <HTML5+CSS3+JavaScript로 배우는 웹 프로그래밍 기초> 천인국 저.