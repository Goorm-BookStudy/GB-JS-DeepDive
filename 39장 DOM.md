# 39장 DOM
- HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조

<br/>

## 노드
### HTML 요소와 노드 객체
- HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소노드 객체로 변환
- 이때 HTML 요소의 어트리뷰트는 어트리뷰트 노드로, HTML 요소의 텍스트 콘텐츠는 텍스트 노드로 변환

```
<div class="greeting">Hello</div>

div 요소 노드
class="greeting" 어트리뷰트 노드
"Hello" 텍스트 노드
```

- HTML 문서는 HTML 요소들의 집합으로 이뤄지고 HTML 요소는 중첩 관계를 갖는다. (부자관계 형성)

<br/>

#### 트리 자료구조
- 노드들의 계층 구조로 이뤄진다. (부모, 자식 노드로 구성)
- 비선형 자료구조(하나의 자료 뒤에 여러 개의 자료가 존재할 수 있는 구조) 
- 하나의 최상위 노드에서 시작하고 최상위 노드는 부모가 없으며 루트 노드라 한다.
- 자식 노드가 없는 노드 -> 리프 노드

> ⭐️ 노드 객체들로 구성된 트리자료구조 -> DOM, 노드 객체의 트리로 구조화되어있기 때문에 DOM 트리라고 부르기도 한다.

<br/>

### 노드 객체의 타입
#### 문서 노드
- DOM 트리의 최상위에 존재하는 루트노드 document를 가리킨다.(진입점 역할)
- document 객체는 브라우저가 렌더링한 HTML 문서 전체를 가리키는 객체로 전역 객체 window의 document 프로퍼티에 바인딩되어있다.
- HTML 문서당 document 객체는 유일하다.

<br/>

#### 요소 노드
- HTML 요소를 가리키는 객체
- 중첩에 의해 부자관계를 가지며 정보를 구조화

<br/>

#### 어트리뷰트 노드
- HTML 요소의 어트리뷰트를 가리키는 객체
- 어트리뷰트가 지정된 HTML 요소의 요소 노드와 연결되어 있다. 단, 어트리뷰트 노드는 부모 노드와 연결되어 있지 않고 요소 노드에만 연결되어 있다.
- 어트리뷰트 노드를 참조하거나 변경할 경우 먼저 요소 노드에 접근해야함.

<br/>

#### 텍스트 노드
- HTML 요소의 텍스트를 가리키는 객체
- 문서의 정보를 표현
- 요소 노드의 자식 노드이며 자식 노드를 가질 수 없는 리프 노드다.
- DOM 트리의 최종단으로 접근을 위해서는 부모 노드인 요소 노드에 접근해야함.

<br/>

- 주석을 위한 Comment 노드, DOCTYPE을 위한 DocumentType 노드, 복수의 노드를 생성하여 추가할 때 사용하는 DocumentFragment 노드 등 총 12개의 노드 타입이 있다.

<br/>

### 노드 객체의 상속 구조
- 브라우저에서 제공하는 호스트 객체지만, JS 객체이므로 프로토타입에 의한 상속 구조를 갖는다.
- 모든 노드 객체는 Object, EventTarget, Node 인터페이스를 상속받는다.
- 문서 노드는 Document, HTMLDocument 인터페이스를 상속받고 어트리뷰트 노드는 Attr, 텍스트 노드는 CharacterData 인터페이스를 각각 상속받는다.
- 요소 노드는 Element 인터페이스를 상속받고 HTMLElement와 태그의 종류별로 세분화된 HTMLHtmlElemnet, HTMLHeadElement, HTMLBodyElement, HTMLUListElement 등의 인터페이스를 상속받는다.

- input 요소의 경우 이벤트를 발생시키는 객체 EventTarge 등의 객체를 상속받기 때문에 요소로 사용 가능하다.

<br/>

- 노드 객체에는 모든 노드가 공통으로 갖는 기능도 있고 타입에 따라 갖는 고유한 기능도 있다.
- HTML 요소가 객체화된 요소 노드 객체는 HTML 요소가 갖는 공통적인 기능이 있다. ex) HTML 요소의 스타일을 나타내는 style 프로퍼티의 경우 HTMLElement 인터페이스가 제공한다.
- input 요소 노드 객체에는 value 프로퍼티라는 고유한 기능을 갖는다.

<br/>

## 요소 노드 취득
### id를 이용한 요소 노드 취득
- Document.prototype.getElementById 메서드는 인수로 전달한 id 어트리뷰트 값을 갖는 하나의 요소 노드를 탐색하여 반환한다.
- Document.prototype의 프로퍼티 메서드이므로(getElementById) 반드시 document를 통해 호출해야함
- id는 문서 내에 유일한 값이어야 하며, class 어트리뷰트와 달리 공백 문자로 구분하여 여러 개의 값을 가질 수 없다. (중복된 값 존재해도 에러 , 이 경우 첫 번째 요소 노드만 반환)
- 인수로 전달된 id 값을 갖는 HTML 요소가 존재하지 않는 경우 null 반환

<br/>

### 태그 이름을 이용한 요소 노드 취득
- Document.prototype/Element.prototype.getElementsByTagName 메서드는 인수로 전달한 태그 이름을 갖는 모든 요소 노드들을 탐색하여 반환
- Elements는 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 전체인 HTMLCollection 객체를 반환한다. 이 때 메서드가 반환하는 DOM 컬렉션 객체인 HTMLCollection 객체는 유사 배열 객체이면서 이터러블이다.

