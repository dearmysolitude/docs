서버(가상 머신, Ubuntu 22.04)에서 DB(MySQL)를 사용하도록 개발환경을 설정해 보자.
## 네트워크 설정: VM 포트포워딩
1. 포트포워딩 설정: **MySQL → 외부 접근 port: 33060, 내부 접근 port: 3360** 내 pc의 MySQL(설치 했을 경우)은 3306으로 이으므로(기본값) 혼동되지 않도록 33060으로 설정해 준다.
2. `/etc/mysql/mysql.conf.d/mysqld.cnf` 파일에 들어가 127.0.0.1만 연결되게 되어 있는 부분을 주석처리 해 준다.
	`#bind-address           = 127.0.0.1`
	`#mysqlx-bind-address    = 127.0.0.1`
3. 처리후 재부팅`
## MySQL 설정
처음 설치 된 MySQL은 Root에 대한 패스워드가 설정되어 있지 않다. 원격 연결을 위해 MySQL의 설정을 바꾸어 주자.
- `use mysql`: mysql데이터베이스는 MySQL db의 주요 정보가 들어있다.
- `SELECT host, User, plugin, authentication_string FROM mysql.user;`: mysql의 정의된 사용자를 찾아본다, 또 패스워드가 정의 되어 있는지 확인한다.
- `ALTER user 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'kopoctc(너의root비번)';`: root의 패스워드를 설정한다(서버 내부에서만 사용되는 root 사용자).
- `create user 'root'@'%' identified by '[설정할비밀번호]';`: 모든 원격사용자(모든 IP의 뜻 %)가 사용하는 root의 패스워드를 설정함.
- `grant all privileges on *.* to 'root'@'%';`: 원격에서 접속하는 root사용자는 모든 권한, (\*.\*읽기쓰기만들기등등)을 가지도록.
- `flush privileges;`: 마지막으로 권한 설정을 확인한다.
`quit`로 빠져나간후 다시 mysql 로 접속해보자: Id password가 생겨서 `mysql –uroot -p[설정한비밀번호]`로 접속이 가능해졌다.