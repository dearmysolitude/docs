## 도커 설치
```shell
host$ sudo apt-get update

host$ sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common

host$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

host$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

host$ sudo apt-get update

host$ sudo apt-get install docker-ce docker-ce-cli containerd.io

host$ sudo systemctl status docker

host$ sudo usermod -aG docker $USER

host$ sudo docker run hello-world
```

### 설치 확인
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217903&authkey=%21ADzNMImlC9Agk54&width=1254&height=708" alt="">
  <figcaption>Docker 설치 확인</figcaption>
</figure>
---------------
## 도커 기본 명령어
https://docs.docker.com/get-started/overview/
### 이미지/컨테이너 목록 확인
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

> 여기까지가 기본적인 docker의 사용법: 제대로 활용하기 위해서는 여러 Docker를 조율하여 사용해야 한다 Docker Compose → Docker Swarm → Kubernetes
### 컨테이너 삭제하기
`docker rm + [컨테이너 ID] 혹은 [컨테이너 이름]`
### 리소스 삭제
```shell
host$ docker container prune (중지된 컨테이너 모두 삭제)
host$ docker image prune (이름 없는 이미지 모두 삭제)
host$ docker network prune (미사용 네트워크 모두 삭제)
host$ docker volume prune (미사용 볼륨 모두 삭제)
host$ docker system prune -a (한꺼번에 모두 삭제)
```

---------------
## Docker Network
<figure style="width: 100%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217896&authkey=%21ABrUYpX_qIvshVA&width=1173&height=379" alt="">
  <figcaption>Docker 네트워크 구조</figcaption>
</figure>
[Docker: Network, 호스트와 컨테이너의 구조 - 피터의 개발이야기](https://peterica.tistory.com/646)
## Docker Volume: Stateless
Windows - VM - Docker 형태임.
<figure style="width: 90%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217895&authkey=%21AEkti-BtopOucbY&width=775&height=374" alt="">
  <figcaption>Docker를 외부로 노출시켜보자: 포트포워딩은 필수, Apache2 서버</figcaption>
</figure> 
`host# docker run -p 80:80 -v ~/html:/var/www/html ubuntu/apache2`
- `-p`: 호스트의 포트를 게스트 포트로 연결하지만,  `-v`는 호스트의 디렉터리/파일을 게스트 디렉터리/파일로 연결한다. 
- 호스트 머신의 `~/html` 디렉터리를 컨테이너의 `/var/www/html` 디렉터리(Apache 기본 디렉터리)에 **마운트**한다. 호스트의 파일을 컨테이너에서도 사용할 수 있다.
- 컨테이너가 날아가는 경우, 데이터를 잃어버리지 않기위해 사용하는 방법이다.
### 1. 호스트 볼륨 공유
1) DB 이미지 만들기
`host$ docker run -d --name wordpressdb -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=wordpress mysql:5.7`
`-d`: 데몬으로 실행,
`-e`: 컨테이너 만든 후 환경변수를 등록

2) 워드프레스 웹서버 만들기
`host$ docker run -d -e WORDPRESS_DB_HOST=mysql -e WORDPRESS_DB_USER=root -e WORDPRESS_DB_PASSWORD=password --name wordpress --link wordpressdb:mysql -p 80:80 wordpress`

### 2. 볼륨 컨테이너
<figure style="width: 100%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217901&authkey=%21ALB8sZI5T3d5ku8&width=357&height=267" alt="">
  <figcaption>볼륨 컨테이너</figcaption>
</figure>
```shell
host$ docker run -it --name con0 -v ~/data:/data ubuntu:22.04
host$ docker run -it --name con1 --volumes-from con0 ubuntu:22.04
host$ docker run -it --name con2 --volumes-from con0 ubuntu:22.04
```

### 3. Docker Volume
<figure style="width: 100%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217902&authkey=%21ALoEB-fwO3d4ooM&width=523&height=375" alt="">
  <figcaption>Docker volume</figcaption>
</figure>
```shell
host$ docker volume create --name myvolume
host$ docker volume ls
host$ docker run -it --name con1 -v myvolume:/root/ ubuntu:22.04
con1# echo hello volume >> /root/volume
host$ docker run -it --name con2 -v myvolume:/root/ ubuntu:22.04
con2# cat /root/volume
host$ sudo cat /var/lib/docker/volumes/myvolume/_data
```

----------------
## 도커 네트워크

-------------

## 컨테이너 로그
오류가 발생하면 log에서 ERROR 메세지로 출력되므로 keyword를 ERROR로 하여 쉽게 찾을 수 있다.
`host$ docker logs mysql`
`host$ docker logs --tail 3 mysql`: 끝에 3줄만 보기
`host$ docker logs -f mysql`: 로그를 스트림으로 확인
`host# cat /var/lib/docker/containers/\${CONTAINER_ID}/\${CONTAINER_ID}-json.log`: docker가 종료된 경우
- id는 full name으로
- root 권한으로 해야 함: 아래 명령은 `$`이 아니라 `#`인 것을 확인
로그가 JSON 형식으로 생성되어 개발에 용이하게 되어 있다.
<figure style="width: 100%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217908&authkey=%21AOTuFt5P0vGZgUA&width=673&height=256" alt="">
  <figcaption>Docker 로그의 저장</figcaption>
</figure>
----------------

