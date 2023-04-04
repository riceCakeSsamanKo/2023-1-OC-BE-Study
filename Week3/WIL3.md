# 의존과 의존성
예를들어 Member라는 클래스가 있고 이를 다루는 MemberController, MemberService, MemberRepository가 있다고 하자. MemberController에는 필드로서 MemberService가 있고, MemberService에는 필드로서 MemberRepository가 있다고 하자. 
그 경우 MemberController는 MemberService에 의존하는 것이고, MemberService는 MemberRepository에 의존한다고 볼 수 있다. 

# @Autowired 의존성 주입
스프링에는 DI(Dependency Injection)라는 개념이 존재한다. 말그대로 의존성 주입으로 해석이 가능하다. 다시 의존 의존성에서의 예를 들어 MemberController의 내부 필드로 memberService라는 인스턴스가 존재한다고 하자. 그렇다면 이 객체는 외부 어딘가에서 MemberController 내부로 넣어주어야지 사용이 가능할 것이다. 보통 생성자를 통해서 의존관계 주입을 많이 해준다.

예시를 보자.

@Controller
public class MemberController{
  private MemberService memberService;

  public MemberController(MemberService memberService){
    this.memberService = memberService;
  }
}

위 코드를 보면 MemberController의 생성자로 memberService가 주입됨을 알 수 있다. 이런 것을 DI라고 한다.
그렇다면 이러한 문제가 발생할 수 있다. 만일 MemberController가 스프링 빈으로 등록된다면 내부에 들어가는 MemberService는 어디서 가져와야 한다는 말인가?

이런 경우에 우리는 기존 스프링 빈으로 등록되어 있는 MemberService 빈을 MemberController 빈에 DI하는 것으로 문제를 해결 수 있다. 그리고 이것을 @Autowired 어노테이션을 통해서 진행한다.

다음은 @Autowired 어노테이션을 사용하여 MemberController를 스프링 빈으로 등록할 때, 내부 필드인 MemberService memberService에 자동으로 의존관계 주입을 해주는 예시이다.

@Controller
public class MemberController{
  private MemberService memberService;

  @Autowired  // 생성자가 하나만 있을 때는 @Autowired가 자동으로 삽입
  public MemberController(MemberService memberService){
    this.memberService = memberService;
  }
}

사실 생성자가 하나만 있는 경우에는 @Autowired를 붙이지 않아도 자동으로 DI가 진행되므로 지금 예시에서는 굳이 써주지 않아도 된다. 

# DIP
객체 지향의 5개 원칙 중 하나로서, 추상화에 의존해야지 구체화에 의존해서는 안된다는 말이다. 쉽게 말해 구현 클래스가 아닌 인터페이스에 의존해야 한단 말이다. 인터페이스 MemberRepository와 MemberService, 클래스인 MemberServiceImpl로 예시를 들겠다.

public interface MemberRepository{
  public void setMember(Member member);
  public Member getMember();
}

public interface MemberService{
  public void setMemberRepository(MemberRepository memberRepository);
  public MemberRepository getMemberRepository(); 
}

위와 같은 MemberRepository, MemberService 인터페이스가 존재한다. 그리고 MemberService를 구현하는 MemberServiceImpl 클래스도 존재한다.

@Service
public class MemberServiceImpl implements MemberService{
  // 인터페이스에 의존(DIP 원칙 준수)
  private MemberRepository memberRepository;
  
  @Autowired // 수정자 주입
  public void setMemberRepository(MemberRepository memberRepository){
    this.memberRepository = memberRepository;
  }

  public MemberRepository getMemberRepository(){
    return memberRepository;
  }
}

지금 보면 MemberServiceImpl은 인터페이스인 MemberService를 implements하고 있고, 또한 내부 필드에는 MemberRepository memberRepository라는 인터페이스에 의존하고 있다. 즉 다시말해 MemberServiceImpl 내부에는 클래스는 없고 오직 인터페이스에만 의존하고 있다.