```
<script>
	const $elems = document.getElementsByTagName('li');
    
    [...$elems].forEach(elem => {elem.style.color = 'red'; });
</script>
```

<br/>

- HTML 문서의 모든 요소 노드를 취득하려면 getElementsByTagName 메서드의 인수로 *를 전달한다.
- Document.prototype.getElementsByTagName는 document를 통해 호출하며 DOM 전체 요소 노드를 탐색하고 반환 Element.prototype.getElementsByTagName는 특정 요소 노드의 자손 노드 중에서 요소노드를 탐색하여 반환

<br/>

### class를 이용한 요소 노드 취득
- Document.prototype/Element.prototype.getElementByClassName 메서드 인수로 전달한 class 어트리뷰트 값을 갖는 모든 요소 노드들을 탐색하여 반환
- 인수로 전달할 class 값을 공백으로 구분하여 여러 개의 class를 지정할 수 있다.
- TagName 메서드와 동일하게 여러 개의 요소 노드 객체 반환이 가능하다.
- 인수로 전달된 class 값을 갖는 요소가 존재하지 않으면 getElementByClassName 메서드는 빈 HTMLCollection 객체를 반환한다.

<br/>

### CSS 선택자를 이용한 요소 노드 취득
- CSS 선택자는 스타일을 적용하고자하는 HTML 요소를 특정할 때 사용하는 문법

```
// 전체 선택자: 모든 요소 선택
* {}

// 태그 선택자: 모든 p 태그 요소를 모두 선택
p {}

// id 선택자: id 값이 'foo'인 요소를 모두 선택
#foo {}

// class 선택자: class 값이 'foo'인 요소를 모두 선택
.foo {}

// 어트리뷰트 선택자: input 요소 중에 type 어트리뷰트 값이 'text'인 요소를 모두 선택
input[type=text} {}

// 후손 선택자: div 요소의 후손 요소 중 p 요소를 모두 선택
div p {}

// 자식 선택자: div 요소의 자식 요소 중 p 요소를 모두 선택
div > p {}

// 인접 형제 선택자: p 요소의 형제 요소 중에 p 요소 바로 뒤에 위치하는 ul 요소를 선택
p + ul {}

// 일반 형제 선택자: p 요소의 형제 요소 중에 P 요소 뒤에 위치하는 ul 요소를 모두 선택
p ~ ul {}

// 가상 클래스 선택자: hover 선택인 a 요소를 모두 선택
a: hover {}

// 가상 요소 선택자: p 요소의 콘텐츠의 앞에 위치하는 공간을 선택 일반적으로 content 프로퍼티와 함께 사용
p::before {}
```

<br/>

- Document.prototype/Element.prototype.querySelector 메서드는 인수로 전달한 CSS 선택자를 만족시키는 하나의 요소 노드를 탐색해 반환
- 인수로 전달한 CSS 선택자를 만족시키는 요소 노드가 여러개인 경우 첫 번째 요소만 반환, 존재하지 않으면 null 반환, 문법에 맞지 않으면 DOMException 에러 발생

<br/>

- Document.prototype/Element.prototype.querySelectorAll 메서드는 인수로 전달한 CSS 선택자를 만족시키는 모든 요소 노드를 탐색하여 반환
- 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 NodeList 객체를 반환 이 객체는 유사 배열 객체이면서 이터러블이다.
- 만족시키는 요소가 존재하지 않으면 빈 NodeList 객체 반환, 문법에 맞지 않으면 DOMException 에러 발생
- HTML 문서 내 모든 요소를 취득하려면 querySelectorAll 인수로 * 전달

<br/>

- Document.prototype에 정의된 메서드는 DOM 전체에서 요소 노드를 탐색하여 반환하고 Element.prototyp에 정의된 메서드는 특정 요소 노드만 호출하여 특정 요소 노드의 자손 노드 중에서 탐색하여 반환
- querySelector, querySelectorAll 메서드의 경우 getElementById 메서드보다 느린 것으로 알려져있지만 CSS 문법을 사용하여 좀 더 구체적인 조건으로 요소 노드를 취득할 수 있고 일관된 방식으로 요소 노드를 취득할 수 있다는 장점이 있다.

<br/>

### 특정 요소 노드를 취득할 수 있는지 확인
- Element.prototype.matches 메서드는 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할  수 있는지 확인

```
<ul id="fruits>
	<li class="apple">
	<li class="banana">
</ul>
<script>
const $apple = document.querySelector('.apple');

console.log($apple.matches('#fruits > li.apple')); // true
console.log($apple.matches('#fruits > li.banana')); // false
</script>
```

<br/>

### HTMLCollection과 NodeList
- DOM API가 여러 개의 결과값을 반환하기 위한 DOM 컬렉션 객체다.
- 유사 배열 객체이면서 이터러블이다. ( for...of 문으로 순회 가능, 스프레드 문법 사용 가능)
- 상태 변화를 실시간으로 반영하는 살아있는 객체다. HTMLCollection의 경우 실시간으로 반영, NodeList의 경우 none-live 객체로 동작하지만 경우에 따라 live 객체라고 부르기도 한다.

<br/>

#### HTMLCollection
- getElementByTagName, getElementByClassName 메서드가 반환하는 HTMLCollection 객체는 노드 객체의 상태 변화를 실시간으로 반영하는 살아있는 DOM 컬렉션 객체다.

