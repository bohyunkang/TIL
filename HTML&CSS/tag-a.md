# <a> 태그

## `href` 속성

속성 `href`의 값은 "http://www.naver.com"처럼 절대 경로로 지정할 수도 있고, "doc/info.html"처럼 현재 페이지의 위치에 상대적으로 지정할 수도 있다.

![href](https://user-images.githubusercontent.com/65386533/110562452-ddb58f00-818c-11eb-8b64-6c3b3a42573e.png)

## `target` 속성

```html
<a href="www.naver.com" target="_blank">네이버</a>
<a href="www.naver.com" target="_self">네이버</a>
<a href="www.naver.com" target="_parent">네이버</a>
<a href="www.naver.com" target="_top">네이버</a>
```

`target` 속성은 각 링크가 클릭되었을 때, 새로운 페이지가 어디에 열리는지를 지정한다. 예를 들어 새로운 윈도우가 열리는지 아니면 현재 윈도우에서 새로운 페이지가 열리는지를 지정한다.

`target`의 속성 값 중에서 중요한 것은 "_blank"와 "_self"이다. "_self"가 디폴트 값이다.

![target](https://user-images.githubusercontent.com/65386533/110562456-dee6bc00-818c-11eb-9657-783d611d1d59.png)

_parent이 경우, 예를 들면 만약 어떤 창 A에서 창 B를 새로 열었을 때, 창 B에서 새로운 링크를 클릭하면 창 A에서 열리게 되는 것이다.

_top은 최상위(가장 최고 부모) 창에서 열립니다.

## `<base>` 태그

헤드 섹션에서 `<base>` 태그를 사용하면 모든 링크에 대한 기본 디렉토리를 지정할 수 있다.

```html
<head>
	<base href="http://www.company.com/"/>
</head>

<a href="info.html" target="_blank">참고사항</a>
<-- 여기서 info.html은 base 태그의 기본 디렉토리를 포함하여 "http://www.company.com/info.html"을 의미한다. -->
```

- 참조

    <HTML5+CSS3+JavaScript로 배우는 웹 프로그래밍 기초> 천인국 저.