Server side rendering - Ajax - jQuery - Angular.js  - React - VueJS - Svelte
데이터를 서버로부터 받았으면 이를 DOM을 통해 구조화하여 렌더링 하는 것이 브라우저의 역할
## Ajax

## jQuery

## AngularJS
- Data two-way binding
	- 스코프 안에서 컴포넌트끼리 데이터를 공유
- digest loop/cycle
- Typescript를 사용하는 Angular 프레임워크와는 구분됨, AngularJS를 재작성한 2016 출시된 프로젝트
## React
- Data one-way binding:  사용자가 실수로 데이터를 변경했을 때 변경되지 않도록.
- Virtual DOM: 
	- 부모노드가 변경되면 다시 렌더링해야 했던 원래 DOM의 단점을 극복하기 위함
	- JS로 복제시킨 후 변경된 부분을 VirtualDOM과 실제 DOM을 비교하여 구해서 
	- 변경된 부분만 렌더링을 해준다.
- 페이지를 여러 부분으로 쪼개서 관리하는 기능을 제공함: React는 페이지 요소의 컴포넌트화하여 이어붙이는 역할을 한다.
	- state: 자기 자신의 데이터를 관리
	- props: 다른 컴포넌트에서 주는 데이터를 관리
	- 컴포넌트는 HTML처럼 생긴 JSX로 UI를 그린다
	- 컴포넌트 = customized HTML Tag
### 예제
- 선언
```html
<CustomComponent name = "버튼"/>
```
- 선언한 Custom 컴포넌트는 함수로 정의해준다.
- Attribute로 선언된 properties가 인자로 들어온다.
- State로 컴포넌트의 값을 관리한다: userState = React hook
- 리턴은 렌더링할 HTML태그를 반환해야 함: JSX
```Javascript
export function CustomComponent(props) {
	const [count, setCount]
	return (
		<div>
			<h1>Hello, {props.name}</h1>
			<button onClick{() => setCount((count) => count + 1)}<{count} </button>
		<div>
	)
}
```

CRA: Create React App
### 참고할만한 사이트
[https://react.dev/](https://react.dev/)
[https://create-react-app.dev/](https://create-react-app.dev/)
[https://d2.naver.com/helloworld/59361](https://d2.naver.com/helloworld/59361)
[https://devbusan.github.io/rygit-ko/](https://devbusan.github.io/rygit-ko/)
## Vue.JS
Angular와 React의 장점을 흡수하려는 프로젝트
- Two way binding:

## Svelte
- 컴파일 언어