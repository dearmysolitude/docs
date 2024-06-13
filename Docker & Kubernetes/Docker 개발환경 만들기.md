## 도커 명령어
https://docs.docker.com/get-started/overview/
## 이미지/컨테이너 목록 확인
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217897&authkey=%21APHJX5PFwTMjJKQ&width=885&height=202" alt="">
  <figcaption>docker images, docker ps 명령어</figcaption>
</figure>

`docker images`
- 이미지 확인
`docker ps`
- `-a`: 모든 컨테이너 출력
- `docker insprct`: 자세한 컨테이너 정보 확인 가능
### 컨테이너 생성하기: 이미지 파일을 가져와서 실행하는 경우
`docker run -d -p 82:80 --name apache_docker -v /home/lucid/html:/usr/local/apache2/htdocs httpd:latest`
- httpd:latest 도커 이미지를 검색하여 run한다. 
- `-d`: 데몬에서 실행하여 백그라운드 실행/로그 기록
- `-p`: 포트포워딩으로 도커 80 포트를 호스트 82 포트에 연결한다.
- `-name`: 도커 컨테이너의 이름
- `-v` 바인딩할 스토리지 설정
### 컨테이너 생성하기: 이미지 다운로드하여 실행하는 경우
- `docker run -it ubuntu:22.04`: Ubuntu 22.04 버전을 다운로드
- `docker pull 이미지:버전`: 이미지 다운 명령어
- `docker images`: 이미지 확인 명령어
### 동작하는 컨테이너에 접근
`docker exec -it [컨테이너 id] 혹은 [컨테이너 이름] /bin/bash`
- `-i`: interactive, 컨테이너와 대화형 세션을 유지하도록 한다. 입력을 전송하고 출력을 받을 수 있음. 
- `-t`: 가상 터미널 할당. 이 옵션으로 터미널 기반 애플리케이션을 실행하고 명령할 수 있다.
- `/bin/bash` ← Bash shell을 실행하겠다, 
### 도커 이미지 생성
`docker build -t [생성할 이미지 이름] .`
- Dockerfile을 빌드하여 이미지를 생성한다. 
- `.`은 이미지로 만들 해당 위치의 Dockerfile을 지칭하기 위한 것이다.
### 컨테이너 생성하기: 스토리지 바인딩 없이 만든 이미지로
`docker run -d -p 83:80 --name docker-apache2`
- 생성한 이미지를 바로 사용할 경우에는 스토리지 바인딩이 없어도 ㅇㅋ

[도커 컨테이너 / 이미지 관리 - 오준석 brunch](https://brunch.co.kr/@hopeless/10)

> 여기까지가 기본적인 docker의 사용법: 제대로 활용하기 위해서는 여러 Docker를 조율하여 사용해야 한다Docker Compose → Docker Swarm → Kubernetes
### 컨테이너 삭제하기
`docker rm + [컨테이너 ID] 혹은 [컨테이너 이름]`
## Docker Network
<figure style="width: 100%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217896&authkey=%21ABrUYpX_qIvshVA&width=1173&height=379" alt="">
  <figcaption>Docker 네트워크 구조</figcaption>
</figure>
[Docker: Network, 호스트와 컨테이너의 구조 - 피터의 개발이야기](https://peterica.tistory.com/646)
## Docker 외부로 노출시키기
Windows - VM - Docker 형태임.
<figure style="width: 90%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217895&authkey=%21AEkti-BtopOucbY&width=775&height=374" alt="">
  <figcaption>Docker를 외부로 노출시켜보자: 포트포워딩은 필수, Apache2 서버</figcaption>
</figure> 
`host# docker run -p 80:80 -v ~/html:/var/www/html ubuntu/apache2`
- `-p`: 호스트의 포트를 게스트 포트로 연결하지만,  `-v`는 호스트의 디렉터리/파일을 게스트 디렉터리/파일로 연결한다. 
- 호스트 머신의 `~/html` 디렉터리를 컨테이너의 `/var/www/html` 디렉터리(Apache 기본 디렉터리)에 **마운트**한다. 호스트의 파일을 컨테이너에서도 사용할 수 있다.
- 컨테이너가 날아가는 경우, 데이터를 잃어버리지 않기위해 사용하는 방법이다.