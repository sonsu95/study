
HTTP 요청 파라미터를 Java 객체로 변환하는 매커니즘. 

클라이언트로부터 받은 데이터를 서버측 객체와 자동으로 매핑하여 개발을 좀더 편리하게 만들어주는 역할



해당 개념에 대해 직접적으로 인지를 한 상황은 다음과 같다. 

```java
// entity
@Entity  
@Data  
public class Item {  
  
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)  
    private Long id;  
  
    @Column(nullable = false)  
    private String title;  
  
    @Column(nullable = false)  
    private BigDecimal price;  
  
    @CreationTimestamp  
    @Column(updatable = false)  
    private LocalDateTime createdAt;  
}

// controller
@GetMapping("/modify/{id}")
public String modify(@PathVariable Long id, Model model) {  
    Item item = itemRepository.findById(id).orElse(null);  
    if (item == null) {  
        System.out.println("에러발생");  
        return "redirect:/error";  
    }  
    model.addAttribute("item", item);  
    return "write";  
}
```

```html
<!-- 수정 폼 -->  
<form th:if="${item != null and item.id != null}"  
      th:action="@{'/modify/' + ${item.id}}"  
      method="post">  
    <label>
	    <span>제목</span>  
        <input name="title" th:value="${item.title}"/>  
    </label>    
    <label>
	    <span>가격</span>  
        <input name="price" th:value="${#numbers.formatInteger(item.price, 0)}"/>  
    </label>
    <button type="submit">수정</button>  
</form>
```

위와 같은 상황에서 

```java
@PostMapping("/modify/{id}")  
public String modify(@PathVariable Long id, @ModelAttribute Item item) {  
    System.out.println("itemId: " + item.getId());  
    if (!itemRepository.existsById(id)) {  
        return "redirect:/error";  
    }   
    itemRepository.save(item);  
    return "redirect:/list";  
}
```

이런 컨트롤러를 추가하였다. 

여기서 문제로 여겼던 부분은 HTML 폼에서 id 값을 명시적으로 전송하고 있지 않음에도 불구하고, 컨트롤러에서 받은 item 객체에 id 값이 정상적으로 설정되어 있다는 부분이었다. 

@PathVariable 을 통해 받은 id값을 직업 item 객체에 적용해주고 있지 않은데도 불구하고 itemRepository.save(item)이 정상적으로 동작하고 있는지 의문이 생겼다. 

---
관련 의문에 대해서는 Spring 문서에서 

