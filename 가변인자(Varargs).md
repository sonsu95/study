

```java
public List createMembers(String... names) {  
    return Stream.of(names)  
        .map(this::createMember)  
        .collect(Collectors.toList());  
}
```

## 개요
- 위 방식처럼 `String...`으로 처리되는 것을 가변 인자(Varargs)라고 한다.
- 가변 인자는 "0개 이상의 문자열을 받을 수 있다"는 의미로 이해할 수 있다.
  - "여러 개"뿐만 아니라 "아무 것도 없는 경우"도 포함한다.

## String[]과의 차이점
1. 컴파일러 처리:
   - 가변 인자(`String...`)는 런타임 환경에서 컴파일러가 자동으로 배열로 변환한다.
   - `String[]`은 처음부터 배열로 정의된다.

2. 메서드 호출 방식:
   - 가변 인자: `createMembers("a", "b", "c")` 또는 `createMembers()`와 같이 호출 가능
   - 배열: `createMembers(new String[]{"a", "b", "c"})` 형태로 호출해야 함

3. null 처리:
   - 가변 인자: null을 직접 전달할 수 없으며, 전달 시 컴파일러 경고와 런타임 예외 발생
   - 배열: null을 전달할 수 있어 NullPointerException 발생 가능성 있음

4. 빈 입력 처리:
   - 가변 인자: 인자가 없을 때 자동으로 빈 배열로 처리됨
   - 배열: 빈 배열을 명시적으로 전달해야 함 (예: `new String[0]`)

## 주의사항
- 가변 인자는 메서드의 마지막 매개변수로만 사용 가능
- 하나의 메서드에는 하나의 가변 인자만 사용 가능

## 사용 이점
- 메서드 오버로딩을 줄일 수 있다.
- 호출 시 더 유연하고 직관적인 문법을 제공.
- null 처리에 대한 안전성을 높일 수 있음.