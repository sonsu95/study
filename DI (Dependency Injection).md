
Dependency Injection(이하 DI)는 의존성 역전을 이야기한다. 

의존성 역전이란 한 객체가 다른 객체에 의존할 경우, 그 의존성을 외부에서 주입하는 디자인 패턴 중 하나이다. 
의존성을 외부에서 직접 제어할 경우 객체간의 결합도가 낮아지며 코드의 재사용성과 테스트하기 편한 환경을 만들어준다. 

예시를 들어보면 다음과 같다. 

MemberRepository가 존재하는 상황을 가정하자. 
MemberService 에서 하나의 MemberRepository의 인스턴스를 생성하였다. 

그리고 테스트를 위한 MemberServiceTest 에서 다시 다른 MemberRepository의 인스턴스를 생성하여 테스트를 실행하고 있다고 보자. 

위 경우 결과적으로 MemberRepository의 인스턴스는 두개가 되어 각각 따로 실행된다. 즉, 레포지토리를 두개를 만들어 따로따로 사용하고 있는 것이다. 



```java
```