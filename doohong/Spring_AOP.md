# Spring AOP
---

## AOP(Aspect Oriented Programing)

AOP는 관점 지향 프로그래밍이다.

쉽게 말해서 AOP는 애플리케이션 전체에 걸쳐 사용되는 기능을 재사용 하도록 지원하는 것이다.

관점 지향 프로그래밍이라는 단어가 AOP를 이해하는데 더 어려움을 일으킨다.

쉽게 설명하면 프로젝트 구조를 바라 보는 관점을 바꿔 보자는 것이다.
![그림1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory&fname=http%3A%2F%2Fcfile23.uf.tistory.com%2Fimage%2F2108D041584969DA190575)

(제 3자의 관점)
![그림2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory&fname=http%3A%2F%2Fcfile4.uf.tistory.com%2Fimage%2F273B244458496A0A1B7AC5)

(핵심기능에서 바라본 관점)
![그림3](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory&fname=http%3A%2F%2Fcfile3.uf.tistory.com%2Fimage%2F2473C33D58496A2A0F6DF9)

(부가기능에서 바라본 관점)

부가적 기능의 관점엑서 보면 각각의 서비스는 before와 after 메소드를 공통으로 사용하고 있다.

기존의 OOP에서 바라보던 관점을 다르게하여 부가기능적인 측면에서 보았을때 공통된 요소를 추출하자는 것이 관점 지향 프로그래밍이다.

- OOP : 비지니스 로직의 모듈화
    - 모듈화의 핵심 단위는 비지니스 로직
- AOP : 인프라 혹은 부가기능의 모듈화
    - 대표적 예 : 로깅, 트랜잭션, 보안 등
    - 각각의 모듈들의 주 목적 외에 필요한 부가적인 기능들

AOP는 새로운 개념은 아니다!

결국은 공통된 기능을 재사용하는 기법

기존 OOP같은 경우 상속이나 위임을 통해 처리하지만 자바의 경우 다중 상속을 지원하지 않을뿐더러 상속이나 위임으로 처리하기에는 깔끔하게 모듈화가 어렵다고 한다.

그래서 이러한 문제를 해결하기 위해 AOP가 등장하게 되었고 AOP의 큰 장점은 2가지가 있다.
1. 어플리케이션 전체에 흩어진 공통 기능이 하나의 장소에서 관리된다는 점
2. 다른 서비스 모듈들이 본인의 목적에만 충실하고 그외 사항들은 신경쓰지 않아도 된다는 점 

## AOP용어

### 타겟(Target)
부가기능을 부여할 대상을 말한다.
위에 그림에선 공통된 기능을 가진 Service들을 얘기한다.

### 애스펙트(Aspect)
객체지향 모듈을 오브젝트라 부르는것과 같게 부가기능 모듈을 애스펙트라 부르며, 핵심기능에 부가되어 의미를 갖는 특별한 모듈이라 생각하면 된다.

### 어드바이스(Advice)
실질적으로 부가기능을 담은 구현체
어드바이스는 타겟 오브젝트에 종속되지 않기 때문에 순수하게 부가기능에만 집중 할 수 있다.
어드바이스는 애스펙트가 무엇을 언제할지를 정의한다.

### 포인트컷(PointCut)
부가기능이 적용될 대상(메소드)를 선정하는 방법을 얘기한다.
즉, 어드바이스를 적용할 조인포인트를 선별하는 기능을 정의한 모듈을 얘기한다.

### 조인포인트(JoinPoint)
어드바이스가 적용될 수 있는 위치를 얘기한다.
Spring에서는 메소드 조인포인트만 제공하고 있다.
따라서 Spring 내에서 조인포인트라 하면 메소드를 가르킨다고 생각하면 된다.

### 프록시(Proxy)
타겟을 감싸서 타겟의 요청을 대신 받아주는 랩핑(Wrapping)오브젝트이다.
클라이언트에서 타겟을 호출하게 되면 타겟이 아닌 타겟을 감싸고 있는 프록시가 호출되어, 타깃 메소드 실행전에 선처리, 메소드 실행 후, 후처리를 하도록 구성되어 있다.

