# 39장 DOM

- DOM은 HTML문서의 계층적 구조와 정보 그리고 여러 메서드들로 이루어진 트리 자료주조이다.

## 39.1 노드

<br>

### HTML요소
- HTML 요소(HTML Element)는 HTML 문서를 구성하는 개별적인 요소를 의미한다.
- 즉, HTML 문서에서 볼 수 있는 하나하나의 태그 요소들을 HTML 요소라고 부른다.
- HTML요소는 시작 및 종료 태그, 어트리뷰트, 컨텐츠로 이루어진다.
- 이때, 컨텐츠는 텍스트 또는 다른 HTML 요소가 될 수 있다.

```html
<div class="hey">Hey</div>

 <!-- 위 HTML 요소는 다음과 같이 구성된다. -->
<!-- 시작 태그: div -->
<!-- 어트리뷰트: { 이름: class, value: "hey" } -->
<!-- 컨텐츠: "hey" -->
<!-- 종료 태그: div -->
```

<br>

### 노드 객체
- 렌더링 엔진은 HTML 요소를 파싱해 노드 객체를 생성한다.
- 이때, HTML 요소, 어트리뷰트, 텍스트 컨텐츠 등은 각각 타입에 맞는 노드 객체로 변환된다.
  - 예를들어 요소 노드 객체, 어트리뷰트 노드 객체, 텍스트 노드 객체
- 앞서 이야기했듯 HTML 요소의 컨텐츠는 또 다른 HTML 요소가 될 수 있다. 즉, HTML 문서는 각 HTML요소들의 계층적 구조로 이루어진다.
- 따라서 이를 표현하는 노드 객체 또한 계층적 구조를 가지며 루트 노드를 기준으로 트리가 형성된다.
- 이렇게 생성된 노드 객체의 트리를 DOM이라고 부른다.

<br>

### 노드 객체의 타입

- 노드 객체에는 종류가 있고 각각 종류에 따라 다른 상속 구조를 갖는다.
- 노드 객체는 총 12개의 종류가 있다.
- 대표적인 노드 타입은 다음과 같다.
  1. 문서 노드(document node)
      - 문서노드는 DOM 트리의 최상위에 존재하는 루트 노드이다.
      - 실제로 document객체가 이 문서 노드에 해당한다.
      - document객체는 전역 객체인 window의 document 프로퍼티에 바인딩 되어있다.
      - HTML 문서당 document객체는 유일하다.
  2. 요소 노드(element node)
      - HTML 요소를 가리키는 객체이다.
  3. 어트리뷰트 노드(attribute node)
      - HTML 요소의 어트리뷰트를 나타내는 객체
      - 어트리뷰트 노드는 해당 어트리뷰트가 지정된 HTML 요소의 요소 노드와 연결되어 있다.
  4. 텍스트 노드
      - HTML 요소의 텍스트를 가리키는 객체
      - 텍스트 노드는 항상 트리의 리프 노드이다.
- 위 4가지 말고도 주석을 위한 Comment노드, DOCTYPE을 위한 DocumentType노드, DocumentFragment 노드, 공백 텍스트 노드 등이 존재한다.

<br> 