```
<head>
	<style>
    	.red {color: red;}
        .blue {color: blue;}
   	</style>
</head>

<ul id="fruits>
	<li class="red">Apple</li>
	<li class="red">Banana</li>
   	<li class="red">Orange</li>
</ul>
<script>
	const $elems = document.getElementByClassName('red');
    console.log($elems); // HTMLCollenction(3) [li.red,li.red,li.red]
    
    for(let i = 0; i < $elems.length; i++){
    	$elems[i].className = 'blue';
    }
    
    // HTMLCollection 객체의 요소가 3개에서 1개로 변경
    	console.log($elems); // HTMLCollenction(1) [li.red]
</script>
```

- 모든 class 값을 red에서 blue로 변경한다. 이 과정에서 오류가 발생하는데 그 이유는 실시간으로 변화가 반영되기 때문에 3번 반복될 때 처음 red에서 blue로 변경되며 두 번째 반복 때 HTMLCollection이 2개가 되어버리고 elems[1]이 기존 세 번째 li 요소로 변경된다
- for 문 역방향 순회, while 문으로 HTMLCollection 요소가 남지 않을 때까지 무한 반복, 부작용 발생 원인인 HTMLCollection 객체 사용 지양 등의 방법이 있다.

```
// 유사 배열 객체이면서 이터러블인 HTMLCollection을 배열로 변환하여 순회
[...$elems].forEach(elem => elem.className = 'blue');
```

<br/>

#### NodeList
- querySelectorAll 메서드는 DOM 컬렉션 객체인 NodeList 객체를 반환한다. 이때 NodeList 객체는 실시간으로 노드 객체의 상태 변경을 반영하지 않는 객체다.
- querySelectorAll이 반환하는 NodeList 객체는 NodeList.prototype.forEach 메서드를 상속받아 사용할 수 있다. 
- 대부분의 경우 실시간으로 노드 객체의 변경을 반영하지 않고 과거의 정적 상태를 유지하는 non-live 객체로 동작한다. 하지만 childNodes 프로퍼티가 반환하는 NodeList 객체는 실시간으로 노드 객체의 상태 변경을 반영하는 live 객체로 동작하므로 주의가 필요하다.

<br/>

- HTMLCollection, NodeList의 경우 예상과 다르게 동작할 때가 있다.
- 노드 객체의 상태 변경과 상관없이 안전하게 DOM 컬렉션을 사용하려면 HTMLCollection이나 NodeList 객체를 배열로 변환하여 사용하는 것을 권장한다. (변환 시 배열의 유용한 고차 함수(forEach, map) 사용 가능)
- 스프레드 문법 Or Array.from 메서드로 간단하게 배열로 변경 가능

<br/>

## 노드 탐색
- 모두 접근자 프로퍼티다 단, 노드 탐색 프로퍼티는 setter 없이 getter만 존재하여 참조 가능한 읽기 전용 프로퍼티다.
- 값을 할당해도 아무런 에러 없이 무시된다.

<br/>

### 공백 텍스트 노드
- HTML 요소 사이의 스페이스, 탭, 줄바꿈(개행) 등의 공백 문자는 텍스트 노드를 생성한다.
- HTML 문서의 공백 문자를 제거하면 생성하지 않지만 가독성이 좋지 않다.

<br/>

### 자식 노드 탐색
- 노드 탐색 프로퍼티를 사용해야 한다.

|프로퍼티|설명|
|:---:|:---:|
|Node.prototype.childNodes|자식 노드를 모두 탐색하여 DOM 컬렉션인 NodeList에 담아 반환 chidNodes 프로퍼티가 반환한 NodeList에는 요소 노드 뿐만 아니라 텍스트 노드도 포함되어 있다.|
|Element.prototype.children|자식 노드 중에서 요소 노드만 모두 탐색하여 DOM 컬렉션 객체인 HTMLCollectio에 담아 반환 children 프로퍼티가 반환한 HTMLCollectio에는 텍스트 노드 포함 x|
|Node.prototype.firstChild|첫 번째 자식 노드 반환, firstChild 프로퍼티가 반환한 노드는 텍스트 노드이거나 요소 노드다.|
|Node.prototype.lastChild|마지막 자식 노드 반환, lastChild 프로퍼티가 반환한 노드는 텍스트 노드이거나 요소 노드다.|
|Element.prototype.firstElementChild|첫 번째 자식 요소 노드를 반환, firstElementChild 프로퍼티는 요소 노드만 반환|
|Element.prototype.lastElementChild|마지막 자식 요소 노드를 반환, lastElementChild 프로퍼티는 요소 노드만 반환|

<br/>

### 자식 노드 존재 확인
- Node.prototype.hasChildNodes 메서드 사용
- 존재하면 true 아니면 false 반환
- childNodes 프로퍼티와 마찬가지로 텍스트 노드를 포함하여 자식 노드의 존재를 확인
- 자식 노드 중 텍스트 노드가 아닌 요소 노드가 존재하는지 확인하려면 children.length or Element 인터페이스의 childElement 프로퍼티 사용

```jsx
<body>
	<ul id="fruits">
	</ul>
</body>
<script>
	const $fruits = document.getElementById('fruits');
  
  	console.log($fruits.hasChildNodes()); // true
  
  // 자식 노드 중 텍스트 노드가 아닌 요소 노드가 존재하는지 확인
  	console.log(!!$fruits.hasChildNodes()); // 0 -> false
  	console.log(!!$fruits.childElementCount); // 0 -> false
</script>
```

<br/>

### 요소 노드의 텍스트 노드 탐색
- 요소 노드의 텍스트 노드는 firstChild 프로퍼티로 접근 가능

