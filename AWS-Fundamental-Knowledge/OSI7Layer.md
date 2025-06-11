## OSI 7 Layer Model
<img src="images/osi7.png" alt="osi 7 layer" width="400">

- 컴퓨터 네트워크 및 통신을 7개의 계층으로 표현한 모델
  - 총 7개의 레이어로 구성
- 각 계층은 하위 계층의 기능을 활용해 역할을 수행하고 상위 계층으로 처리 결과를 전달
  - 낮은 계층부터 높은 계층 순서

<hr/>

### Physical Layer
- 장치를 연결하기 위한 매체의 물리적인 사항을 정의
  - 전압, 주기, 시간, 전선의 규격, 거리 등
- 주요 단위 : Bits
- 대표 구성 요소
  - 케이블/안테나/RF등 전송 매체, **허브, 리피터**
- 허브
    - Physical Layer 단위에서 다수의 기기들을 연결해주는 장치
    - 특징
      - 에러/충돌(Collision) / 디바이스 별 제어 기능 없음
      - 받은 내용을 그대로 전달 -> 무조건 Broadcast

<hr/>

### Data Link Layer
- 물리적인 통신을 제어하여 디바이스와 디바이스 간의 통신 및 전송을 안정화 하기 위한 프로토콜
- 주요 단위 : Frame
<img src="images/frame.png" alt="Frame">
<img src="images/data-link-frame.png" alt="Phsical-DataLink">
- 주요 구성 요소
  - **Mac Address, Switch**
- 주요 특징
  - CSMA/CD(Carrier-Sense Multiple Access with Collision Detection) 방식을 활용해서 각 디바이스 간의 통신을 원활하게 연결 (phsical Layer의 Collision 방지)
  - 대상을 구별하여 디바이스 간의 통신을 지원(Unicast)
  - Broadcast도 지원

#### MAC
- MAC(Media Access Control) Address
  - 네트워크 인터페이스에 부여된 고유의 주소
  - 데이터가 지정한 대상에게 잘 전달될 수 있도록 대상 식별에 사용
  - 2개의 Hexadecimal(=byte) 단위로 6개를 나열 = 48bits = 6bytes
  - 예) 00:1A:2B:3C:4D:5E
  - 두 파트로 구분
    - 첫 3개 bytes는 OUI(Organizationally Unique Identifier): 제조사에 부여된 고유 식별자
    - 나머지 3개 bytes는 NIC(Network Interface Controller) : 네트워크 인터페이스 별 고유 번호
    - 네트워크 인터페이스의 MAC Address는 고유의 값이며 변하지 않음 (IP는 변동)

#### Switch
<img src="images/switch.png" alt="Switch">
충돌이 발생하면 Frame Storage에 보관했다가 충돌이 해소되면 전송
대상 MAC이 FF:FF:FF:FF:FF:FF 이면 Broad Cast도 가능

<hr/>

### Network Layer
- 여러 노드의 경로를 찾고 올바르게 전달 될 수 있는 기능과 수단을 정의
- 주요 단위 : 패킷
- 주요 구성 요소
  - Router, IP, ARP
    - IP(Internet Protocol) Address
      - Internet Protocol상에서 통신 주체를 식별하기 위한 아이디
      - 두 가지 종류
        - IPv4 : 32Bits(2^32=약43억개)
          - 아이피를 최대로 활용하기 위해 사설(Private) Ip와 공인(Public) IP로 분류
        - IPv6 : 128Bits(2^128)
          - IPv4보다 2^96배 더 많음
          - 따라서 사설 IP 개념이 불필요함
      - MAC Address와 다르게 수시로 변동 가능
- 주요 특징
  - 서로 떨어진 Local Network 간의 통신을 지원
  - Network 간의 -> Inter Network -> Internet
  - 중간중간 Node들을 거쳐서 목적지까지 도달할 수 있는 방법을 지원
    
#### Classless Inter Domain Routing(CIDR)
- Classless Inter Domain Routing
  - IP는 주소의 영역을 여러 네트워크 영역으로 나누기 위해 IP를 묶는 방식
  - 여러 개의 사설망을 구축하기 위해 망을 나누는 방법
- CIDR Notation
  - IP 주소의 집합
  - 네트워크 주소와 호스트 주소로 구성
  - 각 호스트 주소 숫자 만큼의 아이피를 가진 네트워크 망 형성 가능
  - A.B.C.D/E 형식
    - 예)10.0.1.0/24, 172.16.0.0/12
    - A,B,C,D: 네트워크 주소 + 호스트 주소 표시
    - E:0~32 네트워크 주소가 몇 bit인지 표시
  - CIDR Block
    - 호스트 주소 비트만큼 IP 주소를 보유 가능
    - 예)192.168.2.0/24
      - 네트워크 비트 24
      - 호스트 주소 = 32-24 = 8
      - 즉 2^8 = 256개 IP 주소 보유
      - 192.168.2.0 ~ 192.168.2.255까지 총 256개 주소를 의미
    - Subnet Mask
    <img src="images/subnetmask.png" alt="subnet mask">
      - 어느 부분이 호스트 비트인지, 어느 부분이 네트워크 비트인지 구분해주는 Mask
        - AND 연산을 활용해 네트워크 주소를 필터링
        - AND 연산 : 두 비트 중 둘 다 1일때만 1
      - 네트워크 비트 수만큼 1을 보유한 마스크를 IP에 적용하면 네트워크 비트만 추출 가능

