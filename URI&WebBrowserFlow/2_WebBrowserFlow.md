## 웹 브라우저 요청 흐름
### 요청
1. 주소창에 URL을 입력
2. 브라우저는 DNS를 조회한다
3. IP와 포트 정보를 받는다
4. HTTP 요청 메세지를 생성한다
   1. EX) GET /path?querystring [HTTP/version] HOST: hostname
5. 생성된 HTTP 요청 메세지를 socket library에 전달
6. 소켓 라이브러리가 TCP/IP 계층에 전달
7. TCP/IP가 구글서버와 핸드셰이킹
8. TCP/IP 패킷을 Http 메세지(전송데이터)를 포함해서 생성
9. 랜카드를 통해 인터넷에 전달
10. 요청 패킷이 수많은 노드를 지나 도착
11. 서버가 TCP/IP 패킷을 벗겨내고 전송 데이터 확인 및 처리

### 응답
1. 서버는 TCP/IP로 핸드셰이킹 된 상태
2. 도착한 데이터에 대한 HTTP 응답 메세지 작성
   1. EX) HTTP/version HttpStatusCode OK Content-type: type;charset=utf-8
   Content-length: 1234, contents (json object, xml, text, html..)
3. 이후 과정은 동일, 클라이언트 까지 응답 패킷 도착
4. 웹브라우저는 해당 컨텐츠를 표현
