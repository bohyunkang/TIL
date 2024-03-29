# 브라우저 파싱과 DOM 트리, 렌더 트리

브라우저 동작 원리에서 5장, 6장의 내용을 따로 정리하였습니다.

## 5. 파싱과 DOM 트리 구축

### 5.1 파싱 일반

문서 파싱(parsing)은 브라우저가 코드를 이해하고 사용할 수 있는 구조로 변환하는 것을 의미한다. 파싱 결과는 보통 문서 구조를 나타내는 노드 트리인데 파싱 트리(parse tree) 또는 문법 트리(syntax tree)라고 부르다.

![parse-tree](https://user-images.githubusercontent.com/65386533/110413434-2c9dee80-80d1-11eb-8312-60c51394e7dd.png)

▲ 수학 표현식을 파싱한 트리 노드

예를 들어 2+3-1과 같은 표현식은 위 그림과 같은 트리가 된다.

#### 5.1.1 문법

파싱은 문법에 작성된 언어 또는 형식의 규칙에 따르는데 파싱할 수 있는 모든 형식은 정해진 용어와 구문 규칙에 따라야 한다. 이것을 '문맥 자유 문법'이라고 한다. 인간의 언어는 이런 모습과는 다르기 때문에 기계적으로 파싱이 불가능하다. 

#### 5.1.2 파서-어휘 분석기 조합

파싱은 어휘 분석과 구문 분석이라는 두 가지로 구분할 수 있다.

어휘 분석은 자료를 토큰으로 분해하는 과정이다. 토큰은 유효하게 구성된 단위의 집합체로 용어집이라고도 할 수 있는데 인간의 언어로 말하자면 사전에 등장하는 모든 단어가 해당된다.

구문 분석은 언어의 구문 규칙을 적용하는 과정이다.

파서는 보통 두 가지 일을 하는데 자료를 유효한 토큰으로 분해하는 언어 분석기(토큰 변환기라고도 부름)가 있고 언어 구문 규칙에 따라 문서 구조를 분석함으로써 파싱 트리를 생성하는 파서가 있다. 어휘 분석기는 공백과 줄 바꿈 같은 의미 없는 문자를 제거한다.

![parse-tree-2](https://user-images.githubusercontent.com/65386533/110413435-2d368500-80d1-11eb-9a74-f321f62c4835.png)

▲ 문서 소스로부터 파싱 트리를 만드는 과정

파싱 과정은 반복된다. 파서는 보통 어휘 분석기로부터 새 토큰을 받아서 구문 규칙과 일치하는지 확인한다. 규칙에 맞으면 토큰에 해당하는 노드가 파싱 트리에 추가되고 파서는 또 다른 토큰을 요청한다.

규칙에 맞지 않으면 파서는 토큰을 내부적으로 저장하지 않고 토큰과 일치하는 규칙이 발견될 때까지 요청한다. 맞는 규칙이 없는 경우 예외로 처리하는데 이것은 문서가 유효하지 않고 구문 오류를 포함하고 있다는 의미이다.

#### 5.1.3 변환

파서 트리는 최종 결과물이 아니다. 파싱은 보통 문서를 다른 양식으로 변환하는데 '컴파일'이 하나의 예가 된다. 소스코드를 기계 코드로 만드는 컴파일러는 파싱 트리 생성 후 이를 기계 코드 문서로 변환한다.

![compile](https://user-images.githubusercontent.com/65386533/110580050-398f1080-81ab-11eb-87c2-24267b092dd7.png)

▲ 컴파일 과정

#### 5.1.4 파싱의 예

![parse-tree](https://user-images.githubusercontent.com/65386533/110413434-2c9dee80-80d1-11eb-8312-60c51394e7dd.png)

▲ 수학 표현식을 파싱한 트리 노드

앞서 봤던 수학 표현식을 보고, 간단한 수학 언어를 정의하고 파싱 과정을 살펴 보자.

어휘: 수학 언어는 정수, 더하기 기호, 빼기 기호를 포함한다.

구문: 

1. 언어 구문의 기본적인 요소는 표현식, 항, 연산자이다.
2. 언어에 포함되는 표현식의 수는 제한이 없다.
3. 표현식은 "항" 뒤에 "연산자" 그 뒤에 또 다른 항이 따르는 형태로 정의한다.
4. 연산자는 더하기 토큰 또는 빼기 토큰이다.
5. 정수 토큰 또는 하나의 표현식은 항이다.

입력된 값 2+3-1을 분석해보자.

규칙에 맞는 첫 번째 부분 문자열은 2이다. 규칙 5번에 따르면 이것은 하나의 항이다. 두 번째로 맞는 것은 2+3인데 이것은 항 뒤에 연산자와 또 다른 항이 등장한다는 세 번째 규칙과도 일치한다. 입력 값의 마지막 부분까지 진행하면 또 다른 일치를 발견할 수 있다. 2+3은 항과 연산자와 항으로 구성된 하나의 새로운 항이라는 것을 알고 있기 때문에 2+3-1은 하나의 표현식이 된다. 하지만 2++은 어떤 규칙과도 맞지 않기 때문에 유효하지 않은 입력이 된다.

#### 5.1.5 어휘와 구문에 대한 공식적인 정의

어휘는 보통 정규 표현식으로 표현한다. 예를 들면 언어는 다음과 같이 정의될 것이다.

```html
INTEGER : 0|[1-9][0-9]*
PLUS : +
MINUS : -
```

💡 **여기서 정규 표현식이란?**
문자열에 나타나는 특정 문자 조합과 대응시키기 위해 사용되는 패턴이다. 줄여서 '정규식'이라고도 한다.

구문은 보통 BNF라고 부르는 형식에 따라 정의한다. 언어는 다음과 같이 정의될 것이다.

```html
expression := term operation term
operation := PLUS | MINUS
term := INTEGER | expression
```

💡 **여기서  BNF란?**
BNF는 배커스-나우르 표기법의 약칭으로 문맥 자유 문법을 나타내기 위해 만들어진 표기법이다.
기본 문법 : <기호> : := <표현식>

문법이 문맥 자유 문법이라면 언어는 정규 파서로 파싱할 수 있다. 문맥 자유 문법을 쉽게 말하면 완전히 BNF로 표현 가능한 문법이다.

#### 5.1.6 파서의 종류

파서는 기본적으로 하향식 파서와 상향식 파서가 있다.

하향식 파서는 구문의 상위 구조로부터 일치하는 부분을 찾기 시작하는데 반해 상향식 파서는 낮은 수준에서 점차 높은 수준으로 찾는다.

- 하향식 파서
2+3과 같은 표현식에 해당하는 높은 수준의 규칙을 먼저 찾는다. 그 다음 표현식으로 2+3-1을 찾을 것이다. 표현식을 찾는 과정은 일치하는 다른 규칙을 점진적으로 더 찾아내는 방식인데 어쨌거나 가장 높은 수준의 규칙을 먼저 찾는 것으로부터 시작한다.
- 상향식 파서
입력 값이 규칙에 맞을 때까지 찾아서 맞는 입력 값을 규칙으로 바꾸는데 이 과정은 입력 값의 끝까지 진행된다. 부분적으로 일치하는 표현식은 파서 스택에 쌓인다.

![parser](https://user-images.githubusercontent.com/65386533/110580052-3a27a700-81ab-11eb-918a-9a267d07fd73.PNG)

상향식 파서는 입력 값의 오른쪽으로 이동하면서(입력 값의 처음을 가리키는 포인터가 오른쪽으로 이동하는 것을 상상) 구문 규칙으로 갈수록 남는 것이 점차 감소하기 때문에 이동-감소 파서라고 부른다.

#### 5.1.7 파서 자동생성

파서를 생성해줄 수 있는 도구를 파서 생성기라고 한다. 언어에 어휘나 구문 규칙 같은 문법을 부여하면 동작하는 파서를 만들어 준다. 파서를 생성하는 것은 파싱에 대한 깊은 이해를 필요로 하고 수동으로 파서를 최적화하여 생성하는 것은 쉬운 일이 아니기 때문에 파서 생성기는 매우 유용하다.

웹킷은 잘 알려진 두 개의 파서 생성기를 사용한다. 어휘 생성을 위한 플렉스(Flex)와 파서 생성을 위한 바이슨(Bison)이다.

플렉스는 토큰의 정규 표현식의 정의를 포함하는 파일을 입력 받고 바이슨은 BNF 형식의 언어 구문 규칙을 입력 받는다.

### 5.2 HTML 파서

HTML 파서는 HTML 마크업을 파싱 트리로 변환한다.

#### 5.2.1 HTML 문법 정의

HTML의 어휘와 문법은 W3C에 의해 명세로 정의되어 있다.

#### 5.2.2 문맥 자유 문법이 아님

위의 5.1 파싱 일반에서 알게 된 것처럼 문법은 BNF와 같은 형식을 이용하여 공식적으로 정의할 수 있다.

안타깝게도 모든 전통적인 파서는 HTML에 적용할 수 없다. 하지만 파싱은 CSS와 자바스크립트를 파싱하는 데 사용된다. HTML은 파서가 요구하는 문맥 자유 문법에 의해 쉽게 정의할 수 없다.

HTML 정의를 위한 공식적인 형식으로 DTD(문서 형식 정의)가 있지만 이것은 문맥 자유 문법이 아니다.

HTML이 XML과 유사하기 때문에 이상하다고 생각할 수 있다. 사용할 수 있는 XML 파서는 많다. HTML을 XML 형태로 재구성한 XHTML도 있는데 무엇이 큰 차이점일까?

차이점은 HTML이 더 "너그럽다"는 점이다. HTML은 암묵적으로 태그에 대한 생략이 가능하다. 가끔 시작 또는 종료 태그를 생략해도 오류가 나지 않는다. 전반적으로 뻣뻣하고 부담스러운 XML에 반하여 HTML은 "유연한" 문법이다.

이런 작은 차이가 큰 차이를 만들어 낸다. 웹 제작자의 실수를 너그럽게 용서하고 편하게 만들어주는 이것이야 말로 HTML이 인기 있는 이유다. 하지만 다른 한편으로는 공식적인 문법으로 작성하기 어렵게 만드는 문제가 있다.

즉, 정리하자면 HTML은 파싱하기 어렵고 전통적인 구문 분석이 불가능하기 때문에 문맥 자유 문법이 아니라는 것이다. XML 파서로도 파싱하기 쉽지 않다.

#### 5.2.3 HTML DTD

HTML의 정의는 DTD 형식 안에 있는데 SGML 계열 언어의 정의를 이용한 것이다. 이 형식은 허용되는 모든 요소와 그들의 속성 그리고 중첩 구조에 대한 정의를 포함한다. 앞서 말한대로 HTML DTD는 문맥 자유 문법이 아니다.

💡 **여기서 SGML이란?**
Standard Generalized Markup Language의 약자로, 문서용 마크업 언어를 정의하기 위한 메타언어를 말한다.

DTD는 여러 변종이 있다. 엄격한 형식은 명세만을 따르지만 다른 형식은 낡은 브라우저에서 사용된 마크업을 지원한다. 낡은 마크업을 지원하는 이유는 오래된 콘텐츠에 대한 하위 호환성 때문이다.

#### 5.2.4 DOM

"파싱 트리"는 DOM 요소와 속성 노드의 트리로서 출력 트리가 된다. DOM은 문서 객체 모델(Document Object Model)의 약자로, HTML 문서의 객체 표현이고 외부를 향하는 자바스크립트와 같은 HTML 요소의 연결 지점이다. 트리의 최상위 객체는 문서이다.

DOM은 마크업과 1:1 관계를 맺는다. 예를 들면 아래와 같은 마크업이 있을 때,

```html
<html>
  <body>
   <p>Hello World</p>
   <div><img src="example.png" /></div>
  </body>
</html>
```

아래와 같은 DOM 트리로 변환할 수 있다.

![dom-tree](https://user-images.githubusercontent.com/65386533/110580053-3ac03d80-81ab-11eb-816d-1e19a7185d5b.png)

▲ 예제 마크업의 DOM 트리

HTML과 마찬가지로 DOM은 W3C에 의해 명세가 정해져 있다. 이것은 문서를 다루기 위한 일반적인 명세인데 부분적으로 HTML 요소를 설명하기도 한다.

트리가 DOM 노드를 포함한다고 말하는 것은 DOM 접점의 하나를 실행하는 요소를 구성한다는 의미이다. 브라우저는 내부의 다른 속성들을 이용하여 이를 구체적으로 실행한다.

#### 5.2.5 파싱 알고리즘

앞서 말한대로 HTML은 일반적인 하향식 또는 상향식 파서로 파싱이 안되는데, 그 이유는 다음과 같다.

1. 언어의 너그러운 속성.
2. 잘 알려져 있는 HTML 오류에 대한 브라우저의 관용.
3. 변경에 의한 재파싱. 일반적으로 소스는 파싱하는 동안 변하지 않지만 HTML에서 `document.write`을 포함하고 있는 스크립트 태그는 토큰을 추가할 수 있기 때문에 실제로는 입력 과정에서 파싱이 수정된다.

일반적인 파싱 기술을 사용할 수 없기 때문에 브라우저는 HTML 파싱을 위해 별도의 파서를 생성한다.

알고리즘은 '토큰화(5.2.6)'와 '트리 구축(5.2.7)' 이렇게 두 단계로 되어 있다.

토큰화는 어휘 분석으로서 입력 값을 토큰으로 파싱한다. HTML에서 토큰은 시작 태그, 종료 태그, 속성 이름과 속성 값이다.

토큰화는 토큰을 인지해서 트리 생성자로 넘기고 다른 토큰을 확인하기 위해 다음 문자를 확인한다. 그리고 입력의 마지막까지 이 과정을 반복한다.

![parsing-process](https://user-images.githubusercontent.com/65386533/110580054-3ac03d80-81ab-11eb-84e4-d751adca7e20.png)

▲ HTML 파싱 과정

#### 5.2.6 토큰화 알고리즘

알고리즘의 결과물은 HTML 토큰이다. 알고리즘은 상태 기계(State Machine)라고 볼 수 있다. 각 상태는 하나 이상의 연속된 문자를 입력받아 이 문자에 따라 다음 상태를 갱신한다. 그러나 결과는 현재의 토큰화 상태와 트리 구축 상태의 영향을 받는데, 이것은 같은 문자를 읽어 들여도 현재 상태에 따라 다음 상태의 결과가 다르게 나온다는 것을 의미한다.

다음은 HTML 토큰화를 설명하기 위한 기본적인 예제이다.

```html
<html>
   <body>
      Hello world
   </body>
</html>
```

초기 상태는 "자료 상태"이다. < 문자를 만나면 상태는 "태그 열림 상태"로 변한다. a부터 z까지의 문자를 만나면 "시작 태그 토큰"을 생성하고 상태는 "태그 이름 상태"로 변하는데 이 상태는 > 문자를 만날 때까지 유지한다. 각 문자에는 새로운 토큰 이름이 붙는데 이 경우 생성된 토큰은 html 토큰이다.

> 문자에 도달하면 현재 토큰이 발행되고 상태는 다시 "자료 상태"로 바뀐다. 태그는 동일한 절차에 따라 처리된다. 지금까지 `html` 태그와 `body` 태그를 발행했고, 다시 "자료 상태"로 돌아왔다. Hello World의 H 문자를 만나면 문자 토큰이 생성되고 발행될 것이다. 이것은 종료 태그의 < 문자를 만날 때까지 진행된다. Hello World의 각 문자를 위한 문자 토큰을 발행할 것이다.

다시 "태그 열림 상태"가 되었다. / 문자는 종료 태그 토큰을 생성하고 "태그 이름 상태"로 변경될 것이다. 이 상태는 > 문자를 만날 때까지 유지된다. 그리고 새로운 태그 토큰이 발행되고 다시 "자료 상태가 된다. 또한 동일하게 처리될 것이다.

![token-process](https://user-images.githubusercontent.com/65386533/110580058-3b58d400-81ab-11eb-9a8c-4f827c9b4dfb.png)

▲ 입력 예제의 토큰화

#### 5.2.7 트리 구축 알고리즘

파서가 생성되면 문서 객체가 생성된다. 트리 구축이 진행되는 동안 문서 최상단에서는 DOM 트리가 수정되고 요소가 추가된다. 토큰화에 의해 발행된 각 노드는 트리 생성자에 의해 처리된다. 각 토큰을 위한 DOM 요소의 명세는 정의되어 있다. DOM 트리에 요소를 추가하는 것이 아니라면 열린 요소는 스택(임시 버퍼 저장소)에 추가된다. 이 스택은 부정확한 중첩과 종료되지 않은 태그를 교정한다. 알고리즘은 상태 기계라고 설명할 수 있고, 상태는 "삽입 모드"라고 부른다.

아래 입력 예제의 트리 생성 과정을 보자.

```html
<html>
   <body>
      Hello world
   </body>
</html>
```

트리 구축 단계의 입력 값은 토큰화 단계에서 만들어지는 일련의 토큰이다. 받은 html 토큰은 "html 이전" 모드가 되고 토큰은 이 모드에서 처리된다. 이것은 HTMLHtmlElement 요소를 생성하고 문서 객체의 최상단에 추가된다.

상태는 "head 이전" 모드로 바뀌었고 "body" 토큰을 받았다. "head" 토큰이 없더라도 HTMLHeadElement는 묵시적으로 생서오디어 트리에 추가될 것이다.

곧이어 "head 안쪽" 모드로 이동했고 다음은 "head 다음" 모드로 간다. body 토큰이 처리되었고, HTMLBodyElement가 생성되어 추가됐으며 "body 안쪽" 모드가 되었다.

"Hello world" 문자열의 문자 토큰을 받았다. 첫 번째 토큰이 생성되고 "본문" 노드가 추가되면서 다른 문자들이 그 노드에 추가될 것이다.

body 종료 토큰을 받으면 "body 다음" 모드가 된다. html 종료 태그를 만나면 "body 다음 다음" 모드로 바뀐다. 마지막 파일 토큰을 받으면 파싱을 종료한다.

![tree-process](https://user-images.githubusercontent.com/65386533/110580060-3bf16a80-81ab-11eb-830f-24b0cfb59079.png)

▲ 예제 html 트리 구축

#### 5.2.8 파싱이 끝난 이후의 동작

이번 단계에서 브라우저는 문서와 상호작용할 수 있게 되고 문서 파싱 이후에 실행되어야 하는 "지연" 모드 스크립트를 파싱하기 시작한다. 문서 상태는 "완료"가 되고 "로드" 이벤트가 발생한다.

#### 5.2.9 브라우저의 오류처리

HTML 페이지에서 "유효하지 않은 구문"이라는 오류를 본 적이 없을 것이다. 이는 브라우저가 모든 오류 구문을 교정하기 때문이다. 

아래 오류가 포함된 HTML 예제를 보자.

```html
<html>  
   <mytag></mytag>
   <div>
     <p>
   </div>
   Really lousy HTML
   </p>
</html>
```

위 코드에는 여러 오류가 있는데, "mytag"는 표준 태그가 아니고 "p"태그와 "div"태그는 중첩 오류가 있다. 그러나 브라우저는 투덜거리지 않고 올바르게 표시하는데 이는 파서가 HTML 제작자의 실수를 수정했기 때문이다.

파서는 토큰화된 입력 값을 파싱하여 문서를 만들고 문서 트리를 생성한다. 규칙에 맞게 잘 작성된 문서라면 파싱이 수월하겠지만 불행하게도 형식에 맞지 않게 작성된 많은 HTML 문서를 다뤄야 하기 때문에 파서는 오류에 대한 아량이 있어야 한다.

파서는 적어도 다음과 같은 오류를 처리해야 한다.

1. 어떤 태그의 안쪽에 추가하려는 태그가 금지된 것일 때 일단 허용된 태그를 먼저 닫고 금지된 태그를 외부에 추가한다.
2. 파서가 직접 요소를 추가해서는 안된다. 문서 제작자에 의해 뒤늦게 요소가 추가될 수 있고 생략 가능한 경우도 있다. `HTML`, `HEAD`, `BODY`, `TBODY`, `TR`, `TD`, `LI` 태그가 이런 경우에 해당한다.
3. 인라인 요소 안쪽에 블록 요소가 있는 경우 부모 블록 요소를 만날 때까지 모든 인라인 태그를 닫는다.
4. 이런 방법이 도움되지 않으면 태그를 추가하거나 무시할 수 있는 상태가 될 때까지 요소를 닫는다.

웹킷이 오류를 처리하는 예는 다음과 같다.

##### `<br>` 대신 `</br>`

어떤 사이트는 `<br>`대신 `</br>`을 사용한다. 인터넷 익스플로러, 파이어폭스와 호환성을 갖기 위해 웹킷은 이것을 `<br>`로 간주한다.

##### 어긋난 표

어긋난 표는 표 안에 또 다른 표가 `th` 또는 `td` 셀 내부에 있지 않은 것을 의미한다. 이런 경우 웹킷은 표의 중첩을 분해하여 형제 요소가 되도록 처리한다.

웹킷은 이런 오류를 처리하는데 스택을 사용한다. 안쪽의 표는 바깥쪽 표의 외부로 옮겨져서 형제 요소가 된다.

##### 중첩된 폼 요소

폼 안에 또 다른 폼을 넣은 경우 안쪽의 폼은 무시된다.

##### 태그 중첩이 너무 깊을 때

최대 20개의 중첩만 허용하고 나머지는 무시한다.

##### 잘못 닫힌 `<html>` 또는 `<body>` 태그

깨진 `html`을 지원한다. 일부 바보같은 페이지는 문서가 끝나기 전에 `body`를 닫아버리기 때문에 브라우저는 `body` 태그를 닫지 않는다. 대신 종료를 위해 `end()`를 호출한다.

### 5.3 CSS 파싱

HTML과는 다르게 CSS는 문맥 자유 문법이고 앞서 설명한 파서 유형을 이용하여 파싱이 가능하다.

#### 5.3.1 웹킷 CSS 파서

웹킷은 CSS 문법 파일로부터 자동으로 파서를 생성하기 위해 플렉스와 바이슨 파서 생성기를 사용한다. 앞서 말했던 거처럼 바이슨은 상향식 이동 감서 파서를 생성한다. 파이어폭스는 직접 작성한 하향식 파서를 사용한다. 두 경우 모두 각 CSS 파일은 스타일 시트 객체로 파싱되고 각 객체는 CSS 규칙을 포함한다. CSS 규칙 객체는 선택자와 선언 객체 그리고 CSS 문법과 일치하는 다른 객체를 포함한다.

![css-parsing](https://user-images.githubusercontent.com/65386533/110580061-3bf16a80-81ab-11eb-9087-b70f7a0c3749.png)

▲ CSS 파싱

### 5,4 스크립트와 스타일 시트의 진행 순서

#### 5.4.1 스크립트

웹은 파싱과 실행이 동시에 수행되는 동기화(synchronous) 모델이다. 제작자는 파서가 <script> 태그를 만나면 즉시 파싱하고 실행하기를 기대한다. 하지만 스크립트가 실행되는 동안 문서의 파싱은 중단된다. 스크립트가 외부에 있는 경우 우선 네트워크로부터 자원을 가져와야 하는데 이 또한 실시간으로 처리되고 자원을 받을 때까지 파싱은 중단된다. 제작자는 스크립트를 "지연(defer)"으로 표시할 수 잇는데 지연으로 표시하게 되면 문서 파싱은 중단되지 않고, 문서 파싱이 완료된 이후에 스크립트가 실행된다. HTML5는 스크립트를 비동기(asynchronous)로 처리하는 속성을 추가했기 때문에 별도의 맥락에 의해 파싱되고 실행된다.

#### 5.4.2 예측 파싱

웹킷과 파이어폭스는 예측 파싱과 같은 최적화를 지원한다. 스크립트를 실행하는 동안 다른 스레드는 네트워크로부터 다른 자원을 찾아 내려받고 문서의 나머지 부분을 파싱한다. 이런 방법은 자원을 병렬로 연결하여 받을 수 있고 전체적인 속도를 개선한다. 참고로 예측 파서는 DOM 트리를 수정하지 않고 메인 파서의 일로 넘긴다. 예측 파서는 외부 스크립트, 외부 스타일 시트와 외부 이미지와 같이 참조된 외부 자원을 파싱할 뿐이다.

#### 5.4.3 스타일 시트

한편 스타일 시트는 다른 모델을 사용한다. 이론적으로 스타일 시트는 DOM 트리를 변경하지 않기 때문에 문서 파싱을 기다리거나 중단할 이유가 없다. 그러나 스크립트가 문서를 파싱하는 동안 스타일 정보를 요청하는 경우라면 문제가 된다. 스타일이 파싱되지 않은 상태라면 스크립트는 잘못된 결과를 내놓기 때문에 많은 문제를 야기한다. 이런 문제는 흔치 않은 것처럼 보이지만 매우 빈번하게 발생한다. 파이어폭스는 아직 로드 중이거나 파싱 중인 스타일 시트가 있는 경우 모든 스크립트의 실행을 중단한다. 한편 웹킷은 로드되지 않은 스타일 시트 가운데 문제가 될 만한 속성이 있을 때에만 스크립트를 중단한다.

## 6. 렌더 트리 구축

DOM 트리가 구축되는 동안 브라우저는 렌더 트리를 구축한다. 표시해야 할 순서와 문서의 시각적인 구성 요소로써 올바른 순서로 내용을 그려낼 수 있도록 하기 위한 목적이 있다.

파이어폭스는 이 구성 요소를 "형상(frames)"이라고 부르고 웹킷은 "렌더러(renderer)" 또는 "렌더 객체(render object)"라는 용어를 사용한다.

렌더러는 자신과 자식 요소를 어떻게 배치하고 그려내야 하는지 알고 있다.

각 렌더러는 CSS 명세에 따라 노드의 CSS 박스에 부합하는 사각형을 표시한다. 렌더러는 너비, 높이 그리고 위치와 같은 기하학적 정보를 포함한다.

### 6.1 DOM과 렌더 트리의 관계

렌더러는 DOM 요소에 부합하지만 1:1로 대응하는 관계는 아니다. 예를 들어 "head" 요소와 같은 비시각적 DOM 요소는 렌더 트리에 추가되지 않는다. 또한 display 속성에 "none"으로 할당된 요소는 트리에 나타나지 않는다*(visibility 속성에 "hidden" 값이 할당된 요소는 트리에 나타난다).*

여러 개의 시각 객체와 대응하는 DOM 요소도 있는데 이것들은 보통 하나의 사각형으로 묘사할 수 없는 복잡한 구조다. 예를 들면, "select" 요소는 '표시 영역, 드롭다운 목록, 버튼' 표시를 위한 3개의 렌더러가 있다. 또한 한 줄에 충분히 표시할 수 있는 문자가 여러 줄로 바뀔 때 새 줄은 별도의 렌더러로 추가된다. 여러 렌더러와 대응하는 또 다른 예는 깨진 HTML이다. CSS 명세에 의하면 인라인 박스는 블록 박스만 포함하거나 인라인 박스만을 포함해야 하는데 인라인과 블록 박스가 섞인 경우 인라인 박스를 감싸기 위한 익명의 블록 렌더러가 생성된다.

어떤 렌더 객체는 DOM 노드에 대응하지만 트리의 동일한 위치에 있지 않다. float으로 처리된 요소 또는 position 속성 값이 absolute로 처리된 요소는 흐름에서 벗어나 트리 다른 곳에 배치된 상태로 형상이 그려진다. 대신 자리 표시자가 원래 있어야 할 곳에 배치된다. 

![render-tree](https://user-images.githubusercontent.com/65386533/110580062-3c8a0100-81ab-11eb-90d7-38b0e0f59487.png)

▲ 렌더 트리와 DOM 트리 대응.
여기서 "뷰포트"는 최초의 블록이다. 웹킷에서는 "RenderView" 객체가 이 역할을 한다.

### 6.2 트리를 구축하는 과정

파이어폭스에서 프레젠테이션은 DOM 업데이트를 위한 리스너로 등록된다. 프레젠테이션은 형상 만들기를 FrameConstructor에 위임하고 FrameConstructor는 스타일을 결정하고 형상을 만든다.

웹킷에서는 스타일을 결정하고 렌더러를 만드는 과정을 "어태치먼트(attachment)"라고 부른다. 모든 DOM 노드에는 "attach" 메서드가 있다. 어태치먼트는 동기적인데 DOM 트리에 노드를 추가하면 새 노드의 "attach" 메서드를 호출한다.

html 태그와 body 태그를 처리함으로써 렌더 트리 루트를 구성한다. 루트 렌더 객체는 CSS 명세에서 포함 블록(다른 모든 블록을 포함하는 최상위 블록)이라고 부르는 그것과 일치한다. 파이어폭스는 이것을 ViewPortFrame이라고 부르고, 웹킷은 RenderView라고 부른다. 이것이 문서가 가리키는 렌더 객체다. 트리의 나머지 부분은 DOM 노드를 추가함으로써 구축된다.

### 6.3 스타일 계산

렌더 트리를 구축하려면 각 렌더 객체의 시각적 속성에 대한 계산이 필요한데 이것은 각 요소의 스타일 속성을 계산함으로써 처리된다.

스타일은 인라인 스타일 요소와 HTML의 시각적 속성(예를 들면 bgcolor 같은 HTML 속성)과 같은 다양한 형태의 스타일 시트를 포함하는데 HTML의 시각적 속성들은 대응하는 CSS 스타일 속성으로 변환된다.

최초의 스타일 시트는 브라우저가 제공하는 기본 스타일 시트인데 페이지 제작자 또는 사용자도 이를 제공할 수 있다. 브라우저는 사용자가 선호하는 스타일을 정의할 수 있도록 지원하는데 파이어폭스의 경우 "파이어폭스 프로필" 폴더에 있는 스타일 시트를 변경함으로써 사용자 선호 스타일을 정의할 수 있다.

스타일을 계산하는 일에는 다음과 같은 몇 가지 어려움이 따른다.

1. 스타일 데이터는 구성이 매우 광범위한데 수 많은 스타일 속성들을 수용하면서 메모리 문제를 야기할 수 있다.
2. 최적화되어 있지 않다면 각 요소에 할당된 규칙을 찾는 것은 성능 문제를 야기할 수 있다. 각 요소에 할당된 규칙 목록을 전체 규칙으로부터 찾아내는 것은 과중한 일이다. 맞는 규칙을 찾는 과정은 얼핏 보기에는 약속된 방식으로 순탄하게 시작하는 것 같지만 실상 쓸모가 없거나 다른 길을 찾아야만 하는 복잡한 구조가 될 수 있다.
3. 규칙을 적용하는 것은 계층 구조를 파악해야 하는 꽤나 복잡한 다단계 규칙을 수반한다.

브라우저가 어떻게 이 문제들을 처리하는지 살펴보자.

#### 6.3.1 스타일 정보 공유

웹킷 노드는 스타일 객체(RenderStyle)를 참조하는데 이 객체는 일정 조건 아래 공유할 수 있다. 노드가 형제이거나 또는 사촌일 때 공유하며 다음과 같은 조건일 때 공유할 수 있다.

1. 동일한 마우스 반응 상태를 가진 요소여야 한다. 예를 들어 한 요소가 :hover 상태가 될 수 없는데 다른 요소는 :hover가 될 수 있다면 그건 동일한 마우스 상태가 아니다.
2. 아이디가 없는 요소
3. 태그 이름이 일치해야 한다.
4. 클래스 속성이 일치해야 한다.
5. 지정된 속성이 일치해야 한다.
6. 링크(link) 상태가 일치해야 한다.
7. 초점(focus) 상태가 일치해야 한다.
8. 문서 전체에서 속성 선택자의 영향을 받는 요소가 없어야 한다. 여기서 영양이라 함은 속성 선택자를 사용한 경우를 말한다.(속성 선택자 ex. input[type=text]{...})
9. 요소에 인라인 스타일 속성이 없어야 한다.(인라인 스타일 ex. <p style="...">...</p>).
10. 문서 전체에서 형제 선택자를 사용하지 않아야 한다. 웹 코어는 형제 선택자를 만나면 전역 스위치를 열고 전체 문서의 스타일 고유를 중단한다. 형제 선택자는 + 선택자와 :first-child 그리고 :last-child를 포함한다.

#### 6.3.2 파이어폭스 규칙 트리

파이어폭스는 스타일 계산을 쉽게 처리하기 위해 규칙 트리와 스타일 문맥 트리라고 하는 두 개의 트리를 더 가지고 있다. 웹킷도 스타일 객체를 가지고 있지만 스타일 문맥 트리처럼 저장되지 않고 오직 DOM 노드로 관련 스타일을 처리한다.

![style-context](https://user-images.githubusercontent.com/65386533/110580063-3d229780-81ab-11eb-849b-6e27f13d2239.png)

▲ 파이어폭스 스타일 문맥 트리

스타일 문맥에는 최종 값이 저장되어 있다. 값은 올바른 순서 안에 부합하는 규칙을 적용하고 논리로부터 구체적인 값으로 변환함으로써 계산된다. 예를 들어 논리적인 값이 화면의 백분율(%)이라면 이 값은 곗ㄴ에 의해 절대적인 단위(px)로 변환된다. 이런 규칙 트리 아이디어는 정말 현명하다. 노드 사이에서 이 값을 공유함으로써 그것들을 다시 계산하는 일은 방지하기 때문이다.

부합하는 모든 규칙은 트리에 저장하는데 경로의 하위 노드가 높은 우선순위를 갖는다. 규칙 저장은 느리게 처리된다. 트리는 처음부터 모든 노드를 계산하지 않지만 노드 스타일이 계산될 필요가 있을 때 계산된 경로를 트리에 추가한다.

트리 경로를 어휘 목록 속에 있는 단어라고 생각하고 이미 규칙 트리를 계산했다고 가정해보자.

![tree-rule](https://user-images.githubusercontent.com/65386533/110580057-3b58d400-81ab-11eb-9d8e-748d45c81643.png)

내용 트리에서 또 다른 요서에 부합하는 규칙이 필요하다고 가정하고 부합하는 규칙이 순서에 따라 B-E-I라고 치면, 브라우저는 이미 A-B-E-I-L 경로를 계산했기 때문에 트리 안에 이 경로가 있고 할 일이 줄었다.

**트리가 작업량을 줄이는 방법**

- 구조체로 분리

스타일 문맥은 구조체(structs)로 나뉘는데 선 또는 색상과 같은 종류의 스타일 정보를 포함한다. 구조체의 속성들은 상속되거나 또는 상속되지 않는다. 속성들은 요소에 따라 정해져 있지 않은 한 부모로부터 상속된다. 상속되지 않는 속성들은 "재설정(reset)" 속성이라 부르는데 상속을 받지 않는 것으로 정해져 있다면 기본 값을 사용한다.

트리는 최종적으로 계산된 값을 포함하여 전체 구조체를 저장하는 방법으로 도움을 준다. 하위 노드에 구조체를 위한 속성 선언이 없다면 저장된 상위 노드의 구조체 속성을 그대로 받아서 사용하는 것이다.

- 규칙 트리를 사용하여 스타일 문맥을 계산

어떤 요소의 스타일 문맥을 계산할 때 가장 먼저 규칙 트리의 경로를 계산하거나 또는 이미 존재하는 경로를 사용한다. 그 다음 새로운 스타일 문맥으로 채우기 위해 경로 안에서 규칙을 적용한다. 가장 높은 우선순위(보통 가장 구체적인 선택자)를 지닌 경로의 하위 노드에서 시작하여 구조체가 가득 찰 때까지 트리의 상단으로 거슬러 올라간다. 규칙 노드 안에서 구조체를 위한 특별한 선언이 없다면 상당한 최적화를 할 수 있다. 선언이 가득 채워질 때까지 노드 트리의 상위로 찾아 올라가서 간단하게 적용하면 최상의 최적화가 되고 모든 구조체는 공유된다. 이것은 최종 값과 메모리 계산을 절약한다.

선언이 완전하지 않으면 구조체가 채워질 때까지 트리의 상단으로 거슬러 올라간다.

구조체에서 어떤 선언도 발견할 수 없는 경우 구조체는 "상속(inherit)" 타입인데 문맥 트리에서 부모 구조체를 향하면서 성공적으로 구조체를 공유한다. 재설정 구조체라면 기본 값들이 사용될 것이다.

가장 구체적인 노드에 값을 추가하면 실제 값으로 변환하기 위해 약간의 추가적인 계산을 할 필요가 있는데, 트리 노드에서 결과를 저장하기 때문에 자식에게도 사용할 수 있다.

같은 트리노트를 가리키는 형제 요소가 있는 경우 전체 스타일 문맥이 이들 사이에서 공유된다.

#### 6.3.3 쉬운 선택을 위한 규칙 다루기

스타일 규칙을 위한 몇 가지 소스가 있다.

- CSS 규칙을 외부 스타일 시트에서 선언하거나 style 요소에서 선언

`p {color:blue}`

- 인라인 스타일 속성

`<p style="color:blue"></p>`

- HTML의 시각적 속성(이것들은 CSS 규칙으로 변환됨)

`<p bgcolor="blue"></p>`

마지막 두 가지 스타일은 자신이 스타일 속성을 가지고 있거나 HTML 속성을 이용하여 연결할 수 있기 때문에 요소에 쉽게 연결된다.

스타일 시트를 파싱한 후 규칙은 선택자에 따라 여러 해시맵 중 하나에 추가된다. 아이디, 클래스 이름, 태그 이름을 사용한 맵이 있고, 이런 분류에 맞지 않는 것을 위한 일반적인 맵이 있다. 선택자가 아이디인 경우 규칙은 아이디 맵에 추가되고 선택자가 클래스인 경우 규칙은 클래스 맵에 추가된다.

이런 처리 작업을 통해 규칙을 찾는 일은 훨씬 쉬워진다. 맵에서 특정 요소와 관련 있는 규칙을 추출할 수 있기 때문에 모든 선언을 찾아볼 필요가 없다. 이러한 최적화는 찾아야 할 규칙의 95% 이상을 제거하기 때문에 규칙을 찾는 동안 모든 선언을 고려할 필요가 없다.

```css
p.error {color:red}
#messageDiv {height:50px}
div {margin:5px}
```

첫 번째 규칙은 클래스 맵에 추가된다. 두 번째는 아이디 맵에 추가되고, 세 번째는 태그 맵에 추가된다.

위 스타일과 관련된 HTML 코드는 다음과 같다.

```html
<p class="error">an error occurred </p>  
<div id=" messageDiv">this is a message</div>
```

우선 p 요소의 규칙을 찾아보자. 클래스 맵은 발견된 "p.error"를 위한 규칙 하부의 "error"키를 찾았다. div 요소는 아이디 맵(키는 아이디)과 태그 맵에 관련 규칙이 있다. 그러므로 이제 남은 작업은 키를 사용하여 추출한 규칙 중에 실제로 일치하는 규칙을 찾는 것이다.

예를 들어 div에 해당하는 다음과 같은 또 다른 규칙이 있다고 가정하자.

`table div {margin:5px}`

이 예제는 여전히 태그 맵에서 규칙을 추출할 것이다. 가장 우측에 있는 선택자가 키이기 때문이다. 그러나 앞서 작성한 div 요소와는 일치하지 않는다. 상위에 table이 없기 때문이다.

웹킷과 파이어폭스 모두 이런 방식으로 처리하고 있다.

#### 6.3.4 다단계 순서에 따라 규칙 적용하기

스타일 객체는 모든 CSS 속성을 포함하고 있는데 어떤 규칙과도 일치하지 않는 일부 속성은 부모 요소의 스타일 객체로부터 상속 받는다. 그 외 다른 속성들은 기본 값으로 설정된다.

문제는 하나 이상의 속성이 시작될 때 시작되고 다단계 순서가 이 문제를 해결하게 된다.

**스타일 시트 다단계 순서**

스타일 속성 선언은 여러 스타일 시트에서 나타날 수 있고 하나의 스타일 시트 안에서도 여러 번 나타날 수 있는데 이것은 규칙을 적용하는 순서가 매우 중요하다는 것을 의미한다. 이것을 "다단계(cascade)" 순서라고 한다. CSS 명세에 따르면 다단계 순서는 다음과 같다(우선순위가 낮은 것에서 높은 순서임).

1. 브라우저 선언(browser delarations)
2. 사용자 일반 선언(user normal declarations)
3. 저작자 일반 선언(author normal declarations)
4. 저작자 중요 선언(author important declarations)
5. 사용자 중요 선언(user important declarations)

브라우저 선언의 중요도가 가장 낮으며 사용자가 저작자 선언을 덮어 쓸 수 있는 것은 선언이 중요하다고 표시한 경우 뿐이다. 같은 순서 안에서 동일한 속성 선언은 특정성(specificity)에 의해 정렬이 되고 이 순서는 곧 특정성이 된다. HTML 시각 속성은 CSS 속성 선언으로 변환되고 변환된 속성들은 저작자 일반 선언 규칙으로 간주된다.

**특정성**

선택자 특정성은 CSS 명세에 다음과 같이 정의되어 있다.

- 선택자 없이 'style' 속성이 선언된 것이면 1을 센다. 그렇지 않으면 0을 센다. (=a)
- 선택자에 포함된 아이디 선택자 개수를 센다. (=b)
- 선택자에 포함된 속성 선택자(클래스 선택자와 속성 선택자)와 가상 클래스 선택자의 숫자를 센다. (=c)
- 선택자에 포함된 요소 선택자와 가상 요소 선택자의 숫자를 센다. (=d)

네 개의 연결된 숫자 a-b-c-d (큰 진법의 숫자)를 연결하면 특정성의 값이 된다.

예제:

```css
*{} /* a=0 b=0 c=0 d=0 -> specificity = 0,0,0,0 */
li{} /* a=0 b=0 c=0 d=1 -> specificity = 0,0,0,1 */  
li:first-line{} /* a=0 b=0 c=0 d=2 -> specificity = 0,0,0,2 */  
ul li{} /* a=0 b=0 c=0 d=2 -> specificity = 0,0,0,2 */  
ul ol+li{} /* a=0 b=0 c=0 d=3 -> specificity = 0,0,0,3 */  
h1+*[rel=up]{} /* a=0 b=0 c=1 d=1 -> specificity = 0,0,1,1 */  
ul ol li.red{} /* a=0 b=0 c=1 d=3 -> specificity = 0,0,1,3 */  
li.red.level{} /* a=0 b=0 c=2 d=1 -> specificity = 0,0,2,1 */  
#x34y{} /* a=0 b=1 c=0 d=0 -> specificity = 0,1,0,0 */
style="" /* a=1 b=0 c=0 d=0 -> specificity = 1,0,0,0 */
```

**규칙 정렬**

맞는 규칙을 찾으면 다단계 규칙에 따라 정렬된다. 웹킷은 목록이 적으면 버블 정렬을 사용하고 목록이 많을 때는 병합 정렬을 사용한다. 웹킷은 규칙에 ">" 연산자를 덮어 쓰는 방식으로 정렬을 실행한다.

💡 **여기서 버블 정렬이란?**
버블 정렬(bubble sort)이란, 서로 인접한 두원소를 검사하여 정렬하는 알고리즘을 말한다. 인접한 2개의 레코드를 비교하여 크기가 순서대로 되어 있지 않으면 서로 교환한다.

💡 **여기서 병합 정렬이란?**
병합 정렬(merge sort)이란, 분할 정복(divide and conquer) 알고리즘의 하나로, 문제를 작은 2개의 문제로 분리하고 각각을 해결한 다음, 결과를 모아서 원래의 문제를 해결하는 전략이다.

## 6.4 점진적 처리

웹킷은 @import를 포함한 최상위 수준의 스타일 시트가 로드되었는지 표시하기 위해 플래그를 사용한다. DOM 노드와 시각정보를 연결하는 과정(attaching)에서 스타일이 완전히 로드되지 않았다면 문서에 자리 표시자를 사용하고 스타일 시트가 로드됐을 때 다시 계산한다.

💡 **여기서 자리표시자란?**
자리표시자(placeholder)는 나중에 최종 텍스트가 삽입될 때까지 일시적으로 문서 등에 배치되는 텍스트 섹션을 말한다.

---

- 참조

    [https://d2.naver.com/helloworld/59361](https://d2.naver.com/helloworld/59361)

    [https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Regular_Expressions](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Regular_Expressions)

    [https://ko.wikipedia.org/wiki/배커스-나우르_표기법](https://ko.wikipedia.org/wiki/%EB%B0%B0%EC%BB%A4%EC%8A%A4-%EB%82%98%EC%9A%B0%EB%A5%B4_%ED%91%9C%EA%B8%B0%EB%B2%95)

    [https://ko.wikipedia.org/wiki/SGML](https://ko.wikipedia.org/wiki/SGML)

    [https://gmlwjd9405.github.io/2018/05/06/algorithm-bubble-sort.html](https://gmlwjd9405.github.io/2018/05/06/algorithm-bubble-sort.html)

    [https://gmlwjd9405.github.io/2018/05/08/algorithm-merge-sort.html](https://gmlwjd9405.github.io/2018/05/08/algorithm-merge-sort.html)

    [https://educalingo.com/ko/dic-en/placeholder](https://educalingo.com/ko/dic-en/placeholder)