```java
public List<Member> createMembers(String... names) {  
  return Stream.of(names)  
      .map(this::createMember)  
      .collect(Collectors.toList());  
}
```

위 방식처럼 String... 으로 처리되는걸 가변인자라고 한다. 
가변인자는 정확히 말해 `여러개의 문자열을 받는다` 라는 의미로 이해할 수 있다. 

String[] 배열과 String... 는 거의 동일하게 처리되지만 

가변인자방식은 런타임 환경에서 컴파일러가 배열로 변화시키고, String[] 은 애초부터 배열이라는 차이가 있다. 
또한 `여러개의 문자열을 받는다`라는 의미처럼, 가변인자 방식으로 처리할 경우 null에 대한 처리가 안된다. 
즉, null이 들어올 가능성을 배제한 배열 형태로 이해해도 좋다. 

String... 을 사용하면 인자가 없을때는 자동으로 빈배열로 처리된다. 
String[]을 사용하는 경우에는 null을 전달할 수 있어서 NullPointException 에러의 가능성이 존재 

