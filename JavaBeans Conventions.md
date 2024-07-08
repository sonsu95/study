
# JavaBeans Conventions

Spring MVC Data Binding 개념을 찾아보면서 본 내용.

JavaBeans는 특정 디자인 패턴을 준수하여 많은 객체를 단일 객체(Bean)로 캡슐화하는 Java의 클래스이다. 

JavaBean Convention은 재사용, 캡슐화, 인트로스펙션 및 사용자 지정과 같은 기능을 하게하는 일련의 프로그래밍 지침이라고 볼 수 있다. 


주요 규칙은 다음과 같다

## Property Accessor Methods

- JavaBean에는 Property 값에 접근(Access)하고 업데이트하기 위한 공용 gettor와 constructor가 있는 private fields가 있어야 한다. 이러한 규칙은 캡슐화 원칙을 준수한다. 
- 메서드의 이름은 일반적으로 get 또는 set으로 시작하고, 그 뒤에 첫 글자가 대문자로 된 속성 이름이 붙는다. 
- Boolean property의 경우 get 대신 is로 시작할 수 있다. 

## Serializable Interface

- 객체의 상태를 저장하고 나중에 복원하기 위해 JavaBean에는 Serializable 인터페이스를 구현하는 것이 좋다. 이렇게 처리하는 경우 네트워크를 통해 전송하는것 또한 가능하다. 
- 반드시 필요한 과정은 아니지만, Bean이 상태를 유지할 수 있도록 하기 위한 일반적인 관행으로서, 가능한 처리해주는 것이 좋다. 


## Default Constructor

JavaBean은 인수가 없는 생성자를 가져야 한다. 이를 통해 런타임 중에 빈을 쉽게 인스턴스화하고 구성할 수 있으며, 특히 동적으로 Bean을 생성하고 관리하는 도구와 프레임워크에서 중요하게 여겨진다. 

## Instrospection

메서드에 대한 명명 규칙을 따르면 Instrospection, 즉 빈이 지원하는 속성, 이벤트 처리를 위해 설계되었는지 여부 등 빈의 기능을 분석하는 것이 더 쉬워진다. 

## Event Handling

JavaBean은 bound 또는 constrained 프로퍼티를 지원할 수 있다. 바인딩된 프로퍼티는 값이 변경되는 경우 옵저버 패턴을 사용하여 관심있는 리스터에 알려준다. 제약된 프로퍼티는 제안된 변경이 부적절하다고 판단될 경우 거부할 수 있다. 