```
<div id="foo">hello</div>
<scirpt>
	console.log(document.getElementById('foo').firstChild); // #text
</scirpt>
```

<br/>

### 부모 노드 탐색
- Node.prototype.parentNode 프로퍼티 사용
- 텍스트 노드는 DOM 트리의 최종단 노드인 리프 노드이므로 부모 노드가 텍스트 노드인 경우는 없다.

<br/>

### 형제 노드 탐색
- 어트리뷰트 노드는 요소 노드와 연결되어 있지만 부모 노드가 같은 형제 노드가 아니기 때문에 반환 x

```
<ul id="fruits>
	<li class="red">Apple</li>
	<li class="red">Banana</li>
   	<li class="red">Orange</li>
</ul>
<script>
	const $fruits = document.getElementById('fruits');
    
    // firstChild 프로퍼티는 요소 노드 뿐만 아니라 텍스트 노드를 반환할 수도 있디.
	const { firstChild } = $fruits;
    console.log(firstChild); // #text
    
    // nextSibling 프로퍼티는 요소 노드뿐만 아니라 텍스트 노드를 반환할 수도 있다.
    const { nextSibling } firstChild;
    console.log(nextSibling); // li.apple
    
    // previousSibling 프로퍼티는 요소 노드뿐만 아니라 텍스트 노드를 반환할 수도 있다.
    const {previousSibling} = nextSibling;
    console.log(previousSibling); // #text
    
    // firstElementChild 프로퍼티 요소 노드만 반환
    const { firstElementChild } = $fruits;
    console.log(firstElementChild); // li.apple
    
    // nextElementSibling 프로퍼티는 요소 노드만 반환
    const {nextElementSibling} = firstElementChild;
    console.log(nextElementSibling); // li.banana
    
    // previousElementSibling 프로퍼티 요소 노드만 반환
    const {previousElementSibling} = nextElementSibling;
    console.log(previousElementSibling); // li.apple
</script>
```

<br/>

## 노드 정보 취득

|프로퍼티|설명|
|:---:|:---:|
|Node.prototype.nodeType|노드 객체의 종류, 즉 노드 타입을 나타내는 상수를 반환 노드 타입 상수는 Node에 정의되어있다. 1. Node.ELEMENT_NODE: 요소 노드 타입을 나타내는 상수 1을 반환 2. Node.TEXT_NODEL 텍스트 노드 타입을 나타내는 상수 3을 반환 3. Node.DOCUMENT_NODE: 문서 노드 타입을 나타내는 상수 9를 반환|
|Node.prototype.nodeName|노드의 이름을 문자열로 반환 1. 요소 노드: 대문자 문자열로 태그 이름("UL", "LI" 등)을 반환, 2. 텍스트 노드: 문자열 "#text"를 반환, 3. 문서 노드: 문자열 "#document"를 반환|

```
<scrpit>
	console.log(document.nodeType); // 9
    console.log(document.nodeName); // #document
    
    const $foo = document.getElementById('foo');
    console.log($foo.nodeType); // 1
    console.log($foo.nodeName); // DIV
</scrpit>
```

<br/>

## 요소 노드의 텍스트 조작
### nodeValue
- 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티다. => 참조와 할당 모두 가능
- 프로퍼티를 참조하면 노드 객체의 값을 반환한다. 노드 객체의 값이란 텍스트 노드의 텍스트다.
- 텍스트 노드가 아닌 노드, 즉 문서 노드나 요소 노드의 nodeValue 프로퍼티를 참조하면 null을 반환

<br/>

- 요소 노드의 텍스트를 변경하려면 다음과 같은 순서의 처리가 필요하다
1. 텍스트를 변경할 요소 노드를 취득한 다음 취득한 요소 노드의 텍스트 노드를 탐색, 텍스트 노드는 요소 노드의 자식 노드이므로 firstChil 프로퍼티를 사용하여 탐색 <br/>
2. 탐색한 텍스트 노드의 nodeValue 프로퍼티를 사용하여 테스트 노드의 값을 변경

<br/>

### textContent
- Node.prototype.textContent 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 텍스트와 모든 자손 노드의 텍스트를 모두 취득하거나 변경한다.
- 요소 노드의 textContent 프로퍼티를 참조하면 요소 노드의 콘텐츠 영역 내의 텍스트를 모두 반환
- 요소 노드의 childNodes 프로퍼티가 반환한 모든 노드들의 텍스트 노드의 값 즉 텍스트를 모두 반환 이때 HTML 마크업 무시

<br/>

```
<div id="foo">heelo<span>world</span></div>
<script>
	// // #foo 요소 노드의 텍스트를 모두 취득한다 이때 HTML 마크업 무시
	console.log(document.getElementById('foo').textContent); // hello world
</scirpt>
```

<br/>

- nodeValue 프로퍼티를 사용하면 textContent 프로퍼티를 사용할 때와 비교해서 코드가 더 복잡하다.
- textContent 프로퍼티와 유사한 동작을 하는 innerText 프로퍼티가 있다.
- innerText 프로퍼티는 CSS에 순종적이다. 예를 들어 innerText 프로퍼티는 CSS에 의해 비표시(visibility: hidden;)로 지정된 요소 노드의 텍스트를 반환하지 않는다.
- innerText 프로퍼티는 CSS를 고려해야 하므로 textContent 프로퍼티보다 느리다.(권장 x)

<br/>

