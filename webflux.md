webflux

스프링 5.0 버전에 추가된 스프링 웹플럭스는 리액티브 스택 웹 프레임워크다.
완전하게 논블로킹으로 동작
Reactive Streams back pressure를 지원하고, Netty, Undertow, 서블릿 3.1+ 컨테이너 서버에서 실행된다.

spring-webmvc, spring-webflux
스프링 프레임워크에 공존
원하는 모듈을 선택하면 된다.
둘 중 하나를 사용해 어플리케이션을 개발할 수 있고,
둘 다 사용할 수 있다.


탄생 이유 : 적은 쓰레드로 동시 처리를 제어하고 적은 하드웨어 리소스로 확장하기 위해 논블로킹 웹 스택이 필요했기 때문
함수형 프로그래밍이 가능한 자바 8 덕분

https://godekdls.github.io/Reactive%20Spring/contents/
