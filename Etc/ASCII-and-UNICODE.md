# 문자를 표현하는 방법

## 1. ASCII와 UNICODE

디지털 컴퓨터의 메모리에는 비트만 저장할 수 있으므로 컴퓨터를 이용하여 작업하고자 하는 모든 것은 비트의 형태로 구성되어 있어야 한다.

컴퓨터 세상에서 가장 많이 사용하는 두가지 코드로, 첫 번째는 8비트 단위로 약속을 정한 ASCII(이하 아스키)이고, 다른 하나는 16비트 단위로 약속을 정한 Unicode(유니코드)이다.

8비트는 2를 8번 곱한 256가지 약속을 할 수 있고, 16비트는 2를 16번 곱해서 나온 숫자인 65,536가지의 약속을 할 수 있다.

### 1.1 ASCII

**알파벳을 위한 약속.**

아스키는 미국표준협회에서 만든 코드로, American Standard Code for Information Interchange(정보교환을 위한 미국 표준 약속)의 약자이다. 아스키에서는 영어 대문자, 영어 소문자, 숫자, 특수문자를 8비트 안에 약속했다.

### 1.2 UNICODE

**모든 언어를 위한 약속.**

컴퓨터에서 영문자만 사용한다면 아스키로도 충분하지만 시간이 흘러 다른 나라 사람들도 사용하다 보니 자국어를 사용하고자 했다. 영문자 외에 세계 여러 나라의 문자도 표기해야 할 상황이 된 것이다. 그래서 유니코드라는 새로운 약속을 만들어 약속 공간을 더 늘렸다.

## 2. UTF-8

앞서 설명한 유니코드의 정확한 명칭은 UTF(Unicode Transformation Format, 유니코드 변환 양식)이다. 정확하게는 유니코드는 국제표준 문자표이고 UTF-8은 인코딩 방식을 의미한다.

16비트를 하나의 문자 기준으로 하는 UTF-16도 있으며 그 외에 32비트 공간으로 확장한 UTF-32도 있는데 이는 언어 외에 이모지와 같은 특수기호도 포함된 약속이다.

Web **인코딩**의 대부분은 UTF-8이 차지하고 있다. 저장, 통신 용량에 민감하다면 UTF-8, UTF-16 중에서 고민해야한다. 문서에서 많이 사용된 CodePoint들이 몇 바이트로 표현될지 고민 후 결정하면 저장, 통신 용량을 아낄 수 있다. 예를 들어 영문으로 가득찬 문서의 경우 UTF-8로 표현하면 대부분 1 byte로 표현되고 UTF-16으로 표현하면 2 byte로 표현되니 UTF-8이 유리하고, 한글의 경우 UTF-8은 3 byte, UTF-16은 2 byte로 표현되니 UTF-16이 유리하다.

💡 **인코딩(Encoding)?**
사람의 언어를 컴퓨터 언어로 바꾸는 과정을 인코딩(Encoding), 그 반대 과정을 디코딩(Decoding)이라고 한다.

---

- 참조

  [https://ko.wikipedia.org/wiki/UTF-8](https://ko.wikipedia.org/wiki/UTF-8)

  [https://jeongdowon.medium.com/unicode와-utf-8-간단히-이해하기-b6aa3f7edf96](https://jeongdowon.medium.com/unicode%EC%99%80-utf-8-%EA%B0%84%EB%8B%A8%ED%9E%88-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-b6aa3f7edf96)

  [https://pickykang.tistory.com/13](https://pickykang.tistory.com/13)

  <Do it! 첫 코딩 with 자바> 정동균 저.
