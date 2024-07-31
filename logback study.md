
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

### 로그백 환경 구성 방식

로그백은 3가지 주요 클래스를 기반으로 구축됨. 

Logger, Appender, Layout 인터페이스

Logger 클래스는 logback-classic 모듈의 일부
Appender, Layout 클래스틑 logback-core 모듈의 일부

logback-core 모듈의 경우 logger이라는 개념을 가지고 있지 않는다.

logger는 이름이 붙여진 엔티티로 볼 수 있는데, 이는 대소문자를 구분하여 계층적인 명명 규칙을 따른다. 


## Architecture

logback 종류
1. logback-core
2. logback-classic
3. logback-access