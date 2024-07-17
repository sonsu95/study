스프링을 처음 배우면서 exception에 대한 처리는 기본적으로 
1. 공통 처리
2. 상세 처리
이 두가지를 기준으로 생각하였었다. 

그 결과 공통 처리를 위해 GlobalExceptionHandler에서 Exception의 종류별로 정의한 후, 각 Exception에 HttpsStatus 코드를 맞춰서 정리하였다. 

그 결과 GlobalExceptionHandler 클래스는 매우 거대해졌으며, 예상치 못한 에러에 대해서는 무조건적으로 500을 리턴하는 결과를 가져왔다. 

즉, 사용하는 서비스단에서는 400에러에 대한 리턴을 예상하고 GlobalExceptionHandler에 정의해 둔 클래스를 사용했는데, 500에러를 리턴한다던가 하는 상황이 빈번하게 발생했다. 
(사실 그냥 내가 코드를 잘못짠거같긴 하다)

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