## DOM 조작
- 새로운 노드를 생성하여 DOM에 추가하거나 기존 노드를 삭제 또는 교체하는 것
- DOM 조작에 의해 DOM에 새로운 노드가 추가되거나 삭제되면 리플로우와 리페인가 발생하는 원인이 되므로 성능에 악영향을 준다.
- 복잡한 컨텐츠를 다루는 DOM 조작은 성능 최적화를 위해 주의가 필요

<br/>

### innerHTML
- Element.prototype.innerHTML 프로퍼티는 setter, getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 HTML 마크업을 취득하거나 변경한다.
- 요소 노드의 innerHTML 프로퍼티를 참조하면 요소 노드의 콘텐츠 영역 내에 모든 HTML 마크업을 문자열로 반환한다.
- innerHTML 프로퍼티는 HTML 마크업이 포함된 문자열을 그대로 반환 <-> textContent
- 요소 노드의 innerHTML 프로퍼티에 문자열을 할당하면 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열에 포함되어 있는 HTML 마크업이 파싱되어 요소 노드의 자식 노드로 DOM에 반영

```
<div id="foo">Hello <span>world!</span></div>
<script>
	document.getElementById('foo').innerHTML = `hi <span>there</span>`;
</script>
```

<br/>

#### innerHTML 프로퍼티를 사용하면 HTML 마크업 문자열로 간단히 DOM 조작이 가능하다.
- 요소 노드의 innerHTML 프로퍼티에 할당한 HTML 마크업 문자열을은 렌더링 엔진에 의해 파싱되어야 하는 요소 노드의 자식으로 DOM에 반영된다.
- 이때 입력받은 데이터를 그대로 innerHTML프로퍼티에 할당하는 것은 크로스 사이트 스크립팅 공격에 취약하므로 위험하다.(HTML 마크업 내에 JS 악성 코드가 포함되어 있으면 파싱 과정에서 그대로 실행될 가능성이 있다.)
- 요소 노드의 innerHTML 프로퍼티에 HTML 마크업 문자열을 할당할 경우 요소 노드의 모든 자식 노드를 제거하고 할당한 HTML 마크업 문자열을 파싱하여 DOM을 변경하게 된다. -> 기존의 자식노드를 제거해 다시 새롭게 자식노드를 생성하여 DOM에 반영 -> 비효율적
- 새로운 요소를 삽입할 때 삽입될 위치를 지정할 수 없다는 단점이 있다

<br/>

> 복잡하지 않은 요소를 새롭게 추가할 때는 유용하지만 기존 요소를 제거하지 않고 새로운 요소를 삽입해야할 경우에는 사용 적합 x

<br/>

### insertAdjacentHTML
- Element.prototype.insertAdjacentHTML 메서드는 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소 삽입
- 두 번째 인수로 전달한 HTML 마크업 문자열을 파싱하고 그 결과로 생성된 노드를 첫 번째 인수로 전달한 위치에 삽입하여 DOM에 반영
- 첫 번째 인수로 전달 가능한 문자열은 beforebegin, afterbegin, beforeend, afterend의 4가지다.
- 기존 요소에 영향 x, 새롭게 삽입될 요소만을 파싱하여 자식 요소로 추가 -> innerHTML의 새롭게 자식 노드를 생성하여 생성하는 방식보다 효율적이고 빠르다.
- 단, innerHTML 프로퍼티와 동일하게 HTML 마크업 문자열을 파싱하므로 크로스 사이트 스크립팅 공격에 취약하다.

<br/>

### 노드 생성과 추가
#### 요소 노드 생성
- Document.prototype.createElement(tagName) 메서드는 요소 노드를 생성하여 반환
- 매개변수에는 태그 이름을 나타내는 문자열을 인수로 전달
- 생성한 요소는 기존 DOM에 추가하는 것이 아닌 홀로 존재하는 상태이다 생성만 하고 DOM에 추가는 X
- 따라서 아무런 자식 노드가 없다.

```
const $li = document.createElement('li');
```

<br/>

#### 텍스트 노드 생성
- Document.prototype.createTextNode(text) 메서드는 텍스트 노드를 생성하여 반환 매개변수로 텍스트 노드의 값으로 사용할 문자열을 전달.
- 마찬가지로 홀로 존재하는 상태이다.

```
const textNode = document.createTextNode('Banana');
```

<br/>

#### 텍스트 노드를 요소 노드의 자식 노드로 추가
- Node.prototype.appendChild(childNode) 메서드는 매개변수 childNode에게 인수로 전달한 노드를 appendChild 메서드를 호출한 노드의 마지막 자식 노드로 추가
- 인수로 createTextNode 메서드로 생성한 텍스트 노드를 전달하면 appendChild 메서드를 호출한 노드의 마지막 자식 노드로 텍스트 노드가 추가

```
$li.appendChild(textNode);
```


- 부자관계로 연결은 되었지만 아직 기존 DOM에 추가되지 않은 상태
- 자식 노드가 하나도 없는 경우에는 textContent 프로퍼티를 사용하는 편이 더 간결하다.

```
$li.textContent = 'Banana';
```

<br/>

#### 요소 노드를 DOM에 추가
- Node.prototype.appendChild 메서드를 사용하여 텍스트 노드와 부자 관계로 연결한 요소 노드를 #fruits 요소 노드의 마지막 자식 요소로 추가
- 이 과정에서 DOM에 추가되고 DOM에 한번 추가하므로 DOM은 한 번 변경된다. (이때 리플로우, 리페인트 실행)

```
$fruits.appendChild($li);
```

