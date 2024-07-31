
Logback은 추상화 모듈로서, 단독으로서는 사용이 불가능한 SLF4J 모듈의 구현체이다. 

https://www.slf4j.org/manual.html 해당 페이지에서 
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class HelloWorld {

  public static void main(String[] args) {
    Logger logger = LoggerFactory.getLogger(HelloWorld.class);
    logger.info("Hello World");
  }
}
```

이런 예시코드를 두고 실행시키고 있다. 

그런데 해당 코드를 실행시키면 

```terminal
SLF4J: No SLF4J providers were found.
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See https://www.slf4j.org/codes.html#noProviders for further details.
```

이런 에러가 발생한다고 이야기한다. 

해당 에러가 발생하는 원인에 대해  `no slf4j provider (or binding) could be found on your class path.` 이렇게 설명한다. 

즉, slf4j는 단독으로 실행시킬 수 없는 인터페이스이고, 별도의 구현체가 필요하다는 소리인데 

그러면서 하는 소리가 `need to obtain slf4j artifacts` 인데, 이 말은 slf4j의 아티팩트, 즉 구현체가 필요하다는 말이다. 



---

Logback은 자바에서 사용되는 로깅 프레임워크로, Log4j의 단점을 개선하여 개발되었다. Logback은 성능이 우수하고, 유연하며, 설정이 용이하여 자바 애플리케이션에서 표준 로깅 인터페이스로 널리 사용되고 있다. 또한, SLF4J(Simple Logging Facade for Java)라는 로깅 파사드의 기본 구현체 중 하나로, 다양한 로깅 프레임워크와의 호환성을 제공한다. Logback은 Spring Framework의 기본 로깅 프레임워크로 사용될 만큼 스프링과의 통합이 잘 이루어져 있다.

## 특징
Logback은 모듈화된 구조를 가지고 있으며, 각 컴포넌트는 고유한 역할을 수행하도록 설계되었다. 주요 특징은 다음과 같다:

**다양한 로그 레벨 지원**
• TRACE, DEBUG, INFO, WARN, ERROR의 다섯 가지 로그 레벨을 지원하여 로그의 중요도에 따라 세분화된 기록이 가능하다.
• 개발 중 디버깅 로그와 운영 중 중요한 로그를 구분할 수 있다.

**높은 성능**
• Logback은 동기화 비용을 최소화하고 효율적인 버퍼링을 통해 높은 성능을 제공한다.
• 대용량 로그를 처리할 때도 안정적인 성능을 유지한다.

**유연한 설정**
• XML 및 Groovy를 사용한 유연한 설정이 가능하며, 설정 파일을 통해 로그의 출력 형식, 레벨, 대상 등을 쉽게 관리할 수 있다.
• 설정 변경 시 애플리케이션 재시작 없이 실시간으로 반영할 수 있다.

**강력한 통합 기능**
• Spring Framework와의 긴밀한 통합을 제공하여 스프링 애플리케이션에서 쉽게 사용할 수 있다.
• JMX를 통한 모니터링과 관리 기능을 제공한다.

**다양한 Appender 지원**
• 콘솔, 파일, 데이터베이스, 원격 서버 등 다양한 로그 출력 대상을 지원하는 Appender를 제공한다.
• 사용자 정의 Appender를 작성하여 특정 요구 사항에 맞게 로그를 처리할 수 있다.

**레이아웃과 인코더**
• 로그 메시지의 형식을 지정할 수 있는 다양한 레이아웃과 인코더를 제공한다.
• 패턴 레이아웃을 사용하여 로그 메시지의 형식을 자유롭게 정의할 수 있다.

**필터링 기능**
• 로그 이벤트를 필터링하여 특정 조건에 맞는 로그만 기록할 수 있다.
• 필터를 사용하여 로그의 노이즈를 줄이고 필요한 로그만 남길 수 있다.

**호환성**:
• SLF4J와의 호환성을 통해 다른 로깅 프레임워크로의 전환이 용이하다.
• 기존 Log4j 1.x 설정 파일을 쉽게 변환하여 사용할 수 있다.


## Structure

 

## Logback의 주요 특징
1. 모듈화된 구조
2. 효율적인 성능
3. 다양한 로그레벨 지원
4. 유연한 로그 설정
5. 다양한 Appender 지원
6. 동적 로그 레벨 조절 가능
7. 패턴 기반의 로그 포매팅 가능
8. 확장성