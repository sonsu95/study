
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

https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%A7%81%EB%A0%AC%ED%99%94Serializable-%EC%99%84%EB%B2%BD-%EB%A7%88%EC%8A%A4%ED%84%B0%ED%95%98%EA%B8%B0
