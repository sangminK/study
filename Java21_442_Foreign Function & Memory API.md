## JDK 21
### JEP 442: Foreign Function & Memory API

네이티브 코드와 상호 작용하고 외부 메모리를 다루는 데 사용되는 API
- 자바에서 C나 C++와 같은 네이티브 라이브러리를 자바 애플리케이션에서 쉽게 호출하고 통합할 수 있도록 함
- 외부 메모리를 효율적으로 관리하고 사용할 수 있도록 함
-> 자바 애플리케이션과 네이티브 코드 간의 통신을 담당하는 중간 레이어
<br>

### 구성
1. Foreign Function Interface (FFI): FFI를 사용하면 자바 코드에서 네이티브 함수를 직접 호출할 수 있습니다. 이를 통해 기존에 작성된 C, C++ 등의 라이브러리를 자바 애플리케이션에서 활용할 수 있습니다. FFI를 통해 자바 코드에서 네이티브 함수를 호출할 때, 메서드 디스크립터와 네이티브 라이브러리의 경로를 지정하여 호출합니다.
2. Foreign Memory Access (FMA): FMA를 사용하면 자바 애플리케이션에서 외부 메모리에 직접 접근할 수 있습니다. ByteBuffer와 같은 기존 메모리 API와 달리 FMA를 사용하면 네이티브 메모리에 직접 접근할 수 있습니다. 이를 통해 메모리의 저수준 세부 사항을 조작할 수 있습니다.

### 대안 또는 비슷한 역할 
java.nio.ByteBuffer, sun.misc.Unsafe, JNI(Java Native Interface), 다른 서드파티 프레임워크 


<img width="673" alt="image" src="https://github.com/sangminK/study/assets/47021861/5f3600f7-ebe7-478b-b851-827936670260">

##### JNI 문제점
- Native-first programming model, brittle combination of Java and C
- Expensive to maintain and deploy
- Passing data to/from JNI is cumbersome and inefficient



### 예제 코드 
C 언어로 작성된 간단한 네이티브 함수 
```c
#include <stdio.h>

int add(int a, int b) {
    return a + b;
}
```

<br>
Java 21의 Foreign Function & Memory API를 사용하여 이 네이티브 함수를 호출하는 Java 코드

```java
import jdk.incubator.foreign.CLinker;
import jdk.incubator.foreign.FunctionDescriptor;
import jdk.incubator.foreign.MemoryAddress;
import jdk.incubator.foreign.MemoryHandles;

import java.lang.invoke.MethodHandle;
import java.lang.invoke.MethodType;
import java.nio.file.Path;
import java.nio.file.Paths;

public class Main {
    public static void main(String[] args) throws Throwable {
        // 네이티브 라이브러리 로드
        System.loadLibrary("add");

        // 네이티브 함수의 FunctionDescriptor 생성
        FunctionDescriptor descriptor = FunctionDescriptor.of(CLinker.C_INT, CLinker.C_INT, CLinker.C_INT);

        // 네이티브 함수에 대한 MethodHandle 생성
        Path path = Paths.get("add");
        MethodHandle handle = CLinker.getInstance().downcallHandle(
                path, "add", MethodType.methodType(int.class, int.class, int.class), descriptor);

        // 네이티브 함수 호출
        int result = (int) handle.invokeExact(10, 20);
        System.out.println("Result: " + result); // 출력: 30
    }
}
```

- jdk.incubator.foreign 패키지의 클래스들을 사용하여 네이티브 함수를 호출
- System.loadLibrary()를 사용하여 라이브러리를 로드
- FunctionDescriptor와 MethodHandle을 사용하여 네이티브 함수에 대한 호출을 설정하고 수행


### 장점
- 네이티브 코드와의 쉬운 통합
- 성능 향상 → 자바 코드보다 직접 하드웨어에 접근할 수 있기 때문
- 외부 메모리 관리 → 직접 접근으로 메모리 누수 같은 문제 방지
- 네이티브 라이브러리 재사용
- 플랫폼 독립성 유지 → 자바의 플랫폼 독립성을 유지하면서도 네이티브 기능 활용 가능

### 단점
- 보안 이슈 → 네이티브 코드가 적절히 검증되지 않았거나, 악의적인 코드일 경우 보안 취약점이 발생 가능성 
- 메모리 관리 어려움 → 외부 메모리를 직접 다루기 때문에, 메모리 누수 및 메모리 관리 문제를 야기 가능성
- 플랫폼 의존성 → 네이티브 코드 자체는 플랫폼에 의존적일 수 있어, 네이티브 코드를 활용하는 경우, 특정 플랫폼에 종속될 수 있음
- 성능 향상에 대한 보장이 없음 → 네이티브 호출로 인해 오버헤드 발생 가능성
- 디버깅 어려움 → 자바 코드와는 달리 네이티브 코드는 더 낮은 수준에서 동작하기 때문에 디버깅 어려움

### 활용 가능한 영역
- 시스템 프로그래밍: 운영 체제 수준의 기능을 활용. ex) 파일 시스템 액세스, 네트워킹, 운영 체제의 시스템 호출 등을 처리
- 프로세스 간 통신 (IPC): 다른 프로세스와의 통신을 수행
- 데이터베이스 및 하드웨어 인터페이스: 네이티브 라이브러리를 사용하여 데이터베이스에 접근하거나 하드웨어와의 상호 작용
- 게임 개발: 고성능 게임 엔진을 개발하거나, 그래픽 라이브러리를 활용
- 빅데이터 및 과학적 컴퓨팅: 고성능 컴퓨팅 작업을 수행하거나, 대용량 데이터를 처리

### JVM에서 네이티브 접근을 위한 기반

> 여기에서는 Java의 새로운 인터럽트 스토리에 대해 설명하고 있습니다. 이 스토리는 JCT라는 도구를 기반으로 하며, 이를 통해 네이티브 라이브러리를 직접 타깃팅할 수 있습니다. FFM을 작업하는 동안 예상치 못한 많은 사용 사례들이 나타났다고 합니다. FFM은 상당히 저수준의 라이브러리이기 때문에, JVM 위에 구축된 다른 언어들인 Scala, Closure, 루비 등이 FFM 레이어를 사용하여 네이티브 함수를 타깃팅할 수 있게 해줍니다. 이는 JNI를 사용할 때 비용이 많이 들었던 작업을 가능하게 합니다. 또한 FFM은 JDK 14부터 incubating 및 preview 기능으로 제공되었으며, Apache Lucene, Tomcat 등의 피드백을 통해 많은 개선이 이루어졌습니다. 이를 통해 Lucene을 Java 21에서 실행할 때 FFM을 사용할 수 있으며, 이는 메모리 해제를 위해 Unsafe를 사용해야 하는 문제를 해결해 주었습니다. Tornado VM도 FFM을 사용하고 있으며, GPU 내부 메모리를 모델링하기 위해 메모리 세그먼트를 사용하는 흥미로운 사례를 제공합니다. 마지막으로, FFM의 미리보기 기능은 많은 피드백을 받았고, JDK 팀 내에서 해당 주제에 대한 지식이 많이 향상되었다고 합니다.

<img width="406" alt="image" src="https://github.com/sangminK/study/assets/47021861/5fb22769-2c8b-4b63-ae92-3e92cb99f1f6">