그렇다면 굳이 왜 클래스가 아닌 인터페이스에 의존해야 하는 것일까? 
이는 클래스 보다 인터페이스가 변화가 적기 때문이다.  어떤 클래스가 의존성을 가질 때 구체적인 클래스보다 인터페이스에 의존관계를 맺는 경우, 이러한 설계가 변화에 유연하게 대처가 가능하기 때문이다. MemberRepository를 implements하는 MemberRepository1과 MemberRepository2 클래스가 있다고 하자. 그 경우 MemberServiceImpl이 처음에는 MemberRepository1을 사용했는데, 개발하다보니 MemberRepository2를 사용해야 된다고 해보자. 만일 MemberServiceImpl을 구현할 때 내부필드로 인터페이스가 아닌 MemberRepository1으로 구체화를 해놓았다면 모든 코드를 다 뜯어 고쳐야하는 불상사가 발생할 수 있다. 하지만 지금처럼 인터페이스에 의존관계를 해놓으면 그냥 의존관계 주입시 MemberRepository2를 넣기만 하면 모든 문제가 해결되기 때문이다. 
즉, DIP 원칙을 준수함으로서 유지 보수의 이점을 가져올 수 있다.

# 스프링 빈, 스프링 컨테이너
수동으로 빈을 등록할 때는 @Bean 어노테이션으로 스프링 빈을 등록 할 수 있다. 스프링 빈은 스프링 컨테이너에 등록된 객체이다. 
스프링 컨테이너는 스프링 빈들이 저장되어 있는 공간이라고 생각하면 된다.

스프링 컨테이너를 등록하는 방법은 몇가지 있다. 대표적으로는 @Configuration, @Controller, @Repository, @Service등이 있다. 

등록된 스프링 빈들은 
ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);

이후

applicationContext.getBean("빈이름",클래스); 로 컨테이너에서 가져올 수 있다.

# jdbc와 jpa
## jdbc
jdbc는 자바에서 DB에 접근할 수 있도록 지원하는 API이다. jdbc에는 plain jdbc와 spring jdbc가 있다. spring에서 지원하는 spring jdbc는 plain jdbc에 비해서 따로 예외처리를 해주지 않아도 되고, 또한 트랜잭션 처리도 해주지 않아도 되기 때문에 plain jdbc에 비해 훨신 코드가 간결하다는 장점이 존재한다. spring jdbc에서 자체적으로 connnection 여닫기, 예외처리를 해주기 때문에 훨씬 더 유리하지만 spring jdbc에서도 직접 sql 쿼리문을 작성해야지 DB에 접근을 할 수 있다. 물론 DB를 다루기 위해선 sql을 사용하는 것이 당연하지만 sql을 사용하기 때문에 객체 지향 개발이 어렵다는 단점이 존재한다.

## jpa
위에서 jdbc에 수많은 기능이 존재하지만 그럼에도 불구하고 sql 쿼리를 사용하기 때문에 객체지향 개발이 어렵다는 단점이 있다고 말했다. 따라서 이러한 문제점을 해결하기 위해서 jpa를 사용한다. 
먼저 jpa를 설명하기 위해선 ORM 기술에 대해서 알아야한다. ORM은 데이터 베이스 테이블을 자바 객체로 매핑해서 SQL 쿼리를 작성할 필요 없이 자동으로 SQL을 생성해주는 기술을 의미한다. 따라서 ORM 기술을 사용한다면 굳이 SQL을 사용하지 않고 개발이 가능하다. 그리고 자바에서 지원하는 ORM API를 jpa(java persistence api)라고 한다. persistence라는 것은 말 그대로 영속성이라는 뜻을 가진다. 
영속성에 대해서 설명하자면, 예를 들어 우리가 int형 변수에 데이터를 담는 프로그램을 실행한다고 하자. 그러면 그 프로그램을 재 실행했을 때 그 변수에 내가 이전에 담은 데이터가 담겨 있을까? 아니다. 일반적인 경우에 데이터는 휘발성을 가지기에 따로 파일이나 DB, 저장장치에 저장을 하지 않는 이상 데이터가 날라가 버린다. 영속성은 프로그램이 종료되어도 데이터가 사라지지 않는 특성을 의미한다. 즉, 영속성을 가진다면 데이터는 나중에 프로그램을 실행하더라도 데이터는 남아있게 된다.

암튼... 결론적으로 jpa는 객체를 이용해 sql 쿼리를 사용하지 않고 DB에 접근할 수 있도록 자바에서 지원하는 API이다. jpa의 대표적인 구현체로는 hibernate가 있다.
