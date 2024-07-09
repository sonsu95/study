
Spring에서 Data Binding에 대한 개념을 찾아보던 도중 공식 문서에서 Data Binding이 JavaBeans 규칙을 따른다는 내용을 발견했다.  JavaBeans 규칙이 무엇인지, 그리고 Data Binding 개념이 왜 해당 규칙을 따르고 있다고 적혀있는 것인지 이해하기 위해 해당 문서를 작성


# What is JavaBeans?

JavaBeans라는 개념은 무엇인가?

JavaBeans는 Java 프로그래밍 언어에서 사용되는 일종의 표준 규격을 의미한다.

일부에서는 reusable software component model (재사용 가능한 소프트웨어 모델) 이라는 표현으로 JavaBeans를 표현된다. 

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
class NoDefaultConstructor {
	int value; 
}
```

위의 경우  `new NoDefaultConstructor()` 로 인스턴스를 생성할 수 있다. 생성자를 지정하지 않았음에도 인스턴스를 생성할 수 있는 이유는, 생성자를 지정하지 않은 경우 자바 컴파일러에서 자동으로 기본 생성자를 생성해주기 때문이다.

```java
class UseConstructor { 
	int value; 
	
	UseConstructor(int value) { 
		this.value = value; 
	} 
}
```

위와 같이 기본 생성자 대신 인자를 받는 다른 생성자를 만든 경우,  `new UseConstructor()` 은 실행될 수 없다. 직접 우리가 생성한 `new UseConstructor(10)` 만 가능하다. 

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

JavaBeans 규칙에서 기본 생성자를 필요로 하는 이유는 크게 4가지로 나눌 수 있다. 

1. 리플렉션을 통한 동적 객체 생성
2. 직렬화와 역직렬화
3. 객체 생성의 유연성

#### 리플렉션을 통한 동적 객체 생성
리플렉션 API를 필수로 사용해야하는 것은 아니지만, 많은 Java 프레임워크나 라이브러리에서 리플렉션 API를 자주 사용하는 편이다. 즉, 리플렉션 API를 사용하는 Java 프레임워크나 라이브러리는 JavaBeans 규칙을 따라야 한다는 의미라고 볼 수 있다. 

#### 직렬화와 역직렬화 (Serialization & Deserialization)

우선 자바에서 직렬화와 역직렬화에 대한 개념을 짚고 넘어가보면
직렬화는 자바 객체를 바이트 스트림으로 변환하여 파일이나 네트워크를 통해 저장하거나 전송할 수 있도록 하는 전처리 과정이고, 역직렬화는 바이트 스트림을 다시 자바 객체로 변환하는 과정을 이야기한다. 

보통 이같은 직렬화와 역직렬화 과정은
1. 영속성(persistence)
2. 네트워크 전송
3. 캐싱
같은 상황에서 자주 사용되는데, 영속성의 경우에는 객체를 파일 시스템에 저장하거나 DB에 저장할 때 사용되며, 네트워크 전송의 경우 객체를 네트워크를 통해 다른 JVM으로 전송할 때 사용된다. 

#### 객체 생성의 유연성
해당 부분은 객체 생성 패턴의 발생과정 관점에서 이해해야 한다. 

자바에서 객체를 생성하는 방식은 다음과 같은 역사를 따른다. 

1. 점층적 생성자 패턴
2. JavaBeans 패턴
3. Builder 패턴

각 패턴이 어떤 장점이 있는지에 대해서는 따로 기록하기로 하고, 이 순서만 따져보았을 때, JavaBeans 패턴에서 기본 생성자를 필요로 하는 이유는 점층적 생성자 패턴에서 어떤 문제가 발생하였고, 그 문제를 해결하기 위해 등장한 패턴이라고 이해할 수 있다. 

점층적 생성자 패턴은 클래스에서 생성자를 만들때 파라미터를 받아서 처리하는 방식, 즉 
```java
class UseConstructor { 
	int value; 
	
