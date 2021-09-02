## TCP / UDP

### 인터넷 프로토콜 스택 4계층
- Application Layer : HTTP, FTP
- Transfer Layer : TCP, UDP
- Internet Layer : IP
- Network Interface Layer : 랜카드 등의 장비 / 드라이버

### 프로토콜 계층
- Application : 브라우저, 채팅프로그램, 네트워크 게임, Socket library
- OS : TCP, UDP, IP
- Network Interface : LAN 드라이버, LAN 장비

> EX) 친구에게 메세지를 작성해 보낸다고 가정할 때의 과정
1. 키보드를 통해 메세지를 작성해서 프로그램에 전달
2. 메세지 데이터를 Socket 라이브러리를 통해 전달
3. TCP에서 메세지 데이터를 받아 TCP 정보를 생성
4. TCP 데이터를 포함한 IP 패킷 생성
5. 랜카드를 통해 이더넷 프레임을 래핑하여 인터넷으로 전달
6. 서버에 전송

### TCP : 전송 제어 프로토콜 (Transmission Control Protocol)
- 연결 지향 (TCP handshake)
  - TCP 3way handshake : 연결이 됐다는 가정(논리적 연결) - 물리적으로 연결됐는지는 확신할 수 없다.
  1. SYN : 접속요청
     1. 클라이언트가 서버에 SYN을 보냄
  2. SYN+ACK : 수락 및 접속요청
     1. 서버가 SYN을 수락하고(ACK) 연결에 대한 SYN을 보냄
  3. ACK : 수락
     1. 클라이언트가 서버에 요청을 수락함
> 이 과정이 서로 요청-수락을 나눈다는 점에서 악수와 같은 느낌(handshake)   
> ACK와 함께 데이터를 전송할 수 있다

- 데이터 전달 보증
  1. 데이터를 전송
  2. 서버에서 데이터를 잘 받았다는 응답을 보냄
- 순서의 보장
  1. 패킷을 순서대로 전송
  2. 도착할 때는 순서대로 도착하지 않을 수 있음
  3. 순서대로 도착하지 않았을 경우 순서가 달라진 순서부터 패킷을 다시 요청
  4. 잘못 온 부분에 대해 클라이언트가 패킷을 다시 전송
> 패킷에 대한 최적화는 여러 방법이 있을 수 있다.
* 위의 특징을 통해 TCP는 신뢰할 수 있는 프로토콜로 여겨진다.
* 따라서 대부분의 어플리케이션에서 사용된다

### UDP : 사용자 데이터그램 프로토콜 (User Datagram Protocol)
- 하얀 도화지에 비유 (기능이 거의 없다)
- 데이터 전달 및 순서가 보장되지 않지만 속도가 빠르다 (스타할때 1:1 UDP로 방파면 네트워크 딜레이가 빠르다)
- IP와 거의 같지만 PORT와 체크섬 정도가 추가된다
- 어플리케이션에서 추가 작업이 필요하다




