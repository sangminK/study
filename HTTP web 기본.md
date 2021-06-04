## 인터넷 네트워크

### IP(인터넷 프로토콜)

#### 역할
- 패킷(packet)이라는 통신 단위로, 지정한 IP 주소에 데이터 전달

#### 한계
- 비연결성 : 패킷을 받을 대상이 없거나 서비스 불능 상태여도 패킷 전송(대상 서버가 패킷을 받을 수 있는 상태인지 모름)
- 비신뢰성 : 중간에 패킷이 사라지거나, 순서대로 안 올 수 있음
- 프로그램 구분 : 같은 IP를 사용하는 서버에서 통신하는 애플리케이션이 둘 이상이라면? 
> **✔ 한계가 있기 때문에 TCP로 보완 해준다.**
<br>

### TCP, UDP

#### 인터넷 프로토콜 스택의 4계층
<table>  
  <tr>
    <th>계층</th>
    <th>역할</th>
  </tr>
  <tr>
    <td>애플리케이션 계층 - HTTP, FTP</td>
    <td>
      1. 프로그램이 Hello, world! 메시지 생성 <br>
      2. SOCKET 라이브러리를 통해 전달
    </td>
  </tr>
  <tr>
    <td>전송 계층 - TCP, UDP</td>
    <td>3. TCP 정보 생성, 메시지 데이터 포함</td>
  </tr>
  <tr>
    <td>인터넷 계층 - IP</td>
    <td>4. IP 패킷 생성, TCP 데이터 포함</td>
  </tr>
  <tr>
    <td>네트워크 인터페이스 계층</td>
    <td>5. Ethernet frame을 씌워 LAN 카드에서 전송</td>
  </tr>  
</table>

#### TCP 특징
- 연결 지향 : TCP 3 way handshake (가상 연결)
  - 가상 연결 : 논리적 연결. 그 안에 수많은 거쳐가는 노드들은 연결이 됐는지 안 됐는지는 모름
  - 상대방과 내가 연결됐는지 확인하고 메시지를 보냄
- 데이터 전달 보증 : 패킷이 중간에 누락되면 알 수 있음      
- 순서 보장
 
#### TCP 3 way handshake
1. 클라이언트 ----> 서버 : SYN(접속 요청)
2. 클라이언트 <---- 서버 : SYN + ACK(요청 수락)
3. 클라이언트 ----> 서버 : ACK
4. 데이터 전송(3.ACK과 함께 전송 가능)

#### UDP
- TCP는 이미 널리 사용하고 있고, 수정을 더 이상 못함
- 최근, 최적화를 추가한 UDP도 뜨고 있음
- TCP 3 way handshake X
- 데이터 전달 보증 X
- 순서 보장 X
- 데이터 전달 및 순서가 보장되지 않지만, 단순하고 빠름
- IP와 거의 같음 

<br>

### PORT
- 같은 IP 내에서 프로세스 구분(cf.IP가 한 아파트면 동 호수는 port)
- 0~65535 : 할당 가능
- 0~1023 : 잘 알려진 포트, 사용하지 않는 것이 좋음
  - FTP : 20,21
  - TELNET : 23
  - HTTP : 80
  - HTTPS : 443

<br>

### DNS(Domain Name System)
- IP는 기억하기 어렵고 바뀔 수 있음
- 도메인 명을 IP 주소로 변환(like 전화번호부) -> DNS 서버

<br>

### URI(Uniform Resource Identifier)
URL(Locator),
URN(Name)

#### 문법
- 대부분의 URL 스킴의 문법은 9개 부분으로 나뉨
- 9개 모든 컴포넌트를 가지는 URL은 거의 없음 
- 가장 중요한 컴포넌트는 **스킴, 호스트, 경로**

> - <스킴>://<사용자 이름>:<비밀번호>@<호스트>:<포트>/<경로>;<파라미터>?<질의>#<프래그먼트> <br>
> - scheme://[userinfo@]host[:port][/path][?query][#fragment] <br>
> - https://www.google.com:443/search?q=hello&hl=ko 

<br>