<br/>

### 복수의 노드 생성과 추가
- 요소를 DOM에 여러 번 추가하면 매번 리플로우와 리페인트가 발새하므로 횟수를 줄여야한다.
- 컨테이너 요소를 미리 생성하고 컨테이너 요소에 자식 노드를 추가하고 컨테이너 요소를 #fruits 요소에 자식 노드로 추가한다면 DOM은 한 번만 변경된다.
- div같은 불필요한 요소를 생략하는 방법으로는 DocumentFragment 노드가 있다. 부모 노드가 없어서 기존 DOM과 별도로 존재한다.
- DocumentFragment 노드에 자식 노드를 추가하여도 가존 DOM에는 변경 x 또한 기존 DOM에 추가하면 자신은 제거되고 자식 노드만 DOM에 추가된다.
- 여러 개의 요소 노드를 DOM에 추가하는 경우 DocumentFragment 노드를 사용하는 것이 더 효율적이다.

<br/>

### 노드 삽입
#### 마지막 노드로 추가
- Node.prototype.appendChild 메서드는 인수로 전달받은 노드를 자신을 호출한 노드의 마지막 자식 노드로 DOM에 추가 이때 위치 지정 불가하고 마지막 자식 노드로 추가

<br/>

#### 지정한 위치에 노드 삽입
- Node.prototype.insertBefore 메서드는 첫 번째 인수로 전달받은 노드를 두 번째 인수로 전달받은 노드 앞에 삽입
- 두 번째 인수로 전달받은 노드는 반드시 insertBefore 메서드를 호출한 노드의 자식 노드이어야 한다. 그렇지 않으면 DOMException 에러 발생, 두 번째 인수로 전달받은 노드가 null이면 첫 번째 인수로 전달받은 노드를 insertBefore 메서드를 호출한 노드의 마지막 자식 노드로 추가 즉 appendChild 메서드처럼 동작

<br/>

### 노드 이동
- DOM에 이미 존재하는 노드를 appendChild or insertBefore 메서드를 사용해 DOM에 다시 추가하면 현재 위치에서 노드를 제거하고 새로운 위치에 노드를 추가 (노드 이동)

```
const $fruits = document.getElementById('fruits');

const [$apple, $banan] = $fruits.children;

$fruits.appendChild($apple);

// 이미 존재하는 $banana 요소 노드를 #fruits 요소의 마지막 자식 노드 앞으로 이동
$fruits.isertBefore($banana, $fruits.lastElementChild);
```

<br/>

### 노드 복사
- Node.prototype.cloneNode([deep:true | false]) 메서드는 노드의 사본을 생성하여 반환 매개변수 depp에 true를 인수로 전달하면 노드를 깊은 복사하여 모든 자손 노드가 포함된 사본 생성, false를 전달하거나 생략하면 얕은 복사를 실행하여 노드 자신만의 사본 생성

<br/>

### 노드 교체
- Node.prototype.replaceChild(newChild, oldChild) 메서드는 자신을 호출한 노드의 자식 노드를 다른 노드로 교체
- 첫 번째 매개변수 newChild에는 교체할 새로운 노드를 인수로 전달하고 두 번째 매개변수에는 이미 존재하는 교체될 노드를 인수로 전달(replaceChild 메서드를 호출한 노드의 자식 노드이어야 한다.)


<br/>

### 노드 삭제
- Node.prototype.removeChild(child) 메서드는 child 매개변수에 인수로 전달한 노드를 DOM에서 삭제한다. ( 호출한 노드의 자식 노드이어야 한다. )

```
<ul>
	<li>Apple</li>
  	<li>Banana</li>
</ul>

<scrpit>
	const $fruits = document.getElementById('fruits');
    
    $fruits.removeChild($fruits.lastElementChild);
</scrpit>
```

<br/>

## 어트리뷰트
### 어트리뷰트 노드와 attributes 프로퍼티
- HTML 요소는 여러 개의 어트리뷰트(속성)을 가질 수 있다.

```
<input id="user" type="text">
```

- 글로벌 어트리뷰트(id, class, style 등)와 이벤트 핸들러 어트리뷰트 (onclick, onfocus 등)은 모든 HTML 요소에서 공통적으로 사용 가능하지만 type, value같은 어트리뷰트는 input 요소에서만 사용 가능
- HTML 문서 파싱 시 요소의 어트리뷰트는 어트리뷰트 노드로 변환되어 요소 노드와 연결
- 유사 배열 객체이자 이터러블인 객체에 담겨서 요소 노드의 attributes 프로퍼티에 저장된다.
- attributes 프로퍼티는 읽기 전용 접근자 프로퍼티이며 요소 노드의 모든 어트리뷰트 노드의 참조가 담긴 NamedNodeMap 객체를 반환한다.

<br/>

### HTML 어트리뷰트 조작
- Element.prototype.getAttribute/setAttribute 메서드를 사용하면 attributes 프로퍼티를 통하지 않고 요소 노드에서 메서드를 통해 직접 HTML 어트리뷰트 값을 취득/변경 가능

```
<input id="user" type="text" value="ungmo">
<script>
	const $li = document.getElementById('user');
    
    // value 어트리뷰트 값 취득
    const inputValue = $input.getAttribue('div');
    console.log(inputValue); // ungmo2
    
    // 값 변경
    $input.setAttribute('value', 'foo');
    console.log($input.getAttribute('value')); // foo
</script>

// 특정 어트리뷰트 존재 확인
$input.hasAttribute('vlaue'))

// 어트리뷰 삭제
$input.removeAttribute('vlaue'))
```

