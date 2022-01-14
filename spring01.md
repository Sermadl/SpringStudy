# spring 체험기!!



오늘은 유튜브에서 개발자 관련 영상을 보고 아직 대학교 1학년이지만 굉장한 위기감(!!)을 느껴서 자바 스프링을 공부해봤다.



평소 내가 자바에 굉장히 약하다고 생각하고 있었기 때문에 스프링에도 흥미를 별로 느끼지 못할 것이라고 생각했는데 예상 외로 스프링에 흥미가 생겼다.



아는 선배가 인프런에서 스프링의 기초에 대한 괜찮은 무료 강의가 있다고 하길래 한 번 들어봤다.



아직 처음 배운 것이라 뭐가 뭔지도 잘 모르겠지만 내가 여태까지 이해한 것은 프론트엔드(html, js, css)를 표시해주기 위한 백엔드 작업이라는 것이다.



오늘 배운 스프링 코드에서는 두가지 모듈을 사용했다. Spring web 모듈과 thymeleaf 라는 모듈이었다. 각 모듈 안에는 필수 인자(?) 같은 것이 있어서 저 두 가지 모듈만 호출해도 굉장히 많은 양의 모듈들이 함께 호출되는 것을 확인했다.



그중에서도 중요한 모듈들이 몇 가지 있었는데 내가 지금 기억나는 것은 톰켓 한 가지이다. 예전에는 손수 타이핑으로 모든 것을 다 했는데 이제는 스프링 사이트에서 템플릿(?)을 만들어 다운 받으면 모든 준비작업이 끝난다고 했다. 신기하다.



사실 자바와 친해지기 위해 스프링을 배우는 것도 없지않아있다. 하지만 정말 앞으로 열심히 해서 나를 한 층 더 성장시키기 위해 노력할 것이다(!!)

---





```
 .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.6.2)

2022-01-14 18:05:08.107  INFO 88896 --- [           main] s.hellospring.HelloSpringApplication     : Starting HelloSpringApplication using Java 15.0.2 on gimseeun-ui-MacBookAir.local with PID 88896 (/Users/serma/spring/hello-spring/out/production/classes started by serma in /Users/serma/spring/hello-spring)
2022-01-14 18:05:08.109  INFO 88896 --- [           main] s.hellospring.HelloSpringApplication     : No active profile set, falling back to default profiles: default
2022-01-14 18:05:08.994  INFO 88896 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8080 (http)
2022-01-14 18:05:09.023  INFO 88896 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2022-01-14 18:05:09.023  INFO 88896 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.56]
2022-01-14 18:05:09.065  INFO 88896 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2022-01-14 18:05:09.065  INFO 88896 --- [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 903 ms
2022-01-14 18:05:09.273  INFO 88896 --- [           main] o.s.b.a.w.s.WelcomePageHandlerMapping    : Adding welcome page: class path resource [static/index.html]
2022-01-14 18:05:09.389  INFO 88896 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
2022-01-14 18:05:09.414  INFO 88896 --- [           main] s.hellospring.HelloSpringApplication     : Started HelloSpringApplication in 1.733 seconds (JVM running for 2.338)
2022-01-14 18:05:21.805  INFO 88896 --- [nio-8080-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring DispatcherServlet 'dispatcherServlet'
2022-01-14 18:05:21.805  INFO 88896 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : Initializing Servlet 'dispatcherServlet'
2022-01-14 18:05:21.807  INFO 88896 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : Completed initialization in 2 ms

Process finished with exit code 130 (interrupted by signal 2: SIGINT)

```

프로젝트 실행 시 출력되는 화면

저 스프링 글자를 보자마자 확 



```
package serma.hellospring.contoller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HelloController {

    @GetMapping("hello") //"hello"라는 문구가 url에 적혀있는지 확인
    public String hello(Model model) //적혀있다면 실행되는 메소드
    {
        model.addAttribute("data", "hello!!"); //"data"키에 "hello!"라는 값을 넣음
        return "hello"; //"hello"라는 이름의 html 반환
    }
}
```

컨트롤러 클래스에 작성한 코드



```
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org"> //이 부분은 아직 무엇을 의미하는지 설명듣지 못했다
<head>
    <title>Title</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" /> //여기도 마찬가지
</head>
<body>
<p th:text="'안녕하세요. ' + ${data}" >안녕하세요. 손님</p> //아까 입력한 키와 값에 따라 ${data} 자리에 data의 value인 "hello!!"가 대신 호출된다
</body>
</html>
```

src/main/resources/hello.html에 작성한 코드

<img width="589" alt="스크린샷 2022-01-14 오후 6 41 10" src="https://user-images.githubusercontent.com/89215928/149495982-582cdc10-1945-4ede-9665-264715321236.png">


<img width="1440" alt="스크린샷 2022-01-14 오후 6 44 52" src="https://user-images.githubusercontent.com/89215928/149496002-ca2024e6-6752-4605-a3ea-d9f62a3d9814.png">


실제로 링크로 들어가보면 잘 실행된 것을 확인할 수 있다.


---



스프링에 나름 흥미가 생겨서 다행이다..ㅜㅜ 오늘은 너무 졸려서 집중력이 흐려질 것 같아 여기까지 하고 내일 마저 진도를 나가보겠다.
