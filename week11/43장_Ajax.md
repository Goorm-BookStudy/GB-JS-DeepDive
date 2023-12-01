# ✏️ 43장 Ajax

## 📌 43.1 Ajax란?

- Ajax(Asynchronous JavaScript and XML)란 JS를 사용해 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신해 웹페이지를 동적으로 갱신하는 프로그래밍 방식
- 브라우저에서 제공하는 Web API인 XMLHttpRequest 객체를 기반으로 동작한다.
- 해당 객체는 HTTP 비동기 통신을 위한 메서드와 프로퍼티를 제공한다.

![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcWVKTZ%2FbtrvyhKmNHQ%2FKT5LQKT02FFMlQnYXsQ2F1%2Fimg.png)

- 이전의 웹페이지는 html 태그로 시작, html 태그로 끝나는 완전한 HTML을 서버로부터 받아 웹페이지 전체를 처음부터 다시 렌더링하는 방식으로 동작했다.
- 때문에 변경할 필요가 없는 부분까지 모두 재전송 받아 불필요한 데이터 통신이 발생했다.
- 이로 인해 화면 전환이 일어날 때 순간적으로 화면이 깜빡이는 현상이 발생한다.
- 클라이언트와 서버의 통신이 동기 방식으로 동작해 서버로부터 응답이 있을 때까지 다음 처리는 블로킹된다.

![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FW7c3q%2FbtrvoQVlp4G%2FQPaJXFtu6wYzFKOmAKK2Kk%2Fimg.png)

- Ajax의 등장으로 필요한 데이터만 비동기 방식으로 전달 받을 수 있게 됐다.
- 따라서 필요한 부분만 재렌더링 해 빠른 퍼포먼스와 부드러운 화면 전환이 가능하게 되었다.
- 화면이 깜빡이는 현상이 발생하지 않고, 비동기 방식을 동작하기에 블로킹이 발생하지 않는다.

## 📌 43.2 JSON

- JSON(JavasScript Object Notation)은 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷이다.
- JS에 종속되지 않는 언어 독립형 데이터 포맷으로 대부분의 프로그래밍 언어에서 사용한다.

### 1. JSON 표기 방식

- JS의 객체 리터럴과 유사하게 키와 값으로 구성된 순수 텍스트다.
- JSON의 키는 반드시 큰따옴표로 묶어야 한다.
- JSON의 값은 객체 리터럴과 같은 표기법을 그대로 사용할 수 있으나 문자열은 반드시 큰따옴표로 묶어야 한다.

### 2. JSON.stringify

- 객체를 JSON 포맷의 문자열로 변환한다.
- 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화 해야 하는데 이를 직렬화(serializing)라고 한다.

```
const obj = {
  name: 'Lee',
  age: 20,
  alive: true,
  hobby: ['traveling', 'tennis']
};

// 객체를 JSON 포맷의 문자열로 변환한다.
const json = JSON.stringify(obj);
console.log(typeof json, json);
// string {"name":"Lee","age":20,"alive":true,"hobby":["traveling","tennis"]}
```

- 객체 뿐만 아니라 배열도 JSON 포맷의 문자열로 변환한다.

```
const todos = [
  { id: 1, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 3, content: 'Javascript', completed: false }
];

// 배열을 JSON 포맷의 문자열로 변환한다. //null, 2는 들여쓰기를 위한 매개변수
const json = JSON.stringify(todos, null, 2);
console.log(typeof json, json);
/*
string [
  {
    "id": 1,
    "content": "HTML",
    "completed": false
  },
  {
    "id": 2,
    "content": "CSS",
    "completed": true
  },
  {
    "id": 3,
    "content": "Javascript",
    "completed": false
  }
]
*/
```

### 3. JSON.parse

- JSON 포맷의 문자열을 객체로 변환한다.
- 서버로부터 클라이언트에게 전송된 JSON 데이터는 문자열이다.
- 이 문자열을 객체로 사용하려면 JSON 포맷의 문자열을 객체화해야 한다.
- 이를 역직렬화(deserializing)이라고 한다.

```
const obj = {
  name: 'Lee',
  age: 20,
  alive: true,
  hobby: ['traveling', 'tennis']
};

// 객체를 JSON 포맷의 문자열로 변환한다.
const json = JSON.stringify(obj);

// JSON 포맷의 문자열을 객체로 변환한다.
const parsed = JSON.parse(json);
console.log(typeof parsed, parsed);
// object {name: "Lee", age: 20, alive: true, hobby: ["traveling", "tennis"]}
```

- 배열이 JSON 포맷의 문자열로 되어 있는 경우 JSON.parse는 문자열을 배열 객체로 변환한다.
- 배열의 요소가 객체인 경우 배열의 요소까지 객체로 변환한다.

## 📌 43.3 XMLHttpRequest

- 브라우저는 주소창이나 form, a 태그를 통해 HTTP 요청 전송 기능을 기본 제공한다.
- JS를 사용해 HTTP 요청을 전송하려면 XMLHttpRequest 객체를 사용한다.
- Web API인 해당 객체는 HTTP 요청 전송과 응답 수신을 위한 메서드, 프로퍼티를 제공한다.

