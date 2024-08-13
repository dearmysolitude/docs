JAVA Database Connectivity. 자바에서 데이터베이스에 접속할 수 있도록 하는 자바 API. 자바 프로그램에서 데이터베이스에 자료를 쿼리하거나 업데이트하는 방법을 제공한다. 즉 **쿼리문을 DBMS예 전달하여 execute, commit, result(ResultSet)를 받는 것을 자바 프로그램 내에서 수행할 수 있게 해준다.** DBMS에 종속되지 는 관련 API를 제공하며, JDBC API를 사용하기 위해 각 DBMS 업체는 **JDBC Driver Manager**를 제공한다.
> [!note] 참고
> JDBC와 다르게 Root passwd 숨겨짐: JNDI
> ETL(extract transfer load): 일련의 추출/변환/적재 작업을 일컬음
## JDBC
![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217894&authkey=%21AEDACF3f_N-WYJw&width=800&height=362)
다음 예시 코드를 통해 JDBC가 어떤 형태인지 감을 잡을 수 있다.
```java
public class select_data {
	public static void main(String[] args) throws ClassNotFoundException, 
		SQLException, UnsupportedEncodingException {
		Class.forName("com.mysql.cj.jdbc.Driver");
		Connection kopo13_con = DriverManager.getConnection(
			"jdbc:mysql://IP:Port No./linux 명칭", "계정", "비밀번호");
		Statement kopo13_stmt = kopo13_con.createStatement();
		String kopo13_QueryTxt;
		
		// 삼성 전자만 쿼리
		kopo13_QueryTxt = String.format("select * from stockdailyprice 
			where shrn_iscd = '%s';", "A005930");
		
		ResultSet kopo13_rset = kopo13_stmt.executeQuery(kopo13_QueryTxt);
		int kopo13_iCnt = 0;
		while(kopo13_rset.next()) {
			System.out.printf("*(%d)******************\n", kopo13_iCnt++);
			System.out.printf("단축 코드 : %s\n", kopo13_rset.
				getString(1));
			System.out.printf("일자 : %s\n", kopo13_rset.getString(2));
			System.out.printf("시가 : %s\n", kopo13_rset.getInt(3));
			System.out.printf("고가 : %s\n", kopo13_rset.getInt(4));
			System.out.printf("저가 : %s\n", kopo13_rset.getInt(5));
			System.out.printf("종가 : %s\n", kopo13_rset.getInt(6));
			System.out.printf("거래량 : %s\n", kopo13_rset.getDouble(7));
			System.out.printf("거래대금 : %s\n", kopo13_rset.getDouble(8));
			System.out.printf("*****************************\n");
		}
		kopo13_rset.close();
		kopo13_stmt.close();
		kopo13_con.close();
	}
}
```
1. MySQL Driver Manager로부터 Connection 객체를 생성한다: 연결할 DB 정보 설정
2. Connection 객체로부터 Statement 생성: 쿼리 전달하는 객체
3. Statement 객체의 executeQuery 메서드는 결과가 있다면 ResultSet으로 결과를 반환한다.
4. Statement 객체는 Cursor를 가지고 있어 ResultSet의 결과를 한 줄 씩 가져온다.
> [!Note]
> Try-with-resources 구문을 사용하면 예외 처리와 리소스 관리를 더 효과적으로 할 수 있다.
### 개념
- **commit**
	DB에서 SQL문을 데이터베이스에 전송하고 실행하는 과정. JDBC에서 실행된 결과는 ResultSet 객체나 영향받은 행 수로 반환된다.
- **execute**
	DB에서 트랜잭션을 완료하고 데이터베이스에 영구적으로 변경 사항을 적용하는 과정. 데이터베이스는 기본적으로 자동 커밋 모드(auto-commit mode)에 있어 매 SQL 쿼리마다 자동으로 commit 된다.
- **cursor**
	Statement 객체 하나당 ResultSet 객체는 하나씩 존재하며, 이 ResultSet의 데이터를 1row씩 가르키는 포인터를 cursor라고 한다.
- **auto commit**
	- 일반적으로 db처리는 insert, update 문장 수행마다 저장공간(디스크)에 쓰기 때문에 오래 걸린다.
	- 이것을 오토 커밋이라고 하는데, 수만건을 메모리와 같은 버퍼에서 처리한후 , 수만건 되었을때만 디스크에서 쓰게 한다면 처리 속도를 크게 줄일수 있다.
- **PreparedStatement**
	파이썬에서는 default 옵션으로 auto-commit이 deactivate 되어 있고 execute() 후 execute된 각 트랜잭션을 commit()으로 db에 반영한다. JDBC는 auto-commit 이 기본적으로 활성화 되어 있으며, 
## 추가
### JDBC와 JPA
![](https://www.youtube.com/watch?v=Ppqc3qN75EE)
데이터베이스와의 상호작용을 추상화하여 개발자가 데이터베이스 연산을 보다 쉽게 수행할 수 있도록 돕는다.
#### JDBC
- JDBC는 설정이 간단하고, SQL을 직접 제어할 수 있다: 성능 최적화가 중요한 애플리케이션에 유리하다.
- 개발자가 SQL 쿼리를 직접 작성해, 세밀한 제어가 가능하다.
#### JPA
- 자바 표준. ORM 기술을 사용하여 객체와 관계형 데이터베이스 테이블 사이의 매핑을 관리한다.
- 엔티티 클래스로 데이터 테이블과의 매핓 정보를 관리해, 코드의 **가독성과 유지보수성**을 향상시킨다.

