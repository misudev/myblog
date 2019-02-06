---
layout: post
title: "[Spring] 의존 자동 주입 "
category: Spring
tags: [Spring]
---

#의존 자동 주입

의존 대상을 설정코드에서 직접 주입하지 않고 스프링이 자동으로 의존하는 빈 객체를 주입해주는 기능이 있다. 이를 자동 주입이라고 한다. 



## @Autowired를 이용한 의존 자동 주입

@Autowired 애노테이션을 이용하면 스프링이 알아서 의존 객체를 찾아서 주입한다.

예시코드를 보면 다음과 같다.



@Autowired 는 메소드에도 붙일 수 있는데 다음과 같이 Setter 메소드에 붙여주면 설정 클래스에서 Setter메소드를 호출하지 않아도 스프링에서 자동으로 찾아서 주입해준다.

```java
import org.springframework.beans.factory.annotation.Autowired;

public class MemberService {
    private MemberDao memberDao;
    
    @Autowired
    public void setMemberDao(MemberDao memberDao){
        this.memberDao = memberDao;
    }
}
```

```java
@Configuration
public class AppConfig{
    @Bean
    public MemberDao memberDao(){
        return new MemberDao();
    }
    @Bean
    public MemberService memberService(){
        // @Autowired애노테이션을 필드나 Setter메서드에 붙이면
        // 스프링은 타입이 일치하는 빈 객체를 찾아서 주입한다.
        // setter메소드를 호출하지 않아도 자동으로 memberDao빈이 주입된다.
        return new MemberService();
    }
}
```



##@Qualifier 애노테이션을 이용한 의존 객체 선택

위에서 @Autowired를 이용해 자동 주입을 할 때 해당 타입의 빈이 어떤 빈인지 정해져야 하는데 자동 주입 가능한 빈이 두개 이상이면 익셉션이 발생한다. 

이 때 @Qualifier 애노테이션을 사용하면 자동 주입 대상 빈을 정해줄 수 있다.

```java
@Configuration
public class AppCtx{
    ...
    @Bean
    @Qualifier("printer")
    public MemberPrinter memberPrinter1(){
        return new MemberPrinter();
    }
    @Bean
    public MemberPrinter memberPrinter2(){
        return new MemberPrinter();
    }
}
```

```java
public class MemberService {
    private MemberDao memberDao;
    private MemberPrinter printer;
    
    @Autowired
    public void setMemberDao(MemberDao memberDao){
        this.memberDao = memberDao;
    }
    
    @Autowired
    @Qualifier("printer")
    public void setMemberPrinter(MemberPrinter printer){
        this.printer = printer;
    }
}
```

 빈 설정에 @Qualifier 애노테이션이 없으면 빈의 이름을 한정자로 지정한다.

---------------------

**어떤 A클래스와 A클래스를 상속받은 B클래스가 빈에 등록 있을 경우 A클래스 타입의 빈을 주입받도록 하면 익셉션이 발생한다. **

**→ 이때 @Qualifier로 빈을 한정해주거나 자식클래스인 B타입의 빈을 자동주입 받도록 설정해주어야 한다.**

-----------------------

### @Autowired 애노테이션 필수 여부

자동 주입할 대상이 필수가 아닌경우 required속성을 false로 지정하면 된다. 

→ @Autowired(required = false) 

또는 의존 주입 대상에 자바8의 Optional을 이용한다.

→ Optional<DataTimeFormatter> formatterOpt	

→ if(formatterOpt.isPresent()){	...	} else { ...	 }

마지막으로 @Nullable 애노테이션을 이용하는 방법도 있다.

→ @Nullable DataTimeFormatter dataTimeFormatter





--------------

출처 : 초보 웹 개발자를 위한 스프링5프로그래밍 입문s

