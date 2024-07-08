
# JavaBeans Conventions

## What is JavaBean?

JavaBean은 Java 언어에서 사용되는 소프트웨어 컴포넌트 모델로, 특정한 컨벤션을 준수하는 클래스를 통해 객체의 재사용성과 캡슐화를 증진시키는 디자인 패턴입니다.

## Property Accessor Methods

- JavaBean은 private 필드에 값을 설정하고 접근하기 위한 public accessor 메서드(getter와 setter)를 가져야 합니다. 이는 캡슐화 원칙을 준수합니다.
- 메서드 이름은 일반적으로 `get`이나 `set`으로 시작하며, 그 뒤에는 속성 이름의 첫 글자를 대문자로 시작하는 이름이 붙습니다.
- Boolean 속성의 경우, `get` 대신 `is`를 사용할 수 있습니다.

```java
public class EncapsulatedStudent {
    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

## Serializable Interface

- JavaBean은 `Serializable` 인터페이스를 구현함으로써 객체의 상태를 저장하고 나중에 복원할 수 있습니다. 이는 네트워크를 통한 객체의 전송을 가능하게 합니다.
- `Serializable` 구현은 필수는 아니지만, 객체가 상태를 유지하도록 지원하는 일반적인 관행입니다.

```java
import java.io.Serializable;

public class SerializableStudent implements Serializable {
    private static final long serialVersionUID = 1L;
    private String name;
    private int age;
}
```

## Default Constructor

- JavaBean은 인자가 없는 생성자(default constructor)를 포함해야 합니다. 이는 런타임 중에 Bean을 쉽게 인스턴스화하고 구성할 수 있도록 하며, 특히 동적으로 Bean을 생성하고 관리하는 도구 및 프레임워크에서 중요합니다.

```java
public class StudentWithDefaultConstructor {
    private String name;
    private int age;

    public StudentWithDefaultConstructor() {
        
    }
}
```

## Introspection

- 메서드에 대한 명명 규칙을 준수함으로써, Introspection(자체 검사)이 용이해집니다. 이는 Bean이 지원하는 속성, 이벤트, 메서드 등을 분석할 때 도움이 됩니다.

```java
import java.beans.Introspector;
import java.beans.PropertyDescriptor;

public class IntrospectionExample {
    public static void main(String[] args) {
        try {
            // Introspector를 사용하여 Student 클래스의 BeanInfo를 얻습니다.
            Class<?> clazz = Student.class;
            for (PropertyDescriptor propertyDescriptor : Introspector.getBeanInfo(clazz).getPropertyDescriptors()) {
                // propertyDescriptor를 사용하여 속성 이름과 해당 getter 및 setter 메서드를 출력합니다.
                System.out.println("Property: " + propertyDescriptor.getName());
                if (propertyDescriptor.getReadMethod() != null) {
                    System.out.println("  Getter: " + propertyDescriptor.getReadMethod().getName());
                }
                if (propertyDescriptor.getWriteMethod() != null) {
                    System.out.println("  Setter: " + propertyDescriptor.getWriteMethod().getName());
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

// 기본 Student 클래스
class Student {
    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

## Event Handling

- JavaBean은 bound 또는 constrained 속성을 지원할 수 있습니다. bound 속성은 값이 변경될 때 Observer 패턴을 사용하여 리스너에게 알립니다. constrained 속성은 제안된 변경이 부적절하다고 판단될 경우 그 변경을 거부할 수 있습니다.

```java
import java.beans.PropertyChangeListener;
import java.beans.PropertyChangeSupport;

public class StudentWithEvents {
    private String name;
    private PropertyChangeSupport support;

    public StudentWithEvents() {
        support = new PropertyChangeSupport(this);
    }

    public void addPropertyChangeListener(PropertyChangeListener pcl) {
        support.addPropertyChangeListener(pcl);
    }

    public void setName(String name) {
        String oldName = this.name;
        this.name = name;
        support.firePropertyChange("name", oldName, name);
    }
}
```


---

## Reference

예시는 GPT4 기반으로 작성되었습니다. 

https://www.codecademy.com/resources/docs/java/javabeans

https://velog.io/@dion/what-is-javabeans-and-why-use-javabeans

https://docs.oracle.com/cd/E19159-01/819-3669/bnais/index.html

https://docstore.mik.ua/orelly/java-ent/jnut/ch06_02.htm