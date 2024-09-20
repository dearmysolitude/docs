## 인트로
웹서비스는 다양한 형태로 제작되어 배포된다. 웹을 통해 데이터를 전송하는 방법은 시간에 따라 변화해 왔다. 이 페이지에서는 웹 서비스가 데이터를 주고받기 위한 데이터 포맷과 이를 전달하는 프로토콜들에 대해 과거로부터 현재까지의 기술을 대략적으로 확인해 본다.
> 좀 더 포괄적인 개념인 모듈 간 데이터의 연계인 <**인터페이스**>에 대한 내용은 다음 문서를 참고할 것 [[인터페이스 개발|ComputerScience/인터페이스]]
## 다양한 웹 서비스들
이들은 상호 배타적이지 않고, 필요에 따라 여러 방법을 활용할 수 있다.
### 웹 페이지 퍼블리싱
- HTML, CSS, JavaScript를 사용하여 정적 또는 동적 웹페이지를 만든다.
- 웹 호스팅 서비스를 통해 이 페이지들을 인터넷에 게시.
- 사용자들이 웹 브라우저를 통해 직접 접근하도록 한다.
### 웹 애플리케이션
소개글: [웹 애플리케이션 소프트웨어 - Scopic 기술 포스트](https://scopicsoftware.com/ko/blog/web-application-software/)
- 서버 사이드 프로그래밍 언어(예: Python, Java, Ruby)와 프레임워크를 사용하여 동적 웹사이트를 만든다.
- 데이터베이스와 연동하여 사용자 정보 관리, 콘텐츠 제공 등의 기능을 구현한다.
### Open API
- 다른 개발자나 애플리케이션이 사용할 수 있는 데이터나 기능을 제공한다.
- API 키를 통해 접근을 제어하고 사용량을 모니터링할 수 있다.

> [!note] Open API
> **The OpenAPI Specifications provides a formal standard for describing HTTP APIs.**
> The OpenAPI Specification (OAS) provides a consistent means to carry information through each stage of the API lifecycle. It is a specification language for HTTP APIs that defines structure and syntax in a way that is not wedded to the programming language the API is created in. API specifications are typically written in YAML or JSON, allowing for easy sharing and consumption of the specification.
> 
> 출처: [Open API Initiative](https://www.openapis.org/)

간단히 말하자면, open api는 누군가가 서비스를 공개한 것이 Open API와, 개발자는 그것을 사용하여 외부 기능을 쉽게 사용할 수 있게 해 준다.
### 프로그래시브 웹앱
소개: [프로그래시브 웹 앱 소개 - mozilla](https://developer.mozilla.org/ko/docs/Web/Progressive_web_apps/Tutorials/js13kGames)
- 웹과 네이티브 앱의 기능 모두의 이점을 갖도록 수 많은 특정 기술과 표준 패턴을 사용해 개발된 웹 앱. 웹 기술로 만들어졌지만 네이티브 앱과 유사한 사용자 경험을 제공한다.
- 오프라인 기능, 푸시 알림 등을 지원한다.
- 다양한 참여자에 의해 지속적으로 논의되고 있는 개발 철학으로, 새로운 형태의 웹 애플리케이션이다: 현재 진행형
## 데이터 포맷
### SGML
Standard Generalized Markup Language. 텍스트, 이미지, 오디오 및 비디오 등 멀티미디어 전자문서들을 다른 기종의 시스템들과 정보 손실 없이 효율적으로 전송, 저장 및 자동 처리하기 위한 언어. 1986년에 GML을 개선하여 개발되었다. SGML은 **복잡**하고 **브라우저 간 HTML 문법의 호환이 불가**했기 때문에, XML이 등장하게 된다.  
### XML
- eXtreme Markup Language. 
-  다목적 마크업 언어로 구조화된 데이터를 표현하여 저장하고 전송하기 위한 마크업 언어.
- 사람과 기계 모두가 읽을 수 있도록 설계된 텍스트 기반 형식
- 구조
	- 루트: 문서 최상위 요소
	- 요소(Elements): 시작 태그와 종료 태그로 둘러쌓여 있다.
	- 속성(Attribute): 요소의 추가 정보를 제공하는 이름-값 쌍
- 주요 특징
	- 자체 설명적: 데이터와 구조를 동시에 표현
	- 확장 기능: 사용자가 자신만의 태그를 정의
	- 플랫폼 독립적: 다양한 시스템과 애플리케이션에 사용 가능
```xml
<?xml version="1.0" encoding="UTF-8"?>
<person>
  <name>John Doe</name>
  <age>30</age>
  <city>New York</city>
  <hobbies>
    <hobby>reading</hobby>
    <hobby>swimming</hobby>
  </hobbies>
  <isStudent>false</isStudent>
</person>
```
- 장점
	- 높은 유연성: 사용자 정의 태그 생성 가능
	- 데이터 검증: DTD, XML Schema를 통한 문서 구조 검증 가능
	- 데이터와 표현의 분리: XSLT를 통해 다양한 형식으로 변환 가능
- 단점
	- 복잡(verbose): JSON에 비해 데이터 크기가 크다.
	- 파싱 복잡성: JSON에 비해 파싱이 더 복잡할 수 있다.
- 사용 사례
	- 설정 파일(Maven의 pom.xml, Android의 AndroidManifest.xml)
	- 웹 서비스(SOAP 프로토콜)
	- RSS 피드
	- SVG 그래픽
	- 데이터 교환 방식(복잡한 구)
### JSON
- JavaScript Object Notation.
- 경량화된 데이터 교환 형식. 사람이 읽기 쉽고, 기계가 파싱하고 생성하기 쉬운 텍스트 기반 형식이다. 데이터 객체를 **속성-값 쌍 형태(Attribute-Value Pair)로 표현하는 개방형 표준 포맷**
- 구조
	- 객체: 중괄호{} 로 둘러쌓인 키-값 쌍의 집합
	- 배열: 대괄호[] 로 둘러싸인 값들의 순서 있는 목록
- 데이터 타입: 문자열, 숫자, 불리언, 객체, 배열
```json
{
  "name": "John Doe",
  "age": 30,
  "city": "New York",
  "hobbies": ["reading", "swimming"],
  "isStudent": false
}
```
- 장점
	- 언어 독립적: 대부분의 프로그래밍 언어에서 지원
	- 간결성: XML에 비해 더 간결하고 읽기 쉬움
	- 파싱 용이성: JavaScript에서 쉽게 파싱 가능
- 사용 사례
	- API 응답 데이터 형식
	- 설정 파일
	- NoSQL 데이터베이스의 데이터 저장 형식
> [!note] **XML vs. JSON**
>
>- XML은 더 복잡하고 풍부한 데이터 구조를 표현할 수 있지만, JSON이 더 간결함
>- XML은 속성을 통해 메타데이터를 더 쉽게 표현할 수 있음
>- JSON은 JavaScript와의 호환성이 더 높아 웹 애플리케이션에서 더 선호됨
### 🪄 Ajax
[AJAX  - mdn](https://developer.mozilla.org/ko/docs/Glossary/AJAX)
[AJAX란? JQuery를 이용한 AJAX 사용법 - LightSourceCoder](https://scoring.tistory.com/entry/AJAX%EB%9E%80-JQuery%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-AJAX%EC%82%AC%EC%9A%A9%EB%B2%95)
- Asynchronous JavaScript and XML
- 비동기적으로 서버와 데이터를 주고받고 웹 페이지를 업데이트하는 기술의 집합. **자바스크립트(JavaScript)를 사용하여 클라이언트와 서버 간에 XML 데이터를 주고 받는 비동기 통신 기술**.
- 구성 요소
	- JavaScript: 클라이언트 측 스크립팅
	- XML HttpRequest 객체 또는 Retch API: 서버와의 비동기 통신
	- DOM: 동적으로 페이지 내용 변경
	- JSON 또는 XML 사용
- 작동 방식
	1. 사용자 액션 발생
	2. JavaScript 가 XMLHttpReqeust 객체를 생성하고 서버에 요청 전송
	3. 서버가 요청을 처리하고 응답 반환
	4. JavaScript 가 응답을 받아 필요한 부분만 페이지 업데이트
```javascript
fetch('https://api.example.com/data')
  .then(response => response.json())
  .then(data => {
    console.log(data);
    // 여기서 DOM 업데이트
  })
  .catch(error => console.error('Error:', error));
```
- 장점
	- 비동기 통신: 페이지 전체를 새로고침 하지 않고 일부만 업데이트 가능
	- 사용자 경험 향상: 빠른 응답 시간과 부드러운 인터랙션
	- 서버 부하 감소: 필요한 데이터만 전송받음
- 사용 사례
	- 실시간 검색 제안
	- 폼 유효성 검사
	- 무한 스크롤
	- 댓글 시스템

> [!note] JSON과 AJAX의 관계
> - AJAX는 원래 XML을 주로 사용했지만, 지금은 JSON을 더 많이 사용함
> - JSON의 간결성과 JavaScript와의 호환성 때문에 AJAX 요청의 응답 형식으로 JSON이 선호됨

**이외 자주 등장하는 마크업 언어들에 대한 이야기는 다른 문서에서 해보도록 한다. 모두 특수한 목적을 가지고 만들어졌다: yaml, html, TeX**
## 프로토콜
### SOAP
![Soap message](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%218050&authkey=%21AOxuTxgVmqqVRTg&width=380&height=664)
- XMl 기반의 메시지 프로토콜
- 주로 웹서비스에 이용
#### SOAP 아키텍처
![soap architecture](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%218049&authkey=%21AI5aBvlgpsA9PJc&width=660&height=543)
SOAP는 UDDI 레지스트리라는 걸 통해서 웹서비스를 등록하고, 탐색하고, 바인딩해서 사용한다. WSDL은 XML, UDDI는 검색엔진 이라고 생각하자...
1. Service requestor가 SOAP로 인코딩하여 웹 서비스를 Service provider 에게 요청하면
2. Service provider는 이걸 디코딩해서 요청한 거에 맞는 비지니스 로직을 수행하고, 그결과를 다시 SOAP로 인코딩해서 리턴 한다.
#### WSDL
- Web Services Description Language
- 웹 서비스와 관련된 서식이나 프로토콜 등을 표준적인 방법으로 기술하고 게시하기위한 언어
- XML로 작성되며 UDDI의 기초가 된다.
- 클라이언트는 WSDL파일을 읽어 서버에서 어떠한 조작이 가능한지 파악할 수 있다.
###  ✨✨✨ REST ✨✨✨
- HTTP 프로토콜을 최대한 활용하는 아키텍처 스타일
- 리소스 중심의 설계, 간단하고 확장성이 좋다.

RESTfull API 살펴보기: [[TODO. REST]]
### GraphQL
- 클라이언트가 필요한 데이터를 정확히 요청할 수 있는 쿼리 언어
- 오버페칭과 언더페칭 문제 해결
## 그래서 뭘 써?
### 사용 기술
- 웹 애플리케이션: 다양한 방식 사용 가능, 주로 REST와 JSON 조합이 인기
- 웹페이지: 전통적인 HTTP 통신, 동적 요소에 AJAX 활용
- Open API: REST와 JSON이 주류, 때에 따라 SOAP/XML 사용
- PWA: 모던 웹 기술 활용, REST/GraphQL과 JSON 주로 사용
### 트렌드
- JSON이 XML보다 선호되는 추세
- REST가 SOAP를 대체하고 있음
- GraphQL의 사용이 증가하는 추세
- 실시간 통신을 위한 WebSocket 사용 증가
## 참고 자료
[Open API - 코딩의 성지](https://devkingdom.tistory.com/11)
[XML, JSON, Ajax, SOAP, REST - gkdisakdmaqk](https://blog.naver.com/gkdisakdmaqk/221320917084)
