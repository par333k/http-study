### HTTP 메서드
> GET, POST, PUT, PATCH, DELETE
* GET: 리소스 조회
  * 서버에 전달하고 싶은 데이터는 query(쿼리 파라미터, 쿼리 스트링)을 통해서 전달
  * 메시지 바디를 사용해서 데이터를 전달할 수 있지만, 지원하지 않는 곳이 많아서 권장하지 않음
* POST: 요청 데이터 처리, 주로 등록에 사용
  * 메시지 바디를 통해 서버로 요청 데이터 전달
  * 서버는 요청 데이터를 처리
    * 메시지 바디를 통해 들어온 데이터를 처리하는 모든 기능을 수행한다
  * 주로 전달된 데이터로 신규 리소스 등록, 프로세스 처리에 사용
  * 공식 스펙: '대상 리소스가 리소스의 고유한 의미 체계에 따라 요청에 포함된 표현을 처리하도록 요청'
  * EX) HTML 양식에 입력 된 필드와 같은 데이터 블록을 데이터 처리 프로세스에 제공
    게시판, 뉴스 그룹, 메일링 리스트, 블로그 또는 유사한 기사 그룹에 메세지 게시
    서버가 아직 식별하지 않은 새 리소스 생성
    기존 자원에 데이터 추가
  * 이 리소스 URI에 POST 요청이 오면 요청 데이터를 어떻게 처리할 지 리소스마다 따로 정해야 함
    -> 정해진 것이 없다.
  * 다른 메서드로 처리하기 애매한 경우에도 쓰기도 한다
    * 가령 JSON으로 조회 데이터를 넘겨야 하는데 GET 메서드 사용이 어려운 경우 POST를 쓴다
  * POST의 결과로 새로운 리소스가 생성되지 않을 수도 있다 - 리소스 생성을 넘어 프로세스 처리로 이어지는경우
  * EX) POST /orders/{orderId}/start-delivery (컨트롤 URI) - 주문에 대한 배달을 시작한다
    * 부득이하게 동사가 URI에 오는 경우가 있는데 이를 컨트롤 URI라 한다.
* PUT: 리소스를 대체, 해당 리소스가 없으면 생성 (덮어쓰기)
  * '완전히' 리소스를 대체
  * 없으면 생성한다
  * PUT은 클라이언트가 리소스를 식별한다
    * 클라이언트가 리소스 위치를 알고 URI 지정 - POST 와의 큰 차이점.
* PATCH: 리소스 부분 변경 (아직 지원 안되는 서버도 있긴 함)
  * 리소스의 '일부분'을 대체
  * PATCH 역시 클라이언트가 리소스를 식별
* DELETE: 리소스 삭제
  * 리소스를 제거한다
  * DELETE 역시 클라이언트가 리소스를 식별
> 의미적으로 PUT과 DELETE는 실무에서 늘 고민되는 주제   
> 리소스를 지운다고 하지만 실제로 DB에서는 해당 데이터가 지워지는게 아니라 비활성화 되는 경우가 많다   
> 이 때 PUT이나 PATCH를 활용하게 된다.   
> 이러한 고민에 대해 여러 사람들마다 의견이 갈림.   
> 백기선 개발자는 해당 문제를 DELETE를 메서드로 쓰는 것이 맞고, 다만 실제 쿼리를 UPDATE로 처리 권장   
> 반면 다른 외국인 개발자 컬럼에서는 PUT이나 PATCH를 써야 한다고 이야기, 실제 DB에서 삭제하지 않기 때문   
> HTTP method는 행위의 표현이므로 DELETE를 의미적으로 쓰는게 맞지 않나 싶음(백기선 의견이 더 타당하게 느껴짐)
* HEAD: GET과 동일하지만 메시지 부분을 제외하고, 상태 줄과 헤더만 반환
* OPTIONS: 대상 리소스에 대한 통신 가능 옵션(메서드)을 설명(주로 CORS에서 사용)
* CONNECT: 대상 자원으로 식별되는 서버에 대한 터널을 설정
* TRACE: 대상 리소스에 대한 경로를 따라 메세지 루프백 테스트를 수행

#### HTTP 메서드의 속성
> 안전(safe), 멱등(idempotent), 캐시가능(cacheable)
* 안전 : 호출해도 리소스를 변경하지 않는다. - GET, HEAD, OPTIONS, TRACE
  - 계속 호출해서 로그가 쌓여 장애가 발생하면? 이건 고려사항이 아님. 리소스에 집중!
