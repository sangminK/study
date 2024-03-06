# JDK21
## JEP 444: Virtual Threads


### 왜?
- IO 중심 작업에서, 처리량 늘리기 위해

##### 해결방법
- 비동기IO + 적은 쓰레드
- 경량 쓰레드 + IO 연동

##### 가상 쓰레드 목적
- 요청 당 쓰레드 구조의 서버 어플리케이션의 HW 최적 사용
- 최소 변경으로 기존 코드에 가상 쓰레드 적용



### 주의할 점
- 응답이 빠르지는 않다. 처리량이 높은거다
- ThreadLocal 사용을 신중히 : 무수히 많이 생성될 수 있기 떄문에 ThreadLocal 처럼 스레드마다 생성되는 것 신중히 사용
- Synchronized, Native Method 를 지양 -> Avoid Pinning : 캐리어스레드가 대기상태 > virtual thread 도 같이 대기상태


##### Pinning
- 가상스레드가 캐리어 스레드에 고정되는 것 (synchronized 에서 IO 블로킹) -플랫폼 스레드도 같이 블로킹
- 두가지 상황에서 발생
1. synchronized 블록 : 최소화 (예를 들면, 초기화 코드)
2. JNI 를 통해 네이티브 메서드 사용 또는 foreign 함수

##### 가장 큰 장점 : 소스를 수정할 필요가 없다 webflux 러닝커브 싫다



참고
- https://www.youtube.com/watch?v=Q1jZtN8oMnU
- https://www.youtube.com/watch?v=srpOD6WIasM
- https://www.youtube.com/watch?v=vQP6Rs-ywlQ
- https://d2.naver.com/helloworld/1203723


> - 이팩티브 자바

리팩터링 
https://www.11st.co.kr/products/3405264097?catalog_no=23645489&lowest_yn=Y&trTypeCd=PW51&trCtgrNo=585021

스프링 배치
https://godekdls.github.io/Spring%20Batch/introduction/#google_vignette