#### Router
- 네트워크 간에 패킷을 주고 받는 Layer 3 장치
- IP 대역별 최적 경로를 수집 및 관리
  - 어떤 경로의 노드를 경유해야 가장 효율적으로 대상에 도착하는지 알고 있음(Route Table)
  - 이 경로를 바탕으로 특정 IP 주소가 대상인 패킷의 전달을 요청받을 때 해당 경로로 요청
- 로컬 네트워크는 자신의 로컬 네트워크 주소가 아니라면 라우터로 전달
  - 확인 방법 : 네트워크 주소가 같은지 확인(subnet mask 등)
- 이후 Router는 패킷을 Frame 안에 넣어서 최적 경로에 따른 다른 Router로 전달
  - IP 주소에 따른 Frame 확인 방법 : ARP

#### ARP(Address Resolution Protocol)
- IP로 Mac Address를 찾는 프로토콜
- 순서
  - Broadcast(MAC Address FF:FF:FF:FF:FF:FF)로 IP 요청
  - 응답 받은 IP MAC Address를 기반으로 MAC 확정 후 테이블에 저장
  <img src="images/arp1.png" alt="arp1">
  1. ClientA 가 Switch에 Broadcast로 IP 확인 요청
    <img src="images/arp2.png" alt="arp2">
  2. 대상 Client B가 자신이라고 알려주면 Client A는 자신의 테이블에 IP와 MAC 주소를 추가

#### 서로 다른 Local Network 간 데이터 전달하기
<img src="images/network-physical.png" alt="network->physical"/>

1. IP 대상(63.12.33.12)이 로컬이 아닌 것을 식별
   - 네트워크 주소가 다름
2. 같은 네트워크가 아니라면 라우터로 전달
   - 하지만 라우터의 IP는 알지만 MAC 주소를 모름
3. ARP(Address Resolution Protocol) 활용
   - IP <-> Mac Address를 찾는 프로토콜
4. 이후 해당 MAC Address로 Frame 생성 후 전달

<img src="images/router.png" alt="router 역할">

5. Router가 Route Table을 통해 최적 경로를 찾고 Frame 재생성

<img src="images/router2.png" alt="router 역할2">

6. Netflix Router도 자신의 목적지가 아니기 때문에 다른 Router를 찾아 Frame을 재생성
   (대상을 찾을 때까지 반복)

<img src="images/physical-network.png" alt="physical->network"/>

7. 목적지에서는 물리계층에서 네트워크계층까지 데이터를 다시 보냄

#### Network Layer 해결하지 못 한 문제
- 한 번에 하나의 통신만 가능
  - 즉 여러 애플리케이션이 동시에 통신 불가능
- 패킷 등의 순서 보장 / 유실에 대한 대응 불가능

<hr/>

### Transport Layer
- 통신 주체끼리 데이터 전달의 신뢰성을 확보하는 방법을 정의
- 주요 단위 : 세그먼트
- 주요 구성 요소
  - TCP/UDP
- 주요 특징
  - Network Layer로 성립된 통신 위에서 실질적인 활용을 위한 다양한 기능을 정의
    - 패킷의 순서 보장, 에러 처리, 포트 기반 분할 등

#### TCP(Transmission Control Protocol)
- 패킷의 전달 과정에서 순서를 보장하고 유실되지 않도록 보장할 수 있는 통신 규약
  - 패킷 안에 **세그먼트**를 담아 주고 받아서 로직을 처리
- 연결 지향
  - 즉 지속적으로 채널을 수립하여 전달 여부를 확인하고 무결성을 확인
  - 지속적으로 무결성을 확보하는 과정에서 비교적 느리고 복잡한 과정 필요
- 주요 사용 사례
  - 웹 페이지(HTTP/HTTPS)
  - 이메일
  - 파일 전송
  - SSH

#### Segment
<img src="images/tcp-segment.png" alt="segment"/>

- TCP/UDP의 데이터 전달 단위
- TCP 세그먼트의 주요 구성
  - Port (Source/Destination)
  - Sequence/Acknowledgement Number : 통신 주체끼리 데이터를 주고 받았는지 확인에 사용
  - Flags : Segment의 목적 등을 정의 (ACK, SYN, FIN)
  - Window Size : 세그먼트를 만든 주체가 얼마나 많은 데이터를 받을지 전달
  - Urgent Pointer : 세그먼트의 중요도를 설정(먼저 처리해야 하는 세그먼트 등)
  - 기타 (Checksum 등)
  - 실제 데이터
- 처리 과정
  - 대상 클라이언트에 패킷을 보내면 패킷 안에 있는 세그먼트 순서를 확인해서 다음 패킷을 요청한다.
    - 1번 세그먼트를 받은 후 2번 세그먼트 요청 다음 3번, 4번 순으로 순차 요청을 한다.

