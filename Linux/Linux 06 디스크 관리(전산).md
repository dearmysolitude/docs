## Extra: RAID
Redundunt Array of Inexpensive Disks. HPC(High Performance Computing)의 현실적인 사용이 어려운 대다수 사업환경에서 RAID를 사용하여 전산망을 구성하게 된다.
RAID 구분 / 용량 계산 정도는 기본적으로 숙지하고 지나가자.
https://hihighlinux.tistory.com/64
### RAID 0
초기 RAID. 블록 레벨 striping 방식을 사용한다.
### RAID 1

### RAID 5

### RAID 6

## 디바이스
윈도우에서는 장치관리자에서 디바이스를 확인할 수 있다. 리눅스에서는 파일로 관리하기 때문에 장치 파일로 접근할 수 있다. 
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217089&authkey=%21AOt19NbCp_rydFo&width=673&height=341" alt="">
  <figcaption>`ls  -al /dev` 의 결과</figcaption>
</figure>
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217088&authkey=%21ABWG2OKTP-arDQ4&width=678&height=66" alt="">
  <figcaption>드라이버 파일 중 stderror, stdin, stdout을 확인할 수 있다.</figcaption>
</figure>
### `b`: block device
하드디스크나 CD/DVD, 플로피 디스크 등의 장치를 말하며, 블록이나 섹터 등의 정해진 단위로 데이터를 전송, I/O 전송 속도가 높은 것이 특징이다.
### `c`: character device
키보드, 마우스, 테이프, 모니터, 프린터 등의 장치가 있으며, byte 단위로 데이터를 전송한다. I/O 전송 속도가 다소 느리다. 이는 어플리케이션 단에서 버퍼링을 제어하므로 성능에 따라 차이가 있을 수 있다.
### 하드디스크
- IDE/SATA
- SCSI
- MVMe
### 블록디바이스
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217090&authkey=%21ANvAoLnIrD2Zsx4&width=469&height=253" alt="">
  <figcaption>lsblk로 확인할 수 있는 블록 디바이스</figcaption>
</figure>
윈도우에서는 디스크 관리로 확인해볼 수 있다.
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217091&authkey=%21AKd_LK8wwQnLPD4&width=754&height=597" alt="">
  <figcaption>윈도우 디스크: 파티셔닝 + NTFS로 포맷 + C혹은 D로 할당된 것을 확인할 수 있다.</figcaption>
</figure>
|     드라이브명 할당     | => 리눅스에서는 "마운트"라고 칭한다.
| 파일 시스템으로 포맷 |
|           파티셔닝           |
|          물리 디스크       |
### 실습: 디스크 추가

#### 참고
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217092&authkey=%21AGEEtiF4UIFHXIk&width=826&height=192" alt="">
  <figcaption>마운트 이전에 만들어뒀던 abc.txt를 찾을 수 없다.</figcaption>
</figure>
마운트하기 전, 마운트 할 디렉터리에 파일을 하나 만들어보자. 이 파일은 /dev/sda2에 생성되었다. 이 때 /dev/sdb1에 /home1에 마운트해버리면 sdb1에서 덮어씌워 버리므로 먼저 만들어버린 파일은 찾을 수 없게 된다.
#### 명령어 및 파일
- `du`: 개인 사용자가 디스크 사용량을 확인하고자 할 때
- `blkid`: 블록 장치의 정보를 보여 준다. UUID, file system type 출력.
- `edquota`: 사용자/그룹에 쿼터 설정. 사용자 할당량을 편집한다.
- `df` 마운트된 디스크의 상태를 확인. 파일 시스템의 사용량 정보를 보여준다.
- `lsblk`: 블록 장치 정보를 보여준다.
`/etc/fstab`: 파일 시스템의 마운트 정보를 설정
#### 마운트 설정
`fstab`의 마운트를 부팅시킬 때마다 재설정할 필요 없이 설정하기
