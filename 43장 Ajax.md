# Ajax
## Ajax란?
- JS를 사용하여 브라우저가 서버에 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식
- 브라우저에 제공하는 Web API인 XMLHttpRequest 객체를 기반으로 동작 XMLHttpRequest는 HTTP 비동기 통신을 위한 메서드와 프로퍼티를 제공
- 기존 웹페이지는 화면 전환 시 서버로부터 새로운 HTML을 전송받아 웹페이지 전체를 처음부터 다시 렌더링 했다.

![](https://velog.velcdn.com/images/mintmin0320/post/182523c0-d8d2-4844-9989-6ed06485dbf5/image.png)

#### 단점
- 이전 웹페이지와 변경할 필요가 없는 부분도 재전송되어 받기 때문에 불필요한 데이터 통신 발생
- 변경 시 화면 전환이 일어나며 화면이 순간적으로 깜빡이는 현상 발생
- 클라이언트와 서버 통신이 동기 방식으로 동작하기 때문에 서버로부터 응답이 있을 때까지 다음 처리는 블로킹

<br/>

#### Ajax 방식 등장

![](https://velog.velcdn.com/images/mintmin0320/post/78471163-6aa4-4a09-9794-fa4a7328fec7/image.png)

- 변경이 필요한 부분의 데이터만 서버로부터 전송받기 때문에 불필요한 통신 절약
- 변경이 불필요한 부분은 렌더링 되지 않기 때문에 화면 깜빡임 x
- 클라이언트와 서버의 통신이 비동기 방식으로 작동해 블로킹 x

<br/>

## JSON
- 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷
- JS에 종속되지 않는 언어 독립형 데이터 포맷으로 대부분의 프로그래밍 언어에서 사용

<br/>

### JSON 표기 방식
- JS 객체 리터럴과 유사하게 키와 값으로 구성된 순수 텍스트
- JSON의 키는 반드시 큰따옴표로 묶고 값은 객체 리터럴과 같은 표기법을 그대로 사용 가능 하지만 문자열은 반드시 큰따옴표로 묶어야함

<br/>

### JSON.stringify
- 객체를 JSON 포맷의 문자열로 변환
- 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화 해야한다. 이를 객렬화라 한다.

```
const obj = {
	name: 'Lee";
    age: 20
};

// 객체를 JSON 포맷의 문자열로 변환
const json = JSON.stringify(obj);
console.log(type of json, json);
// string {"name": "Lee", "age": 20}
```

- 객체뿐 아니라 배열도 JSON 포맷의 문자열로 변환한다.

```
const arrs = [
  {content:'HTML', id:1},
  {content:'CSS', id:2},
  {conent:'Javascript', id:3}
 ];

JSON.stringify(arrs);
//'[{"content":"HTML","id":1},{"content":"CSS","id":2},{"conent":"Javascript","id":3}]'
```

<br/>

### JSON.parse
- JSON 포맷의 문자열을 객체로 변환
- 서버로 부터 전달된 JSON 데이터는 문자열이다. 이 문자열을 객체로 사용하려면 JSON 포맷의 문자열을 객체화 해야 한다. 이를 역직렬화라 한다.

```
const obj = {
	name: 'Lee',
    age: 20
}

const json = JSON.stringify(obj);

const parsed = JSON.parse(json);
console.log(typeof parsed, parsed);
// object {name: "Lee", age: 20}
```

- 배열 JSON 포맷의 문자열로 변환되어 있는 경우 JSON.parse는 무누자열을 배열 객체로 변환한다. 배열의 요소가 객체인 경우 배열의 요소까지 객체로 변환한다.

```
const todo = [
	{id:1, contnet: 'HTML', completed: false}
];

const json = JSON.stringify(todos);

const parsed = JSON.parse(json);
console.log(typeof parsed, parsed);
/*
	object [
    	{id: 1, content: "HTML', completed: false}
    ]
*/
```

<br/>

## XMLHttpRequest
- 브라우저는 주소창이나 HTML의 form 태그 또는 a 태그를 통해 HTTP 요청 전송 기능을 기본적으로 제공한다.
- XMLHttpRequest 객체는 HTML 요청 전송/응답 수신을 위한 다양한 메서드와 프로퍼티를 제공한다.

<br/>

### XMLHttpReuest 객체 생성
- XMLHttpRequest 생성자 함수를 호출해 생성
- 이 객체는 브라우저에서 제공하는 Web API로 브라우저 환경에서만 정상적으로 실행

```
const xhr = new XMLHttpRequest();
```

<br/>

### XMLHttpReuqest 객체의 프로퍼티와 메서드

XMLHttpRequest 객체의 프로포타입 프로퍼티

|프로토타입|프로퍼티|설명|
|---|-----|
|readyState|HTTP 요청의 현재 상태를 나타내는 정수 UNSETL 0, OPENDED: 1, HEADERS_RECEIVED: 2|
|status|	HTTP 요청에 대한 응답 상태를 나타내는 정수 ex 200|
|statusText|	HTTP 요청에 대한 응답 메시지를 나타내는 문자열 ex ok|
|responseType|	HTTP 응답 타입 ex document, json|
|response|	HTTP 요청에 대한 응답 몸체|
|responseText|	서버가 전송한 HTTP 요청에 대한 응답 문자열|
|XMLHttpRequest| 객체의 이벤트 핸들러 프로퍼티|

<br/>

#### XMLHttpRequest 객체의 이벤트 핸들러 프로퍼티

|이벤트 핸들러 프로퍼티	|설명|
|---|---|
|onreadystatechange|	readyState 프로퍼티 값이 변경된 경우|
|onloadstart|	HTTP 요청에 대한 응답을 받기 시작한 경우|
|onprogress|	HTTP 요청에 대한 응답을 받는 도중 주기적으로 발생|
|onabort|	abort 메서드에 의해 HTTP 요청이 중단된 경우|
|onerror|	HTTP 요청에 에러가 발생한 경우|
|onload|	HTTP 요청이 성공적으로 완료한 경우|
|ontimeout|	HTTP 요청 시간이 초과한 경우|
|onloadend|	HTTP 요청이 완료한 경우,HTTP 요청이 성공 또는 실패하면 발생|

<br/>

#### XMLHttpReuqest 객체의 메서드

|메서드|설명|
|---|---|
|open|HTTP 요청 초기화|
|send|HTTP 요청 전송|
|abort|이미 전송된 HTTP 요청 중단|
|setRequestHeader|특정 HTTP 요청 헤더의 값을 설정|
|getRequestHeader|특정 HTTP 요청 헤더의 값을 문자열로 반환|

<br/>

#### XMLHttpRequest 객체의 정적 프로퍼티

|정적 프로퍼티|값|설명|
|---|---|---|
|UNSET|0|open 메서드 호출 이전|
|OPEND|1|open 메서드 호출 이후|
|HEADERS_RECEIVED|2|send 메서드 호출 이후|
|LOADING|3|서버 응답 중(응답 데이터 미완성 상태)|
|DONE|4|서버 응답 완료|

<br/>

### HTTP 요청 전송
- HTTP 요청을 전송하는 경우 다음 순서를 따른다.
1. XMLHttpRequest.prototype.open 메서드로 HTTP 요청 초기화
2. 필요에 따라 XMLHttpRequest.prototype.setRequestHeader 메서드로 특정 HTTP 요청의 헤더 값을 설정
3. XMLHttpRequest.prototype.send 메서드로 HTTP 요청 전송

<br/>

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

<br/>

#### XMLHttpRequest.prototype.open

|매개변수|설명|
|---|---|
|method|HTTP 요청 메서드(get, post, put, delete 등)|
|url|HTTP 요청을 전송할 URL|
|async|비동기 요청 여부, 옵션으로 기본값은 true이며 비동기 방식으로 동작|

<br/>

|HTTP 요청 메서드|	종류|	목적|	페이로드|
|--|--|--|--|
|GET|	index/retrieve|	모든/특정 리소스 취득|	X|
|POST|	create|	리소스 생성	|O|
|PUT|	replace|	리소스의 전체 교체|	O|
|PATCH|	modify|	리소스의 일부 수정	|O|
|DELETE|	delete|	모든/특정 리소스 삭제|	X|

<br/>

#### XMLHttpRequest.prototpye.send
- send 메서드는 open 메서드로 초기화된 HTTP 요청을 서버에 전송한다. 기본적으로 서버로 전송하는 데이터는 GET, POST 요청 메서드에 따라 전송 방식에 차이가 있다.
- GET 요청 메서드의 경우 데이터를 URL의 일부분인 쿼리 문자열로 서버에 전송한다.
- POST 요청 메서드의 경우 데이터(페이로드)를 요청 몸체에 담아 전송한다.

<br/>

- send 메서드에는 요청 몸체에 담아 전송할 데이터(페이로드)를 인수로 전달할 수 있다.
- 페이로드가 객체인 경우 반드시 JSON.stringify 메서드를 사용하여 직렬화한 다음 전달해야 한다.

```
xhr.send(JSON.stringify({ id: 1, content: 'HTML', completed: false}));
```

- HTTP 요청 메서드가 GET인 경우 send 메서드에 페이로드로 전달한 인수는 무시되고 요청 몸체는 null로 설정된다.

#### XMLHttpRequest.prototype.setRequestHeader
setRequestHeader 메서드는 특정 HTTP 요청의 헤더 값을 설정한다. setRequestHeader 메서드는 반드시 open 메서드를 호출한 이후에 호출해야 한다. 자주 사용하는 HTTP 요청 헤더인 Content-type과 Accept에 대해 살펴보자.

Content-type은 요청 몸체에 담아 전송할 데이터의 MIME 타입의 정보를 표현한다. 자주 사용되는 MIME 타입은 다음과 같다.

####  
|MIME 타입| 서브타입|
|--|--|
|text|	text/plain, text/html, text/css, text/javascript|
|application|	application/json, application/x-www-form-urlencode|
|multipart|	multipart/formed-data|

- 다음은 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정하는 예다.

```
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('POST', '/users');

// HTTP 요청 헤더 설정
// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정: json
xhr.setRequestHeader('content-type', 'application/json');

// HTTP 요청 전송
xhr.send(JSON.stringify({ id: 1, content: 'HTML', completed: false }));
```

- HTTP 클라이언트가 서버에 요청할 때 서버가 응답할 데이터의 MIME 타입을 Accept로 지정할 수 있다. 다음은 서버가 응답할 데이터의 MIME 타입을 지정하는 예다.

```
// 서버가 응답할 데이터의 MIME 타입 지정: json
xhr.setRequestHeader('accept', 'application/json');
```

- 만약 Accept 헤더를 설정하지 않으면 send가 호출될 때 Accept 헤더가 */*으로 전송된다.

<br/>

### HTTP 응답 처리
- 서버가 전송한 응답을 처리하려면 XMLHttpRequest 객체가 발생시키는 이벤트를 캐치해야 한다. XMLHttpRequest 객체는 onreadystatechange, onload, onerror 같은 이벤트 핸들러 프로퍼티를 갖는다. 
- 이 이벤트 핸들러 프로퍼티 중에서 HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티 값이 변경된 경우 발생하는 readystatechange 이벤트를 캐치하여 다음과 같이 HTTP 응답을 처리할 수 있다.
- XMLHttpRequest 객체는 브라우저에서 제공하는 Web API이므로 다음 예제는 반드시 브라우저 환경에서 실행해야 한다.
- 참고로 HTTP 요청을 전송하고 응답을 받으려면 서버가 필요하다. 다음 예제에서는 JSONPlaceholder에서 제공하는 가상 REST API를 사용한다.

```
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
// https://jsonplaceholder.typicode.com은 Fake REST API를 제공하는 서비스다.
xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

// HTTP 요청 전송
xhr.send();

// readystatechange 이벤트는 HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티가
// 변경될 때마다 발생한다.
xhr.onreadystatechange = () => {
  // readyState 프로퍼티는 HTTP 요청의 현재 상태를 나타낸다.
  // readyState 프로퍼티 값이 4(XMLHttpRequest.DONE)가 아니면 서버 응답이 완료되지 않은 상태다.
  // 만약 서버 응답이 아직 완료되지 않았다면 아무런 처리를 하지 않는다.
  if (xhr.readyState !== XMLHttpRequest.DONE) return;
  
  // status 프로퍼티는 응답 상태 코드를 나타낸다.
  // status 프로퍼티 값이 200이면 정상적으로 응답된 상태이고
  // status 프로퍼티 값이 200이 아니면 에러가 발생한 상태다.
  // 정상적으로 응답된 상태라면 response 프로퍼티에 서버의 응답 결과가 담겨 있다.
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
    // {userId: 1, id: 1, title: "delectus aut autem", completed: false}
  } else {
    console.error('Error', xhr.status, xhr.statusText);
  }
};

```

- send 메서드를 통해 HTTP 요청을 서버에 전송하면 서버는 응답을 반환한다. 하지만 언제 응답이 클라이언트에 도달할지는 알 수 없다. 따라서 readystateonchange 이벤트를 통해 HTTP 요청의 현재 상태를 확인해야 한다.
- readystatechange 이벤트는 HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티가 변경될 때마다 발생한다.
- onreadystatechange 이벤트 핸들러 프로퍼티에 할당한 이벤트 핸들러는 HTTP 요청의 현재 상태를 나타내는 xhr.readyState가 XMLHttpRequest.DONE인지 확인하여 서버의 응답이 완료되었는지 확인한다.
- 서버의 응답이 완료되면 HTTP 요청에 대한 응답 상태(HTTP 상태 코드)를 나타내는 xhr.status가 200인지 확인하여 정상 처리와 에러 처리를 구분한다. HTTP 요청에 대한 응답이 정상적으로 도착했다면 요청에 대한 응답 몸체를 나타내는 xhr.response에서 서버가 전송한 데이터를 취득한다. 만약 xhr.status가 200이 아니면 에러가 발생한 상태이므로 필요한 에러 처리를 한다.
- readystatechange 이벤트 대신 load 이벤트를 캐치해도 좋다. load 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생한다. 따라서 load 이벤트를 캐치하는 경우 xhr.readyState가 XMLHttpRequest.DONE인지 확인할 필요가 없다.

```
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
// https://jsonplaceholder.typicode.com은 Fake REST API를 제공하는 서비스다.
xhr.open("GET", "https://jsonplaceholder.typicode.com/todos/1");

// HTTP 요청 전송
xhr.send();

// readystatechange 이벤트는 HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티가
// 변경될 때마다 발생한다.
xhr.onreadystatechange = () => {
  // readyState 프로퍼티는 HTTP 요청의 현재 상태를 나타낸다.
  // readyState 프로퍼티 값이 4(XMLHttpRequest.DONE)가 아니면 서버 응답이 완료되지 상태다.
  // 만약 서버 응답이 아직 완료되지 않았다면 아무런 처리를 하지 않는다.
  if (xhr.readyState !== XMLHttpRequest.DONE) return;

  // status 프로퍼티는 응답 상태 코드를 나타낸다.
  // status 프로퍼티 값이 200이면 정상적으로 응답된 상태이고
  // status 프로퍼티 값이 200이 아니면 에러가 발생한 상태다.
  // 정상적으로 응답된 상태라면 response 프로퍼티에 서버의 응답 결과가 담겨 있다.
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
    // {userId: 1, id: 1, title: "delectus aut autem", completed: false}
  } else {
    console.error("Error", xhr.status, xhr.statusText);
  }
};

```

