# SPRING

**:Contents**
* [spring boot의 동작원리](#spring_boot의_동작원리)
* [스프링 프레임워크는 왜 생긴 것인가](#스프링_프레임워크는_왜_생긴_것인가)
* Bean이란
* Bean의 생명주기 / 소멸방법 / 초기화방법
* IOC(Inversion of Control, 제어의 역전)란
* MVC 패턴 / 사이클주기
* DI(Dependency Injection, 의존성 주입)란
* AOP(Aspect Oriented Programming)란
* Filter와 Interceptor 차이



### spring AOP

관점지향프로그래밍 == 횡단관심에 따라 하는 프로그래밍

`aop 용어`<br>
* targer
어떤 대상에 부가 기능을 부여할 것인가

* advice
어떤 부가 기능 ? Before , AfterReturing , AfterThrowing , After , Around 

* join point
어디에 적용할 것인가? 메서드 , 필드 , 객체 , 생성자등

* point cut
실제 advice가 적용될 시점 , spring aop에서는 advice가 적용될 메서드를 선정


`aop 구현 방법`<br>
* 컴파일
 J.java = > j.class 컴파일 하는 시점에 적용

* 클래스 로드시
j.class 메모리상에 올릴 때 적용

* 프록시 패턴(spring aop)
타겟클래스를 프록시 객체로 감싸서 부가기능을 제공



