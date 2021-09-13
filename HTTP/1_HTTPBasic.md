## HTTP 기본
> HTTP 스펙 : RFC 7230~7235 스펙으로 공부해야 함    
> https://tools.ietf.org/html/rfc7230
1. HTTP란 ?
   1. HyperText Transfer Protocol의 약자
      1. 지금은 HyperText가 아닌 모든것에 사용
   2. HTTP 메세지를 통해 모든 것을 전송한다
      1. HTML, TEXT, IMAGE, 음성, 영상, 파일, JSON, XML 등등
      2. 거의 모든 형태의 데이터 전송가능
      3. 대부분의 데이터 통신에 HTTP를 쓰고있다
   3. 현재 가장 흔하게 쓰는 버전은 97년에 나와서 2014년 까지 개선된 HTTP/1.1 버전
      1. TCP/IP 형식 사용
   4. HTTP/2 는 기존 1.1에 비해 성능이 개선됨 - 아직 사용 점유율이 높지는 않음
      1. TCP/IP 형식 사용
   5. HTTP/3 은 현재 진행형, TCP대신 UDP를 사용해 성능을 더 개선
2. 클라이언트 - 서버 구조
   1. 요청(Request) - 응답(Response) 구조
   2. 클라이언트는 서버에 요청을 보내고, 응답을 대기
   3. 서버가 요청에 대한 결과를 만들어서 응답
3. stateful - stateless
   1. 무상태 프로토콜 (stateless)
      1. 서버가 클라이언트의 상태를 저장하지 않는다
         1. 장점: 서버의 스케일 아웃에 유리하다. 장애가 날 경우 다른 서버로 교체하기 좋다.
         2. 단점: 클라이언트와 추가적인 데이터를 주고받아야하고, 해당 데이터의 저장에 대한 이슈가 있다
         데이터의 전송량이 비교적 많다
            1. JWT를 이용한 OAuth 정책
   2. 상태 유지 (stateful)
      1. 서버가 클라이언트의 상태를 저장한다
         1. 상태를 유지하기 위해 서버가 요청-응답 과정에서 바뀔 경우 상태가 유지되지 않는다
            1. 추가적인 작업을 통해 상태 변경을 전파해야한다.
         2. 서버의 스케일아웃이 까다롭다(상태를 별도의 공간에 저장해야한다 -DB, Cache 등)
> 모든 상황을 완전히 Stateless하게 설계할 수 없는 경우도 존재한다   
> 브라우저 쿠키, 서버 세션 등을 활용해야 하는 경우 별도의 세션서버나 스토리지를 이용하게된다.   
> 상태 유지는 최소한만으로 사용하는게 좋다.   
4. 비 연결성
   1. HTTP는 기본이 연결을 유지하지 않는 모델이다
   2. 일반적으로 초 단위 이하의 빠른 속도로 응답
   3. 1시간 동안 수천명이 서비스를 사용해도 실제 서버에서 동시에 처리하는 요청은 매우 적다
      1. 연속해서 요청을 끊임없이 보내지 않기 때문
   4. 서버 자원을 매우 효율적으로 사용할 수 있다
   5. TCP/IP를 계속 새로 연결해야해서 핸드셰이크 시간이 추가된다
   6. 웹브라우저로 요청시 HTML이나 JSON 같은 데이터 뿐만 아니라 정적 파일들을 전부 요청하여 다운로드 한다.
   7. 지금은 HTTP 지속 연결(Persistent Connections)로 문제를 해결한다
      1. 요청에 필요한 자원을 한꺼번에 요청 - 자원에 대한 응답이 전부 돌아올때까지 TCP/IP 연결을 유지한다
      2. HTTP2,3에서 해당 과정이 더욱 빨라졌다
> Stateless의 중요성   
> 백엔드에서 가장 힘든 것? 특정 시간에 트래픽이 확 몰리는 이벤트 같은 것들   
> Stateless를 최대한 유지해야 스케일 아웃을 통해 대비할 수 있다.
5. HTTP 메세지
   1. 메세지 구조 (rfc7230 스펙)
      1. start - line
         1. start-line = request-line / status-line
         2. request-line = method SP(공백) request-target SP HTTP-version CRLF(엔터)
            1. EX) GET /search?q=hello&hl=ko HTTP/1.1 
            HTTP 메서드: GET, 요청 대상(path), HTTP Version
            2. HTTP method : GET, POST, PUT, PATCH, DELETE, OPTIONS..,
            3. absolute-path[?query] 절대경로[?쿼리] => "/"로 시작하는 경로
            4. 다른 유형의 경로 지정 방법도 있다(fullpath - http blahblah)
         3. status-line (응답)
            1. status-line = HTTP-version SP status-code SP reason-phrase CRLF
               1. EX) HTTP/1.1 200 OK
                  Content-Type..
                  HTML..
            2. HTTP 버전, HTTP 상태코드(200 성공 400 클라이언트 요청 오류 500 서버 내부 오류)
            이유문구: 사람이 이해할 수 있는 짧은 상태 코드 설명 글
      2. header-field CRLF
         1. header-field = field-name ":" OWS field-value OWS(띄어쓰기 허용)
         2. field-name은 대소문자 구분 없음
         3. HTTP헤더는 전송에 필요한 모든 부가정보가 들어있다
            1. EX) 메세지 바디의 내용, 바디의 크기, 압축, 인증, 요청 클라이언트(브라우저) 정보,
            서버 애플리케이션 정보, 캐시 관리 정보...
         4. 표준 헤더는 몹시 많다. 필요시 임의의 헤더도 추가 가능
      3. message-body
         1. 실제 전송할 데이터
         2. HTML 문서, 이미지, 영상, JSON 등등 byte로 표현할 수 있는 모든 데이터 전송 가능
   