	UseConstructor(int value) { 
		this.value = value; 
	} 
}
```
이런 방식을 의미한다. 

해당 방식은 객체의 일관성은 보장되지만, 사용하는 방식이 너무 번거롭다는 단점이 있다. 

매개변수가 많아지면, 각 매개변수가 어떤 역할을 하는 매개변수인지 외우고 있어야하며, 매개변수 순서상 사용하지 않는 매개변수의 경우에는 직접 초기화 용도의 변수를 넣어줘야하거나 그에 맞는 생성자를 새로 생성해줘야하는 상황이 발생한다. 

```java
public class Hamburger {
    private int buns;
    private int patties;
    private int lettuce;
    private int tomato;
    private int cheese;
    private int bacon;
    private int pickles;
    private int onions;

    // 모든 재료를 포함하는 생성자
    public Hamburger(int buns, int patties, int lettuce, int tomato, int cheese, int bacon, int pickles, int onions) {
        this.buns = buns;
        this.patties = patties;
        this.lettuce = lettuce;
        this.tomato = tomato;
        this.cheese = cheese;
        this.bacon = bacon;
        this.pickles = pickles;
        this.onions = onions;
    }

    // 치즈버거를 위한 생성자
    public Hamburger(int buns, int patties, int lettuce, int tomato, int cheese) {
        this(buns, patties, lettuce, tomato, cheese, 0, 0, 0);
    }

    // 베이컨 치즈버거를 위한 생성자
    public Hamburger(int buns, int patties, int lettuce, int tomato, int cheese, int bacon) {
        this(buns, patties, lettuce, tomato, cheese, bacon, 0, 0);
    }

    // 빵과 베이컨만 있는 햄버거를 위한 생성자
    public Hamburger(int buns, int bacon) {
        this(buns, 0, 0, 0, 0, bacon, 0, 0);
    }
    
    // => 생성자 오버로딩 증가
}

// 매개변수가 많은 경우
Hamburger fullBurger = new Hamburger(2, 1, 2, 1, 1, 2, 3, 1);

// 선택적 매개변수 처리가 곤란한 경우
Hamburger baconOnlyBurger = new Hamburger(2, 0, 0, 0, 0, 2, 0, 0);
```

이런 상황을 피하기 위해 만들어진게 JavaBeans 패턴에서 기본 생성자를 요구하는 방식이라고 볼 수 있다. 

해당 방식은 초기화 된 필드들을 지니는 인스턴스를 생성하고, 필요한 값들만 채우는 방식으로 객체를 생성한다. 
```java
import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
public class Hamburger {
    private int buns;
    private int patties;
    private int lettuce;
    private int tomato;
    private int cheese;
    private int bacon;
    private int pickles;
    private int onions;
}

// 모든 재료를 포함하는 햄버거
Hamburger fullBurger = new Hamburger();
fullBurger.setBuns(2);
fullBurger.setPatties(1);
fullBurger.setLettuce(2);
fullBurger.setTomato(1);
fullBurger.setCheese(1);
fullBurger.setBacon(2);
fullBurger.setPickles(3);
fullBurger.setOnions(1);

// 치즈버거
Hamburger cheeseBurger = new Hamburger();
cheeseBurger.setBuns(2);
cheeseBurger.setPatties(1);
cheeseBurger.setLettuce(2);
cheeseBurger.setTomato(1);
cheeseBurger.setCheese(1);

// 베이컨만 있는 햄버거
Hamburger baconOnlyBurger = new Hamburger();
baconOnlyBurger.setBuns(2);
baconOnlyBurger.setBacon(2);
```

이 방식은 점층적 생성자 패턴에 비해 좋은 부분도 있지만, 거꾸로 객체 일관성이 깨질 수 있고 불변 객체를 만들기 어렵다는 단점이 발생한다. 


---





%% 다만 JavaBeans는 하나의 개념으로 이해되어야 하기에, 실제 구현체는 해당 개념과 상이할수도 있다는 점을 이해하여야 한다.  %%


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