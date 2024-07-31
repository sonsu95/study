



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
- logback-classic
	- SLF4J의 구현체
	- Logger과의 상호작용을 관리.
	- 해당 모듈에서 Logger 개념을 사용되며, Logger을 구현
- logback-core
	- 로깅의 기반이 되는 라이브러리. 직접적으로 Logger 개념을 가지고 있지는 않음. Appender, Layout이 구현됨. 