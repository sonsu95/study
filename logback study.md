
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



## Architecture

logback 종류
1. logback-core
2. logback-classic
3. logback-access