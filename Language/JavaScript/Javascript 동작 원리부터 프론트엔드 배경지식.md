### Bootstrap flex 객체 관리
읽어보기: https://getbootstrap.com/docs/5.3/utilities/flex/

## Javascript의 동작 원리
### 들어가기 전에
#### 자바스크립트의 변수
자바스크립트의 변수는 primitive type과 object가 될 수 있다. 함수도 obejct의 일종이며, Primitive는 메소드와 속성을 가질 수 없지만, object는 속성을 가질 수 있다. 하지만, primitive value의 속성에 접근할 경우 js 런타임이 자동으로 해당 primitive를 위한 object wrapper를 씌워준다. 실행 후에 해당 object는 바로 폐기한다. 따라서 primitive 타입에 대해 새로운 속성을 저장하거나 수정하는 것은 의미가 없다.
#### Closure
함수에서 함수를 정의하여 반환하는 경우, 새로 정의된 함수는 블록 바로 밖 함수에 정의되어 있는 함수의 지역 변수나 파라미터를 함수가 끝나도 계속 참조할 수 있다. 이는 함수가 정의되는 순간 바깥 환경을 capture 해서 heap에 별도의 closure를 형성하기 때문이다.
### JS동작 원리
![JS 환경](https://1drv.ms/i/s!Ano-rmQ7e_nEv31lKO6LT3Rzq-xI?embed=1&width=552&height=529)
자바스크립트의 런타임은 stack, queue, heap 같은 요소들로 구성된다. 자바스크립트는 싱글 스레드 런타임을 가진다. 스택 하나, 큐 하나에 저장하여 저장되어 있는 일들을 한번에 처리하는 것이다.

1.  Heap Memory: 객체들을 저장하기 위한 메모리 영역 힙
2. Call Stack: 함수를 호출했을 때 콜스택이 쌓이는 스택 영역
3.  Web API: 타이머, 네트워크 요청, 파일 입출력, 이벤트 처리 등 브라우저에서 제공하는 다양한 API를 포괄하는 총칭.
	비동기적으로 실행되는 작업들을 전담하여 처리한다(Ajax 호출, 타이머함수, DOM 조작). 
		Chrome에서 멀티스레드로 구현되어 있으며, 비동기 작업에 대해 메인 스레드를 차단하지 않고 다른 스레드를 사용하여 동시에 처리가 가능한 것이다.
1. Callback Queue: 런타임이 처리해야할 일들을 줄 세우는 메시지(태스크) 큐.
	각 메세지는 해당 메세지를 처리하기 위한 연관 함수(콜백 함수 -  비동기적 작업이 완료되면 실행되는 함수들)를 가진다.
![자바스크립트 런타임](https://1drv.ms/i/s!Ano-rmQ7e_nEv2t0V6dpQBJqsowa?embed=1&width=882&height=810)
5.  Event Loop: JS 런타임 모델의 기반이 되는 기능. 코드를 실행하고, 이벤트를 모으고 처리하고, 큐에 저장된 내용을 실행한다.
	JS 엔진의 코드를 살펴보면 main 실행 후 event loop를 실행하는데, 작성한 코드의 흐름이 모두 끝나고 반환이 되어야 event loop가 동작한다. **그리고, 이벤트 루프가 작동해야 큐에 저장된 메시지들이 처리된다.**
![이벤트 루프](https://1drv.ms/i/s!Ano-rmQ7e_nEv20l5HY_RUwjokll?embed=1&width=1008&height=843)
이벤트 루프는 phase에 따라 비동기 I/O나 timer를 기다리고 있는지 등 여러 이벤트를 체크하고 이를 처리한다. 어떤 이벤트도 기다리고 있지 않다면 이벤트 루프도 종료된다.
 이벤트 루프가 실행하는 큐는 단일하게 되어있는 큐가 아님: 일반 태스크를 위한 큐와 마이크로 태스크를 위한 큐로 나누어 관리한다.
 태스크들은 시분할되지 않고 다음 메세지를 처리하기 전에 완전히 처리된다. 현재 **실행 함수는 메인 스레드에서 절대 다른코드에 의해 선점되지 않는다: 메시지 처리가 늦어지면 기다리는 메시지들은 늦게 시작한다. 따라서 각 메세지 처리는 가능한 짧게 분할해야 한다.**
 자바스크립트는 동시성이 없나: **메인 스레드와 이벤트 루프에 한해서는 동시성이 없다고 말할 수 있다.** 비동기적으로 코드의 실행을 뒤로 미루는 것이다. 하지만 특정 작업에 대해서는 다르게 처리된다.
후략: [[JavaScript 의 동시성 처리]]
#### 일반  태스크
- 콘솔이나 \<script> 요소에 의해 직접 실행된 새로운 JS 코드
- 이벤트가 발생했을 때 그 이벤트를 처리하기 위한 콜백 함수
- setTimeout()이나 setInterval()등 timer에 의한 콜백 함수
타이머 함수는 설정한 시간 후 콜백을 바로 실행하지 않고 메세지 큐로 보내기 때문에 실행시간이 정확히 보장되지는 않는다.
#### 마이크로 태스크
해당 마이크로태스크를 생성한 프로그램 혹은 함수가 끝나고 콜스택이 비었을 때, 이벤트 루프로 통제권이 넘어가기 전에 실행되는 짧은 함수.
각 일반 태스크의 처리가 끝나면, 이벤트 루프는 해당 태스크가 다른 JS 코드로 통제권을 넘기는지 확인하고 그렇지 않으면 마이크로태스크 큐의 모든 마이크로 태스크를 먼저 실행한다.
- Promise callbacks: 비동기 프로그래밍을 지원하는 JS기능
	마이크로태스크로 처리되어 일반 태스크에 대해 우선권을 가지면서 큐로 들어가지만, new Promise 등으로 새로운 프로미스를 생성할 때 실행되는 executor 콜백 자체는 생성과 함께 다른 코드와 마찬가지로 동기 실행된다. 내부의 비동기 코드는 비동기로 실행된다.
- Mutation Observer API: DOM 트리의 변경을 감지하는 기능의 브라우저 API
## 패키지 매니저
> apt-get, yum, npm, yarn 

이들은 모두 패키지 매니저로, 설치된 소프트웨어를 관리하는 프로그램이다. 각각 다음 환경에서 패키지를 관리한다.
- apt-get: Ubuntu
- yum: 레드햇 계열 리눅스 배포판
- npm: 자바스크립트
- yarn: Node.js 자바스크립트 런타임 환경
## ECMAScript / ES6 
JavaScript 가 넷스케이프 커뮤니케이션즈로부터 개발된 후, MS에서 JScript를 개발하였는데, 호환되지 못하는 문제로 인해 크로스 브라우징 이슈(기능이 모든 브라우저에서 동일하게 동작하지 않는 이슈)가 발생하였다.
이를 해결하기 위해 표준화된 자바스크립트가 **ECMAScript**이다. _ECMAScript는 JavaScript 프로그래밍 언어가 작동하는 방식에 대한 사양을 제공한다._ ES5, ES6는 버전 숫자를 붙여 ECMAScript를 줄여쓰는 말이다. ES6(2015)는 현대 자바스크립트 개발의 기초가 되는 버전으로, let, const, 클래스, 화살표 함수, 모듈 등의 기능과 코드 가독성을 향상시킨 버전이다.
가장 최근 버전은 ES15로 2024년에 발표되었다([참고: ECMAScript2024(ES15)의 새로운 기능](https://cok2exe.medium.com/%EB%B2%88%EC%97%AD-javascript-ecmascript-2024-es15-%EC%9D%98-%EC%83%88%EB%A1%9C%EC%9A%B4-%EA%B8%B0%EB%8A%A5-%EC%8B%AC%EC%B8%B5-%EA%B0%80%EC%9D%B4%EB%93%9C-c22d580dc204)).
## SPA / MPA / PWA 
SPA와 MPA의 구분하는 기준은 페이지의 갯수이다.
1. SPA(Single Page Application)은 **하나의 페이지로 이루어진 애플리케이션**이다.
	- 최초 렌더링에 필요한 파일을 서버에서 단 한번만 요청하여 전달받게 된다. 최초 렌더링 이후에는 필요 데이터만 요청하여 페이지 내부에서 부분적으로 업데이트 및 렌더링한다.
	- 클라이언트에서 화면 전환을 담당한다.
	- 화면 깜빡임 없이 부드러운 인터렉션을 제공하여 사용자 경험이 향상된다.
2. MPA(Multi Page Application): **여러 페이지로 이루어진 어플리케이션**
	- 필요에 따라 서버로부터 파일을 전달받는다.
	- 대부분 파일 파싱 및 렌더링은 서버에서 담당하게 된다.
	- 필요할 때에만 파일을 요청하기 때문에 초기 렌더링 속도가 빠르다.
### PWA
Progressive Web App. 웹과 네이티브 앱의 기능 모두 이점을 갖도록 특정 기술과 표준 패턴을 사용해 개발된 웹 앱이다.
- 웹은 설치보다 웹사이트 방문이 쉽고 빠르므로 공유도 쉽다.
- 네이티브 앱은 운영체제와 잘 통합되어 부드러운 사용자 경험을 제공한다. 오프라인에서도 동작하며, 선호에 따라 접근이 쉽다.
HTML, CSS, JS 같은 웹 기술로 만드는 앱이어서 브라우저에서 실행되지만, 네이티브 앱처럼 동작하게 된다. 트위터가 예시.
Google I/O 2016 처음 소개되었다.
## CSR / SSR / SSG 
1. CSR(Client Side Rendering), **클라이언트에서 렌더링하는 방식**
	- 화면 전환을 서버가 아닌 클라이언트가 담당한다.
	- SPA는 클라이언트에서 화면을 담당하도록 웹 애플리케이션을 동작시키기 위해 CSR을 사용하게 된다.
2. SSR(Server Side Rendering), **서버에서 렌더링하는 방식**
	- 서버에서 파일에 대한 파싱과 렌더링을 실행하고 최종 결과물 파일을 클라이언트에 전달하는 방식
	- MPA와 같이 필요에 따라 파일을 서버로부터 전달받는 애플리케이션의 경우 이러한 처리가 필요하다.
### SSG
Static Site Generator. **원시 데이터와 템플릿 세트를 기반으로 완전한 정적 HTML 웹사이트를 생성하는 도구이다.**  개별 HTML 페이지를 코딩하는 작업을 자동화하고 해당 페이지를 사용자에게 미리 제공할 수 있도록 준비한다.미리 빌드되어 있어 사용자의 브라우저에서 빠르게 로드할 수 있다.
## 웹 표준과 sementic web
여러 **국제 표준 기구가 정한 규칙으로 웹이 작동하는 방식을 정의**한다. 웹사이트와 네트워크 시스템이 준수해야 하느 표준에 대한 아이디어는 다음과 같은 기관에서 담당한다.
- IETF: URI, HTTP, MIME 사용 등에 대한 인터넷 표준(STD)
- W3C: 마크업 언어(HTML)의 명세, 스타일 정의(CSS), DOM, 접근성
- IANA: 이름과 숫자 레지스트리
- Ecma Intl.: JavaScript 스크립팅 표준
- ISO: 문자 인코딩, 웹사이트 관리, 사용자 인터페이스 디자인 등
### Semantic Web
사람이 읽고 이해할 수 있는 **HTML이나 xml 같은 리소스의 의미를 프로그램이 이해할 수 있도록 하는 확장된 웹의 개념.** RDF, Microformats, Microdata 등의 방식이 있음
## 웹 브라우저의 동작 원리
![브라우저의 동작](https://1drv.ms/i/s!Ano-rmQ7e_nEwAHIn06EwNzi40vw?embed=1&width=934&height=578)
### 브라우저의 기본 구조
![웹브라우저의 구조](https://1drv.ms/i/s!Ano-rmQ7e_nEv3sx1h9V7wioP13V?embed=1&width=496&height=380)
1. 사용자 인터페이스: 사용자가 접근할 수 있는 영역. 페이지를 보여주는 창을 제외한 나머지 부분
2. 브라우저 엔진: 사용자 인터페이스와 렌더링 엔진 사이 동작을 제어한다. Data Storage를 참조하여 데이터를 로컬에 읽고 쓴다.
3. 렌더링 엔진: 웹 서버로부터 받은 자원을 브라우저에 표시한다. HTML문서를 응답받으면 HTML 파서와 CSS 파서에 의해 파싱되어 DOM, CSSOM트리로 변환되고 렌더 트리로 결합한다. 이를 기반으로 페이지를 표시하게 된다.
4. 통신; 서버와 통신을 가능하게 하는 네트워크 통신부
5. UI 백엔드: select, input등 기본 위젯을 그리는 인터페이스
6. 자바스크립트 해석기: 자바스크립트를 해석하고 실행
7. 자료 저장소: Cookie, Local Storage, IndexedDB등 브라우저 메모리를 활용하는 저장영역
### 렌더링 엔진의 동작 과정
![렌더링 엔진의 동작 과정](https://1drv.ms/i/s!Ano-rmQ7e_nEv395ZuoYdLVdwuaR?embed=1&width=634&height=122)
1. HTML문서를 파싱하여 **DOM** 트리를 구축한다.
2. 외부 CSS 파일과 함께 포함된 스타일 요소를 파싱한다(**CSSOM**).
3. DOM 트리와 2의 결과물을 합쳐 **렌더 트리**를 구축한다.
4. 렌더 트리 각 노드에 대해 화면 상에서 배치할 곳을 결정한다.
5. UI 백엔드에서 렌더 트리의 각 노드를 그린다.
### 자바스크립트는?
1. 자바스크립트 엔진이 처리한다. HTML 파서는 \<script>를 만나면 JavaScript 코드를 실행하기 위해 DOM 생성 프로세스를 중지하고 자바스크립트 엔진으로 권한을 넘긴다. 
2. 자바스크립트 엔진은 태그 내의 코드, src속성에 정의된 JavaScript 파일을 로드하고 파싱, 실행한다.
3. 실행이 완료되면 다시 HTMl 파서로 제어 권한을 넘겨서 중지했던 시점으로 돌아가 DOM 생성을 재개한다.
### 정리
- 브라우저는 동기적으로 HTMl, CSS, JavaScript를 처리한다.
- 하지만 자바스크립트 엔진에 제어 권한이 있을 때 JavaScript 코드가 완성되지 않은 DOM을 조작하게 되면 에러가 발생한다.
- HTML파일에서 JavaScript 코드를 \<body> 태그 하단에 위치시키는 이유이다.
## 참고 자료
[자바스크립트 동작 원리에 대한 깊은 이해](https://medium.com/@lifthus531/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B9%8A%EC%9D%80-%EC%9D%B4%ED%95%B4-dd4fd47d8917)
[자바스크립트 동작 원리와 call stack, event loop](https://bbangson.tistory.com/89)
[자바스크립트 이벤트 루프 동작 구조](https://inpa.tistory.com/entry/%F0%9F%94%84-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84-%EA%B5%AC%EC%A1%B0-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC#%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%9D%98_%EB%82%B4%EB%B6%80_%EA%B5%AC%EC%84%B1%EB%8F%84)
[자바스크립트 원리 - 엘도라도님 블로그](https://it-eldorado.tistory.com/86)

[SPA, MPA 그리고 CSR과 SSR](https://medium.com/@sebinndev/spa-mpa-%EA%B7%B8%EB%A6%AC%EA%B3%A0-csr%EA%B3%BC-ssr-89de972ab153)
[프로그래시브 웹 앱 소개](https://developer.mozilla.org/ko/docs/Web/Progressive_web_apps/Tutorials/js13kGames)
[SSG란?](https://www.cloudflare.com/ko-kr/learning/performance/static-site-generator/)
[웹 표준- mdn](https://developer.mozilla.org/ko/docs/Glossary/Web_standards)
[Semantic Web의 의미는 무엇일까?](https://medium.com/@pranne1224/semantic-web-%EC%9D%98%EB%AF%B8%EB%8A%94-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C-feat-microformats-vs-schema-org-%EC%9D%B4%EB%A1%A0%ED%8E%B8-bd1fc8345d7e)
[웹 페이지를 표시한다는 것: 브라우저는 어떻게 동작하는가 - mdn](https://developer.mozilla.org/ko/docs/Web/Performance/How_browsers_work)
[브라우저는 어떻게 동작하는가? - Naver D2](https://d2.naver.com/helloworld/59361)

