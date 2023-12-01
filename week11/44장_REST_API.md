# ✏️ 44장 REST API

- REST(REpresentational State Transfer)는 HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍쳐다.
- REST API는 이런 REST를 기반으로 서비스 API를 구현한 것을 의미한다.
- REST의 기본 원칙을 성실히 지킨 서비스 디자인을 RESTful이라고 표현한다.

## 📌 44.1 REST API의 구성

- 자원, 행위, 표현 3가지의 요소로 구성된다.
- REST는 자체 표현 구조로 REST API만으로 HTTP 요청의 내용을 이해할 수 있다.

![image](https://velog.velcdn.com/images/saiani1/post/77dd8078-708b-461d-aef3-b6bad25de645/image.png)

## 📌 44.2 REST API 설계 원칙

- URI는 리소스를 표현하는데 집중하고, 행위에 대한 정의는 HTTP 요청 메서드를 통해 해야 한다.
  - URI에 리소스를 식별할 수 있는 이름으로는 명사를 사용한다. (동사 지양)
  - 리소스에 대한 행위는 HTTP 요청 메소드에 표현되므로 URI에는 표현하지 않는다.