* 멱등 : f(f(x)) = f(x)
  * 한 번 호출하든 두 번 호출하든 100번 호출하든 결과가 같다
  * 멱등 메서드
    * GET: 몇 번을 조회하든 같은 곳이면 같은 결과가 조회된다.
    * PUT: 결과를 대체한다. 따라서 같은 요청을 여러번 해도 최종 결과는 같다(완전히 대체하기 때문)
    * DELETE: 결과를 삭제한다. 같은 요청을 여러번해도 삭제된 결과는 똑같다.
  * POST는 멱등이 아니다. 두 번 호출 하면 같은 결제가 중복되어 일어나거나 중복 값이 쌓일 수 있다.
  * 활용
    * 자동 복구 메커니즘
    * 서버가 TIMEOUT 등으로 정상 응답을 못주었을 때, 클라이언트가 같은 요청을 다시 해도 되는가? 판단 근거
  * 외부 요청에 의해 리소스가 변경되는 것은 고려하지 않는다.
* 캐시가능 : 응답 결과 리소스를 캐시해서 사용해도 되는가?
  * GET, HEAD, POST. PATCH 캐시 가능
  * 실제로는 GET, HEAD 정도만 캐시로 사용
    * POST, PATCH는 본문 내용까지 캐시 키로 고려해야 하는데, 구현이 쉽지 않다
    
#### HTTP API 만들기
* 회원 정보 관리 API 설계
  * 회원 정보에 대한 CRUD
* API URI 설계 - 가장 중요한 것은 리소스 식별!!!
  * 리소스의 의미? 회원이라는 개념 자체가 바로 리소스. 행위는 리소스가 아니다.
  * 회원이라는 리소스만 식별하면 된다 -> 회원 리소스를 URI에 매핑
    * 회원 목록 조회: /members GET
    * 회원 조회: /members/{id} GET
    * 회원 등록: /members/{id} POST
    * 회원 수정: /members/{id} PUT, PATCH, POST
    * 회원 삭제: /members/{id} DELETE
  * 계층 구조상 상위를 컬렉션으로 보고 복수단어 권장
  * 어떻게 구별하지? 리소스와 행위를 분리!
  * 리소스: 회원, 행위: 조회, 등록, 삭제, 변경
  * 행위는 HTTP 메서드로 표현한다.

* 회원 관리 시스템 예시
  * POST - 신규 자원 등록 특징 : 빈번
    * 클라이언트는 등록될 리소스의 URI를 모른다.
      * 회원 등록 /members -> POST
      * POST /members
    * 서버가 새로 등록된 리소스 URI를 생성해준다.
      * HTTP/1.1 201 Created
        Location: /members/100
    * 컬렉션(Collection)
      * 서버가 관리하는 리소스 디렉토리
      * 서버가 리소스의 URI를 생성하고 관리
      * 여기서 컬렉션은 /members
* 파일 관리 시스템
  * PUT 기반 등록 : 희귀
    * 파일 등록 /files/{filename} PUT
    * 파일 대량 등록 /files POST
  * PUT 으로 신규 자원을 등록할때 - 왜 POST가 아니야?
    * 클라이언트가 리소스 URI를 알고 있다
      * 파일 등록 /files/{filename} PUT
      * PUT /files/star.jpg
    * 클라이언트가 직접 리소스의 URI를 지정한다.
    * 스토어 (Store)
      * 클라이언트가 관리하는 리소스 저장소
      * 클라이언트가 리소스의 URI를 알고 관리
      * 여기서 스토어는 /files

#### HTTP 메서드 활용
* HTML Form 데이터 전송
  * POST, GET만 가능
  * content-type은 application/x-www-form-urlencoded, multipart/form-data 사용
  * application/x-www-form-urlencoded 는 form 내용을 메세지 바디를 통해 전송(key=val 쿼리파라미터)
  * 전송 데이터를 url encoding 처리
  * multipart/form-data 는 파일 업로드 같은 바이너리 데이터 사용
  * 다른 여러 종류의 파일/폼 내용과 함께 전송 가능(그래서 이름이 multipart)
* HTTP API 데이터 전송
  * 서버 to 서버 - 백엔드 시스템 통신으로 자주 사용
  * 앱 클라이언트 - 아이폰, 안드로이드 앱에서도 사용
  * 웹 클라이언트 - 비동기 데이터 전송(Ajax), react, vue
  * POST, PUT, PATCH 전부 사용가능
  * GET 쿼리파라미터로 데이터 전달
  * Content-type: application/json을 주로 사용(사실상 표준)- 예전에는 xml을 많이 활용
    * TEXT, XML, JSON 등등

#### 참고
> URI 설계 개념
* 문서 (document)
  * 단일 개념 (파일 하나, 객체 인스턴스, 데이터베이스 row)
  * 예) /members/100, /files/star.jpg
* 컬렉션 (collection)
  * 서버가 관리하는 리소스 디렉터리
  * 서버가 리소스의 URI를 생성하고 관리
  * 예) /members
* 스토어 (store)
  * 클라이언트가 관리하는 자원 저장소
  * 클라이언트가 리소스의 URI를 알고 관리
  * 예) /files
* 컨트롤러(controller), 컨트롤 URI
  * 문서, 컬렉션, 스토어로 해결하기 어려운 추가 프로세스 실행
  * 동사를 직접 사용
  * 예) /members/{id}/delete

> https://restfulapi.net/resource-naming   
> 참고할 것 