### 노드 객체의 상속 구조
- DOM을 구성하는 노드 객체는 자신의 구조와 정보를 제어할 수 있는 DOM API를 사용할 수 있다. 
- 이러한 API는 자바스크립트의 프로토타입에 의해 메서드의 형태로 상속된다.
- 노드 객체의 상속 구조는 다음과 같다.
  <br>
  ![노드객체구조](https://user-images.githubusercontent.com/57767891/139620868-a2dcc469-1b34-40e6-9493-bff3d27200dd.png)
- 모든 노드는 Object, EventTarget, Node 객체를 상속받는다.
  - 이벤트 관련 기능은 EventTarget으로부터, 트리 탐색이나 노드 정보 제공 등의 기능은 Node로부터 상속 받는다.
- 문서노드(document)는 Document, HTMLDocument를, 어트리뷰트 노드는 Attr을 텍스트 노드는 CharacterData를 각각 상속받는다.
- 요소 노드는 Element와 HTMLElement를 상속 받는데 각 태그 종류에 따라 HTMLBodyElement, HTMLDivElement 등을 상속받는다.
  - style프로퍼티와 같이 공통의 정보는 HTMLElement로부터 각 요소의 고유 정보는 각 인터페이스로부터 제공받는다.

<br>

## 39.2 요소 노드 취득

<br>

- HTML요소를 동적으로 조작하기위해 특정 요소 노드를 취득할 수 있다.

<br>

### id를 이용한 요소 노드 취득
- Document.prototype.getElementById를 사용
- 인수로 전달한 문자열을 id 어트리뷰트로 갖는 첫번째 요소를 반환한다.
- 해당하는 요소가 존재하지 않으면 null을 반환한다.
- Document.prototype의 메서드이므로 문서노드인 document 객체를 통해 호출해야한다.
- HTML요소에 id 어트리뷰트를 부여하면 id값과 동일한 이름의 전역변수에 해당 노드 객체가 할당된다.
  - id값에 식별자 규칙을 준수하지 않는 문자열이 들어가는 경우는 해당하지 않는다.
  - 이미 id값과 동일한 이름의 전역 변수가 있어도 해당하지 않음.

<br>

### 태그 이름을 이용한 요소 노드 취득
- Document.prototype(or Element.prototype).getElementsByTagName
- 인수로 전달한 태그 이름을 갖는 모든 요소 노드를 HTMLCollection객체로 반환한다.
- HTMLCollection은 유사배열이면서 이터러블이다.
- 모든 요소 노드를 취득하려면 인수로 '*'를 전달한다.
- Document.prototype.getElementsByTagName은 문서 전체를 탐색, Element.prototype.getElementsByTagName은 특정 엘리먼트의 자손을 탐색한다.
- 해당하는 요소가 없다면 빈 HTMLCollection객체를 반환한다.

<br>

### class를 이용한 요소 노드 취득
- Document.prototype(or Element.prototype).getElementsByClassName
- 인수로 전달한 class 어트리 뷰트를 갖는 모든 요소 노드를 HTMLCollection 객체로 반환한다.
- 이하 성질은 getElementsByTagName과 동일하다.

<br>

### CSS 선택자를 이용한 요소 노드 취득
- Document.prototype(or Element.prototype).querySelector 사용
- 인수로 전달한 CSS 선택자를 만족시키는 요소 노드 중 첫번째를 반환한다.
- 만족하는 노드가 없다면 null을 반환한다.
- 인수로 전달한 CSS 선택자가 문법에 맞지 않는 경우 DOMException에러가 발생한다.

<br>

- Document.prototype(or Element.prototype).querySelectorAll을 사용 할 수도 있다.
- 이때는 인수로 전달한 CSS 선택자를 만족시키는 모든 요소 노드를 NodeList 객체로 반환한다.
- NodeList객체도 HTMLCollection과 마찬가지로 유사배열이면서 이터러블이다.
- 만족하는 요소가 없다면 빈 NodeList를 반환한다.
- 인수로 전달한 CSS 선택자가 문법에 맞지 않는 경우 DOMException에러가 발생한다.

<br>

### 특정 요소 노드를 취득할 수 있는지 확인
- Element.prototype.matches 메서드 사용
- 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인하고 결과를 boolean값으로 반환한다.

<br>

### HTMLCollection과 NodeList
- HTMLCollection과 NodeList 모두 DOM API가 여러개의 결과 값을 반환하기 위해 사용하는 DOM 컬렉션 객체이다.
- HTMLCollection과 NodeList 모두 유사 배열이면서 이터러블이다.
- HTMLCollection은 항상 live 객체로 동작한다.
- NodeList의 경우 대부분 non-live 객체로 동작하지만 childNodes 프로퍼티에 의해 참조되는 NodeList의 경우 live 객체로 동작한다.

<br>

> live 객체
> 특정 DOM 컬렉션이 각 노드 객체의 상태 변화를 실시간으로 반영할 때 이를 live 객체라 부른다.

<br>

### 39.3 노드 탐색

- Node와 Elementsms DOM 트리를 탐색 할 수 있는 접근자 프로퍼티를 제공한다.

#### 1. 자식 노드 탐색
- Node.prototype.childNodes
  - 모든 자식 모드를 NodeList로 반환
  - 텍스트 및 빈 텍스트 노드도 포함한다.
- Element.prototype.children
  - 자식 노드 중 요소 노드만 HTMLCollection에 담아 반환
- Node.prototype.fitstChild
  - 첫 번째 자식 노드 반환
  - 텍스트 노드일 수도 있다.
- Node.prototype.lastChild
  - 마지막 자식 노드 반환
  - 텍스트 노드일 수 있다.
- Element.prototype.firstElementChild
  - 첫 번째 자식 '요소' 노드를 반환
- Element.ptototype.lastElementChild
  - 마지막 자식 '요소' 노드를 반환
- Node.prototype.hasChildNodes()
  - 자식노드의 존재여부를 boolean으로 반환
  - 이때, 텍스트노드도 포함해 조회한다.
- Element.prototype.childElementCount
  - 자식 '요소' 노드의 수를 반환

<br>

#### 2. 부모 노드 탐색
- Node.prototype.parentNode
  - 부모 요소 노드를 반환

#### 3. 형제 노드 탐색
- Node.prototype.previousSibling
  - 부모가 같은 형제 노드중 자신의 이전 형제 노드를 반환
  - 텍스트 노드일 수 있다.
  - 해당하는 노드가 없으면 null을 반환
- Node.prototype.nextSibling
  - 부모가 같은 형제 노드 중에서 자신의 다음 형제 노드를 반환
  - 텍스트 노드일 수 있다.
  - 해당하는 노드가 없으면 null을 반환
- Element.prototype.previousElementSibling
  - 부모가 같은 형제 노드중 자신의 이전 형제 '요소' 노드를 반환
  - 해당하는 노드가 없으면 null을 반환
- Element.prototype.nextElementSibling
  - 부모가 같은 형제 노드중 자신의 다음 형제 '요소' 노드를 반환
  - 해당하는 노드가 없으면 null을 반환

<br>

### 39.4 노드 정보 취득

- Node.prototype.nodeType
  - 노드의 타입을 나타내는 상수를 반환
  - 노드 타입 상수는 Node에 정의 되어있다.
    ex)
      Node.ELEMENT_NODE === 1
      Node.TEXT_NODE === 3
      Node.DOCUMENT === 9
      ...
