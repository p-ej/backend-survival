# Separation of Concerns


### IOException
Java에서 'IOException'은 입출력(I/O) 오류 또는 예외를 나타내는 확인된 예외입니다. `java.io` 패키지의 일부이며 파일, 네트워크 연결 또는 기타 I/O 작업에서 읽거나 파일에 쓰는 동안 오류가 발생할 때 발생합니다.

`IOException`은 `Exception` 클래스의 하위 클래스로 I/O 관련 오류에 대한 일반적인 예외 유형입니다. 발생한 특정 유형의 I/O 오류에 대한 자세한 정보를 제공하는 `FileNotFoundException`, `SocketException` 및 `EOFException`과 같은 보다 구체적인 I/O 관련 예외 클래스의 기본 클래스 역할을 합니다.

`IOException`이 발생할 수 있는 몇 가지 일반적인 시나리오는 다음과 같습니다.

1. 파일 I/O 오류: 파일을 읽거나 쓸 때 파일이 존재하지 않거나(`FileNotFoundException`), 권한이 부족하거나, 파일 시스템.

2. 네트워크 I/O 오류: 네트워크 연결로 작업할 때 연결 끊김, 시간 초과 또는 기타 네트워크 관련 문제와 같은 네트워크 통신 문제가 있는 경우 'IOException'이 발생할 수 있습니다.

3. 입/출력 오류: 'IOException'은 스트림, 버퍼 또는 기타 I/O 관련 리소스에서 읽거나 쓰는 것과 같은 다양한 입/출력 작업을 수행할 때 발생할 수 있습니다. 예를 들면 파일 끝에 도달(`EOFException`), 잘못된 형식의 데이터 형식 발생 또는 입력 또는 출력 스트림 문제가 있습니다.

'IOException' 처리에는 일반적으로 try-catch 블록을 사용하여 예외를 포착하고 사용자에게 오류 메시지 표시, 오류 기록 또는 수정 조치와 같은 적절한 오류 처리 또는 복구 조치를 수행하는 것이 포함됩니다. 경우에 따라 적절한 정리를 위해 예외를 처리하기 전에 열려 있는 리소스(예: 파일 스트림)를 닫아야 할 수 있습니다.

`IOException`은 확인된 예외이므로 `throws` 절을 사용하여 메서드 서명에서 명시적으로 포착하거나 선언해야 합니다. 이렇게 하면 호출 코드가 I/O 오류의 가능성을 인식하고 그에 따라 오류를 처리할 수 있습니다.

'IOExceptions'를 적절하게 포착하고 처리함으로써 Java 프로그램은 다양한 I/O 관련 오류를 정상적으로 처리하고 복구하여 소프트웨어의 견고성과 안정성을 향상시킬 수 있습니다.


### 
Java에서 `FileOutputStream`은 `java.io` 패키지에서 제공하는 클래스로 데이터를 바이트 시퀀스로 파일에 쓸 수 있습니다. 출력 작업, 특히 원시 이진 데이터 또는 바이트로 인코딩된 텍스트 데이터를 파일에 쓰는 데 사용됩니다.

`FileOutputStream`을 사용하려면 클래스의 인스턴스를 만들고 파일 경로 또는 쓰려는 파일을 나타내는 `File` 개체를 지정해야 합니다. 그런 다음 `FileOutputStream` 클래스에서 제공하는 다양한 메서드를 사용하여 파일에 데이터를 쓸 수 있습니다.

