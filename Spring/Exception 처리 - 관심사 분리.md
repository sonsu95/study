스프링을 처음 배우면서 exception에 대한 처리는 기본적으로 
1. 공통 처리
2. 상세 처리
이 두가지를 기준으로 생각하였었다. 

그 결과 공통 처리를 위해 GlobalExceptionHandler에서 Exception의 종류별로 정의한 후, 각 Exception에 HttpsStatus 코드를 맞춰서 정리하였다. 

그 결과 GlobalExceptionHandler 클래스는 매우 거대해졌으며, 예상치 못한 에러에 대해서는 무조건적으로 500을 리턴하는 결과를 가져왔다. 

즉, 사용하는 서비스단에서는 400에러에 대한 리턴을 예상하고 GlobalExceptionHandler에 정의해 둔 클래스를 사용했는데, 500에러를 리턴한다던가 하는 상황이 빈번하게 발생했다. 
~~(사실 그냥 내가 코드를 잘못짠거같긴 하다)~~

```java
@ControllerAdvice  
public class GlobalExceptionHandler {  
  
    // 400 Bad Request  
    @ExceptionHandler({MethodArgumentTypeMismatchException.class, MethodArgumentNotValidException.class})  
    public ResponseEntity<String> handleBadRequestException(Exception ex) {  
        return ResponseEntity.status(HttpStatus.BAD_REQUEST)  
                .body("잘못된 요청입니다: " + ex.getMessage());  
    }  
    // 404 Not Found  
    @ExceptionHandler({EntityNotFoundException.class, NoSuchElementException.class})  
    public ResponseEntity<String> handleNotFoundException(Exception ex) {  
        return ResponseEntity.status(HttpStatus.NOT_FOUND)  
                .body("요청한 리소스를 찾을 수 없습니다: " + ex.getMessage());  
    }  
  
    // 405 Method Not Allowed  
    @ExceptionHandler(HttpRequestMethodNotSupportedException.class)  
    public ResponseEntity<String> handleMethodNotAllowedException(HttpRequestMethodNotSupportedException ex) {  
        return ResponseEntity.status(HttpStatus.METHOD_NOT_ALLOWED)  
                .body("지원하지 않는 HTTP 메소드입니다: " + ex.getMessage());  
    }  
  
    // 500 Internal Server Error  
    @ExceptionHandler(DataAccessException.class)  
    public ResponseEntity<String> handleDataAccessException(DataAccessException ex) {  
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)  
                .body("데이터베이스 오류가 발생했습니다: " + ex.getMessage());  
    }  
  
    // 그 외 모든 예외  
    @ExceptionHandler(Exception.class)  
    public ResponseEntity<String> handleAllException(Exception ex) {  
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)  
                .body("서버 내부 오류가 발생했습니다: " + ex.getMessage());  
    }  
}
```

예시를 들면 다음과 같다. 
Spring MVC를 통해 Tymeleaf를 사용하는 과정에서, 클라이언트에서 POST 요청으로 특정 body를 보내주는데, 필드가 누락된 경우. 그런 경우에는 자동으로 잡아서 400 에러를 리턴하고, 메시지만 핸들링하려는 것이 목적이었는데, 계속 에러코드 자체가 500으로 잡혔다. 

정말로 예상치 못한 경우에는 500으로 내려야하는게 맞지만, 예상한 케이스에 대해서도 잘못된 에러 상태 코드가 내려가다보니, 이러한 상황을 방지하기 위해 커스텀 Exception의 핸들러를 만들어 매번 직접 사용하는 방식으로 변경을 시도했다. 

```java
@ControllerAdvice  
public class GlobalExceptionHandler {  
  
    @ExceptionHandler(BaseException.class)  
    public ResponseEntity<ErrorResponse> handleBaseException(BaseException e) {  
        ErrorResponse errorResponse = new ErrorResponse(e.getErrorCode());  
        return new ResponseEntity<>(errorResponse, HttpStatus.valueOf(e.getErrorCode().getCode()));  
    }  
  
}
```

```java
// BaseException.java
@Getter  
public class BaseException extends RuntimeException {  
  private final ErrorCode errorCode;  
  
  public BaseException(ErrorCode errorCode) {  
    super(errorCode.getMessage());  
    this.errorCode = errorCode;  
  }  
  
}
```

```java
// ErrorResponse.java, DTO
@Getter  
public class ErrorResponse {  
  private final LocalDateTime timestamp;  
  private final int code;  
  private final String message;  
  
  public ErrorResponse(ErrorCode errorCode) {  
    this.timestamp = LocalDateTime.now();  
    this.code = errorCode.getCode();  
    this.message = errorCode.getMessage();  
  }  
}
```

```java
// ErrorCode.java, Enum
@Getter  
public enum ErrorCode {  
  // 공통 오류  
  INTERNAL_SERVER_ERROR(HttpStatus.INTERNAL_SERVER_ERROR, "서버 내부 오류가 발생했습니다."),  
  INVALID_INPUT_VALUE(HttpStatus.BAD_REQUEST, "잘못된 입력값입니다."),  
  
  // 게시글 관련 오류  
  NOT_FOUND_POST(HttpStatus.NOT_FOUND, "게시글을 찾을 수 없습니다."),  
  UNAUTHORIZED_POST_ACCESS(HttpStatus.FORBIDDEN, "게시글에 대한 권한이 없습니다."),  
  
  // 사용자 관련 오류  
  NOT_FOUND_USER(HttpStatus.NOT_FOUND, "사용자를 찾을 수 없습니다."),  
  DUPLICATE_USER_EMAIL(HttpStatus.CONFLICT, "이미 사용 중인 이메일입니다."),  
  
  MISSING_REQUIRED_FIELDS(HttpStatus.BAD_REQUEST, "필수 필드가 누락되었습니다.");  
  
  private final int code;  
  private final String message;  
  
  ErrorCode(HttpStatus code, String message) {  
    this.code = code.value();  
    this.message = message;  
  }  
}
```

```java
// CustomException
public class MissingRequiredFieldsException extends BaseException {  
  
  public MissingRequiredFieldsException() {  
    super(ErrorCode.MISSING_REQUIRED_FIELDS);  
  }  
  
}
```

그리고 이를 사용하려고하면 단순히 생성한 Custom Exception을 사용해주면 된다. 

```java
@Override  
public User signupUser(User user) {  
  if (user.getEmail() == null || user.getEmail().isEmpty() ||  
      user.getPassword() == null || user.getPassword().isEmpty()) {  
    throw new MissingRequiredFieldsException();  
  }  
  return user;  
}
```

그러면 이렇게 에러가 잘 나타난다
```json
{
    "timestamp": "2024-07-18T02:07:07.644499",
    "code": 400,
    "message": "필수 필드가 누락되었습니다."
}
```