![그림4](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory&fname=http%3A%2F%2Fcfile29.uf.tistory.com%2Fimage%2F2715394658496A61010951)

### 인트로덕션 (Introduction) 
타겟 클래스에 코드 변경없이 신규 메소드나 멤버변수를 추가하는 기능을 얘기한다. 

### 위빙 (Weaving) 
지정된 객체에 애스팩트를 적용해서 새로운 프록시 객체를 생성하는 과정을 얘기한다. 
예를 들면 A라는 객체에 트랜잭션 애스팩트가 지정되어 있다면, A라는 객체가 실행되기전 커넥션을 오픈하고 실행이 끝나면 커넥션을 종료하는 기능이 추가된 프록시 객체가 생성되고, 이 프록시 객체가 앞으로 A 객체가 호출되는 시점에서 사용된다. 이때의 프록시객체가 생성되는 과정을 위빙이라 생각하면 된다. 
Spring AOP는 런타임에서 프록시 객체가 생성 된다.

## 실습(어노테이션)
```java
@SpringBootApplication
@EnableAspectAutoProxy
public class SomeApplication {

    public static void main(String[] args) {

        ApplicationContext ctx = SpringApplication.run(Application.class, args);
    }
}
```
- @SpringBootApplication 또는 @Configuration을 명시한 클래스에 @EnableAspectAutoProxy를 명시하면 Spring AOP를 사용하기 위한 첫 준비가 끝난다.
- @EnableAspectAutoProxy은 XML 기반의 ApplicationContext 설정에서의 <aop:aspectj-autoproxy />와 동일한 기능을 한다.

```java
@Component
@Aspect
@Order(Ordered.LOWEST_PRECEDENCE)
public class SomeAspect {

    @Around("execution(* com.blogcode.board.BoardService.getBoards(..))")
    public Object calculatePerformanceTime(ProceedingJoinPoint proceedingJoinPoint) {
        Object result = null;
        try {
            long start = System.currentTimeMillis();
            result = proceedingJoinPoint.proceed();
            long end = System.currentTimeMillis();

            System.out.println("수행 시간 : "+ (end - start));
        } catch (Throwable throwable) {
            System.out.println("exception! ");
        }
        return result;
    }
}
```
- 스프링 빈에 @Aspect를 명시하면 해당 빈이 Aspect로 작동한다.
- 클래스 레벨에 @Order를 명시하여 @Aspect 빈 간의 작동 순서를 정할 수 있다. int 타입의 정수로 순서를 정할 수 있는데 값이 낮을수록 우선순위가 높다. 기본값은 가장 낮은 우선순위를 가지는 Ordered.LOWEST_PRECEDENCE이다.
- @Aspect가 명시된 빈에는 어드바이스(Advice)라 불리는 메써드를 작성할 수 있다. 대상 스프링 빈의 메써드의 호출에 끼어드는 시점과 방법에 따라 @Before, @After, @AfterReturning, @AfterThrowing, @Around 등을 명시할 수 있다.

![그림5](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory&fname=http%3A%2F%2Fcfile26.uf.tistory.com%2Fimage%2F2379DB3E58496AA7079CEF)

(포인트컷 표현식)


- @Before (이전)
    - 어드바이스 타겟 메소드가 호출되기 전에 어드바이스 기능을 수행
- @After (이후)
    - 타겟 메소드의 결과에 관계없이(즉 성공, 예외 관계없이) 타겟 메소드가 완료 되면 어드바이스 기능을 수행
- @AfterReturning (정상적 반환 이후)
    - 타겟 메소드가 성공적으로 결과값을 반환 후에 어드바이스 기능을 수행
- @AfterThrowing (예외 발생 이후)
    - 타겟 메소드가 수행 중 예외를 던지게 되면 어드바이스 기능을 수행
- @Around (메소드 실행 전후)
    - 어드바이스가 타겟 메소드를 감싸서 타겟 메소드 호출전과 후에 어드바이스 기능을 수행