### 웹 브라우저 요청 흐름
![webflow](https://github.com/sangminK/study/blob/main/img/%EC%9B%B9%20%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%20%EC%9A%94%EC%B2%AD%20%ED%9D%90%EB%A6%84.jpeg)

<br>

## HTTP 기본

### 모든 것이 HTTP
HTML 문서 뿐만 아니라, IMAGE, 음성, 영상 파일, JSON, XML 등 거의 모든 형태의 데이터 전송 가능 <br>
서버간에 데이터를 주고 받을 때도 대부분 HTTP 사용

#### HTTP 역사
- HTTP/0.9 : GET 메서드만 지원, HTTP 헤더 X
- HTTP/1.0 : 메서드, 헤더 추가
- **_HTTP/1.1 : 대부분의 기능이 들어있고, 가장 많이 사용(가장 중요)❗️_**
- HTTP/2 : 성능 개선
- HTTP/3 : TCP 대신에 UDP 사용, 성능 개선

#### 기반 프로토콜
- HTTP/1.1, HTTP/2 -> TCP 기반
- HTTP/3 -> UDP 기반 
<br>
현재 HTTP/1.1를 주로 사용, HTTP/2, HTTP/3도 점점 증가

#### 웹 페이지에서 프로토콜 확인 
![protocol](https://github.com/sangminK/study/blob/main/img/%EC%9B%B9%20%ED%8E%98%EC%9D%B4%EC%A7%80%EC%97%90%EC%84%9C%20%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C%20%ED%99%95%EC%9D%B8.png)

<br>

### 클라이언트 서버 구조
- 클라이언트는 서버에 요청을 보내고, 응답을 대기
- 서버가 요청에 대한 결과를 만들어서 응답
<table>
  <tr>
    <th rowspan="2">클라이언트</th>
    <td>1.요청 -----> </td>
    <th rowspan="2">서버</th>
  </tr>
  <tr>
    <td> <----- 2.응답 </td>
  </tr>
</table>

#### 클라이언트와 서버를 분리하는게 중요한 이유
- 비즈니스 로직과 데이터는 서버에
- 클라이언트는 UI와 사용성에 집중
> ❗️ **각각 독립적으로 진화 가능!!**

<br>

### stateful, stateless
#### stateless(무상태 프로토콜)
- 서버가 클라이언트의 상태를 보존 X
- 아무 서버나 호출해도 됨 
- 장점 : 
  - 서버 확장성 높음(스케일 아웃-수평 확장 유리) 
  - 중간에 서버 장애나도 다른 서버로 다시 요청하면 됨
- 단점 : 클라이언트가 추가 데이터 전송 

#### stateful
- 상태 유지
- 항상 같은 서버가 유지되어야 함

#### stateless의 실무적 한계
로그인의 경우? <br>
- 로그인한 사용자의 경우, 로그인 했다는 상태를 서버에 유지
- 일반적으로 브라우저 쿠키와 서버 세션 등을 사용해서 상태 유지
- 상태 유지는 최소한만 사용

> 웹 애플리케이션을 설계할 때는 최대한 무상태로 설계

<br>

### 비 연결성(connectionless)

<table>
  <tr>
    <th>연결을 유지하는 모델</th>
    <th>연결을 유지하지 않는 모델</th>
  </tr>
  <tr>
    <td>서버는 연결을 계속 유지, 서버 자원 소모</td>
    <td>서버는 연결 유지X, 최소한의 자원 사용</td>
  </tr>
</table>

#### HTTP의 비 연결성
- HTTP는 기본이 연결을 유지하지 않는 모델
- 일반적으로 초 단위의 이하의 빠른 속도로 응답
- 1시간 동안 수천명이 서비스를 사용해도 실제 서버에서 동시에 처리하는 요청은 수십개 이하로 매우 작음
- 서버 자원을 매우 효율적으로 사용할 수 있음

#### 한계와 극복
- TCP/IP 연결을 새로 맺어야 함 -> 3 way handshake 시간 추가
- 웹 브라우저로 사이트를 요청하면 HTML 뿐만 아니라 자바스크립트, css, 추가 이미지 등 수많은 자원이 함께 다운로드 

> 지금은 HTTP 지속 연결(Persistent Connections)로 문제 해결 <br>
> HTTP/2, HTTP/3에서 더 많은 최적화

#### HTTP 지속 연결(Persistent Connection)
<table>
  <tr>
    <th>HTTP 초기 - 연결, 종료 낭비</th>
    <th>HTTP 지속 연결</th>
  </tr>
  <tr>
    <td>HTML 요청/응답, 자바스크립트 요청/응답, 이미지 요청/응답에 연결과 종료를 각각 진행(각각 시간 소요)</td>
    <td>한번 연결 후, HTML 요청/응답, 자바스크립트 요청/응답, 이미지 요청/응답을 받고 종료(연결과 종료 한번)</td>
  </tr>
</table>

<br>

### HTTP 메시지
<table>
  <tr>
    <th></th>
    <th>HTTP 메시지 구조</th>
    <th>HTTP 요청 메시지</th>
    <th>HTTP 응답 메시지</th>
  </tr>
  <tr>
    <td>구조</td>
    <td>
      <table>
        <tr>
          <td>start-line 시작줄 </td>
        </tr>
        <tr>
          <td>header 헤더 <br><br> </td>
        </tr>     
        <tr>
          <td>empty line 공백 라인 (CRLF) </td>
        </tr>    
        <tr>
          <td>message body <br><br> </td>
        </tr>          
      </table>
    </td>
    <td>
      <table>
        <tr>
          <td><메서드> <요청 URL> <버전> </td>
        </tr>
        <tr>
          <td><헤더> <br><br> </td>
        </tr>     
        <tr>
          <td> <br> </td>
        </tr>    
        <tr>
          <td><엔터티 본문> <br><br> </td>
        </tr>          
      </table>    
    </td>
    <td>
      <table>
        <tr>
          <td><버전> <상태코드> <사유 구절> </td>
        </tr>
        <tr>
          <td><헤더> <br><br> </td>
        </tr>     
        <tr>
          <td> <br> </td>
        </tr>    
        <tr>
          <td><엔터티 본문> <br><br> </td>
        </tr>          
      </table>            
    </td>
  </tr>
  <tr>
    <td>예시</td>
    <td></td>
    <td>
      <table>
        <tr>
          <td>GET /search?q=hello&hl=ko HTTP/1.1 </td>
        </tr>
        <tr>
          <td>Host: www.google.com</td>
        </tr>     
        <tr>
          <td> <br> </td>
        </tr>        
      </table>     
    </td>
    <td>
      <table>
        <tr>
          <td>HTTP/1.1 200 OK </td>
        </tr>
        <tr>
          <td>
            Content-Type: text/html; charset=UTF-8 <br> 
            Content-Length: 3423          
          </td>
        </tr>     
        <tr>
          <td> <br> </td>
        </tr>
        <tr>
          <td>
            html...
          </td>
        </tr>        
      </table>     
    </td>
  </tr>
</table>

- 요청 메시지도 body 본문을 가질 수 있음
- 시작줄만 문법이 다름

#### 메서드
클라이언트 측에서 서버가 리소스에 대해 수행해주길 바라는 동작('GET', 'HEAD', 'POST')

#### 요청 URL
요청 대상이 되는 리소스를 지칭하는 완전한 URL 혹은 URL의 경로 구성요소

#### 버전
이 메시지에서 사용 중인 HTTP의 버전: HTTP/<메이저>.<마이너>
> 정수로 다루는 것에 주의 : HTTP/2.22 는 HTTP/2.3 보다 크다

#### 상태코드
요청 중에 무엇이 일어났는지 설명하는 세 자리의 숫자. 각 코드의 첫 번째 자릿수는 상태의 일반적인 분류('성공', '에러' 등)를 나타냄

#### 사유 구절
숫자로 된 상태 코드의 의미를 사람이 이해할 수 있게 설명해주는 짧은 문구(200 -> OK)

#### 헤더
- HTTP 전송에 필요한 모든 부가정보
- body 제외하고 필요한 메타데이터가 다 들어있음
- 이름, 콜론(:), 선택적인 공백, 값, CRLF(줄바꿈)가 순서대로 나타나는 0개 이상의 헤더들
> field-name ":" OWS field-value OWS (OWS: 띄어쓰기 허용) <br>
> (field-name은 대소문자 구분 없음)

#### 엔터티 본문
실제 전송할 데이터 <br>
HTML 문서, 이미지, 영상, JSON 등등 byte로 표현할 수 있는 모든 데이터 전송 가능 

<br>


## HTTP 메서드

## HTTP 메서드 활용

## HTTP 상태코드

## HTTP 헤더1 - 일반 헤더

## HTTP 헤더2 - 캐시와 조건부 요청



<br><br><br>

> 🧐 출처 <br>
> [강의] [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-웹-네트워크) <br>
> [도서] [HTTP 완벽 가이드](http://www.yes24.com/Product/Goods/15381085)
