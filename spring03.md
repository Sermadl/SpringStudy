# 스프링 3일차

오늘은 웹 회원 관리에 관한 예제를 배웠다.<br/>
회원 id 및 이름 등록, 중복 회원 방지 시스템, 회원 조회 시스템을 구현했다.


---
### 작성한 패키지, 인터페이스, 클래스
![image](https://user-images.githubusercontent.com/89215928/175243168-1b38aea5-9244-4557-92bf-3887392231aa.png)
---
### Main

#### domain 패키지에 작성한 Member 클래스<br/>


```js
package hello.hellospring.domain;

public class Member {

    private Long id;
    private String name;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

#### repository 패키지에 작성한 MemberRepository 인터페이스<br/>

```js
package hello.hellospring.repository;

import hello.hellospring.domain.Member;

import java.util.List;
import java.util.Optional;

public interface MemberRepository {
    Member save(Member member);
    Optional<Member> findById(Long id);
    Optional<Member> findByName(String name);
    List<Member> findAll();
}
```

#### repository 패키지에 작성한 MemoryMemberRepository 클래스<br/>

```js
package hello.hellospring.repository;

import hello.hellospring.domain.Member;

import java.util.*;

public class MemoryMemberRepository implements MemberRepository{

    //회원 목록을 저장할 맵 선언
    private static Map<Long, Member> store = new HashMap<>();
    
    //회원 Id를 부여하기 위한 변수 선언
    private static long sequence = 0L;

    @Override
    public Member save(Member member) {
        member.setId(++sequence); //회원이 늘어날 때마다 sequence에 1을 더한 후 회원 Id에 저장
        store.put(member.getId(), member); //회원  Id와 이름을 store 맵에 저장
        return member;
    }

    @Override
    //매개변수로 받은 id와 일치하는 회원을 store 맵에서 찾고 결과 반환
    public Optional<Member> findById(Long id) {
        return Optional.ofNullable(store.get(id)); //Optional로 감싸 매개변수로 받은 id와 일치하는 결과가 없어도 반환 가능하게 함
    }

    @Override
    //매개변수로 받은 name과 일치하는 회원을 store 맵에서 찾고 결과 반환
    public Optional<Member> findByName(String name) {
        return store.values().stream()
                .filter(member -> member.getName().equals(name))
                .findAny(); //결과가 여러개일 경우 해당 결과만큼 모두 반환
    }

    @Override
    //모든 회원 조회 및 반환
    public List<Member> findAll() {
        return new ArrayList<>(store.values());
    }

    //메모리 초기화를 위한 메소드
    public void clearStore() {
        store.clear();
    }
}

```

#### service 패키지에 작성한 MemberService 클래스<br/>

```js
package hello.hellospring.service;

import hello.hellospring.domain.Member;
import hello.hellospring.repository.MemberRepository;

import java.util.List;
import java.util.Optional;

public class MemberService {
   
    //동일하지 않은 데이터베이스가 사용되는 것을 방지하기 위해 생성한 데이터베이스를 넘김
    private final MemberRepository memberRepository;
    
    public MemberService(MemberRepository memberRepository){
        this.memberRepository = memberRepository;
    }
    

    /**
     * 회원가입
     */
    public Long join(Member member) {
        //같은 이름이 있는 중복 회원 X
        validateDuplicateMember(member);
        memberRepository.save(member);
        return member.getId();
    }

    //중복 회원 찾은 후 예외처리 메소드
    private void validateDuplicateMember(Member member) {
        memberRepository.findByName(member.getName())
                .ifPresent(n ->{
                    throw new IllegalStateException("이미 존재하는 회원입니다.");
                });
    }
    
    /**
     * 전체회원 조회
     */
    public List<Member> findMembers() {
        return memberRepository.findAll();
    }

    public Optional<Member> findOne(Long memberId) {
        return memberRepository.findById(memberId);
    }
}

```

---
### Test

테스트를 할 때는 테스트 하기를 원하는 클래스 이름을 선택한 후 ctrl+shift+t 를 눌러주면<br/>
인텔리제이에서 자동으로 test 폴더에 class가 생성된 패키지와 동일한 패키지에 `{패키지명}Test` 클래스를 생성한다.

#### MemoryMemberRepositoryTest

```js
package hello.hellospring.repository;

import hello.hellospring.domain.Member;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.Test;

import java.util.List;

import static org.assertj.core.api.Assertions.*;

class MemoryMemberRepositoryTest {
    MemoryMemberRepository repository = new MemoryMemberRepository();

    @AfterEach //각 메소드가 실행된 후 메모리 
    public void afterEach() {
        repository.clearStore(); 
    }

    //등록
    @Test
    public void save(){
        Member member = new Member();
        member.setName("spring");

        repository.save(member);

        Member result = repository.findById(member.getId()).get();
        assertThat(member).isEqualTo(result); //잘 저장되었는지 확인
    }
  
    //이름으로 회원 찾기
    @Test
    public void findByName() {
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("spring2");
        repository.save(member2);

        Member result = repository.findByName("spring1").get();

        assertThat(result).isEqualTo(member1); //회원 찾기에 성공했는지 확인
    }

    @Test
    public void findAll() {
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("spring1");
        repository.save(member2);

        List<Member> result = repository.findAll();

        assertThat(result.size()).isEqualTo(2); //등록한 회원이 모두 저장되었는지 확인
    }

}
```

#### MemberServiceTest

```js
package hello.hellospring.service;

import hello.hellospring.domain.Member;

import hello.hellospring.repository.MemoryMemberRepository;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import static org.assertj.core.api.Assertions.*;
import static org.junit.jupiter.api.Assertions.*;

class MemberServiceTest {

    MemberService memberService;
    MemoryMemberRepository memberRepository;

    @BeforeEach //각 메소드가 실행되기 전 서로 같은 데이터베이스를 사용할 수 있게 만들어줌
    public void beforeEach(){
        memberRepository = new MemoryMemberRepository();
        memberService = new MemberService(memberRepository);
    }

    @AfterEach //각 메소드가 실행된 후 메모리 초기화
    public void afterEach() {
        memberRepository.clearStore();
    }

//test를 할 때는 한글로 메소드명을 적으면 더 직관적으로 이해할 수 있음

    @Test
    void 회원가입() {
        //given, when, then 문법을 사용하여 체계적으로 코드 구현 가능
        
        //given
        Member member = new Member();
        member.setName("hello");

        //when
        Long saveId = memberService.join(member);

        //then
        Member findMember = memberService.findOne(saveId).get();
        assertThat(member.getName()).isEqualTo(findMember.getName()); //회원가입이 잘 되었는지 확인
    }

    @Test
    public void 중복_회원_예외() {
        //given
        Member member1 = new Member();
        member1.setName("spring");

        Member member2 = new Member();
        member2.setName("spring");

        //when
        memberService.join(member1);
        
    //중복 회원 예외처리가 잘 되는지 확인
        //1번 방법 (try catch 문을 좀 더 직관적으로 이해할 수 있어서 1번 방법을 더 많이 사용함)
        IllegalStateException e = assertThrows(IllegalStateException.class, () -> memberService.join(member2));

        assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
        
        //2번 방법
        /*try {
            memberService.join(member2);
            fail();
        } catch (IllegalStateException e) {
            assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
        }*/

        //then
    }

    @Test
    void findMembers() {
    }

    @Test
    void findOne() {
    }
}
```
---
### 컴포넌트 스캔과 자동 의존 관계

#### @Component

`@Repository, @Service anotation`을 해준 뒤에 `@Autowired`로 Controller에서 service와 repository 를<br/>
연결할 수 있다.
service와 repository, controller 등을 spring bean이라고 하는데 이를 등록하는 방식은 다음과 같다<br/><br/>
1. 컴포넌트 스캔과 자동 의존관계 설정<br/>
>위에서 `@Service` 등을 써준 것이 컴포넌트(`@Component`) 스캔, `@Autowired`를 써준 것이 자동 의존관계 설정이다.
>`@Component` 를 사용하면 컴포넌트 스캔이 된다. `@Component` 가 포함된 `@Service, @Controller`(유일하게 하나만 등록해서 공유함) 등도 사용 가능
>Application의 패키지의 하위 디렉토리가 기본적인 컴포넌트 스캔 범위이다
<details><summary>컴포넌트 스캔을 이용한 예제
</summary>
controller

  ```js
package hello.hellospring.controller;

import hello.hellospring.service.MemberService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;

@Controller
public class MemberController {

    private final MemberService memberService;

    @Autowired
    public MemberController(MemberService memberService) {
        this. memberService = memberService;
    }
}
```
  
<br/><br/>

repository

  ```js
package hello.hellospring.service;

import hello.hellospring.domain.Member;
import hello.hellospring.repository.MemberRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;
import java.util.Optional;

@Service
public class MemberService {

    private final MemberRepository memberRepository;

    @Autowired
    public MemberService(MemberRepository memberRepository){
        this.memberRepository = memberRepository;
    }

    /**
     * 회원가입
     */
    public Long join(Member member) {
        //같은 이름이 있는 중복 회원 X
        validateDuplicateMember(member);
        memberRepository.save(member);
        return member.getId();
    }

    private void validateDuplicateMember(Member member) {
        memberRepository.findByName(member.getName())
                .ifPresent(n ->{
                    throw new IllegalStateException("이미 존재하는 회원입니다.");
                });
    }
    /**
     * 전체회원 조회
     */
    public List<Member> findMembers() {
        return memberRepository.findAll();
    }

    public Optional<Member> findOne(Long memberId) {
        return memberRepository.findById(memberId);
    }
}
```
  
</details>
2. 자바 코드로 직접 스프링 빈 등록하기<br/>
> `@Service, @repository, @Autowired` 이노테이션을 사용하지 않는다.
>`@Bean` 으로 직접 스프링 빈을 등록한다

<details><summary>hello.hellospring에서 SpringConfig.class 생성 후 작성한 내용
</summary>
  
  ```js
  package hello.hellospring;

import hello.hellospring.repository.MemberRepository;
import hello.hellospring.repository.MemoryMemberRepository;
import hello.hellospring.service.MemberService;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class SpringConfig {

    @Bean
    public MemberService memberService() {
        return new MemberService(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }
}

  ```
  
</details>
  
- DI(Dependency Injection)에는 3가지의 주입 방법이 있다.
  
<details><summary>생성자 주입
    </summary>
    
```js
    
package hello.hellospring.controller;

import hello.hellospring.service.MemberService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;

@Controller
public class MemberController {

    private final MemberService memberService;
    
    @Autowired
    public MemberController(MemberService memberService) {
        this. memberService = memberService;
    }
}
```
 </details>
  
  <details><summary>필드 주입
    </summary>
    
```js
package hello.hellospring.controller;

import hello.hellospring.service.MemberService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;

@Controller
public class MemberController {

    @Autowired private MemberService memberService;
}
```
    
    필드 주입은 주입의 세부사항을 변경할 수 없기 때문에 잘 사용하지 않음
  </details>
  
  <details><summary>setter 주입
    </summary>
    
```js
package hello.hellospring.controller;

import hello.hellospring.service.MemberService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;

@Controller
public class MemberController {

    private MemberService memberService;

    @Autowired
    public void setMemberService (MemberService memberService) {
        this.memberService = memberService;
    }
}
```
    
  public으로 선언하기 때문에 여러 개발자가 class 밖에서 접근할 수 있다.<br/>
  보안상 문제가 발생할 수 있어 잘 사용하지 않음
  </details>
  
  - 실무에서는 정형화된 컨트롤러, 서비스, 리포지토리 같은 코드는 컴포넌트 스캔을 사용한다.
  - 정형화 되지 않거나, 상황에 따라 구현 클래스를 변경해야 하면 설정을 통해 스프링 빈으로 등록한다.
  
  ! 주의 `@Autowired' 는 스프링이 관리하는(컴포넌트 스캔이 되어있는) 객체에서만 동작한다.
  
  ---
  
  오늘은 컴포넌트 스캔과 자동 의존관계에 대해 배웠다.
  
  내일은 홈 화면을 추가해서 실제로 기능하는 웹 페이지에 대한 예제를 배울 것 같다.
  
  나름 1학년 때 자바를 열심히 해놔서 그런지 객체지향적인 개념이 많이 어렵지 않아서 다행이다.
  
  아자아자 화이팅

