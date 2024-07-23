# HTTP 메서드
* HTTP API를 만들어보자
* HTTP 메서드 - GET, POST
* HTTP 메서드 - PUT, PATCH, DELETE
* HTTP 메서드의 속성

## HTTP API를 만들어보자
회원 정보 관리 API를 만들어라
* 회원 목록 조회 /read-member-list
* 회원 조회 /read-member-bt-id
* 회원 등록 /create-member
* 회원 수정 /update-member
* 회원 삭제 /delete-member

이것은 정말 좋은 URI 설계일까?
가장 중요한 것은 리소스 식별!!

### API URI 고민
* 리소스의 의미는 뭘까?
  * 회원을 등록하고 수정하고 조회하는게 리소스가 아니다!
  * 예) 미네랄을 캐라 -> 미네랄이 리소스
  * 회원이라는 개념 자체가 리소스.
* 리소스를 어떻게 식별하는게 좋을까?
  * 회원을 등록하고 수정하고 조회하는 것을 모두 배제
  * 회원이라는 리소스만 식별하면 된다. -> 회원 리소스를 URI에 매핑

* `회원` 목록 조회 /members
* `회원` 조회 /members/{id} -> 어떻게 구분하지?
* `회원` 등록 /members/{id} -> 어떻게 구분하지?
* `회원` 수정 /members/{id} -> 어떻게 구분하지?
* `회원` 삭제 /members/{id} -> 어떻게 구분하지?
* 참고 : 계층 구조상 상위 컬렉션으로 보고 복수단어 사용 권장 (member -> members)

### 리소스와 행위를 분리
가장 중요한 것은 리소스를 식별하는 것!
* URI는 리소스만 식별!
* 리소스와 해당 리소슬르 대상으로 하는 행위를 분리
  * 리소스 : 회원
  * 행위 : 조회, 등록, 삭제, 변경
* 리소스는 명사, 행위는 동사 (미네랄을 캐라)
* 행위(메서드)는 어떻게 구분?

## HTTP 메서드 - GET, POST
HTTP 메서드 종류
* GET : 리소스 조회
* POST : 요청 데이터 처리, 주로 등록에 사용
* PUT : 리소스를 대체, 해당 리소스가 없으면 생성
* PATCH : 리소스 부분 변경
* DELETE : 리소스 삭제

기타 메서드
* HEAD : GET과 동일하지만 메시지 부분을 제외하고, 상태 줄과 헤더만 변환
* OPTIONS : 대상 리소스에 대한 통신 기능 옵션(메서드)을 설명 (주로 CORS에서 사용)
* CONNECT : 대상 자원으로 식별되는 서버에 대한 터널을 설정
* TRACE : 대상 리소스에 대한 경로를 따라 메시지 루프백 테스트를 수행

## GET 
* 리스트 조회
* 서버에 전달하고 싶은 데이터는 query(쿼리 파라미터, 쿼리 스트링)를 통해서 전달
* 메시지 바디를 사용해서 데이터를 전달할 수 있지만, 지원하지 않는 곳이 많아서 권장하지 않음

## POST
* 요청 데이터 처리
* 메시지 바디를 통해 서버로 요청 데이터 전달
* 서버는 요청 데이터를 처리
  * 메시지 바디를 통해 들어온 데이터를 처리하는 모든 기능을 수행한다.
* 주로 전달된 데이터로 신규 리소스 등록, 프로세스 처리에 사용

요청 데이터를 어떻게 처리한다는 뜻일까?
* HTML 양식에 입력 된 필드와 같은 데이터 블록을 데이터 처리 프로세스에 제공
  * 예) HTML, FORM에 입력한 정보로 회원 가입, 주문 등에서 사용
* 게시판, 뉴스 그룹, 메일링 리스트, 블로그 또는 유사한 기사 그룹에 메시지 게시
  * 예) 게시판 글쓰기, 댓글 달기
* 서버가 아직 식별하지 않은 새 리소스 생성
  * 예) 신규 주문 생성
* 기존 자원에 데이터 추가
  * 예) 한 문서 끝에 내용 추가하기
 
다른 메서드로 처리하기 애매한 경우?
* 예) JSON으로 조회 데이터를 넘겨야 하는데, GET 메서드를 사용하기 어려운 경우
* 애매하면 POST

## 클라이언트에서 서버로 데이터 전송
데이터 전달 방식은 크게 2가지

