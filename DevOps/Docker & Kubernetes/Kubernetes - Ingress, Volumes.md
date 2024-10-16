## Ingress 전에: Proxy 와 Reverse Proxy

![Proxy 와 Reverse Proxy](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217931&authkey=%21AIWmBaWfhnt34qk&width=600&height=299)

![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217930&authkey=%21AB5lJRovOokGLbg&width=828&height=195)
## Ingress
proxy가 L4 단에서 이루어지는 것이라면, 쿠버네티스의 Ingress는 L7 레벨에서 이루어진다(즉 앞에서 다뤄봤던 proxy는 도메인 이름을 해석하지 못한다.). 도메인 네임을 해석하는 역할을 함.
![](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217932&authkey=%21AH-YsXAxGTXsBlw&width=1023&height=467)
### Ingress 예시
2 개의 pod를 가진 서비스 2개를 각각 Ingress container가 관리하도록 mapping 후(a.html, b.html), 각 서비스에서 실행되는 컨테이너에 정적 페이지 a.html과 b.html을 만들어 넣었다.
![Ingress 예시](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217934&authkey=%21AJU6kXDz5qzgq2g&width=1290&height=727)
### 참고자료
[Kubernetes Ingress 개념 - alice](https://blog.naver.com/alice_k106/221502890249)
## Volumes
![Volumes, PV와 PVC](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217933&authkey=%21ACa9YBDtDbDCjF4&width=677&height=335)
### EmptyDir
동일 pod 내의 컨테이너 간 볼륨을 공유, pod가 삭제되면 볼륨도 같이 삭제됨
```yml
apiVersion: v1
kind: Pod
metadata:
  name: my-ubuntu-pod
spec:
  containers:
  - name: con1
    image: ubuntu:22.04
    args: ["tail", "-f", "/dev/null"]
    volumeMounts:
      - mountPath: /cache1
        name: empty-dir
  - name: con2
    image: ubuntu:22.04
    args: ["tail", "-f", "/dev/null"]
    volumeMounts:
      - mountPath: /cache2
        name: empty-dir
  volumes:
    - name: empty-dir
      emptyDir: {}
```
### HostPath
호스트 노드의 디렉토리를 마운트해서 볼륨으로 사용, 파드 간 공유 가능
```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ubuntu-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-ubuntu
  template:
    metadata:
      name: my-ubuntu-pod
      labels:
        app: my-ubuntu
    spec:
      containers:
      - name: ubuntu
        image: ubuntu:22.04
        args: ["tail", "-f", "/dev/null"]
        volumeMounts:
          - mountPath: /cache1
            name: hostpath-dir
      volumes:
        - name: hostpath-dir
          hostPath:
            path: /cache_hostpath
```
### PVC/PV
 외부 데이터 저장소 사용
- PV : 데이터를 저장할 볼륨. 볼륨을 생성하고 이를 클러스터에 등록한 것
- PVC : 필요한 저장 공간·RW모드 등 요청사항을 기술한 명세로서 PV에 전달하는 요청. PV와 바인딩을 하는 목적으로 사용
**pv.yml**
```yml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv1
spec:
  capacity:
    storage: 100Mi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteMany
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: "/cache_pv"
```
**pvc.yml**
```yml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc1
spec:
  resources:
    requests:
      storage: 50Mi
  accessModes:
    - ReadWriteMany
```
**mount-pvc.yml**
```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ubuntu-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-ubuntu
  template:
    metadata:
      name: my-ubuntu-pod
      labels:
        app: my-ubuntu
    spec:
      containers:
      - name: ubuntu
        image: ubuntu:22.04
        args: ["tail", "-f", "/dev/null"]
        volumeMounts:
        - name: pv-dir
          mountPath: /cache1
      volumes:
      - name: pv-dir
        persistentVolumeClaim:
          claimName: pvc1
```
**터미널**
```bash
host$ kubectl apply -f pv.yml
host$ kubectl apply -f pvc.yml
host$ kubectl apply -f mount-pvc.yml
host$ kubectl get pods
>> ...
host$ kubectl exec -it [pod 이름] -- /bin/bash # pod 접속
con1$ cd /cache1 && echo a1 > a1 && ls
>> a1
con2$ cd /cache1 && echo a2 > a2 && ls
>> a1 a2
host$ minikube ssh # minikube 접속
minikube$ cd //tmp/hostpath-provisioner/default/pvc1 && echo a3 > a3 && ls
>> a1 a2 a3
```
외부 저장소를 연결해야 하지만 임시로 tmp에 만들어진 저장소 내용을 확인할 수 있다.
## StatefulSets
단순히 여러 서비스의 복제품인 replicaset과 달리 statefulset은 각 pod가 역할을 가지며 역할에 맞게 복제된 pod들. 생성 시 순서가 중요하다.
![StatefulSets](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217935&authkey=%21AHBCJoz-rUl5lxM&width=734&height=376)
```yml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: sts1
spec:
  selector:
    matchLabels:
      app: my-sts
  serviceName: myapp-svc-headless
  replicas: 2
  template:
    metadata:
      labels:
        app: my-sts
    spec:
      containers:
      - name: myapp
        image: ubuntu:22.04
        args: ["tail", "-f", "/dev/null"]
```

### Demonsets
일부 혹은 모든 노드에 동일한 Pod가 하나씩 실행되도록 한다. 클러스터에 새 노드가 추가될 때마다 Pod 복제본이 자동으로 해당 노드에 추가된다. 모니터링/로그/kube-proxy/networking에 사용
**예시**
```yml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: monitoring-daemon
spec:
  selector:
    matchLabels:
      app: monigoring-agent
  template:
    metadata:
      labels:
        app: monigoring-agent
    spec:
      containers:
        - name: monigoring-agent
          image: nginx
```
## Jobs & CronJobs
Pod를 이용해 일회성 또는 정기적인 작업을 실행. 작업 실행 결과를 알려준다.
단일 Job: 하나 이상의 파드를 지정. 지정된 수의 파드가 성공적으로 종료될 때까지 계속해서 실행
```yml
apiVersion: batch/v1
kind: Job
metadata:
  name: job1
spec:
  template:
    spec:
      containers:
      - name: my-job
        image: ubuntu:22.04
        command: ["echo", "helloworld"]
      restartPolicy: Never
```
- Cron Job: 시간 단위로 반복, 정기적으로 반복되어야 하는 작업. 로그 히스토리의 기본 설정은 3개의 최근 job까지만 가능함.
```yml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: cronjob1
spec:
  schedule: "*/1 * * * *" # 1분 마다 실행
  jobTemplate:
    metadata:
      name: my-cronjob
    spec:
      template:
        spec:
          containers:
          - name: test
            image: ubuntu:22.04
            command:
            - /bin/sh
            - -c
            - for i in 1 2 3 4 5 6 7 8 9 ;do echo $i ; done
          restartPolicy: Never
```