## 도커 이미지
### 이미지 생성
<figure style="width: 100%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217912&authkey=%21AIylM4qnbhdJf30&width=827&height=436" alt="">
  <figcaption>이미지 로컬 커밋</figcaption>
</figure>
### 도커 이미지 추출: 이미지 -> 파일
`docker save -o ubuntu-22.04.tar ubuntu:22.04`
`-o`: 이미지를 저장하여 tar 파일로 내보내는 옵션
### 이미지를 registry에 등록
Docker 웹 레지스트리는 Docker 이미지를 저장하고 배포하기 위한 서버 애플리케이션. 개발자들이 Docker 이미지를 공유하고, 버전 관리하며, 배포할 수 있게 해주는 중앙 저장소 역할을 한다.
- docker에서 공식으로 제공하는 Docker hub(public)
- Docker Registry, Harbor, Nexus으로 구축하는 private registry
- AWS의 ECR(Elastic Container Registry), Google Cloud의 GCR(Google Container Registry), Azure의 ACR(Azure Container Registry)
```shell
host$ sudo vi /etc/docker/daemon.json
{
    "insecure-registries": ["192.168.26.61:1950"]
}
// insecure-registries: 비보안 레지스트리 허용(Default: HTTPS를 사용하는 보안 연결만 허용)

host$ sudo systemctl restart docker
host$ docker tag kopo00:1.0 192.168.26.61:1950/kopo00:1.0
// kopo13:22.04의 태그를 192.168.26.61:1950/kopo13:1.0으로 저장(복사), 해당 경로로 push 한다.

host$ docker push 192.168.26.61:1950/kopo00:1.0
host$ docker rmi 192.168.26.61:1950/kopo00:1.0
host$ docker run -it --name con3 192.168.26.61:1950/kopo00:1.0
```
레지스트리는 local 먼저 뒤지고, 없으면 원격지를 뒤진다: 주소가 없다면 default 사이트를 뒤진다.
## 컨테이너 자원 할당
<figure style="width: 100%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217909&authkey=%21AGfE-1HEc49fNXY&width=579&height=243" alt="">
  <figcaption>CGroup</figcaption>
</figure>
- CGroup: Control Group. Linux 운영체제에서 그룹마다 리소스에 대한 제약
- 도커는 CGroup처럼 호스트 디바이스의 리소스에 대한 컨테이너의 리소스 제한을 걸어둔다.
### 메모리 제한
- 컨테이너에 할당된 메모리를 초과하면 자동 종료됨.
- 트래픽이 몰리는 시간대에 한해서 리소스를 유동적으로 분배하도록 할 수 있음: Swap(보조 기억장치를 메모리처럼 사용하도록 하는 메모리 관리 기술)또는 클라우드 서비스 제공자가 제공하는 기능을 사용할 수도 있음.
### CPU 제한
- `con$ apt update && apt install stress`: stress 실행을 위한 설치
- `host$ docker run -it --cpuset-cpus="0,1" --name cpu2 ubuntu:22.04`: (1, 2번째 CPU) 사용
- `con# cat /sys/fs/cgroup/cpu.max`: 사용 cpu 확인 
- `host$ htop`: 호스트 디바이스에서 cpu사용량 확인
- `con$ stress --cpu 1`: 첫 번째 cpu 부하(ctrl + c로 종료).
#### cpu 가상화
`stress`: cpu부하
`/sys/fs/cgroup/cpu.max` 가능한 최대 cpu 확인
`docker run -it --cpus=1 --name cpu4 ubuntu:22.04`: - CPU 자원을 제한. 1개의 CPU 코어만 사용하도록 설정. 호스트 디바이스에서 htop을 실행해보면 cpu점유가 유동적인 것을 확인할 수 있다(cpu 점유율 합 100 %).
## Dockerfile
- 환경에 필요한 프로그램을 설치하여 도커 이미지 생성하는 일련의 과정을 스크립트로 자동화하여 생성하는 파일.
- `docker build -t ex1:1.0 .` : 현재 디렉터리에 있는 Dockerfile을 빌드하여 ex1으로 이미지 생성
- `CMD` / `ENTRYPOINT` : 컨테이너를 생성 및 실행 할 때 실행할 명령어
`-p` 옵션 -> Dockerfile에서 명시했으므로 지금은 필요 없음.
`-P`: \[(호스트)랜덤 포트]:\[(컨테이너)설정 포트]로 설정됨, 이 때 도커 파일 내 EXPOSE 사용.
> [!note]
> `/app`: (관례)생성한 파일들을 정리 해 놓음
- Dockerfile로 docker volume 설정: 자동으로 도커 volume이 잡혀 경로를 설정해주어야 한다. Docker compose를 쓰면 좀 더 유연하게 설정할 수 있음.
<figure style="width: 100%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217910&authkey=%21AEJEyaHS4AVeUYw&width=1274&height=719" alt="">
  <figcaption>Dockerfile 사용 예 1</figcaption>
</figure>
## Docker Compose
<figure style="width: 100%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217911&authkey=%21ANG9WPoagkqAE48&width=1055&height=453" alt="">
  <figcaption>Docker compose 사용 예</figcaption>
</figure>
> [!note] YML
> - 데이터 직렬화 언어. 구성파일 작성에 자주 사용된다. JSON의 상위 집합으로 JSON 파일을 사용할 수 있다.
> - 구조는 map(key-value 쌍) 혹은 list 이다. 목록 하나에 필요한 수의 map 형태 데이터(객체)가 포함될 수 있다.
