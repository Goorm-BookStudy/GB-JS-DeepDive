# 33장 7번째 데이터 타입 Symbol

### 1. 심벌이란?

: 심벌은 ES6에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입 값이다. 심벌 값은 다른 값과 중복되지 않는 유일무이한 값이다. 따라서 주로 이름의 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용한다.

### 2. 심벌 값의 생성

### 2-1. Symbol 함수

: 심벌 값은 Symbol 함수를 호출하여 생성한다.다른 원시값, 즉 문자열, 숫자, 불리언, undefined, null 타입의 값은 리터럴 표기법을 통해 생성할 수 있지만 심벌 값은 Symbol 함수를 호출하여 생성해야 한다. 이때 생성된 심벌 값은 외부로 노출되지 않아 확인할 수 없으며, **다른 값과 절대 중복되지 않는 유일무이한 값이다.**

```jsx
const mySymbol = Symbol();
console.log(typeof mySymbol); // symbol

// 심볼 값은 외부로 노출되지 않아 확인할 수 없다.
console.log(mySymbol); // Symbol();
```

- 심벌 값은 new 연산자와 함께 호출하지 않는다.

```jsx
new Symbol(); // TypeError
```

- Symbol 함수에는 선택적으로 문자열을 인수로 전달할 수 있다. 이 문자열은 생성된 심벌 값에 대한 설명으로 디버깅 용도로만 사용되며, 심벌 값 생성에 어떠한 영향도 주지 않는다. 즉, 심벌 값에 대한 설명이 같더라도 생성된 심벌 값은 유일무이한 값이다.

```jsx
const mySymbol1 = Symbol("mySymbol");
const mySymbol2 = Symbol("mySymbol");

console.log(mySymbol1 === mySymbol2); // false
```

- 심벌 값은 문자열, 숫자, 불리언과 같이 객체처럼 접근하면 암묵적으로 래퍼 객체를 생성한다. 다음 예제의 description 프로퍼티와 toString 메서드는 Symbol.prototype의 프로퍼티다.

```jsx
const mySymbol = Symbol("mySymbol");

console.log(mySymbol.description); // mySymbol
console.log(mySymbol.toString()); // Symbol(mySymbol)
```

- 심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다.

```jsx
const mySymbol = Symbol();

console.log(mySymbol + ""); // TypeError
console.log(+mySymbol); // TypeError
```

- 단, 불리언 타입으로는 암묵적으로 타입 변환된다. 이를 통해 if 문 등에서 존재 확인이 가능하다.

```jsx
const mySymbol = Symbol();

console.log(!!mySymbol); // true

if (mySymbol) console.log("my Symbol is not empty.");
```

### 2-2. Symbol.for / Symbol.keyFor 메서드

: Symbol.for 메서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색한다.

- 검색에 성공하면 새로운 심벌 값을 생성하지 않고 심벌 값을 반환한다.
- 검색에 실패하면 새로운 심벌 값을 생성하여 Symbol.for 메서드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장한 후, 생성된 심벌 값을 반환한다.

```jsx
const s1 = Symbol.for("mySymbol");
const s2 = Symbol.for("mySymbol");

console.log(s1 === s2); // true
```

- Symbol.keyFor 메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출할 수 있다.

```jsx
const s1 = Symbol.for("mySymbol");

Symbol.keyFor(s1); // mySymbol // 심벌값의 키를 추출

const s2 = Symbol.for("foo");

Symbol.keyFor(s2); // foo // 심벌값의 키를 추출
```

### 3. 심벌과 상수

: 예를 들어 4방향, 즉 위, 아래, 왼쪽, 오른쪽을 나타내는 상수를 정의한다고 생각해보자.

```jsx
const Direction = {
  UP: 1,
  DOWN: 2,
  LEFT: 3,
  RIGHT: 4,
};

const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log("You are going UP.");
}
```

⇒ 위 예제처럼 변경/중복될 가능성이 있는 무의미한 상수 대신 중복될 가능성이 없는 유일무이한 심벌 값을 사용할 수 있다.

```jsx
const Direction = {
  UP: Symbol("up"),
  DOWN: Symbol("down"),
  LEFT: Symbol("left"),
  RIGHT: Symbol("right"),
};

const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log("You are going UP.");
}
```

<aside>
💡 **enum**
enum은 명명된 숫자 상수의 집합으로 열거형이라고 부른다. 자바스크립트는 enum을 지원하지 않지만 C, 자바, 파이썬 등 여러 프로그래밍 언어와 자바스크립트의 상위 확장인 타입스크립트에서는 enum을 지원한다. 자바스크립트에서 enum을 흉내 내어 사용하려면 아래와 같이 객체의 변경을 방지하기 위해 객체를 동결하는 Object.freeze 메서드와 심벌 값을 사용한다.

```jsx
const Direction = Object.freeze{
	UP: Symbol('up'),
	DOWN: Symbol('down'),
	LEFT: Symbol('left'),
	RIGHT: Symbol('right')
}

const myDirection = Direction.UP;

if(myDirection === Direction.UP) {
	console.log('You are going UP.')
}
```

</aside>

### 4. 심벌과 프로퍼티 키

: 심벌 값으로 프로퍼티 키를 동적 생성하여 프로퍼티를 만들어보자

```jsx
const obj = {
  [Symbol.for("mySymbol")]: 1,
};

obj[Symbol.for("mySymbol")]; // 1
```

⇒ 심벌 값은 유일무이한 값이므로 심벌 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 절대 충돌하지 않는다. 기존 프로퍼티 키와 충돌하지 않는 것은 물론, 미래에 추가될 어떤 프로퍼티 키와도 충돌할 위험이 없다.

### 5. 심벌과 프로퍼티 은닉

: 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티는 for…in 문이나 Object.keys, Object.getOwnPropertyNames 메서드로 찾을 수 없다. 이처럼 심벌 값을 프로퍼티 키로 사용하여 프로퍼티를 생성하면 외부에 노출할 필요가 없는 프로퍼티를 은닉할 수 있다.

```jsx
const obj = {
  [Symbol("mySymbol")]: 1,
};

for (const key in obj) {
  console.log(key); // 아무것도 출력되지 않는다.
}

console.log(Object.keys(obj)); // []
console.log(Object.getOwnPropertyNames(obj)); // []
```

⇒ 하지만 프로퍼티를 완전하게 숨길 수 있는 것은 아니다. ES6에서 도입된 Object.getOwnPropertySymbols 메서드를 사용하면 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티를 찾을 수 있다.

```jsx
const obj = {
	[Symbol('mySymbol')]: 1
};

console.log(Object.getOwnPropertySymbols(obj)); // [Symbol(mySymbol)]

const symbolKey1 = Object.getOwnPropertySymbols(obj)[0];
console.log(symbolKey1)); // 1
```

### 6. 심벌과 표준 빌트인 객체 확장

: 일반적으로 표준 빌트인 객체에 사용자 정의 메서드를 직접 추가하여 확장하는 것은 권장하지 않는다. 표준 빌트인 객체는 읽기 전용으로 사용하는 것이 좋다.

```jsx

```
