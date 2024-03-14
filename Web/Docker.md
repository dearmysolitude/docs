## 가상화
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217103&authkey=%21AHT9k1OLhSP0_4U&width=1600&height=1360" alt="">
  <figcaption>가상화의 발전</figcaption>
</figure>

## Docker
LXC(리눅스 컨테이너라)는 커널 컨테이너 기술을 이용하여 만든 컨테이너 기술. 컨테이너 기반의 오픈소스 가상화 플랫폼이며 현재 리눅스 컨테이너 기술 부분에서 사실상 표준이 되었다. 다양한 실행 환경을 추상화하고 동일한 인터페이스를 제공하여 프로그램의 배포 및 관리를 단순화하였다.

백엔드 프로그램, 데이터베이스 서버, 메세지 큐 등 어떤 프로그램도 컨테이너로 추상화 가능하며 조립 PC, AWS, Azure, GCP 등 어떤 환경에서나 실행 가능하다.
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217101&authkey=%21AOoJwnPB9h8kRxs&width=655&height=365" alt="">
  <figcaption>Docker와 VM</figcaption>
</figure>
Docker는 완전히 새로운 기술이 아니며, 이미 존재하는 기술을 잘 포장했다고 볼 수 있다. 이미 존재하는 기술을 docker 가 잘 조합하고 사용하기 쉽게 만들었으며 사용자들이 원하는 기능을 간단하지만 획기적인 아이디어로 구현하였다.
### Container
기존의 가상화(Virtual Machine; VM)는 OS를 가상화 하였다: OS가 올라가다보니 사용법이 간단하지만 무겁고 느리다. **성능을 개선하기 위해 격리된 공간에서 process가 동작하는 방식의 가상화를 Linux container라고 한다.**
- 가볍고 빠르며 CPU나 메모리는 process가 필요한 만큼만 추가로 사용한다.
- 하나의 서버에 여러 개의 container를 실행하면 서로 독립적으로 실행
- 실행 중인 container에 접속해 명령어 입력 가능하며, apt-get, yum으로 패키지 설치, 사용자 추가 등이 가능하다.
- 새로운 container 생성 시간 1~2 초로 굉장히 빠르다.
### Image
**이미지란, Container 실행에 필요한 모든 파일과 설정 값 등을 포함하고 있는 것이다.** 따라서 의존성 파일을 컴파일하고 설치할 필요가 없다.
- Container는 image를 실행한 상태로, 추가되거나 변하는 값은 container에 저장되며 같은 image에서 다수의 container 생성이 가능하다.
- Container의 상태와 무관하게 image는 불변이다(Immutable).
- 새로운 서버가 추가되면 미리 만들어 놓은 image를 다운받고 container를 생성하기만 하면 된다.
- 한 서버에서 다수의 container가 실행 가능하며, 수천 대의 서버에도 문제 없다.
- Docker Image는 docker hub에 등록하거나 docker registry 저장소를 직접 만들어 관리 가능하다.
	공개 docker image는 50 만개 이상, docker hub의 이미지 다운로드 수는 80억 회 이상이다.
OS단 레이어는 커널.
도커 이미지 라이브러리: https://hub.docker.com/
라이브러리/The Apache HTTP Server Project: https://hub.docker.com/_/httpd
### Layer 저장 방식
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217100&authkey=%21APv_maisT7culBw&width=894&height=378" alt="">
  <figcaption>이미지</figcaption>
</figure>
- Docker Image는 container를 실행하기 위한 모든 정보를 가지고 있다: 용량이 매우 큼
- 기존 이미지에 파일이 추가되었다고 다시 이미지를 만들고 설치하는 것은 비효율적.
	이를 해결하기 위해 layer개념을 사용하고, union 파일 시스템을 이용해 다수의 layer를 하나의 파일 시스템으로 사용 가능하게 해준다.
- Image는 다수의 read-only layer로 구성되며, 파일이 추가되거나 수정되면 새로운 layer가 생성된다.	변동된 layer만 관리하는 것이다.
- Container 생성 시에도 기존의 image layer 위에 read-write layer를 추가한다.
	Image layer 그대로 사용하면서 container가 실행 중에 생성하는 파일이나 변경된 내용은 read-write layer에 저장되므로 다수의 conatainer를 생성해도 최소한의 용량만 사용한다.
