클라이언트 단에서 돌아가는 Javascript와 달리 JSP는 서버에서 동작한다.

### JSTL
- JSTL로 로직을 윗 부분으로 뽑아내 라이브러리가 객체를 전달하도록 하면 view만 (jsp의) body에서 뽑아낼 수 있다.
- 이 경우 JSP로 구현된 View + Controller를 Spring 적용 시 로직 부분을 빼내어 View와 Controller를 분리하기 쉬워진다.