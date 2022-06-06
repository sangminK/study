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
- 데이터 시퀀스를 0&#126;1개는 <code>Mono</code>, 0&#126;N개는 <code>Flux</code>로 표현할 수 있음
- 리액터는 리액티브 스트림 라이브러리이기 때문에 모든 연산자는 논블로킹 back pressure를 지원 

##### Programming Models
WebFlux는 두 가지 프로그래밍 모델을 지원
- Annotated Controllers
- Functional Endpoints

##### Concurrency Model
스프링 MVC와 스프링 WebFlux 둘 다 annotated controller를 사용할 수 있다는 점은 동일해도, 동시성 모델과 블로킹/쓰레드 기본 전략이 다르다.
- MVC 는 어플리케이션이 처리 중인 쓰레드가 잠시 중단될 수 있다(예를 들어 외부 서비스를 호출하면). 그렇기 때문에 서블릿 컨테이너는 이 블로킹을 대비에 큰 쓰레드 풀로 요청을 처리
- WebFlux는 실행 중인 쓰레드가 중단되지 않는다는 전제가 있다. 따라서 논블로킹 서버는 작은 쓰레드 풀(이벤트 루프 워크)을 고정해놓고 요청을 처리

> 🤓 <br>
> 쓰레드를 중단하지 않는다는 건(그리고 콜백에 처리를 맡기는 건) 요청을 처리할 다른 쓰레드가 필요 없고, 그렇기 때문에 블로킹을 대비할 필요가 없다는 뜻이다.

##### HttpHandler
요청과 응답을 처리하는 메소드를 하나만 가지고 있다. 유일한 역할은 여러 HTTP 서버 API를 추상화하는 것 

##### WebHandler API

##### Filters
CORS

##### Exceptions
_WebHandler API_ 는 <code>WebFilter</code> 체인과 <code>WebHandler</code>에서 발생한 예외를 <code>WebExceptionHandler</code>로 처리

##### Codecs
> 코덱(codec) <br>
> 어떠한 데이터 스트림이나 신호에 대해, 인코딩이나 디코딩, 혹은 둘 다를 할 수 있는 하드웨어나 소프트웨어를 일컫는다. <br>
> 또, 이를 위한 알고리즘을 가리키는 용어로도 쓰인다.

<code>spring-web</code>, <code>spring-core</code> 모듈을 사용하면 리액티브 논블로킹 방식으로 바이트 컨텐츠를 고수준 객체로 직렬화, 역직렬화할 수 있다.
- <code>Encoder</code>, <code>Decoder</code>는 HTTP와는 관계없는 컨텐츠를 인코딩, 디코딩한다.
- <code>HttpMessageReader</code>, <code>HttpMessageWriter</code>는 HTTP 메세지를 인코딩, 디코딩한다.
- 웹 어플리케이션에선 <code>Encoder</code>를 감싸고 있는 <code>EncoderHttpMessageWriter</code>와 <code>Decoder</code>를 감싸고 있는 <code>DecoderHttpMessageReader</code>를 사용할 수 있다.
- 모든 코덱은 라이브러리마다 다른 바이트 버퍼(e.g. Netty ByteBuf, java.io.ByteBuffer 등)를 추상화한 <code>DataBuffer</code>로 처리한다.

1. Jackson JSON <br>
<code>Flux<String></code>으로 JSON 배열을 만들고 싶다면 <code>Flux#collectToList()</code>를 사용해서 <code>Mono<List<String>></code>을 인코딩하라?
2. Form <br>
<code>FormHttpMessageReader</code>, <code>FormHttpMessageWriter</code>는 <code>application/x-www-form-urlencoded</code> 컨텐츠를 인코딩/디코딩한다.
3. Multipart <br>
<code>MultipartHttpMessageReader</code>, <code>MultipartHttpMessageWriter</code>는 "multipart/form-data" 컨텐츠를 인코딩/디코딩한다. <br>
사실 <code>MultipartHttpMessageReader</code>는 다른 <code>HttpMessageReader</code>에 파싱을 위임하고, 돌려받은 <code>Flux&#60;Part&#62;</code>를 <code>MultiValueMap</code>에 수집하는 역할만 한다.
4. Limits
5. Streaming
6. DataBuffer

##### Logging
스프링 웹플럭스는 <code>DEBUG</code> 레벨 로그에 꼭 필요한 정보만 최소한으로 담았다.

### DispatcherHandler
스프링 웹플럭스도 스프링 MVC와 유사한 프론트 컨트롤러 패턴 사용

##### Special Bean Types
|Bean type|Explanation|
|------|---|
|<code>HandlerMapping</code>|요청을 핸들러에 매핑한다.<br>주로 쓰는 구현체는 <code>@RequestMapping</code>을 선언한 메소드를 찾는 <code>RequestMappingHandlerMapping</code>, 함수형 엔드포인트를 라우팅하는 <code>RouterFunctionMapping</code>, URI path 패턴으로 <code>WebHandler</code>를 찾는 <code>SimpleUrlHandlerMapping</code> 등이 있다.|
|<code>HandlerAdapter</code>|<code>HandlerAdapter</code>가 핸들러를 실행하는 방법을 알고 있기 때문에, <code>DispatcherHandler</code>는 어떤 핸들러든지 받아 처리할 수 있다.|
|<code>HandlerResultHandler</code>|핸들러가 건네 준 결과를 처리하고 응답을 종료한다.|

##### WebFlux Config
  
##### Processing
<code>DispatcherHandler</code>는 다음과 같이 요청을 처리한다.
- <code>HandlerMapping</code>을 뒤져 매칭되는 핸들러를 찾는다. 첫 번째로 매칭된 핸들러를 사용한다.
- 핸들러를 찾으면 적당한 <code>HandlerAdapter</code>를 사용해 핸들러를 실행하고, <code>HandlerResult</code>를 돌려 받는다.
- <code>HandlerResult</code>를 적절한 <code>HandlerResultHandler</code>로 넘겨 바로 응답을 만들거나 뷰로 렌더링하고 처리를 완료한다.

##### Result Handling

##### Exceptions

##### View Resolution
  
## Spring WebFlux(2)

## WebClient

## WebSockets

## Testing

## RSocket

## Reactive Libraries
