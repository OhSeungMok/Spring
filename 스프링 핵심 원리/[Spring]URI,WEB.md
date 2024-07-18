# [URI와 웹 브라우저 요청 흐름]
* URI
* 웹 브라우저 요청 흐름

## URI(Uniform Resource Identifier)
URI? URL? URN? <br>
URI는 로케이터(locator), 이름(name) 또는 두다 추가로 분류 될 수 있다. <br>
주민번호를 식별한다 라고 생각하면 편함<br>

URL(Resource Locate), URN(Resource Name) = URI

* Uniform : 리소스 식별하는 통일된 방식
* Resource : 자원, URI로 식별할 수 있는 모든 것(제한 없음)
* Identifier : 다른 항목과 구분하는데 필요한 정보

### URL, URN 
* URL - Locator : 리소스가 있는 위치를 지정(프로토콜, 호스트명, 포트 번호, 패스, 쿼리파라미터)
* URN - Name : 리소스에 이름을 부여
* 위치는 변할 수 있지만, 이름은 변하지 않는다.
* URN 이름만으로 실제 리소스를 찾을 수 있는 방법이 보편화 되지 않음

## 웹 브라우저 요청 흐름
### HTTP 메시지 전송
1. 웹 브라우저가 HTTP 메시지 생성
2. SOCKET 라이브러리를 통해 전달
   -A: TCP/IP 연결(IP, PORT)
   -B: 데이터 전달
3. TCP/IP 패킷 생성, HTTP 메시지 포함

### [HTTP]
* 모든 것이 HTTP
* 클라이언트 서버 구조
* Stateful, Stateless
* 비연결성(connectionless)
* HTTP 메시지
