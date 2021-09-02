## URI (Uniform Resource Identifier)
> URI는 locator, name 또는 둘 다 추가로 분류될 수 있다   
> https://www.ietf.org/rfc/rfc3986.txt 에 스펙이 명시됨
* URI는 리소스를 식별하는 것
  * URL과 URN은 URI에 포함된 관계이다
    * URL: 리소스의 위치를 나타냄 (Uniform Resource Locator)
      * foo://abc.com:8080/over/there?name=abc#nose
      * scheme://authority/path/query/fragment 의 형태
      * 위치를 지정, 위치는 변한다
    * URN: 리소스의 이름을 나타냄 (Uniform Resource Name)
      * urn:example:animal:ferret:nose
      * scheme:path 의 형태
      * 위치는 변할 수 있지만 이름은 변하지 않는다
      * URN 이름만으로 실제 리소스를 찾을 수 있는 방법이 보편화 되어있지 않음
  * URI
    * Uniform: 리소스를 식별하는 통일된 방식
    * Resource: 자원, URI로 식별할 수 있는 모든 것(제한 없음)
    * Identifier: 다른 항목과 구분하는데 필요한 정보

* URL의 문법
  * scheme://[userinfo@]host[:port][/path][?query][#fragment]
  * https://www.google.com:443/search?q=hello&hl=ko
  * 프로토콜(Https), 호스트명(www.google.com), 포트번호(443), 경로(/search), 쿼리파라미터(q~)
    * scheme
      * 주로 프로토콜 사용
        * 프로토콜: 어떤 방식으로 자원에 접근할 것인가에 대한 약속 규칙
        * http, https, ftp
    * userinfo
      * url에 사용자 정보를 인증해야 할 때 사용
        * database나 aws 서비스 cli로 접속할때 저런 형태로 많이 사용
    * host
      * 호스트명
      * 도메인 네임이나 ip주소 사용
    * 포트
      * 생략가능
      * http:80 https:443
    * path
      * 리소스 경로, 계층적 구조
      * /user/1
      * '계층적 구조'는 REST의 전유물이 아님, HTTP API에서도 계층적 구조의 특징이 있음
    * query
      * key=value 형태로 ?로 시작, &로 추가가능
      * query parameter, query string 등으로 불림. 웹서버에 제공하는 파라미터, 문자형태.
    * fragment
      * 서버로 전송되지 않는 데이터
      * html 내부 북마크 등에 사용
