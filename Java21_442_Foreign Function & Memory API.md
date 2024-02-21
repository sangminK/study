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
- 네이티브 코드와의 쉬운 통합: 기존에는 네이티브 코드와의 통합이 복잡했지만, 이 API를 사용하면 자바 코드에서 네이티브 함수를 직접 호출할 수 있습니다. 이를 통해 네이티브 라이브러리를 쉽게 활용할 수 있습니다.
- 성능 향상: 네이티브 코드는 자바 코드보다 직접 하드웨어에 접근할 수 있기 때문에 성능이 뛰어날 수 있습니다. 이 API를 사용하면 자바 애플리케이션의 성능을 향상시킬 수 있습니다.
- 외부 메모리 관리: 이 API를 사용하면 자바 애플리케이션이 외부 메모리에 접근하고 관리하는 것이 더욱 쉬워집니다. 메모리 누수와 같은 문제를 방지할 수 있습니다.
- 네이티브 라이브러리 재사용: 기존에 작성된 네이티브 라이브러리를 자바 애플리케이션에서 쉽게 재사용할 수 있습니다. 이는 개발 시간을 단축시키고 코드의 재사용성을 높입니다.
- 플랫폼 독립성 유지: 자바는 플랫폼 독립적인 언어이지만, 때로는 네이티브 기능이 필요할 때가 있습니다. 이 API를 사용하면 플랫폼 독립성을 유지하면서도 네이티브 기능을 활용할 수 있습니다.

### 단점
- 보안 문제: 네이티브 코드와의 통합은 보안 문제를 야기할 수 있습니다. 네이티브 코드가 적절하게 검증되지 않았거나, 악의적인 코드일 경우 보안 취약점이 발생할 수 있습니다.
- 메모리 관리 어려움: 외부 메모리를 직접 다루는 것은 메모리 누수 및 메모리 관리 문제를 야기할 수 있습니다. 이를 신중하게 관리하지 않으면 예기치 않은 동작이 발생할 수 있습니다.
- 플랫폼 의존성: 네이티브 코드는 플랫폼에 의존적일 수 있습니다. 따라서 네이티브 코드를 활용하는 경우, 특정 플랫폼에 종속될 수 있습니다.
- 성능 향상에 대한 보장이 없음: 네이티브 코드를 호출하는 것이 항상 성능 향상을 가져올 것이라는 보장은 없습니다. 오히려 네이티브 호출로 인해 오버헤드가 발생할 수도 있습니다.
- 디버깅 어려움: 네이티브 코드와의 통합은 디버깅을 어렵게 만들 수 있습니다. 자바 코드와는 달리 네이티브 코드는 더 낮은 수준에서 동작하기 때문에 디버깅이 어려울 수 있습니다.

### 활용 가능한 영역
1. 시스템 프로그래밍: 네이티브 라이브러리를 직접 호출하여 운영 체제 수준의 기능을 활용할 수 있습니다. 예를 들어, 파일 시스템 액세스, 네트워킹, 운영 체제의 시스템 호출 등을 처리할 수 있습니다.
2. 프로세스 간 통신 (IPC): 네이티브 코드를 사용하여 다른 프로세스와의 통신을 수행할 수 있습니다. 이를 통해 다른 언어로 작성된 프로세스와의 상호 작용이 가능해집니다.
3. 데이터베이스 및 하드웨어 인터페이스: 네이티브 라이브러리를 사용하여 데이터베이스에 접근하거나 하드웨어와의 상호 작용을 할 수 있습니다. 이를 통해 더욱 효율적인 데이터베이스 액세스 및 하드웨어 제어가 가능해집니다.
4. 게임 개발: 네이티브 코드를 사용하여 고성능 게임 엔진을 개발하거나, 그래픽 라이브러리를 활용할 수 있습니다. 이를 통해 더욱 현실적이고 높은 성능의 게임을 구현할 수 있습니다.
5. 빅데이터 및 과학적 컴퓨팅: 네이티브 코드를 사용하여 고성능 컴퓨팅 작업을 수행하거나, 대용량 데이터를 처리할 수 있습니다. 이를 통해 데이터 분석, 시뮬레이션, 모델링 등을 효율적으로 수행할 수 있습니다.

### 대안
java.nio.ByteBuffer, sun.misc.Unsafe, JNI(Java Native Interface), 다른 서드파티 프레임워크 