### 1. XMLHttpReqeust 객체 생성

- XMLHttpReqeust 생성자 함수를 호출해 생성한다.
- 브라우저에서 제공하는 Web API라 브라우저 환경에서만 정상적으로 실행된다.

```
const xhr = new XMLHttpReqeust();
```

### 2. XMLHttpReqeust 객체의 프로퍼티와 메서드

#### 프로토타입 프로퍼티

![image](https://velog.velcdn.com/images/saiani1/post/7f77350c-d201-431d-a220-de15a67d47b9/image.png)
![image](https://velog.velcdn.com/images/saiani1/post/1059140a-701c-41cc-b5b5-e21a18b434d4/image.png)

#### 이벤트 핸들러 프로퍼티

![image](https://velog.velcdn.com/images/saiani1/post/42fd88c0-5af3-4f7b-865a-1e891ba0e96d/image.png)

#### 메서드

![image](https://velog.velcdn.com/images/saiani1/post/783d4a87-3197-4340-82ee-150326b248bc/image.png)

#### 정적 프로퍼티

![image](https://velog.velcdn.com/images/saiani1/post/95d0bac8-16ff-4ee0-b0e5-f10493d7ccac/image.png)
![image](https://velog.velcdn.com/images/saiani1/post/46387cab-cd5a-4a09-a508-272babd14838/image.png)

### 3. HTTP 요청 전송

- XMLHttpReqeust.prototype.open 메서드로 HTTP 요청을 초기화한다.
- 필요에 따라 XMLHttpReqeust.prototype.setRequestHeader 메서드로 특정 HTTP 요청의 헤더 값을 설정한다.
- XMLHttpReqeust.prototype.send 메서드로 HTTP 요청을 전송한다.

```
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('GET', '/users');

// HTTP 요청 헤더 설정
// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정: json
xhr.setRequestHeader('content-type', 'application/json');

// HTTP 요청 전송
xhr.send();
```

#### XMLHttpReqeust.prototype.open

- 서버에 전송할 HTTP 요청을 초기화한다.
- HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적을 알리는 방법이다.
  ![image](https://velog.velcdn.com/images/saiani1/post/a2b0be60-f37f-46da-a284-4e87ed730b8b/image.png)
  ![image](https://velog.velcdn.com/images/saiani1/post/6434d7f8-1ec1-4421-990f-687df4d81fa1/image.png)

#### XMLHttpReqeust.prototype.send

- open 요청으로 초기화된 HTTP 요청을 서버에 전송한다.
- 기본적으로 서버로 전송하는 데이터는 GET, POST 요청 메서드에 따라 전송 방식에 차이가 있다.
  - GET은 데이터를 URL의 일부분인 쿼리 문자열로 서버에 전송
  - POST는 데이터를 요청 몸체에 담아 전송
- send 메서드는 요청 몸체에 담아 전송할 데이터(페이로드)를 인수로 전달할 수 있다.
- 페이로드가 객체인 경우 반드시 JSON.stringify 메서드로 직렬화한 다음 전달해야 한다.
- HTTP 요청 메서드가 GET인 경우 send 메서드에 페이로드로 전달한 인수는 무시되고 요청 몸체는 null로 설정된다.

#### XMLHttpReqeust.prototype.setRequestHeader

- 특정 HTTP 요청의 헤더 값을 설정한다.
- 해당 메서드는 반드시 open 메서드 호출 후 호출해야 한다.
- Content-type은 요청 몸체에 담아 전송할 데이터의 MIME 타입의 정보를 표현한다.
  ![image](https://velog.velcdn.com/images/saiani1/post/838f8a69-246c-4b21-bd56-0ab47e2776a8/image.png)
  \*Multipurpose Internet Mail Extensions로 다양한 데이터를 전달하기 위한 표준을 말한다.
- 서버가 응답할 때 데이터의 MIME 타입을 Accept로 지정할 수 있는데, Accept 헤더를 설정하지 않으면 send 메서드가 호출될 때 */*로 전송된다.

### 4. HTTP 응답 처리

- 서버가 전송한 응답을 처리하기 위해 XMLHttpReqeust 객체가 발생시키는 이벤트를 캐치해야 한다.
- XMLHttpReqeust 객체의 이벤트 핸들러 중에서 HTTP 요청의 현재 상태를 나타내는 프로퍼티 readyState 값이 바뀔 경우 발생하는 이벤트 readystatechange를 캐치해 응답을 처리할 수 있다.

```
xhr.onreadystatechange = () => {
  if (xhr.readyState !== XMLHttpRequest.DONE) return;
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
  } else {
    console.error('Error', xhr.status, xhr.statusText);
  }
};
```

- 또는 load 이벤트를 캐치해도 좋다. 이 경우 XMLHttpRequest.DONE을 확인할 필요가 없다.

```
xhr.onload = () => {
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
  } else {
    console.error('Error', xhr.status, xhr.statusText);
  }
};
```
