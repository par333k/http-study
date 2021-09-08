## HTTP StatusCode
> 클라이언트가 보낸 요청의 처리 상태를 응답에서 알려주는 기능   
> 처리 상태가 이후 변경되더라도 맨 앞자리 숫자가 큰 단위의 기준이 된다(상위 상태코드로 해석)
* 1xx (Informational): 요청이 수신되어 처리중 (잘 사용안함)
* 2xx (Successful): 요청 정상 처리
  * 200 OK
  * 201 Created
    * 응답의 Location 헤더 필드로 생성된 리소스를 식별
  * 202 Accepted
    * 배치 처리 같은 곳에서 사용 (바로 처리가 완료되지 않는 요청)
  * 204 No Content
    * 요청을 성공적으로 수행했으나 응답할 데이터가 없음
* 3xx (Redirection): 요청을 완료하려면 추가 행동이 필요
* 4xx (Client Error): 클라이언트 오류, 잘못된 문법 등으로 서버가 요청을 수행할 수 없음
* 5xx (Server Error): 서버 오류, 서버가 정상 요청을 처리하지 못함