#### Port
<img src="images/port.png" alt="port"/>

- IP (Internet Protocol)에서 패킷을 올바른 프로세스로 라우팅 하기 위한 논리적 단위
- TCP Port / UDP Port로 구분
  - 각 0 ~ 65535까지 정수 범위
- Well Known Port : 주로 서버에서 사용하는 애플리케이션/프로토콜 별로 미리 지정된 포트
  - 주요 사용에 따라 표준으로 공통적으로 사용
  - 예) 80:HTTP, 443:HTTPS, 22:SSH, 3306:Mysql
- Ephemeral Port : 클라이언트에서 사용하는 포트로 연결할 때마다 임의로 지정

#### Tcp Handshake
<img src="images/tcp-handshake.png" alt="tcp handshake" />

- TCP Protocol에서 통신을 수립하고 서로를 인식하는 첫 과정
- 보통 3 Way Handshake로 부르며 3가지 과정으로 구분
  - Syn(Synchronize) : 첫 요청으로 사용할 첫 클라이언트 Sequence Number(CS)를 전달
  - Syn-Ack(Synchronize-Acknowledge) : Syn에 대한 응답으로 CS+1과 서버 Sequence Number(CS)를 전달
  - ACK(Acknowledge) : 마지막 단계로 연결이 수립되었음을 CS+1과 SS+1을 전달
- 처리 과정
  1. 클라이언트 A가 sequence number:13와 flags:syn로 요청을 보내면(acknowledge number는 모르기 때문에 [])
  2. 응답으로 acknowledgement number으로 14를 보내달라고 sequence number:4431과 flags:syn,ack와 함께 요청에 대한 응답을 보낸다.
  3. 대상으로부터 응답을 받으면 sequence number:14와 대상의 acknowledgement number +1, flags:ack를 응답한다.

#### UDP(User Datagram Protocol)
- 빠르고 간단하게 데이터를 주고 받을 수 있는 방법을 정의
- Connectionless
  - 연결 지향과는 달리 데이터의 무결성, 순서, 전달여부를 체크하지 않음
  - 즉 패킷이 순서대로 오지 않거나, 중복되거나, 아예 전달되지 않을 수 있음
- 빠르고 간단
- 주요 사용 사례
  - 스트리밍
  - 보이스톡
  - 온라인 게임

#### UDP Segment
<img src="images/udp-segment.png" alt="udp-segment" />

- TCP Segment에 비해 내용이 적다.
- 대상에 보낸 Segment가 유실되더라도 다음 Segment를 보내고 TCP Segment의 경우 받았는지 확인하는데 UDP는 확인하는 시간이 없어 빠르게 보낼 수 있다.

#### 주요 프로토콜(TCP/UDP)
- TCP
  - HTTP/HTTPS : 80, 443
  - FTP(File Transfer Protocol) : 20, 21
  - SSH(Secure Shell) : 22
  - DNS(Domain Name System) : 53
- UDP
  - DNS(Domain Name System) : 53
  - DHCP(Dynamic Host Configuration Protocol) : 67, 68
  - VoIP(Voice over IP) : 5060

<hr/>

### Session Layer
<img src="images/other-layer.png" alt="other-layer" />

- 통신 주체끼리 연결이 유지할 수 있는 방법을 정의
- 예전의 컴퓨팅 환경에서 Layer 1, 2, 3, 4 이외의 차원에서 지속적인 연결(세션)이 수립될 수 있는 방법을 제공
  - 예) MAC Address(2)와 IP(3)주소와 포트(4)가 동일한 상황에서 어떻게 유저를 구분할 것인가?
  - Mainframe을 사용하던 시절 사람들이 dumb Terminal을 통해서 접근했지만 1,2,3,4 계층까지만 있다면 어떤 사람과 통신하는지 알 수 없다. -> 누가 누구와 통신 중인지 알려면 세션 계층이 필요.
- 현재에서도 마찬가지로 Layer 4 이상의 추가적인 차원에서 지속적인 연결(세션)이 수립할 수 있는 방법을 포함
  <img src="images/http-session.png" alt="http-session" />
  - 예) HTTP Cookie
- 몇 몇 프로토콜의 경우 Session Layer 자체를 구현하지 않음
  - 예)FTP

<hr/>

### Presentation Layer
- 받은 데이터를 해석하는 방법을 정의
<img src="images/presentation.png" alt="presentation">    
- 파싱, 압축 해제, 복호화 등 Application Layer에서 사용할 수 있는 형식으로 변환을 담당
- 데이터 인코딩/디코딩 방식
<hr/>

### Application Layer
- 실제 받은 데이터를 처리하는 방법을 정의
  - 말 그대로 데이터를 가지고 무엇을 어떻게 처리할지에 관한 레이어
- 예)HTTP
  - Method(Get/Post/Put/Delete/Head/Option/Patch)
  - Status Code(2xx,3xx,4xx,5xx)
  - Header
    - Host
    - User-Agent
    - Authorizations
    - Accept-Encoding
    - Content-Type