## HTTP 헤더
> HTTP 전송에 필요한 모든 부가정보를 전달한다   
> EX) 메세지 바디 내용, 메세지 크기, 압축, 인증, 요청 클라이언트, 서버정보, 캐시 관리 정보 등..
* 표준 헤더의 종류가 너무 많다.
* 필요시 임의의 헤더 추가가 가능하다
* 헤더의 분류 - 과거 (현재 폐기)
  * General 헤더: 메세지 전체에 적용되는 정보
  * Request 헤더: 요청 정보
  * Response 헤더: 응답 정보
  * Entity 헤더: 엔티티 바디 정보(Content-Type, Content-Length 등)
    * 메세지 본문은 엔티티 본문을 전달하는데 사용
    * 엔티티 본문은 요청이나 응답에서 전달할 실제 데이터
    * 엔티티 헤더는 엔티티 본문의 데이터를 해석할 수 있는 정보 제공
      * 데이터 유형(html, json), 데이터 길이, 압축 정보 등등
* 헤더의 분류 - 현재
  * 엔티티 -> 표현으로 변경
  * 표현(Representation) = 표현 메타데이터 + 표현 데이터
    * Content-Type: 표현 데이터의 형식
      * 미디어 타입, 문자 인코딩
      * EX) text/html; charset=utf8, image/png, application/json 등
    * Content-Encoding: 표현 데이터의 압축 방식
      * 표현 데이터를 압축하기 위해 사용
      * 데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가
      * 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제
      * EX) gzip, deflate, identity(압축을 안한다)
    * Content-Language: 표현 데이터의 자연 언어
      * 표현 데이터의 자연 언어를 표현
      * EX) ko, en, en-US
    * Content-Length: 표현 데이터의 길이
      * 바이트 단위
      * Transfer-Encoding(전송 코딩)을 사용하면 Content-Length를 사용하면 안됨
    * 표현 헤더는 전송, 응답 둘 다 사용
  * 메시지 본문을 통해 표현 데이터 전달
  * 메시지 본문 = 페이로드(payload)
  * 표현은 요청이나 응답에서 전달할 실제 데이터
  * 표현 헤더는 표현 데이터를 해석할 수 있는 정보 제공
    * 데이터 유형(html, json), 데이터 길이, 압축 정보 등등
  * 표현 헤더는 표현 메타데이터와 페이로드 메세지를 구분할 수 있다.
### 협상 (콘텐츠 네고시에이션)
> 클라이언트가 선호하는 표현 요청   
* Accept: 클라이언트가 선호하는 미디어 타입 전달
* Accept-Charset: 클라이언트가 선호하는 문자 인코딩
* Accept-Encoding: 클라이언트가 선호하는 압축 인코딩
* Accept-Language: 클라이언트가 선호하는 자연 언어
* 협상 헤더는 요청시에만 사용

* 협상과 우선순위: Quality Values(q)
* 다국적 언어를 제공하는 서버에 데이터를 요청할 때, 내가 원하는 언어에 대한 우선순위를 어떻게 설정할까?
> GET /event   
> Accept-Language: ko-KR, ko;q=0.9, en-US;q=0.8,en;q=0.7
* Quality Values(q)값 사용
* 0~1, 클수록 높은 우선순위
* 생략하면 1
  * EX) Accept-Language: ko-KR, ko;q=0.9, en-US;q=0.8,en;q=0.7
  1. ko-KR;q=1(생략)
  2. ko;q = 0.9
  3. en-US;q=0.8
  4. en;q=0.7