다음은 `FileOutputStream`을 사용하여 파일에 데이터를 쓰는 방법을 보여주는 기본 예제입니다.
```java
import java.io.FileOutputStream;
import java.io.IOException;

public class FileOutputStreamExample {
    public static void main(String[] args) {
        String filePath = "output.txt";  // Specify the file path

        try (FileOutputStream fos = new FileOutputStream(filePath)) {
            String data = "Hello, World!";  // Data to be written

            // Convert the data to bytes and write to the file
            byte[] bytes = data.getBytes();
            fos.write(bytes);

            System.out.println("Data has been written to the file.");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

위의 예에서는 파일 경로가 "output.txt"인 `FileOutputStream` 인스턴스를 만듭니다. 그런 다음 문자열 "Hello, World!"를 변환합니다. `getBytes()` 메서드를 사용하여 바이트로 변환하고 `FileOutputStream`의 `write()` 메서드를 사용하여 파일에 바이트를 씁니다. 마지막으로 try-with-resources 구성을 사용하여 `FileOutputStream`을 닫습니다. 그러면 예외가 발생하더라도 리소스가 제대로 닫힙니다.

`FileOutputStream`은 `write(byte[] b)`, `write(byte[] b, int off, int len)` 및 `write(int b)`와 같이 파일에 데이터를 쓰기 위한 다양한 기타 메서드를 제공합니다. . 이러한 메서드를 사용하면 바이트 배열 또는 개별 바이트와 같은 다양한 유형의 데이터를 파일에 쓸 수 있습니다.

`FileOutputStream`은 이미 존재하는 파일의 내용을 덮어쓴다는 점에 유의해야 합니다. 기존 파일에 데이터를 추가하려면 `new FileOutputStream(filePath, true)`과 같이 두 번째 매개변수를 `true`로 설정하여 `FileOutputStream` 인스턴스를 만들 수 있습니다.


###
Spring과 Mockito에서 'verify' 메서드는 모의 객체나 종속성을 실행하는 동안 특정 동작이 발생했는지 확인하는 데 사용됩니다. 모의 개체의 특정 메서드가 예상 인수 및/또는 특정 횟수로 호출되었음을 확인하기 위해 단위 테스트에서 일반적으로 사용됩니다.

`verify` 메서드는 Java용 모킹 프레임워크로 유명한 Mockito 프레임워크의 일부입니다. Mockito는 모의 개체를 만들고 해당 동작을 스텁하여 구성 요소를 격리된 테스트를 허용하는 방법을 제공합니다.

다음은 Mockito에서 `verify` 메서드의 기본 구문입니다.

```java
verify(mockedObject, verificationMode).methodName(expectedArguments);
```

매개변수를 분석해 보겠습니다.

- `mockedObject`: Mockito를 사용하여 모의 객체로 만든 객체입니다. 확인하려는 종속성 또는 협력자를 나타냅니다.
- `verificationMode`: 검증이 얼마나 엄격하게 수행되는지를 결정하는 검증 모드를 지정합니다. 몇 가지 일반적인 확인 모드에는 예상되는 호출 수를 지정하는 `times(n)`, 메소드가 적어도 한 번 호출되어야 함을 지정하는 `atLeastOnce()` 또는 메소드가 절대 호출되지 않도록 하는 `never()`가 포함됩니다. .
- `methodName`: 확인하려는 메서드의 이름입니다.
- `expectedArguments`: 메서드에 전달되어야 하는 예상 인수입니다. 인수를 확인하지 않으려면 이 매개변수를 생략할 수 있습니다.

다음은 Mockito에서 `verify`의 사용법을 설명하는 예입니다.

```java
import org.junit.jupiter.api.Test;
import org.mockito.Mock;
import org.mockito.Mockito;

import static org.mockito.Mockito.verify;

class ExampleTest {
    
    @Mock
    private SomeDependency mockedDependency;
    
    @Test
    void testMethod() {
        // ... code under test ...
        
        // Verify that a method was called once with the expected argument
        verify(mockedDependency, Mockito.times(1)).someMethod("expectedArgument");
    }
}

