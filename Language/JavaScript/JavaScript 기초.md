## 특징
- Dynamic Typing(동적 타이핑): 변수의 타입을 선언할 필요가 없으며, 런타임에 타입이 결정됨.
- Object-Oriented(객체 지향성): 프로토타입 기반의 객체지향 프로그래밍을 지원
- Single-Thread(싱글스레드): 기본적으로 하나의 스레드로만 실행되지만, 비동기 처리를 통해 효율적인 작업 수행이 가능.
- 함수형 프로그래밍: 함수를 일급 객체로 취급하여 함수형 프로그래밍을 지원한다.
- Event-Driven(이벤트 기반): 사용자 상호작용이나 다른 이벤트에 반응하는 프로그래밍 모델을 제공함.
- 인터프리터 언어: 컴파일 없이 인터프리트에 의해 한 줄씩 해석되고 실행된다.
---
> Deeper...
- Closures(클로저)): 함수와 그 함수가 선언되었을 때 렉시컬 환경과의 조합이 지원됨.
- Prototype-based Inheritance(프로토타입 상속): 클래스 기반이 아닌 프로토타입 기반 상속 모델 사용
-  Hoisting(호이스팅): 변수와 함수 선언이 스코프의 최상단으로 끌어올려지는 현상이 있다.
- Dynamic Scope Chain(동적 스코프 체인): 실행 컨텍스트에 따라 변수의 스코프가 동적으로 결정.
- 비동기 프로그래밍: 콜백, 프로미스, async/await 등을 통해 비동기 작업을 효과적으로 처리할 수 있다.
- 브라우저 API 접근: DOM 조작, AJAX 요청 등 브라우저의 다양한 기능에 접근할 수 있다.
## First-class object: 1급 객체
1. 변수에 할당할 수 있음
2. 함수의 매개변수로 전달할 수 있음
3. 함수의 반환값으로 사용할 수 있음
4. 자료구조(배열, 객체)에 저장할 수 있음
5. 동적으로 프로퍼티 할당이 가능함
자바스크립트에서, 함수는 1급 객체로 취급된다. 함수를 다른 데이터 타입과 동이랗게 다룰 수 있다는 것을 의미한다.
```javascript
function add(a, b) {
	return a + b;
}

function calc(func, a, b) {
	return func(a, b);
}

function Main() {
	r = calc(add, 1, 2);
}
```

## JS의 다양한 객체
### Array
```js
var arr = new Array("빨간색", 1010, "deadfire", "Javascript", "XX");
document.write("<H2>Original</H2>");

for(var i = 0; i < arr.length; i++) {
	document.write("arr["+i+"]: " + arr[i] + "<BR>");
}
arr.sort();

document.write("<HR><H2>기본정렬</H2>");
for(var i = 0; i < arr.length; i++) {
	document.write("arr["+i+"]: " + arr[i] + "<BR>");
}

function compare(a, b) {
	if(a < b) return 1;
	else if(a > b) return -1;
	else return 0;
	}
arr.sort(compare);
document.write("<H2>역순 정렬</H2>");
for(var i = 0; i < arr.length; i++) {
	document.write("arr["+i+"]: " + arr[i] + "<BR>");
}
```
### Date
```js
var d = new Date(2020, 11, 31);
document.write("년도는: "+ d.getYear() +
	"월은: " + d.getMonth() +
	"일은: " + d.getDate() +
	"요일은: " + d.getDay() +
	"시는: " + d.getHours() +
	"분은: " + d.getMinutes() +
	"초는: " + d.getSeconds() +
	"밀리초: " + d.getMilliseconds() + "<HR>");
```
### Document
```js
doc = open("", "output"); // output이라는 프레임 생성
doc.document.write('<BODY><H1>Hello!</H1>');
doc.document.write('</BODY>');
doc.document.close();
```
### History
페이지 내에서 서버에 이전 페이지에 대한 요청을 보내지 않고 뒤로가기를 실행하는 기능을 수행할 수 있다.
```HTML
<HTML>
    <HEAD>
        <SCRIPT>
            var name = "hello";
  
            function call1() {
                // history.go(-1);
                // history.back();
                // history.forward();
            }
        </SCRIPT>
    </HEAD>
    <BODY>
        <SCRIPT>call1();</SCRIPT>
        Good...
    </BODY>
</HTML>
```
### Location
```js
function call1() {
	// window.location = "http://www.kopo.ac.kr";
	window.status = "Hello, 어디에 표시되나?";
	// window.open("http://www.kopo.ac.kr", "newWindow", "toolbar=yes, width=200");
}
```
### Navigator
```js
for(obj in navigator) {
	document.write(obj + ": " + navigator[obj] + "<BR>");
}
```
### String
```js
function call() {
	var MyString = "Hello World Welcome to my Channel";
	document.write("Test1: " + MyString.charAt(11) + "<br>")
	document.write("Test2: " + MyString.indexOf("to") + "<br>")
	document.write("Test3: " + MyString.lastIndexOf("to") + "<br>")
	document.write("Test4: " + MyString.substr(5, 5) + "<br>")
	document.write("Test5: " + "AaBbCc".toLowerCase() + "<br>")
	document.write("Test6: " + "AaBbCc".toUpperCase() + "<br>")
}
```