<br/>

### HTML 어트리뷰트 vs DOM 프로퍼티
- 요소 노드 객체에는 HTML 어트리뷰트(이하 DOM 프로퍼티)에 대응하는 프로퍼티가 존재 이 DOM 프로퍼티들은 HTML 어트리뷰트 값을 초기값으로 가지고 있다.
- HTML 어트리뷰트의 역할은 HTML 요소의 초기 상태를 지정하는 것, HTML 어트리뷰트 값은 HTML 요소의 초기 상태를 의미하며 이는 변하지 않음
- input 요소의 value 어트리뷰트는 input 요소가 렌더링될 때만 입력 필드에 표시할 초기값을 지정 즉, input 요소가 렌더링되면 입력 필드에 초기값으로 지정한 value 어트리뷰트 값이 표시됨
- 이때 value 어트리뷰트는 어트리뷰트 노드로 변환되어 요소 노드의 attribute 프로퍼티에 저장 이와 별도로 value 어트리뷰트의 값은 요소 노드의 value 프로퍼티에 할당된다. -> input 요소의 요소 노드가 생성되어 첫 렌더링이 끝난 시점에 어트리뷰트 노드의 어트리뷰트 값과 요소 노드의 value 프로퍼티에 할당된 값은 HTML 어트리뷰트 값과 동일하다.

<br/>

- 값이 변경되면 초기 상태와 최신 상태를 가지게 되고 관리된다.(새로고침, 웹 페이지 처음 표현을 위해) 요소 노드의 초기 상태는 어트리뷰트 노드가 관리하며 요소 노드의 최신 상태는 DOM 프로퍼티가 관리

<br/>

#### 어트리뷰트 노드
- HTML 어트리뷰트로 지정한 HTML 요소의 초기 상태는 어트리뷰트 노드에서 관리
- 사용자 입력에 의해 변하지 않고 어트리뷰트로 지정한 HTML 지정한 초기 상태를 그대로 유지
- 관리하는 초기 상태 값을 취득하거나 변경하려면 getAttribute/setAttribute 메서드를 사용해야 한다. getAttribute로 취득하는 값은 초기값이다. setAttribute는 초기 상태 값을 변경

<br/>

#### DOM 프로퍼티
- 사용자가 입력한 최신 상태는 HTML 어트리뷰트에 대응하는 요소 노드의 DOM 프로퍼티에서 관리
- 상태 변화에 반응하여 언제나 최신 상태를 유지
- DOM 프로퍼티가 사용자의 입력에 의해 변경되는 최신 상태를 관리하지는 않는다 input의 경우 value 프로퍼티가 관리하고 checkbox의 경우 cheked 프로퍼티가 관리한다. id 어트리뷰트에 대응하는 id 프로퍼티는 사용자 입력과 아무런 관계가 없다. 따라서 사용자 입력과 관계없이 항상 동일한 값을 유지 id 어트리뷰트 값이 변하면 id 프로퍼티 값도 변하고 그 반대도 마찬가지다.

<br/>

#### HTML 어트리뷰트와 DOM 프로퍼티의 대응관계
- HTML 어트리뷰트 이름과 동일한 DOM 프로퍼티와 1대1로 대응 단 예외의 경우도 있다.
1. input 요소의 value 프로퍼티와 어트리뷰트의 경우 초기 상태, 최신 상태를 갖는 차이점이 존재하고
2. td 요소의 colspan 어트리뷰트는 대응하는 어트리뷰트가 없다.

<br/>

#### DOM 프로퍼티 값의 타입
- getAttribute 메서드로 취득한 어트리뷰트 값은 언제나 문자열이다.
- 하지만 DOM 프로퍼티로 취득한 최신 상태 값은 문자열이 아닐 수도 있다. (체크박스 true/false)

<br/>

### data 어트리뷰트와 dataset 프로퍼티
- HTML 요소에 정의한 사용자 정의 어트리뷰트와 자바스크립트 간에 데이터를 교환할 수 있다.
- data 어트리뷰트는 data-user-id, data-role과 같이 data- 접두사 다음에 임의의 이름을 붙여 사용
- data 어트리뷰트 값은 HTMLElement.dataset 프로퍼티로 취득 가능하다. dataset 프로퍼티는 HTML 요소의 모든 data 어트리뷰트의 정보를 제공하는 DOMStringMap 객체를 반환한다. 이 객체는 data 어트리뷰트의 data- 접두사 다음에 붙인 임의의 이름인 카멜 케이스로 변환한 프로퍼티를 가지고 있다. 이 프로퍼티로 data 어트리뷰트의 값을 취득/변경 가능
- data- 접두사 다음 존재하지 않는 이름을 키로 사용하여 dataset 프로퍼티에 값을 할당하면 HTML 요소에 data 어트리뷰트가 추가된다. 이때 카멜 케이스의 프로퍼티 키는 data 어트리뷰트 data- 접두사 다음에 케밥케이스로 자동 변경되어 추가된다.


<br/>

## 스타일
### 인라인 스타일 조작
- HTMLElement.prototype.style 프로퍼티는 getter/setter 모두 존재하는 접근자 프로퍼티로 요소 노드의 인라인 스타일을 취득/변경
- style 프로퍼티 참조 시 CSSStyleDeclaration 타입 객체 반환 이 객체에는 다양한 CSS 프로퍼티에 대응하는 프로퍼티를 가지고 있고 이 프로퍼티 값을 할당하면 해당 CSSS 프로퍼티가 인라인 스타일로 HTML 요소에 추가/변경