* 쿼리 파라미터를 통한 데이터 전송
  * GET
  * 주로 정렬 필터(검색어)
* 메시지 바디를 통한 데이터 전송
  * POST, PUT, PATCH
  * 회원가입, 상품 주문, 리소스 등록, 리소스 변경

4가지 상황
* 정적 데이터 조회
  * 이미지, 정적 텍스트 문서
* 동적 데이터 조회
  * 주로 검색,게시판 목록에서 정렬 필터(검색어)
* HTML FORM을 통한 데이터 전송
  * 회원가입, 상품 주문, 데이터 변경
* HTTP API를 통한 데이터 전송
  * 회원가이브 상품 주문, 데이터 변경
  * 서버 TO 서버, 앱 클라이언트, 웹 클라이언트(AJAX)

### 정적 데이터 조회
쿼리 파라미터 미사용
* 이미지, 정적 텍스트 문서
* 조회는 GET사용
* 정적 데이터는 일반적으로 쿼리 파라미터 없이 리소스 경로로 단순하게 조회 가능

### 동적 데이터 조회
쿼리 파라미터 사용
* 주로 검색, 게시판 목록에서 정렬 필터(검색어)
* 조회 조건을 줄여주는 필터, 조회 결과를 정렬하는 정렬 조건에 주로 사용
* 조회는 GET사용
* GET은 쿼리 파라미터 사용해서 데이터를 전달

### HTML FORM 데이터 전송
POST 전송 - 저장, GET 전송 - 저장
* HTML FORM SUBMIT시 POST 전송
  * 회원가입, 상품 주문, 데이터 변경
* Content-Type : application/x-www-form-urlencoded 사용
  * form의 내용을 메시지 바디를 통해서 전송(key=value, 쿼리 파라미터 형식)
  * 전송 데이터를 url encoding 처리
    * abc김 -> abc%eZA%B%*
* HTMl Form은 GET 전송도 가능
* Content-Type : multipart/form-data
  * 파일 업로드 같은 바이너리 데이터 전송시 사용
  * 다른 종류의 여러 파일과 폼의 내용 함께 전송 가능(그래서 이름이 multipart)
* 참고 : HTML Form전송은  GET, POST만 지원

### HTTP API 데이터 전송
* 서버 to 서버
  * 백엔드 시스템 통신
* 앱 클라이언트
  * 아이폰, 안드로이드
* 웹 클라이언트
  * HTML에서 Form 전송 대신 자바 스크립트를 통한 통신에 사용(AJAX)
  * React, VueJs 같은 웹클라이언트 API 통신
* POST, PUT, PATCH : 메시지 바디를 통해 데이터 전송
* GET : 조회, 쿼리 파라미터로 데이터 전달
* Content-Type : application/json을 주로 사용(사실상 표준0
  * TEXT, XML, JSON 등등

## HTTP API 설계 예시
* HTTP API - 컬렉션
  * POST기반 등록
  * 회원관리 API 제공
* HTTP API - 스토어
  * PUT기반 등록
  * 정적 컨텐츠 관리, 원격 파일 관리
* HTML FORM 사용
  * 웹 페이지 회원 관리
  * GET, POST만 지원

## 회원 관리 시스템
API 설계 - POST 기반 등록
* 회원 목록 /member -> GET
* 회원 등록 /member -> POST
* 회원 조회 /member/{id} -> GET
* 회원 수정 /member/{id} -> PATCH, PUT, POST
* 회원 삭제 /member/{id} -> DELETE

POST - 신규 자원 등록 특징
* 클라이언트는 등록될 리소스의 URI를 모른다
  * 회원 등록 /member -> POST
  * POST /member
* 서버가 새로 등록된 리소스 URI를 생성해준다.
  * HTTP/1.1 201 Create
    Location:/members/100
* `컬렉션(Collection)`
  * 서버가 관리하는 리소스 디렉토리
  * 서버가 리소스의 URI를 생성하고 관리
  * 여기서 컬렉션은  /members
 
## 파일 관리 시스템
API 설계 - PUT 기반 등록
* 파일 목록 /files -> GET
* 파일 조회 /files/{filename} -> GET
* 파일 등록 /files/{filename} -> PUT
* 파일 삭제 /files/{filename} -> DELETE
* 파일 대량 등록 /files -> POST

PUT - 신규 자원 등록 특징
* 클라이언트가 리소스 URI를 알고 있어야 한다.
  * 파일 등록 /files/{filename} -> PUT
  * PUT /files/star.jpg
* 클라이언트가 직접 리소스의 URI를 지정한다.
* `스토어(Store)`
  * 클라이언트가 관리하는 리소스 저장소
  * 클라이언트가 리소스의 URI를 알고 관리
  * 여기서 스토어는 /files

## HTML FORM 사용
* HTML FORM은 GET, POST만 지원
* AJAX 같은  기술을 사용해서 해결 가능
* GET, POST만 지원하므로 제약이 있음

* 회원 목록 /members-> GET
* 회원 등록 폼 /members/new -> GET
* 회원 등록 /members/new, /members-> POST
* 회원 조회 /members/{id}-> GET
* 회원 수정 폼 /members{id}/edit-> GET
* 회원 수정 /members{id}/edit, /members/{id}-> POST
* 회원 목록 /members/{id}/delete-> POST

* HTML FORM은 GET, POST만 지원
* 컨트롤 URI
  * GET, POST만 지원하므로 제약이 있음
  * 이런 제약을해결하기 위해 동사로 된 리소스 경로 사용
  * POST의 /new, /edit, /delete가 컨트롤 URI
  * HTTP 메서드로 해결하기 애매한 경우 사용(HTTP API 포함)

### 정리
* `문서(DOCUMENT)`
  * 단일 개념(파일 하나, 객체 인스턴스, 데이터베이스 ROW)
  * 예) /members/100, /files/star.jpg
