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
URL(Locator)
URN(Name)

#### 문법
- 대부분의 URL 스킴의 문법은 9개 부분으로 나뉨
- 9개 모든 컴포넌트를 가지는 URL은 거의 없음 
- 가장 중요한 컴포넌트는 **스킴, 호스트, 경로**

> - <스킴>://<사용자 이름>:<비밀번호>@<호스트>:<포트>/<경로>;<파라미터>?<질의>#<프래그먼트> <br>
> - scheme://[userinfo@]host[:port][/path][?query][#fragment] <br>
> - https://www.google.com:443/search?q=hello&hl=ko 


<br>

## HTTP 기본

## HTTP 메서드

## HTTP 메서드 활용

## HTTP 상태코드

## HTTP 헤더1 - 일반 헤더

## HTTP 헤더2 - 캐시와 조건부 요청



<br><br><br>

> 🧐 출처 <br>
> [강의] [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-웹-네트워크) <br>
> [도서] [HTTP 완벽 가이드](http://www.yes24.com/Product/Goods/15381085)
