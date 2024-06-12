JAVA Database Connectivity. 자바에서 데이터베이스에 접속할 수 있도록 하는 자바 API. 자바 프로그램에서 데이터베이스에 자료를 쿼리하거나 업데이트하는 방법을 제공한다. DBMS에 종속되지 는 관련 API를 제공하며, JDBC API를 사용하기 위해 각 DBMS 업체는 **JDBC Driver Manager**를 제공한다. 
> [!note] 참고
> JDBC와 다르게 Root passwd 숨겨짐: JNDI
> ETL(extract transfer load): 일련의 추출/변환/적재 작업을 일컫는 말

## JDBC 클래스
![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217894&authkey=%21AEDACF3f_N-WYJw&width=800&height=362)
다음 자바 코드를 통해 JDBC가 어떤 형태인지 감을 잡을 수 있다.
## 오토 커밋
- 일반적으로 db처리는 insert, update 문장 수행마다 저장공간(디스크)에 쓰기 때문에 오래 걸린다.
- 이것을 오토 커밋이라고 하는데, 오토 커밋을 하지말고, 수만건을 메모리와 같은 버퍼에서 처리한후 , 수만건 되었을때만 디스크에서 쓰게 한다면 처리속도를 크게 줄일수 있다.

* 리눅스(기본 10g) 디스크 용량 늘려라
* select count(*)로 몇 건인지 세도록 한다: workbench는 1000개 제한

// 3: 키 필드가 두번 들어감: 실제 데이터에 키를 바로 넣어서 작업