```
$div.style.backgroundColor = 'yellow'
$div.style['backgroundColor'] = 'yellow'
```

<br/>

### 클래스 조작
- .으로 시작하는 클래스 선택자를 사용하여 CSS class를 미리 정의하고 HTML 요소의 class 어트리뷰트 값을 변경해 HTML 요소의 스타일을 변경할 수 있다.
- class 어트리뷰트에 대응하는 DOM 프로퍼티는 class가 아닌 className과 classList다 (JS는 class 예약어)

<br/>

#### className
- Element.prototype.className 프로퍼티는 getter/setter 모두 존재하는 접근자 프로퍼티로 참조 시 class 어트리뷰트 값을 문자열로 반환하고 요소 노드의 className 프로퍼티에 문자열을 할당하면 class 어트리뷰트 값을 할당한 문자열로 변경
- 문자열을 반환하므로 공백으로 구분된 여러 개의 클래스를 반환하는 경우 불편하다.

<br/>

#### classList
- Element.prototype.classList 프로퍼티는 class 어트리뷰트의 정보를 담은 DOMTokenList 객체를 반환
- 이 객체는 class 어트리뷰트 정보를 나타내는 컬렉션 객체로 유사 배열이면서 이터러블이다.

<br/>


- add(...className) - 인수로 전달한 1개 이상의 문자열을 class 어트리뷰트 값으로 추가
- remove(...className) - 인수로 전달한 1개 이상의 문자열과 일치하는 클래스를 class 어트리뷰트에서 삭제, 없으면 에러없이 무시
- itme(index) - item 메서드는 인수로 전달한 inde에 해당하는 클래스를 class 어트리뷰트에 반환
- contains(className) - 인수로 전달한 문자열과 일치하는 클래스가 class 어트리뷰트에 포함되어 있는지 확인
- replace(oldClassName, newClassName) - 첫 번째 인수로 전달한 문자열을 두 번째 인수로 전달한 문장열로 변경
- toggle(className[. force]) - toggle 메서드는 class 어트리뷰트에 인수로 전달한 문자열과 일치하는 클래스가 존재하면 제거하고 존재하지 않으면 추가 두 번째 인수로 불리언 값으로 평가되는 조건식 전달 가능 결과가 true면 class 어트리뷰트에 강제로 첫 번째 인수로 전달받은 문자열 추가 flase면 class 어트리뷰트에 강제로 첫 번째 인수로 전달받은 문자열 제거

```
$box.classList.add('foo') // class = "box red foo"
$box.classList.remove('foo') // class = "box red"
$box.classList.item(0) // "box"
$box.classList.contains('box') // true
$box.classList.replace('red', "blue") // class="box blue"
$box.classList.toggle('foo') // class="foo"
$box.classList.toggle('foo', false) // class="box blue"
```

- 이 밖에도 DOMTokenList 객체는 forEach, entries, keys, value, supports 메서드를 제공

<br/>

### 요소에 적용되어 있는 CSS 스타일 참조
- style 프로퍼티는 인라인 스타일만 반환 -> 클래스를 적용한 스타일이나 상속을 통해 암묵적으로 적용된 스타일은 style 프로퍼티로 참조 불가 CSS 스타일을 참조하려면 getComputedStyle 메서드 사용
- window.getComputedStyle(element[, pseudo]) 메서드는 첫 번째 인수로 전달한 요소 노드에 적용되어 있는 평가된 스타일을 CSSStyleDeclaration 개게에 담아 반환
- 평가된 스타일이란 요소 노드에 적용되어 있는 모든 스타일 -> 링크 스타일, 임베딩 스타일, 인라인 스타일, JS에서 적용한 스타일, 기본 스타일 등 모든 스타일이 조합되어 최종적으로 적용된 스타일을 말한다.

```
<style>
	body{
    	color: red;
    }
	.box{
    	width: 100px;
    }
</style>

const #box = document.querySelector('.box');

// .box 요소에 적용된 모든 CSSStyleDeclaration 객체를 취득
const computedStyle = window.getComputedStyle($box);
console.log(computedStyle); // CSSStyleDeclaration

// 임베딩 스타일
console.log(computedStyle.width); // 100px

// 상속 스타일(body -> .box)
console.log(computedStyle.color); // rgb(255, 0, 0)

// 기본 스타일
console.log(computedStyle.display); // block
```

- getComputedStyle 메서드의 두 번째 인수로 :after, :before와 같은 의사 요소를 지정하는 문자열을 전달할 수 있다. 의사 요소가 아닌 일반 요소의 경우 두 번째 인수는 생략

```
<style>
	body{
    	color: red;
    }
	.box:before {
		content: 'Hellow'
    }
</style>
<script>
	const #box = document.querySelector('.box');

	// 의사 요소 :before의 스타일을 취득
	const computedStyle = window.getComputedStyle($box, ':before');
	console.log(computedStyle,.content); // 'Hellow'
</script>
```

<br/>

## DOM 표쥰
- HTML과 DOM 표준은 W3C과 WHATWG 이라는 두 단체가 나름대로 협렵하며 공통된 표준을 만들다가 다른 결과물을 내놓기 시작했고 다른 구글, 애플에서로 구성된 주류 브라우저 벤더사가 주도해 WHATWG이 단일 표준을 내놓기로 두 단체가 합의했다.
- DOM Level1 ~ 4까지 있다.



<br/>




