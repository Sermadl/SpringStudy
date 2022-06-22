#  스프링 2회차

스프링을 쉬다가 공모전을 위해 다시 시작하게 되었다.

오늘은 정적 컨텐츠, MVC/템플릿 엔진, API 기초를 다뤄보았다.


---
#### 정적 컨텐츠

정적 컨텐츠는 요청받은 파일을 그대로 웹 브라우저에 띄움

#### MVC/템플릿 엔진

서버에서 가공한 데이터를 추가하여 파일을 변형한 후 웹 브라우저에 띄움

#### API

.json 형태로 데이터를 클라이언트에 전달


---

<details><summary>오늘 작성한 코드
</summary>


```js

package hello.hellospring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class HelloController {

    @GetMapping("hello")
    public String hello(Model model){
        model.addAttribute("data", "hello!!");
        return "hello";
    }
    @GetMapping("hello-mvc")
    public String helloMvc(@RequestParam(value = "name") String name, Model model){
        model.addAttribute("name", name);
        return "hello-templete";
    }

    @GetMapping("hello-string")
    @ResponseBody
    public String helloString(@RequestParam("name") String name) {
        return "hello " + name;
    }

    @GetMapping("hello-api")
    @ResponseBody
    public Hello helloApi(@RequestParam("name") String name) {
        Hello hello = new Hello();
        hello.setName(name);
        return hello;
    }

    static class Hello {
        private String name;

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }
    }
}
```


</details>


- 서버 가동 시


![spring02_main](https://user-images.githubusercontent.com/89215928/175031310-13423b99-1d84-4eeb-bfd1-ca590e660447.JPG)

<br/><br/>
- hello-static.html 을 불러왔을 때 (정적 컨텐츠 예시)


![image](https://user-images.githubusercontent.com/89215928/175031837-bc4d325a-ebe9-420f-9b57-37fd2ff460ec.png)


파일을 그대로 불러옴<br/><br/>

- hello-mvc 호출 (MVC/템플릿 엔진 예제)

![image](https://user-images.githubusercontent.com/89215928/175032834-84bf5bb6-e471-444a-8667-540a56bbdc32.png)

name에 아무것도 넣어주지 않으면 에러가 뜬다<br/><br/>


![image](https://user-images.githubusercontent.com/89215928/175032697-5dcacb7a-46d6-42a3-aba8-ea51579d3462.png)

함수를 호출할 때 인자에 spring!을 넣어주면 html 파일이 수정되어 보여진다<br/><br/>

- hello-string 호출

![image](https://user-images.githubusercontent.com/89215928/175034988-ae2b068d-795a-473a-a37d-33032205a44e.png)

다른게 없어보이지만

>페이지 소스보기
>![image](https://user-images.githubusercontent.com/89215928/175035111-a8ce78be-e784-4eb2-a842-d7815178b5c4.png)

소스를 보면 문자 그 자체가 그대로 출력된 것을 볼 수 있다

-hello-api 호출 (API 예제)

![image](https://user-images.githubusercontent.com/89215928/175035364-497e9617-2447-4896-9f6e-f7a5ac469812.png)

 json의 형태로 데이터가 출력된다(json은 키와 밸류의 형태로 데이터가 저장되는 파일 형식이다. 웹 어플리케이션에서 데이터를 전송할 때 일반적으로 사용한다.)
 
 API는 주로 서버에서 클라이언트로 데이터를 전송할 때 많이 쓰인다
 
 @ResponseBody 는 데이터를 그대로 넘길 때 사용한다
 
 이 함수에서는 객체를 반환하는데 이 때문에 스프링 서버에서 json파일로 전환하여 웹 브라우저 화면에 표시한다
 
 ---
 
 다음에 볼 강의의 소단원 제목이 '회원 관리 예제' 이다.
 
 실제 쓰이는 기술에 대한 예제를 하게 될 것 같아 기대된다. 스프링 열심히 해야지 ㅜㅜ
