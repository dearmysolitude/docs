Central Processing Unit. CPU는 메인메모리 / 보조 기억장치 / 입력 장치 / 출력 장치 / 통신 장치와 bus(컴퓨터 안의 부품 또는 여러 장치 사이를 연결해 데이터와 주소, 제어 신호 등 정보를 전송하는 통로(통신 시스템)이다)로 연결되어 있다.
**제어 장치, ALU(Arithmetic Logical Unit), 레지스터**로 구성되어 있다.
## Register, 레지스터
소량의 데이터, 처리 중인 중간 결과를 일시적으로 저장해두는 고속의 전용 영역. 한 단어 / 여러 단어 / 수 자릿수 등을 기억하여 수시로 그 내용을 이용할 수 있도록 되어있다. 여러 비터의 데이터를 저장할 수 있다. 하술할 플립 플롭이나 래치들을 병렬로 연결하여 구성한다. 일반적으로 4 비트, 8 비트, 16 비트, 32 비트 등의 크기로 만들어진다.
- Accumulator(누산기)
- Arithmetic Register(연산 레지스터)
- Instruction Register(명령 레지스터)
- Shift Register(자리 이름 레지스터)
- Index Register(지표 레지스터)
- Data Register(데이터 레지스터)
- Status Register(상태 레지스터)
- 등등 ...
### Latch, 래치
가장 기본적인 메모리 소자, 즉 기억 장치를 구성하는 전자 회로. 1 비트의 정보를 저장할 수 있다. 일반적으로 SR 래치, D 래치 등이 있다. 입력 신호가 변화하는 동안 출력이 변할 수 있어 불안정할 수 있다.
### Flip-flop, 플립플롭
하나 이상의 비트들을 저장하기 위한 디지털 논리 회로. 래치를 개선한 형태의 메모리 소자. D 플립플롭, JK 플립플롭 등이 있다. 클럭 엣지에서만 상태가 변하므로 래치보다 안정적이다.

> [!note]
> 레지스터에 대한 자세한 내용은 다음 기회에 좀 더 살펴보자.
## ALU(Arithmetic and Logic Unit)
산술 연산, 논리 연산 및 시프트를 수행하는 중앙처리장치(CPU) 내부의 회로 장치로, 독립적으로 데이터 처리를 수행하지 못하여 반드시 레지스터와 조합하여 처리가 이루어진다(단독으로는 데이터를 저장할 수 없으므로).
### 구성 논리 회로
- 가산기(Adder) : 2진수의 덧셈(산술연산)을 수행하는 회로, 두 개이상의 수의 합을 계산하는 논리 회로.
- 보수기(Complementer) : 이전 데이터의 보수를 만들어주는 논리 회로.
- 시프터(Shifter) : 2 진수의 각 자리를 왼쪽 또는 오른쪽으로 이동해주는 회로.
- 오버플로우(Overflow) 검출기 : 산술기의 결과가 해당 레지스터의 용량을 초과했을 때를 검출해주는 회로.
### 사용 레지스터
- 누산기: 연산 결과를 일시 저장.
- 시프트 레지스터: 비트들을 왼쪽/오른쪽으로 이동
- 지표 레지스터
- 상태 레지스터:  연산 결과의 상태를 나타내는 플래그들을 저장(부호, 오버플로, 언더플로, 자리 올림, 인터럽트 등).
- 데이터 레지스터: 연산에 사용될 데이터 저장
## 부연 설명
데이터 흐름: 레지스터(or RAM) → ALU → 레지스터
입력 시의 데이터는 
- **연산 코드**
- **명령어**
- **하나 이상의 피연산자**
로 구성됨
### 역사
1. 하나의 ALU
2. 부동 소수점 ALU: 인텔 80486 CPU
3. 2 X ALU(단순 정수 / 복합정수) + 1 X FPU: x86 펜티엄
## 제어장치
기억 장치에 축적되어 있는 명령을 해독하고 소요 신호를 내, 각 장치의 동작을 지시한다.
### 구성 논리 회로
- 명령 해독기(decoder): 명령 레지스터의 명령어를 해독
- 부호기(encoder): 해독된 명령에 따라 각 장치로 보낼 제어 신호를 생성하는 회로
### 사용 레지스터
- 프로그램 카운터(Program Counter, PC): 프로그램 계수기. 다음에 실행할 명령어와 번지를 기억하는 레지스터
- 명령 레지스터(Instruction Register, IR): 현재 실행중인 내용을 기억하는 레지스터
- 메모리 주소 레지스터(Memory Address Register, MAR): 기억 장치를 출입하는 데이터의 번지를 기억하는 레지스터
- 메모리 버퍼 레지스터(Memory Buffer Register, MBR): 기억장치를 출입하는 데이터가 잠시 기억되는 레지스터
**데이터를 읽을 때**: 읽을 데이터의 주소를 MAR에 기억시킨다. 제어 장치와 주 기억 장치에 읽기 신호를 보내면 MAR의 주소를 읽어서 찾은 데이터를 MBR에 기억시킨다.
**데이터를 저장할 때**: 저장할 데이터를 MBR에, 주소를 MAR에 저장시킨다. 제어 장치가 주기억 장치에 쓰기 신호를 보내면 MBR의 내용이 MAR 에 기억된 주기억 장치의 주소에 기록된다.
### 제어장치 명령 실행 순서
1. PC에 저장된 명령어의 주소를 MAR에서 인식한다.
2. MAR이 해독한 기억장치의 주소 내용을 MBR로 읽는다.
3. MBR에 저장된 명령어의 해독과 실행을 위해 IR로 이동시킨다.
4. Decoder가 명령 레지스터의 내용을 해독하고, encoder가 명령 실행에 필요한 장치에게 제어신호를 보냄으로써 명령이 실행된다.
![ControlDevice](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%218075&authkey=%21AA50E-XxFO8-VgE&width=960&height=344)
> 호출 > 해독 > 실행 > 저장