### 이미지 vs. 컨테이너
- 이미지
	**정의**: 읽기 전용 템플릿. 애플리케이션 실행에 필요한 파일과 설정을 포함한다. 이미지는 Docker Hub와 같은 레지스트리에 저장될 수 있다.
	**만들기**
		1. Dockerfile을 사용하여 정의한다.
		2. docker build 명령어를 사용하여 빌드한다.
	**주의사항**
		한 번 이미지가 만들어지면 변경할 수 없다, 다시 이미지를 만들어야 한다.

- 컨테이너
	**정의**: 실행 중인 도커 이미지 인스턴스. 독립적이고 실행 가능한 환경을 제공한다.
	**실행**
		1. docker run 명령어를 사용하여 이미지를 실행한다.
		2. 컨테이너는 이미지에서 정의된 설정과 파일을 사용하여 실행된다.
	**주의사항**
		컨테이너는 변경할 수 있지만 데이터를 저장해야하는 경우 볼륨을 사용해야 한다.

| 기능  | 도커 이미지        | 도커 컨테이너           |
| --- | ------------- | ----------------- |
| 정의  | 읽기 전용 템플릿     | 실행 중인 인스턴스        |
| 만들기 | Dockerfile 사용 | docker run 명령어 사용 |
| 용도  | 배포 및 공유       | 애플리케이션 실행         |
| 특징  | immutable     | mutable           |
> 필요에 따라서 이미지 / 컨테이너를 선택하여 사용한다.
### Docker의 작동
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217102&authkey=%21AJGwf8JfUtfx8mg&width=800&height=800" alt="">
  <figcaption>도커의 작동 원리</figcaption>
</figure>
- 도커 클라이언트: 도커 데몬에 요청한다.
- 도커 호스트: The Docker daemon listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes. 
- 도커 레지스트리: A Docker registry stores Docker images. Docker Hub is a public registry that anyone can use.
`run` 명령이 발생하면:
	1. 도커가 이미지를 레지스트리로부터 불러온다.
	2. 도커가 새로운 컨테이너를 만든다.
	3. 도커가 read-write 파일시스템을 도커에 할당한다.
	4. 도커가 컨테이너에 default 네트워크와 연결할 네트워크 인터페이스를 생성한다.
	5. 도커가 컨테이너를 시작한다.
## 도커 명령어
https://docs.docker.com/get-started/overview/
### 컨테이너 생성하기: 이미지 파일을 가져와서 실행하는 경우
`docker run -d -p 82:80 --name apache_docker -v /home/lucid/html:/usr/local/apache2/htdocs httpd:latest`
- httpd:latest 도커 이미지를 검색하여 run한다. 
- `-d`: 데몬에서 실행하여 백그라운드 실행/로그 기록
- `-p`: 포트포워딩으로 도커 80 포트를 호스트 82 포트에 연결한다.
- `-name`: 도커 컨테이너의 이름
- `-v` 바인딩할 스토리지 설정
### 동작하는 컨테이너에 접근
`docker exec -it [컨테이너 id] /bin/bash`
### 도커 이미지 생성
`docker build -t [생성할 이미지 이름] .`
- Dockerfile을 빌드하여 이미지를 생성한다. 
- `.`은 이미지로 만들 해당 위치의 Dockerfile을 지칭하기 위한 것이다.
### 컨테이너 생성하기: 스토리지 바인딩 없이 만든 이미지로
`docker run -d -p 83:80 --name docker-apache2`
- 생성한 이미지를 바로 사용할 경우에는 스토리지 바인딩이 없어도 ㅇㅋ

[도커 컨테이너 / 이미지 관리 - 오준석 brunch](https://brunch.co.kr/@hopeless/10)

> 여기까지가 기본적인 docker의 사용법: 제대로 활용하기 위해서는 여러 Docker를 조율하여 사용해야 한다Docker Compose → Docker Swarm → Kubernetes