Network Address Translation. 네트워크 주소를 번환하는 것. 네트워크에서 주소, 즉 L3 주소는 IP이므로 NAT는 IP 주소를 변환하는 것이다.
- IPv4에는 각 네트워크가 임의로 쓸 수 있는 사설(private)주소 범위가 정의되어 있다: 사설 주소 범위는 각 네트워크 내에서만 유효
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217086&authkey=%21ANZDQcFsWin4VKw&width=870&height=148" alt="">
  <figcaption>IPv4</figcaption>
</figure>
사설 IP는 특정 네트워크 안에서 유효하므로, 사설 IP를 가지고 있는 IP packet은 이 네트워크를 벗어나기 위해 **1) 공용 IP 또는 2) 진입할 네트워크의 사설 IP**로 변환해야 한다.

NAT는 이러한 상황에서 IPv4 변환을 진행해 준다.
	- IP 주소 영역을 다른 IP 주소 영역으로 바꾸어 주는 장치는 모두 NAT이다.
	- 장치라고 해서 반드시 하드웨어이 것은 아니다. 같은 기능을 제공하는 소프트웨어 역시 NAT이다.


