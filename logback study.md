



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

1. Logger
	- Logger는 로그백의 logback-classic 모듈에 포함되어 있음
	- 로거는 로거 이벤트를 생성하는 주체
	- 로거는 이름을 가지며, 이름을 대소문자로 구분
	- 로거 이름은 점(.)을 통해 계층적으로 구분. 예를 들어, com.foo는 com.foo.Bar의 부모 로거
2. Appender
	- 
3. Layout