```

위의 예에서는 `@Mock` 주석을 사용하여 `SomeDependency` 클래스의 모의 객체를 만듭니다. 테스트 메서드 내에서 몇 가지 작업을 수행한 다음 `verify`를 사용하여 조롱된 종속성의 `someMethod` 메서드가 `"expectedArgument"` 인수를 사용하여 정확히 한 번 호출되었는지 확인합니다.

Mockito에서 `verify`를 사용하면 모킹된 객체와의 특정 상호 작용이 예상대로 발생했다고 어설션할 수 있으므로 단위 테스트 중에 코드의 동작을 확인할 수 있습니다.


### 
Spring Testing 프레임워크에서 `@SpyBean`과 `@MockBean`은 모두 단위 테스트에서 의존성의 테스트 더블(모의)을 생성하고 관리하는 데 사용되는 주석입니다. 그러나 목적과 동작이 다릅니다.

1. `@MockBean`:
    - `@MockBean`은 테스트 중인 응용 프로그램에서 일반적으로 사용되는 bean 또는 종속성의 모의 개체를 만드는 데 사용됩니다.
    - 실제 빈 구현을 모의 구현으로 대체하여 Mockito 또는 다른 모의 프레임워크를 사용하여 모의 동작을 정의할 수 있습니다.
    - `@MockBean`은 일반적으로 실제 빈을 모의 구현으로 대체하여 테스트 중에 해당 빈의 동작을 격리하고 제어하려는 경우에 사용됩니다.

2. `@스파이빈`:
    - `@SpyBean`은 bean 또는 종속성의 스파이 객체를 생성하는 데 사용됩니다.
    - 스파이 객체는 실제 객체를 감싸는 부분 모의 객체입니다. 래핑된 개체의 원래 구현을 유지하지만 특정 메서드를 선택적으로 재정의하거나 확인할 수 있습니다.
    - `@SpyBean`은 일반적으로 실제 빈을 테스트하고 싶지만 나머지는 그대로 유지하면서 동작의 특정 부분을 수정하거나 확인해야 할 때 사용됩니다.

요약하면 `@MockBean`과 `@SpyBean`의 주요 차이점은 `@MockBean`은 조롱되는 빈을 모의 구현으로 완전히 대체하는 반면 `@SpyBean`은 실제 빈을 래핑하고 선택적으로 재정의하거나 검증할 수 있도록 합니다. 특정 방법.

차이점을 설명하는 예는 다음과 같습니다.

```java
@Service
public class ExampleService {
    public String methodToMock() {
        return "Real implementation";
    }
}

```

```java
@SpringBootTest
class ExampleTest {
    
    @Autowired
    private ExampleService exampleService;
    
    @MockBean
    private SomeDependency mockDependency;
    
    @SpyBean
    private ExampleService spyService;
    
    @Test
    void testMockBean() {
        Mockito.when(mockDependency.someMethod()).thenReturn("Mocked implementation");
        
        String result = exampleService.methodToMock();
        
        assertEquals("Mocked implementation", result);
    }
    
    @Test
    void testSpyBean() {
        Mockito.doReturn("Overridden implementation").when(spyService).methodToMock();
        
        String result = spyService.methodToMock();
        
        assertEquals("Overridden implementation", result);
    }
}
```

`testMockBean` 테스트 메소드에서 `@MockBean`을 사용하여 실제 bean을 대체하는 `SomeDependency` 모의를 생성합니다. 그런 다음 Mockito를 사용하여 모의 동작을 정의하여 `someMethod`가 호출될 때 특정 값을 반환할 수 있도록 합니다.

`testSpyBean` 테스트 메서드에서 `@SpyBean`을 사용하여 실제 빈을 래핑하는 `ExampleService`의 스파이 개체를 만듭니다. 특정 값을 반환하기 위해 Mockito를 사용하여 `methodToMock`의 동작을 선택적으로 재정의합니다.

Spring 테스트에서 `@MockBean` 및 `@SpyBean`을 사용하면 모의 개체 또는 부분 모의를 쉽게 생성하고 조작하여 종속성의 동작을 제어하고 확인하여 효과적인 단위 테스트를 용이하게 할 수 있습니다.

## 학습 키워드
- IOException
- `FileOutputStream`
- Mockito  `verify`
- @SpyBean 과 @MockBean 차이