* 구체적인 것이 우선한다
  * EX) Accept: text/*, text/plain, text/plain;format=flowed, `*/*`
    1. text/plain;format=flowed
    2. text/plain
    3. text/*
    4. `*/*`
* 구체적인 것을 기준으로 미디어 타입을 맞춘다
* EX) Accept: text/*;q=0.3, text/html;q=0.7, text/html;level=1, text/html;level=2;q=0.4, `*/*`;q=0.5

| Media Type | Quality |
|------------| ------- |
|text/html;level=1|1|
|text/html|0.7|
|text/plain|0.3|
|image/jpeg|0.5|
|text/html;level=2|0.4|
|text/html;level=3|0.7|

### 전송 방식 설명
* 단순 전송
  * 컨텐츠 길이를 알 때 Content-Length를 병기해 전송(한번에 쭉 받음)
* 압축 전송
  * Content-Encoding 에 압축 방식을 같이 병기해 보내야함, 데이터길이가 상당히 줄어듬
* 분할 전송 Transfer-Encoding
  * 데이터를 덩어리(Chunk)로 분할해서 보냄, Transfer-Encodig:Chunked로 병기
  * Content-Length를 예상할 수 없음
* 범위 전송
  * Range, Content-Range
  * 범위를 지정해서 전송

### 일반 정보
* From: 유저 에이전트의 이메일 정보
  * 일반적으로 잘 사용되지 않음
  * 검색 엔진 같은 곳에서 주로 사용
  * 요청에서 사용
* Referer: 이전 웹 페이지 주소
  * 현재 요청된 페이지의 이전 웹 페이지 주소
  * A -> B 로 이동하는 경우 B를 요청할 때 Referer: A를 포함해서 요청
  * Referer를 사용해서 유입 경로 분석 가능
  * 요청에서 사용
  * 참고: referer는 단어 referrer의 오타
* User-Agent: 유저 에이전트 애플리케이션 정보
  * user-agent: 브라우저 정보
  * 클라이언트의 애플리케이션 정보
  * 통계 정보 추출하기 좋음
  * 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
  * 요청에서 사용
* Server: 요청을 처리하는 오리진 서버의 소프트웨어 정보
  * 오리진 서버? 실제 나의 요청에 응답해주는 서버, 프록시서버나 캐시서버가 아닌 내 요청이 도착하고 응답을 만드는 서버
  * 보통 아파치나 nginx 많이 사용
* Date: 메시지가 생성된 날짜와 시간
  * 응답에서 사용

### 특별한 정보
* Host: 요청한 호스트 정보(도메인)
  * 요청에서 사용
  * 필수
  * 하나의 서버가 여러 도메인을 처리해야 할 때
    * 가상호스트를 통해 여러 도메인을 한꺼번에 처리할 수 있는 서버
    * 실제 애플리케이션이 여러개 구동될 수 있다.
  * 하나의 IP 주소에 여러 도메인이 적용되어 있을 때
* Location: 페이지 리다이렉션
  * 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동(리다이렉트)
  * 201 (Created): Location 값은 요청에 의해 생성된 리소스 URI
  * 3xx (Redirection): Location 값은 요청을 자동으로 리디렉션 하기 위한 대상 리소스를 가리킴
* Allow: 허용 가능한 HTTP 메서드
  * 405 (Method Not Allowed) 에서 응답에 포함해야함
  * EX) Allow: GET, HEAD, PUT 
* Retry-After: 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간
  * 503 (Service Unavailable): 서비스가 언제까지 불능인지 알려줄 수 있음
  * Retry-After: Fri, 31 Dec 1999 23:59:59 GMT (날짜 표기)
  * Retry-After: 120 (초단위 표기) 

### 인증
* Authorization: 클라이언트 인증 정보를 서버에 전달
  * Authorization: Basic xxxxxxxxxxxxxxxx
    * 인증 방식마다 값에 대한 기준이 다르다
    * 인증 매커니즘과 상관없이 해당 헤더가 들어간다는것은 공통
* WWW-Authenticate: 리소스 접근시 필요한 인증 방법 정의
  * 401 Unauthorized 응답과 함께 사용
  * WWW-Authenticate: Newauth realm="apps", type=1, title="Login to\"apps\"", Basic realm="simple"
    * 인증을 하려면 이 헤더 정보를 참고해서 다시 요청해라

### 쿠키: 클라이언트와 서버가 무상태 프로토콜(HTTP)로 있을 때, 상태 유지를 위해 쿠키를 쓴다.
* Set-Cookie: 서버에서 클라이언트로 쿠키 전달(응답)
* Cookie: 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달
* EX) set-cookie: sessionId=abcde1234; expires=Sat, 26-EDC-2020 00:00:00 GMT; path=/; domain=.google.com; Secure
* 
* 사용처
  * 사용자 로그인 세션 관리
  * 광고 정보 트래킹
* 쿠키 정보는 항상 서버에 전송됨
  * 네트워크 트래픽 추가 유발
  * 최소한의 정보만 사용(세션 id, 인증 토큰)
  * 서버에 전송하지 않고, 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지(localStorage,sessionStorage) 참고
* 주의!
  * 보안에 민감한 데이터는 저장하면 안됨(주민번호, 신용카드 번호 등등)

* 쿠키 - 생명주기: Expires, max-age
* Set-Cookie: expires=Sat, 26-Dec-2020 04:39:21 GMT(GMT기준이어야함)
  * 만료일이 되면 쿠키 삭제
* Set-Cookie: max-age=3600 (3600초)
  * 0이나 음수를 지정하면 쿠키 삭제
* 세션 쿠키: 만료 날짜를 생략하면 브라우저 종료시 까지만 유지
* 영속 쿠키: 만료 날짜를 입력하면 해당 날짜까지 유지

* 쿠키 - 도메인: Domain
* EX) domain=example.org
* 명시: 명시한 문서 기준 도메인 + 서브 도메인 포함
  * domain=example.org를 지정해서 쿠키 생성
    * example.org 및 서브도메인도 쿠키 접근
* 생략: 현재 문서 기준 도메인만 적용
  * example.org에서 쿠키를 생성하고 domain 지정을 생략
    * 서브도메인에서 쿠키 미접근

* 쿠키 - 경로: Path
* EX) path=/home
* 이 경로를 포함한 하위 경로 페이지만 쿠키 접근
* 일반적으로 path=/ 루트로 지정
* EX)
  * path=/home 지정
  * /home -> 가능
  * /home/level1 -> 가능
  * /home/level1/level2 -> 가능
  * /hello -> 불가능

* 쿠키 - 보안: Secure, HttpOnly, SameSite
* Secure
  * 쿠키는 http, https를 구분하지 않고 전송
  * Secure를 적용하면 https인 경우에만 전송
* HttpOnly
  * XSS 공격 방지
  * 자바스크립트에서 접근 불가(document.cookie)
  * HTTP 전송에만 사용
* SameSite
  * XSRF 공격 방지
  * 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송
