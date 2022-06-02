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

## Spring WebFlux(2)

## WebClient

## WebSockets

## Testing

## RSocket

## Reactive Libraries
