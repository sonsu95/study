
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
한 로거의 이름 뒤에 점 하나가 하위 로거 이름의 접두사인 경우, 그 로거는 다른 로거의 조상이라고 부를 수 있음. 자신과 하위 로거 사이에 조상이 없는 경우에는 해당 로거를 자식 로거의 부모라고 부름

그러니까 간단하게 말하자면, 로거에는 이름을 붙일 수 있고, 명명된 로거의 형식은 계층성을 띄는데, 그 계층성은 `.`을 통해 하위로거로 이동한다. 대충 하위객체 타고가는 느낌이랑 비슷하다. 

예를 들어, “com.foo”라는 로거는 “com.foo.Bar”라는 로거의 부모라고 할 수 있다. 


## Architecture

logback 종류
1. logback-core
2. logback-classic
3. logback-access