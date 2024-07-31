



## Introduction


### 기본 동작 방식
```java
package chapters.introduction;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class HelloWorld1 {

  public static void main(String[] args) {
	// set logger name
    Logger logger = LoggerFactory.getLogger("chapters.introduction.HelloWorld1");
    // set logger message
    logger.debug("Hello world.");

  }
}
```


## Architecture

### 로그백의 구조 및 주요 클래스

1. Logger
	- Logger는 logback-classic 모듈에 포함되어 있음
	- 로거는 로거 이벤트를 생성하는 주체
	- 로거는 이름을 가지며, 이름을 대소문자로 구분
	- 로거 이름은 점(.)을 통해 계층적으로 구분. 예를 들어, `com.foo`는 `com.foo.Bar`의 부모 로거
2. Appender
	- Appender는 logback-core 모듈의 일부
	- Appender는 로그 이벤트를 적절한 목적지(파일, 콘솔, 네트워크 등)에 출력하는 역할을 실행
	- 다양한 Appender를 조합하는 방식으로 로그 출력을 유연하게 관리 가능
3. Layout
	- Layout은 logback-core 모듈의 일부
	- 로그 메세지의 최종 출력 형식을 결정. 예를 들면, 로그 이벤트의 시간, 로거 이름, 메시지 등을 포함한 특정 형식을 설정 가능.

### 모듈 구조
- **logback-classic**
	- SLF4J의 구현체로,  Java에서 로깅을 위한 표준 인터페이스를 제공. 
	- 이 모듈은 Logger 객체와의 상호작용을 관리하며, 로거를 직접 구현하고 조작하는 기능을 제공.
	- Logger 인터페이스를 통해 로그 메시지를 다양한 레벨로 관리하고, 필요에 따라 로그 출력을 조절 가능.
- **logback-core**
	- logback 라이브러리의 핵심을 구성하며, 기본적인 로깅 기능과 인프라를 제공. 
	- 직접적으로 Logger 개념을 포함하지는 않지만, 로그 메시지를 처리하고 적절한 출력 형식과 목적지로 전송하는 데 필요한 Appender와 Layout 클래스를 구현. 
	- Appender는 로그 메시지를 콘솔, 파일, 네트워크 서비스 등 다양한 출력 대상에 보낼 수 있는 구성 요소이며, Layout은 로그 메시지의 최종 형식을 결정하는 역할을 한다.

### 로거 이름의 계층적 명명 규칙
• 로거의 이름은 계층적으로 구성될 수 있으며, 이 계층 구조는 로그의 분류 및 필터링에 활용된다.
• 한 로거가 다른 로거의 조상이 되려면, 그 이름이 후손 로거의 이름에 접두어로 포함되어야 하며 사이에 다른 조상이 없을 때, 바로 다음 하위 로거를 자식 로거라고 한다.