* `컬렉션(COLLECTION)`
  * 서버가  관리하는 리소스 디렉터리
  * 서버가 리소스의 URI를 생성하고 관리
  * 예) /members
* `스토어(STORE)`
  * 클라이언트가 관리하는 자원 저장소
  * 클라이언트가 리소스의 URI를 알고 관리
  * 예) /files
*` 컨트롤러(CONTROLLER)`, `컨트롤 URI`
  * 문서, 컬렉션, 스토어로 해결하기 어려운 추가 프로세스 실행
  * 동사를 직접 사용
  * 예) /members/{id}/delete

## HTTP 상태코드
### 상태코드
클라이언트가 보낸 요청의 처리 상태를 응답에서 알려주는 기능
* 1XX (information): 요청이 수신되어 처리중
* 2XX (successful): 요청 정상 처리
* 3xx (redirection): 요청을 완료하려면 추가행동이 필요
* 4xx (client error): 클라이언트 오류ㅡ 잘못된 문법등으로 서버가 요청을 수행할 수 없음
* 5xx (server error): 서버 오류, 서버가 정상 요청을 처리하지 못함

만약 모르는 상태 코드가 나타나면?
* 클라이언트가 인식할 수 없는 상태코드를 서버가 반환하면?
* 클라이언트는 상위 상태코드로 해석해서 처리
* 미래에 새로운 상태 코드가 추가되어도 클라이언트를 변경하지 않아도 됨
* 예)
  * 299??? -> 2xx(successful)
  * 451??? -> 4xx(client error)
  * 599??? -> 5xx(server error)

### 1xx(Information)
요청이 수신되어 처리중
* 거의 사용하지 않으므로 생략

### 2XX (Successful)
요청 정상 처리
* 200 ok
* 201 created 요청 성공해서 새로운 리소스가 생성됨
* 202 accepted 요청이 접수되었으나 처리가 완료되지 않았음
* 204 no content 서버가 요청을 성공적으로 수행했지만, 응답 페이로드 본문에 보낼 데이터가 없음

### 3xx (Redirection)
요청을 완료하려면 추가행동이 필요
* 300 Multiple Choices
* 301 Moved Permanently
* 302 Found
* 303 See Other
* 304 Not modified
* 307 Temporary Redirect
* 308 Permanent Redirect

### 리다이렉션 이해
* 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동(리다이렉트)

종류
* 영구 리다이렉션 - 특정 리소스의 URI가 영구적으로 이동 (301, 308)
  * 원래의 URL를 사용X, 검색 엔진 등에서도 변경 인지
  * 예) /members -> /users
  * 예) /event -> /new-event
* 일시 리다이렉션 - 일시적인 변경
  * 주문 완료 후 주문 내역 화면으로 이동
  * PRG : Post/Redirect/Get
* 특수 리다이렉션
  * 결과 대신 캐시를 사용
