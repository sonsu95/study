
Spring에서 Data Binding에 대한 개념을 찾아보던 도중 공식 문서에서 Data Binding이 JavaBeans 규칙을 따른다는 내용을 발견했다.  JavaBeans 규칙이 무엇인지, 그리고 Data Binding 개념이 왜 해당 규칙을 따르고 있다고 적혀있는 것인지 이해하기 위해 해당 문서를 작성


# What is JavaBeans?

JavaBeans라는 개념은 무엇인가?

JavaBeans는 Java 프로그래밍 언어에서 사용되는 일종의 표준 규격을 의미한다.

일부에서는 reusable software component model (재사용 가능한 소프트웨어 모델) 이라는 표현으로 JavaBeans를 표현된다. 

%% 다만 JavaBeans는 하나의 개념으로 이해되어야 하기에, 실제 구현체는 해당 개념과 상이할수도 있다는 점을 이해하여야 한다.  %%

JavaBeans 규칙은 여러가지로 나뉘는데 우선 기본적으로는 
1. 모든 속성은 private으로 처리되어야 한다
2. 기본 생성자가 존재하여야 한다
3. Serializable 인터페이스를 구현하여야 한다. 
라는 3가지 원칙을 따른다. 

## 모든 속성은 private으로 처리되어야 한다. 

해당 원칙은 객체지향 프로그래밍의 캡슐화 원칙을 따른다. 
속성을 private 으로 선언하게 되면 클래스 외부에서 해당 속성으로 직접 접근할 수 없게 되며, 이를 통해 데이터의 무결성과 보안이 유지된다고 볼 수 있다. 

이 경우 해당 데이터에 접근하기 위해 public으로 선언된 getter와 setter를 사용해야한다. 

## 기본 생성자가 존재하여야 한다. 

### 기본 생성자와 자바의 동작

우선 자바에서 기본 생성자 관련 동작이 어떻게 수행되고 있는지 살펴보자. 

기본적으로 자바에서는 아무런 생성자 표시를 해주지 않은 경우, 기본 생성자를 자동으로 생성해준다. 

```java
class UseConstructor { 
	int value; 
	
	UseConstructor(int value) { 
		this.value = value; 
	} 
}
```

위와 같이 기본 생성자를 만들지 않았을 경우.  `new UseConstructor()` 은 실행될 수 없다. 직접 우리가 생성한 `new UseConstructor(10)` 만 가능하다. 

```java
class NoDefaultConstructor {
	int value; 
}
```

위와 같은 방식으로 생성자 자체가 없는 경우에도 `new NoDefaultConstructor()` 이 실행된다. 이는 생성자 선언을 해주지 않았을 경우 자바 컴파일러에서 자동으로 기본 생성자를 생성해주기 때문이다.

위와 같은 동작방식에서 UseConstructor 클래스를 사용하는 경우라고 하더라도 JavaBeans 규칙을 따른다면 
```java
class UseConstructor { 
	int value; 
	
	UseConstructor() {
	}
	
	UseConstructor(int value) { 
		this.value = value; 
	} 
}
```

이렇게 기본 생성자를 가능한 만들어주어야 한다. 

### JavaBeans에서 기본 생성자를 필요로 하는 이유
#### 재사용성
기본 생성자가 있으면 개발자는 별도의 초기화 매개변수 없이 언제든지 Bean 인스턴스를 생성할 수 있습니다. 이는 특히 다양한 설정, 상황 또는 컨텍스트에서 Bean을 유연하게 재사용할 수 있게 만들어 줍니다.

#### 프로퍼티 설정 용이성
JavaBeans는 기본 생성자를 통해 객체를 생성한 후, setter 메서드를 통해 프로퍼티를 설정해줄 수 있다. 이는 Bean이 복잡한 초기화 로직을 갖지 않아도 쉽게 설정될 수 있도록 해준다. 

#### 프레임워크와의 호환성
많은 자바 기반 프레임워크 및 도구들은 리플렉션 API를 사용하여 객체를 동적으로 생성하고 관리한다. Spring JPA 같은 프레임워크에서는 기본 생성자가 필수적으로 요구되며, 이는 프레임워크가 자동으로 객체를 인스턴스화 할 수 있게 해준다. 

---

# Reference 

 [What is a JavaBean exactly?](https://stackoverflow.com/questions/3295496/what-is-a-javabean-exactly)






----
# JavaBeans Conventions

## What is JavaBean?

JavaBean은 Java 클래스를 어떻게 작성할지에 대한 표준을 이야기한다. 즉, Java에서 사용하는 클래스들 중에서, 표준 규격에 맞게 작성된 클래스를 JavaBean이라고 부른다. 

JavaBean의 특징은 다음과 같다. 
1. 모든 Properties는 private 속성을 가진다.
2. 기본 생성자를 지녀야 한다. 
3. Serializable을 implement 해야한다. 

## 모든 Properties는 private 속성을 가진다.

JavaBean의 properties는 보통 private 접근 제한자를 통해 클래스 외부에서 직접 접근할 수 없도록 처리한다.
따라서 public 접근이 가능한 getter나 setter를 별도로 설정해주어야 한다. 

## 기본 생성자를 지녀야 한다. 

## Serializable을 implement 해야한다. 

JavaBean에 해당하는 클래스를 예시로 들자면 다음과 같다

```java
import java.io.Serializable;

public class Person implements Serializable {
    private static final long serialVersionUID = 1L;  // 직렬화에 사용되는 버전 관리 UID
    
    private String name;  // 이름 속성
    private int age;      // 나이 속성

    // 기본 생성자
    public Person() {
    }

    // 매개변수가 있는 생성자 - 이름과 나이를 초기화합니다.
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // 이름에 대한 게터 메서드
    public String getName() {
        return name;
    }

    // 이름에 대한 세터 메서드
    public void setName(String name) {
        this.name = name;
    }

    // 나이에 대한 게터 메서드
    public int getAge() {
        return age;
    }

    // 나이에 대한 세터 메서드
    public void setAge(int age) {
        this.age = age;
    }
}
```

모든 속성은 기본적으로 private



## Property Accessor Methods

- JavaBean은 private 필드에 값을 설정하고 접근하기 위한 public accessor 메서드(getter와 setter)를 가져야 한다. 이는 캡슐화 원칙을 준수한다.
- 메서드 이름은 일반적으로 `get`이나 `set`으로 시작하며, 그 뒤에는 속성 이름의 첫 글자를 대문자로 시작하는 이름이 붙는다.
- Boolean 속성의 경우, `get` 대신 `is`를 사용할 수도 있다.

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


---

## Reference

예시는 GPT4 기반으로 작성되었습니다. 

https://www.codecademy.com/resources/docs/java/javabeans

https://velog.io/@dion/what-is-javabeans-and-why-use-javabeans

https://docs.oracle.com/cd/E19159-01/819-3669/bnais/index.html

https://docstore.mik.ua/orelly/java-ent/jnut/ch06_02.htm