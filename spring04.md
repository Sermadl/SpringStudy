# 스프링 4일차

머신러닝이랑 같이 하려니까 약간 바쁜 것 같다,,</br>
그래두 열심히 할게...웅...화이팅..</br></br>

전 시간에 마무리하지 못했던 회원 관리 관련 페이지를 postmapping과 html의 form박스를 이용해 마무리했다! </br></br>
![image](https://user-images.githubusercontent.com/89215928/179355128-190981ea-f47b-43fb-a30b-4b8257a059fd.png)
</br>
spring과 spring1, spring2를 등록시켜봤다</br></br>
![image](https://user-images.githubusercontent.com/89215928/179355174-27ebd4c3-df87-4d50-a258-6081949211c4.png)
![image](https://user-images.githubusercontent.com/89215928/179355204-ce4fe549-0d7c-44f6-b134-ce6eddc78f75.png)
</br>
이미 등록된 회원을 입력하면 오류 페이지가 나오고 터미널(?)에 "이미 존재하는 회원입니다"라는 알림이 출력된다</br></br>
![image](https://user-images.githubusercontent.com/89215928/179355255-c0f22cd5-e144-4223-99a3-cc97d6d47b2a.png)</br></br>

또한 웹페이지와 데이터베이스를 연결했다</br>
build.gradle에 jdbc 드라이버와 h2 클라이언트 라이브러리를 넣고 application.properties 파일에 데이터베이스 주소를 연결하고 h2 드라이버를 넣어주었다.</br></br>
- 웹페이지에서 확인한 회원 목록과 실제 데이터베이스의 데이터</br>
![image](https://user-images.githubusercontent.com/89215928/179356562-42d20293-7e9a-4d59-8c1b-99edf607a405.png)
![image](https://user-images.githubusercontent.com/89215928/179356570-f10f8e53-8dc9-48a8-ba0b-ebc07d99900d.png)</br>
- jpa를 웹페이지에서 추가한 후 웹페이지에서 확인한 회원 목록과 실제 데이터베이스의 데이터</br>
![image](https://user-images.githubusercontent.com/89215928/179356814-9ac213b2-fe55-4844-9754-0037a535c944.png)
![image](https://user-images.githubusercontent.com/89215928/179356804-48b9ca85-6dbc-4c0a-a2be-9205891e68d7.png)

</br></br></br>

메모리 저장 방식에서 데이터베이스 저장 방식으로 전환하는 것이 매우 수월하다. (스프링의 장점)</br>
![image](https://user-images.githubusercontent.com/89215928/179356710-dfcc6584-71c7-48b0-925e-dac76ef9603e.png)



앞으로 추가되는 코드들은 한 번에 올릴 예정이다. 사실 복습을 한다는 생각으로 적는 글이었지만 왠지 이것때문에 공부 시간이 줄어드는 것 같아서...</br></br>
새로 배운 점, 주의할 사항, 진행 상황 등만 간략히 정리해서 작성할 것이다. 😊
