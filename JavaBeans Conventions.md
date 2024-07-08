
# JavaBeans Conventions

Spring MVC Data Binding 개념을 찾아보면서 본 내용.

JavaBeans는 Java의 디자인 패턴을 준수하여 여러 객체를 단일 객체로 캡슐화하는 클래스를 말합니다. JavaBean 규약은 객체를 재사용, 캡슐화, 자체 검사(introspection), 그리고 사용자 지정을 용이하게 하도록 설계된 일련의 프로그래밍 지침입니다.

## Property Accessor Methods

- JavaBean은 private 필드에 값을 설정하고 접근하기 위한 public accessor 메서드(getter와 setter)를 가져야 합니다. 이는 캡슐화 원칙을 준수합니다.
- 메서드 이름은 일반적으로 `get`이나 `set`으로 시작하며, 그 뒤에는 속성 이름의 첫 글자를 대문자로 시작하는 이름이 붙습니다.
- Boolean 속성의 경우, `get` 대신 `is`를 사용할 수 있습니다.

## Serializable Interface

- JavaBean은 `Serializable` 인터페이스를 구현함으로써 객체의 상태를 저장하고 나중에 복원할 수 있습니다. 이는 네트워크를 통한 객체의 전송을 가능하게 합니다.
- `Serializable` 구현은 필수는 아니지만, 객체가 상태를 유지하도록 지원하는 일반적인 관행입니다.

## Default Constructor

- JavaBean은 인자가 없는 생성자(default constructor)를 포함해야 합니다. 이는 런타임 중에 Bean을 쉽게 인스턴스화하고 구성할 수 있도록 하며, 특히 동적으로 Bean을 생성하고 관리하는 도구 및 프레임워크에서 중요합니다.

## Introspection

- 메서드에 대한 명명 규칙을 준수함으로써, Introspection(자체 검사)이 용이해집니다. 이는 Bean이 지원하는 속성, 이벤트, 메서드 등을 분석할 때 도움이 됩니다.

## Event Handling

- JavaBean은 bound 또는 constrained 속성을 지원할 수 있습니다. bound 속성은 값이 변경될 때 Observer 패턴을 사용하여 리스너에게 알립니다. constrained 속성은 제안된 변경이 부적절하다고 판단될 경우 그 변경을 거부할 수 있습니다.

## Example

```java
import java.io.Serializable;

// Serializable interface를 구현하여 객체의 상태를 저장 및 전송 가능하게 함
public class Student implements Serializable {
  
  // Serializable 인터페이스 구현 시 serialVersionUID 제공 권장
  private static final long serialVersionUID = 12345678L;

  // private 필드로 캡슐화 유지
  private String firstName;
  private String lastName;
  private String major;

  // Default constructor는 빈을 동적으로 인스턴스화하고 구성하는 데 필요
  public Student() {
  }

  // Overloaded constructor로 객체 초기화 지원
  public Student(String firstName, String lastName, String major) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.major = major;
  }

  // Getter 메서드로 속성 접근 제공
  public String getFirstName() {
    return this.firstName;
  }

  // Setter 메서드로 속성 값을 변경
  public void setFirstName(String firstName) {
    this.firstName = firstName;
  }

  // Getter 메서드로 속성 접근 제공
  public String getLastName() {
    return this.lastName;
  }

  // Setter 메서드로 속성 값을 변경
  public void setLastName(String lastName) {
    this.lastName = lastName;
  }

  // Getter 메서드로 속성 접근 제공
  public String getMajor() {
    return this.major;
  }

  // Setter 메서드로 속성 값을 변경
  public void setMajor(String major) {
    this.major = major;
  }
}

```