- Node.prototype.nodeName
  - 노드의 이름을 문자열로 반환
    ex)
      요소 노드 - 대문자 문자열로 태그 이름 반환 ("UL, "LI", ...)
      텍스트 노드 - "#text" 반환
      문서 노드 - "#document" 반환

### 39.5 요소 노드의 텍스트 조작

#### 39.5.1 nodeValue

---

- Node.prototype.nodeValue 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티다.

- 노드 객체의 nodeValue 프로퍼티를 참조하면 노드 객체의 값을 반환한다. 여기서 노드 객체의 값이란 텍스트 노드의 텍스트다.

  ```html
  <html>
    <body>
      <div id="foo">Hello</div>
    </body>
    <script>
      // 문서 노드의 nodeValue 프로퍼티를 참조한다.
      console.log(document.nodeValue); // null
  
      // 요소 노드의 nodeValue 프로퍼티를 참조한다.
      const $foo = document.getElementById('foo');
      console.log($foo.nodeValue); // null
  
      // 텍스트 노드의 nodeValue 프로퍼티를 참조한다.
      const $textNode = $foo.firstChild;
      console.log($textNode.nodeValue); // Hello
    </script>
  </html>
  ```

<br>

#### 39.5.2 textContent

---

- Node.prototype.textContent 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 텍스트와 모든 자손 노드의 텍스트를 모두 취득하거나 변경한다.
- 요소 노드의 textContent 프로퍼티를 참조하면 요소 노드의 콘텐츠 영역 내의 텍스트를 모두 반환한다.

```html
<html>
  <body>
    <div id="foo">Hello <span>world!</span></div>
  </body>
  <script>
    // #foo 요소 노드는 텍스트 노드가 아니다.
    console.log(document.getElementById('foo').nodeValue); // null
    // #foo 요소 노드의 자식 노드인 텍스트 노드의 값을 취득한다.
    console.log(document.getElementById('foo').firstChild.nodeValue); // Hello
    // span 요소 노드의 자식 노드인 텍스트 노드의 값을 취득한다.
    console.log(document.getElementById('foo').lastChild.firstChild.nodeValue); // world!
  </script>
</html>
```

<br>

### 39.6 DOM 조작

---

- DOM 조작은 새로운 노드를 생성하여 DOM에 추가하거나 기존 노드를 삭제 또는 교체하는 것을 말한다.
- DOM 조작에 의해 DOM에 새로운 노드가 추가되거나 삭제되면 리플로우와 리페인트가 발생하는 원인이 디므로 성능에 영향을 준다.


#### 39.6.1 innerHTML

---

- Element.prototype.innerHTML 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 HTML 마크업을 취득하거나 변경한다.

- 아래처럼 innerHTML 프로퍼티를 사용하면 HTML 마크업 문자열로 간단히 DOM 조작이 가능하다.

  ```html
  <html>
    <body>
      <ul id="fruits">
        <li class="apple">Apple</li>
      </ul>
    </body>
    <script>
      const $fruits = document.getElementById('fruits');
  
      // 노드 추가
      $fruits.innerHTML += '<li class="banana">Banana</li>';
  
      // 노드 교체
      $fruits.innerHTML = '<li class="orange">Orange</li>';
  
      // 노드 삭제
      $fruits.innerHTML = '';
    </script>
  </html>
  ```

- innerHTML 프로퍼티를 사용한 DOM 조작은 구현이 간단하고 직관적이라는 장점이 있지만 크로스 사이트 스크립트 공격에 취약하다는 단점이 있다.

<br>

#### 39.6.2 insertAdjacentHTML 메서드

---

- Element.prototype.insertAdjacentHTML(position, DOMString) 메서드는 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입한다.

- ```html
  <html>
    <body>
      <!-- beforebegin -->
      <div id="foo">
        <!-- afterbegin -->
        text
        <!-- beforeend -->
      </div>
      <!-- afterend -->
    </body>
    <script>
      const $foo = document.getElementById('foo');
  
      $foo.insertAdjacentHTML('beforebegin', '<p>beforebegin</p>');
      $foo.insertAdjacentHTML('afterbegin', '<p>afterbegin</p>');
      $foo.insertAdjacentHTML('beforeend', '<p>beforeend</p>');
      $foo.insertAdjacentHTML('afterend', '<p>afterend</p>');
    </script>
  </html>
  ```

<br>

#### 39.6.3 노드 생성과 추가

---

- DOM은 노드를 직접 생성/삽입/삭제/치환하는 메서드도 제공한다.

   - 아래의 과정을 통해서 노드의 생성과 추가가 이루어진다. (추후 설명 추가 예정)

          - 요소 노드 생성
          - 텍스트 노드 생성
          - 텍스트 노드를 요소 노드의 자식 노드로 추가
          - 요소 노드를 DOM에 추가

     ```html
     <html>
       <body>
         <ul id="fruits">
           <li>Apple</li>
         </ul>
       </body>
       <script>
         const $fruits = document.getElementById('fruits');
     
         // 1. 요소 노드 생성
         const $li = document.createElement('li');
     
         // 2. 텍스트 노드 생성
         const textNode = document.createTextNode('Banana');
     
         // 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
         $li.appendChild(textNode);
     
         // 4. $li 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
         $fruits.appendChild($li);
       </script>
     </html>	
     ```

<br>

#### 39.6.4 복수의 노드 생성과 추가

---

- DOM을 여러번 변경하는 것은 각각 리플로우와 리페인팅을 야기시킨다. 이를 피하기 위해 컨테이너 요소를 사용할 수 있다.

  ```html
  <html>
    <body>
      <ul id="fruits"></ul>
    </body>
    <script>
      const $fruits = document.getElementById('fruits');
  
      // DocumentFragment 노드 생성
      const $fragment = document.createDocumentFragment();
  
      ['Apple', 'Banana', 'Orange'].forEach(text => {
        // 1. 요소 노드 생성
        const $li = document.createElement('li');
  
        // 2. 텍스트 노드 생성
        const textNode = document.createTextNode(text);
  
        // 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
        $li.appendChild(textNode);
  
        // 4. $li 요소 노드를 DocumentFragment 노드의 마지막 자식 노드로 추가
        $fragment.appendChild($li);
      });
  
      // 5. DocumentFragment 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
      $fruits.appendChild($fragment);
    </script>
  </html>
  ```

- 먼저 DocumentFragment 노드를 생성하고 DOM에 추가할 요소 노드를 생성하여 DocumentFragment 노드에 자식 노드로 추가한 다음, DocumentFragment 노드를 기존 DOM에 추가한다.
  - 이때, 실제로 DOM 변경이 발생하는 것은 한 번뿐이며 리플로우와 리페인트도 한 번만 실행한다.
  - 따라서, 여러 개의 요소 노드를 DOM에 추가하는 경우 DocumentFragment 노드를 사용하는 것이 더 효율적이다.

<br>

#### 39.6.5 노드 삽입

---

**마지막 노드로 추가**

- Node.prototype.appendChild 메서드는 인수로 전달받은 노드를 자신을 호출한 노드의 마지막 자식 노드로 DOM에 추가한다.
  - 단, 노드를 추가할 위치를 지정할 수 없고 언제나 마지막 자식 노드로 추가한다.

```html
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
      <li>Banana</li>
    </ul>
  </body>
  <script>
    // 요소 노드 생성
    const $li = document.createElement('li');

    // 텍스트 노드를 $li 요소 노드의 마지막 자식 노드로 추가
    $li.appendChild(document.createTextNode('Orange'));

    // $li 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
    document.getElementById('fruits').appendChild($li);
  </script>
</html>
```



<br>

**지정한 위치에 노드 삽입**

- Node.prototype.insertBefore(newNode, childNode) 메서드는 첫 번째 인수로 전달받은 노드를 두 번째 인수로 전달받은 노드 앞에 삽입한다.

```html
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
      <li>Banana</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');

    // 요소 노드 생성
    const $li = document.createElement('li');

    // 텍스트 노드를 $li 요소 노드의 마지막 자식 노드로 추가
    $li.appendChild(document.createTextNode('Orange'));

    // $li 요소 노드를 #fruits 요소 노드의 마지막 자식 요소 앞에 삽입
    $fruits.insertBefore($li, $fruits.lastElementChild);
    // Apple - Orange - Banana
  </script>
</html>
```

<br>

#### 39.6.6 노드 이동

---

- DOM에 이미 존재하는 노드를 appendChild 또는 insertBefore 메서드를 사용하여 DOM에 다시 추가하면 현재 위치에서 노드를 제거하고 새로운 위치에 노드를 추가한다. 즉, 노드가 이동한다.

  ```html
  <html>
    <body>
      <ul id="fruits">
        <li>Apple</li>
        <li>Banana</li>
        <li>Orange</li>
      </ul>
    </body>
    <script>
      const $fruits = document.getElementById('fruits');
  
      // 이미 존재하는 요소 노드를 취득
      const [$apple, $banana, ] = $fruits.children;
  
      // 이미 존재하는 $apple 요소 노드를 #fruits 요소 노드의 마지막 노드로 이동
      $fruits.appendChild($apple); // Banana - Orange - Apple
  
      // 이미 존재하는 $banana 요소 노드를 #fruits 요소의 마지막 자식 노드 앞으로 이동
      $fruits.insertBefore($banana, $fruits.lastElementChild);
      // Orange - Banana - Apple
    </script>
  </html>
  ```

<br>

#### 39.6.7 노드 복사

---

- Node.prototype.cloneNode([deep: true | false]) 메서드는 노드의 사본을 생성하여 반환한다.

```html
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');
    const $apple = $fruits.firstElementChild;

    // $apple 요소를 얕은 복사하여 사본을 생성. 텍스트 노드가 없는 사본이 생성된다.
    const $shallowClone = $apple.cloneNode();
    // 사본 요소 노드에 텍스트 추가
    $shallowClone.textContent = 'Banana';
    // 사본 요소 노드를 #fruits 요소 노드의 마지막 노드로 추가
    $fruits.appendChild($shallowClone);

    // #fruits 요소를 깊은 복사하여 모든 자손 노드가 포함된 사본을 생성
    const $deepClone = $fruits.cloneNode(true);
    // 사본 요소 노드를 #fruits 요소 노드의 마지막 노드로 추가
    $fruits.appendChild($deepClone);
  </script>
</html>
```

<br>

#### 39.6.8 노드 교체

---

- Node.prototype.replaceChild(newChild, oldChild) 메서드는 자신을 호출한 노드의 자식 노드를 다른 노드로 교체한다.

```html
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');

    // 기존 노드와 교체할 요소 노드를 생성
    const $newChild = document.createElement('li');
    $newChild.textContent = 'Banana';

    // #fruits 요소 노드의 첫 번째 자식 요소 노드를 $newChild 요소 노드로 교체
    $fruits.replaceChild($newChild, $fruits.firstElementChild);
  </script>
</html>
```

<br>

#### 39.6.9 노드 삭제

---

- Node.prototype.removeChild(child) 메서드는 child 매개변수에 인수로 전달한 노드를 DOM에서 삭제한다.

```html
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
      <li>Banana</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');

    // #fruits 요소 노드의 마지막 요소를 DOM에서 삭제
    $fruits.removeChild($fruits.lastElementChild);
  </script>
</html>
```

<br>

### 39.7 어트리뷰트

#### 39.7.1 어트리뷰트 노드와 attributes 프로퍼티

---

- HTML 문서의 구성 요소인 HTML 요소는 여러 개의 어트리뷰트를 가질 수 있다.
- HTML 요소의 동작을 제어하기 위한 추가적인 정보를 제공한다.
- 대부분의 어트리뷰트는 모든 HTML 요소에서 사용될 수 있지만, type, value, checked 어트리뷰트는 input 요소에만 사용할 수 있는 경우도 있다.
- HTML 문서가 파싱될 때 HTML 요소의 어트리뷰트는 어트리뷰트 노드로 변환되어 요소 노드와 연결된다. 

<br>

#### 39.7.2 HTML 어트리뷰트 조작

---

- HTML 어트리뷰트 값을 참조하려면 Element.prototype.getAttribute(attributeName) 메서드를 사용하고, HTML 어트리뷰트 값을 변경하려면 Element.prototype.setAttribute(attributeName, attributeValue) 메서드를 사용한다.

```html
<html>
<body>
  <input id="user" type="text" value="ungmo2">
  <script>
    const $input = document.getElementById('user');

    // value 어트리뷰트 값을 취득
    const inputValue = $input.getAttribute('value');
    console.log(inputValue); // ungmo2

    // value 어트리뷰트 값을 변경
    $input.setAttribute('value', 'foo');
    console.log($input.getAttribute('value')); // foo
