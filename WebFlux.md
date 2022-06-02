https://godekdls.github.io/Reactive%20Spring/contents/ 를 보고 공부하며 기록한 자료

# Spring Web on Reactive Stack

##### <code>Reactive Streams</code>
asynchronous stream processing with non-blocking back pressure <br>
#비동기 #논블로킹 #JDK>=9(java.util.concurrent.Flow)

## Spring WebFlux(1)
##### <code>spring-webmvc, spring-webflux</code> <br>
- 스프링 프레임워크에 공존
- 원하는 모듈을 선택
  - 둘 중 하나를 사용해 개발할 수 있고, 둘 다 사용할 수도 있다

##### Why?
- 적은 스레드로 동시 처리를 제어하고, 적은 하드웨어 리소스로 확장하기 위해
- 함수형 프로그래밍(자바8 에서 추가된 람다 표현식 덕분에 자바에서도 함수형 API를 작성할 수 있게 됨)

##### Reactive(변화에 반응)
<code>논블로킹 back pressure</code> <br>
동기식 명령형 코드에서 블로킹 호출은 호출자를 강제로 기다리게 하는 일종의 back pressure <br>
논블로킹 코드에선, 프로듀셔 속도가 컨슈머 속도를 압도하지 않도록 이벤트 속도를 제어 

##### Reactive API
- [Reactor](https://github.com/reactor/reactor) 는 스프링 웹플럭스가 선택한 리액티브 라이브러리
- 리액터는 <code>Mono</code>와 <code>Flux</code> API 타입을 제공
- 데이터 시퀀스를 0~1개는 <code>Mono</code>, 0~N개는 <code>Flux</code>로 표현할 수 있음
- 리액터는 리액티브 스트림 라이브러리이기 때문에 모든 연산자는 논블로킹 back pressure를 지원 

##### Programming Models
WebFlux는 두 가지 프로그래밍 모델을 지원
- Annotated Controllers
- Functional Endpoints

##### Concurrency Model
스프링 MVC와 스프링 WebFlux 둘 다 annotated controller를 사용할 수 있다는 점은 동일해도, 동시성 모델과 블로킹/쓰레드 기본 전략이 다르다.
- MVC 는 어플리케이션이 처리 중인 쓰레드가 잠시 중단될 수 있다(예를 들어 외부 서비스를 호출하면). 그렇기 때문에 서블릿 컨테이너는 이 블로킹을 대비에 큰 쓰레드 풀로 요청을 처리
- WebFlux는 실행 중인 쓰레드가 중단되지 않는다는 전제가 있다. 따라서 논블로킹 서버는 작은 쓰레드 풀(이벤트 루프 워크)을 고정해놓고 요청을 처리

> 🤓쓰레드를 중단하지 않는다는 건(그리고 콜백에 처리를 맡기는 건) 요청을 처리할 다른 쓰레드가 필요 없고, 그렇기 때문에 블로킹을 대비할 필요가 없다는 뜻이다.

##### HttpHandler
요청과 응답을 처리하는 메소드를 하나만 가지고 있다. 유일한 역할은 여러 HTTP 서버 API를 추상화하는 것 

##### WebHandler API

##### Filters
CORS

##### Exceptions
_WebHandler API_ 는 <code>WebFilter</code> 체인과 <code>WebHandler</code>에서 발생한 예외를 <code>WebExceptionHandler</code>로 처리

##### 1.2.5 Codecs





## Spring WebFlux(2)

## WebClient

## WebSockets

## Testing

## RSocket

## Reactive Libraries
