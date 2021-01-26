# SPRING

**:Contents**
* Block/Non-Block
* Sync/Async
* [spring boot의 동작원리](#spring_boot의_동작원리)
* [스프링 프레임워크는 왜 생긴 것인가](#스프링_프레임워크는_왜_생긴_것인가)
* Bean이란
* Bean의 생명주기 / 소멸방법 / 초기화방법
* IOC(Inversion of Control, 제어의 역전)란
* DI(Dependency Injection, 의존성 주입)란
* AOP(Aspect Oriented Programming)란
* Filter와 Interceptor 차이


### Block/Non-Block
* Block(막다)상황
제어권이 호출자에서 실행함수로 넘어간 경우<br>
함수가 완료되기 전까지는 호출자는 제어권이 없다.<br>

* Non-Block(막다)상황
제어권이 호출자에서 함수를 실행해도(함수가 실행중이여도) 호출자에게 바로 제어권이 넘어간다.<br>


### Sync/Async
대상들의(제어권의 반환 / 결과값 전달) 시간이 맞줘지는가?<br>
* Sync
함수A => 함수B =><br>

제어권의 반환 =><br>
결과값 전달 =><br>

* Async
함수A =><br>
        함수B =><br>

제어권의 반환 =><br>
           결과값 전달 =><br>
           
           
### 스프링_프레임워크는_왜_생긴_것인가
`왜 '경량' 프레임워크인가?`<br>

초기 서버사이드 처리는 직접 쓰레드, 소켓연결 등을 개발자들이 직접 처리했고, 개발자 마다 구현하는 방법이 다 달라 협업에 불편함이 많았다.<br> 
이런 개발 표준을 잡은것이 Java Enterprise Edition. 그리고 그 안에 많은 스펙들을 담았고,<br>
개발자들은 그 스펙들을 구현하는 방법을 사용하기 시작했다. Servlet/JSP도 그 중 하나이다.<br>
그러나 이 구현방법 또한 여러가지 방법으로 나뉘게 되고, 개발자들은 또 정형화, 표준화 된 방법을 찾기 시작했다.<br> 
그래서 등장한 것이 Framework이다.<br>
많은 종류의 Framework이 등장하고 자바 진영에서는 대표적으로 EJB가 나왔지만 분산환경 처리에 특화된 EJB는 너무 무거운데다 불편한 점이 많았고,<br>
이런 EJB에 반기를 들고 탄생한 '경량화'된 Java 엔터프라이즈용 Framework이 Spring인 것이다.<br>


`왜 스프링을 사용하는가?` <br>

스프링은 Framework이고, Framework은 개발자들이 좀 더 쉽고 편리하게 애플리케이션을 개발할 수 있도록 미리 갖춰진 구조를 말한다.<br>
Framework이 없었다면 개발자들은 처음부터 끝까지 직접 모든 구조를 만들어내야 할 것이다.<br>
즉, 만들어진 프로그램의 성능은 개발자의 역량에 따라 극명히 갈리게 된다.<br>

스프링은 Application 개발에 필요한 하부 구조를 포괄적으로 제공함에 따라<br>
개발자들의 실력의 간극을 메꿔줄 수 있는 데다가 올바른 형태의 코드만 넣어준다면 일정 수준의 성능과 안정성을 보장해 줄 수 있다.<br>
그 틀과 구조 위에서 개발자들은 핵심 비즈니스 로직에만 집중할 수 있어서 생산성 또한 향상된다.<br>


정리 짱짱 잘되어 있다!<br>
cf) https://dreamingdreamer.tistory.com/125

### spring Di
>> 제어의 역행(IOC)으로 특정 객체에 필요한 다른 객체를 외부에서 결정해서 연결시키는 것. 

설정만 해준다면(applicationContext.xml) 스프링은 그 설정 정보를 참고해서 객체를 생성,관리하고 그 관계를 맺어준다.<br>
핵심은 개발자가 new 연산자 등을 통해 객체를 생성할 필요가 없다는 것이다(제어의 역행-IoC).<br>
개발자 입장에서는 스프링 컨테이너에게 그 참조변수만 일러준다면(@Autowired나 setter주입 등) 스프링이 알아서 객체를 생성해주고, 관계를 맺어준다.<br>
이를 의존성 주입(DI : dependency Injection)이라고 하며 개발자는 자신의 코드에 필요한 객체를 스프링을 통해 주입받는 구조로 코드를 작성한다. 


### spring AOP
https://logical-code.tistory.com/118
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



### IOC(Inversion of Control, 제어의 역전)란
IOC(Inversion of Control) : 의존 관계 주입(Dependency Injection)이라고도 하고,<br>
어떤 객체가 사용하는 의존 객체를 직접 만들어 사용하는게 아니라, 주입 받아 사용하는 방법을 말합니다.

스프링 IOC 컨테이너<br>
* BeanFactory 
=> 최상의 인터페이스

* 어플리케이션 컴포넌트의 중앙 저장소
* 빈 설정 소스로 부터 빈 정의를 읽어들이고 , 빈을 구성하고 제공합니다.

스프링 IOC 컨테이너의 역할
* 빈 인스턴스 생성
* 의존 관계 설정
* 빈 제공

빈(bean)이란<br>
* 스프링 IOC 컨테이너가 관리하는 객체
* 장점
    * 의존성 관리
    * 스코프
        * 싱글토 : 하나
        * 프로포토타입 : 매번 다른 객체
    * 라이프사이클 인터페이스



### Filter와 Interceptor 차이
spring mvc lifecycle와 함께 Filter와 Interceptor에 대해 읽기 편하게 정리된 사이트가 있다.
: https://velog.io/@damiano1027/Spring-Spring-MVC-Request-Lifecycle
