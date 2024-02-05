## JDK 21
### JEP 441: Pattern Matching for switch

Record Patterns 에서 처럼 자바는 최근 패턴 매칭 부분을 상당히 개선시키고 있는데, 패턴 매칭을 switch 문까지 확장해서 적용
- 자바의 데이터 중심적인 프로그래밍을 간결하고 안전하게 표현하는 것을 도움


#### Pattern Matching
AS-IS
- 특정 타입 체크가 제한적이기 때문에, 특정 타입 여부를 체크하려면 instance of 에 if-else 문법을 사용
```java
// 자바 21 이전
if (obj instanceof Integer i) {
    System.out.println("Integer");
} else if (obj instanceof String s) {
    System.out.println("String");
} else if (obj instanceof Pair) {
    System.out.println("Pair");
} else {
    System.out.println("other");
}
```

TO-BE
- 타입 체크를 개선하여, 간결한 패턴 매칭을 처리
```java
// 자바 21 부터
switch (obj) {
    case null -> System.out.println("null");
    case Integer i -> System.out.println("Integer");
    case String s -> System.out.println("String");
    case Pair(String s, Integer i) -> System.out.println("Pair");
    default -> ystem.out.println("other");
}
```
Integer i 패턴라벨
Integer, String 이거 자체가 타입을 판단하는 패턴이고
타입에 해당하는 값을 접근하는 변수 i 
Pair 레코드 패턴




#### null 처리
AS-IS
- 별도의 if 문을 통해 null 처리 
```java
// 자바 21 이전
if (s == null) {
    System.out.println("Null!!!");
    return;
}
switch (s) {
    case "Foo", "Bar" -> System.out.println("Great");
    default           -> System.out.println("Ok");
}
```

TO-BE
- 별도의 if문 없이 switch 문 만으로 null 처리 가능해짐
```java
// 자바 21 부터
switch (s) {
    case null         -> System.out.println("Null!!!");
    case "Foo", "Bar" -> System.out.println("Great");
    default           -> System.out.println("Ok");
}
```


#### case 문 세분화
- 패턴 + when(조건)
```java
String desc = switch (o) {
    case null -> "null";
    case Integer i -> "Integer";
    case String s when s.length() > 10 -> "long String";
    case String s when s.length() < 3 -> "short String";
    case String s -> "middle String";
    case Pair(String s, Integer i) -> "Pair(String:%s, Integer:%d)".formatted(s, i);
    default -> "other";
};
```

- 같은 타입 패턴이면 조건 붙은 case가 앞에 위치해야 함


#### enum 개선



#### 주의
1. 식(expression)과 패턴 
- 패턴을 사용할 경우, switch 식은 모든 경우를 다뤄야 함

```java
Object o = ...;
String desc = switch (o) {
   case Integer i -> "Integer";
   case String s -> "String";
};
```
> 컴파일 에러 : the switch expression does not cover all possible input values

```java
Object o = ...;
String desc = switch (o) {
    case Integer i -> "Integer";
    case String s -> "String";
    default -> "Any";
};
```
Integer, String 이외에도 많은 타입이 있기 때문에, 모든 경우의 수를 구현하거나 default 문을 사용해야함

2. 문(statement)과 패턴 
- 패턴 라벨은 fall through에서 사용 못 함

> switch Fall Through란?<br>switch 문에서 case 내에서 의도적으로 break문을 생략하여 다음 case로 이동 시키는 방법

```java
Object o = ...;
switch (o) {
    case Integer i :
        System.out.println("Integer");
    case String s :
        System.out.println("String");
    default :
        System.out.println("default");
};
```

> 컴파일 에러 : illegal fall-through to a pattern

```java
switch (o) {
    case Integer i :
        System.out.println("Integer");
        break;
    case String s :
        System.out.println("String");
        break;
    default :
        System.out.println("default");
        
};
```
break 없으면 나머지 라벨을 다 실행하기 때문에


정리
- record 패턴을 switch 에서 사용가능해서 활용도가 있다
- switch null case : null 비교 위한 if가 필요없어 간결
- switch 에 타입 패턴 매칭이 들어가서 좋다
  - when 들어가서 복잡한 if-else 블록을 switch 로 대체 가능하다

