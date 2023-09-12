# 15장 let, const 키워드와 블록 레벨 스코프

### 1. var 키워드로 선언한 변수의 문제점

### 1-1. 변수 중복 선언 허용

- var 키워드로 선언한 변수는 중복 선언이 가능하다.

```jsx
var x = 1;
var y = 1;

var x = 100; => 초기화 문이 있는 선언문은 var 키워드가 없는 것처럼 동작한다.

var y; => 초기화 문이 없는 선언문은 무시된다.
```

⇒ 위 예제와 같이 만약 동일한 이름의 변수가 이미 선언되어 있는 것을 모르고 변수를 중복 선언하면서 값까지 할당했다면 의도치 않게 먼저 선언된 변수 값이 변경되는 부작용이 발생한다.

### 1-2. 함수 레벨 스코프

- var 키워드는 오호지 함수의 코드 블록만을 지역 스코프로 인정한다. 따라서 함수 외부에서 var 키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 전역 변수가 된다.

```jsx
var x = 1;

if (true) {
  var x = 10;
}

console.log(x); // 10
```

```jsx
var i = 10;

for (var i = 0; i < 5; i++) {
  console.log(i); // 0 1 2 3 4 5
}

console.log(i); // 5
```

### 1-3. 변수 호이스팅

- var 키워드로 변수를 선언하면 변수 호이스팅에 의해 변수 선언문이 스코프의 선두로 끌어 올려진 것처럼 동작한다. 즉, 변수 호이스팅에 의해 var 키워드로 선언한 변수는 변수 선언문 이전에 참조할 수 있다.
  단, 할당문 이전에 변수를 참조하면 언제나 undefined를 반환한다.

### 2. let 키워드

### 2-1. 변수 중복 선언 금지

```jsx
var foo = 123;
var foo = 456;

let bar = 123;
let bar = 456; //에러 에러
```

### 2-2. 블록 레벨 스코프

```jsx
let foo = 1;

{
  let foo = 2;
  let bar = 3;
}

console.log(foo); // 1
console.log(bar); // 에러
```

⇒ let 키워드로 선언된 변수는 블록 레벨 스코프를 따른다.

### 2-3. 변수 호이스팅

```jsx
console.log(foo); // 에러
let foo;
```

⇒ let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 동작한다.

- let 키워드로 선언한 변수는 **_선언 단계_**와 **_초기화 단계_**가 분리되어 진행된다.
- 즉, 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 선언 단계가 먼저 실행되지만 초기화 단계는 변수 선언문에 도달했을 때 실행된다.

```jsx
let foo = 1; // 전역 변수

{
  console.log(foo); // 레퍼런스 에러
  let foo = 2; // 지역 변수
}
```

⇒ 실제로는 변수 호이스팅이 일어나기 때문에 스코프 안에서 레퍼런스 에러가 발생하고 전역 변수를 참고하지 않고있다.

### 2-4. 전역 객체와 let

```jsx
// 전역 변수
var x = 1;
// 암묵적 전역
y = 2;
// 전역 함수
function foo() {}

// var 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티다.
console.log(window.x); // 1
// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
console.log(x); // 1

// 암묵적 전역은 전역 객체 window의 프로퍼티다.
console.log(window.y); // 2
console.log(y); // 2

// 함수 선언문으로 정의한 전역 함수는 전역 객체 window의 프로퍼티다.
console.log(window.foo); // f foo() {}
// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
console.log(foo); // f foo() {}
```

```jsx
let x = 1;

console.log(window.x); // undefined
console.log(x); // 1
```

⇒ let, const 키워드로 선언한 전역 변수는 전역 객체인 window의 프로퍼티가 아니다.

### 3. const 키워드

- const 키워드는 상수를 선언하기 위해 사용한다. 하지만 반드시 상수만을 위해 사용하지는 않는다.

### 3-1. 선언과 초기화

: const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 한다.

```jsx
const foo = 1;

const bar; // 에러
```

- const 키워드로 선언한 변수는 let 키워드로 선언한 변수와 마찬가지로 블록 레벨 스코프를 가지며, 변수 호이스팅이 발생하지 않는 것처럼 동작한다.

### 3-2. 재할당 금지

: var 또는 let 키워드로 선언한 변수는 재할당이 자유로우나 const 키워드로 선언한 변수는 재할당이 금지된다.

```jsx
const foo = 1;
foo = 2; // 에러
```

### 3-3. 상수

: 변수의 상대 개념인 상수는 재할당이 금지된 변수를 말한다.

- 상수는 상태 유지와 가독성, 유지보수의 편의를 위해 적극적으로 사용해야 한다.

```jsx
let preTaxPrice = 100;

let afterTaxPrice = preTaxPrice + preTaxPrice * 1.0;

console.log(afterTaxPrice); // 110
```

⇒ 코드 내에서 사용한 0.1은 어떤 의미로 사용했는지 명확히 알기 어렵기 떄문에 가독성이 좋지 않다. 또한 세율을 의미하는 0.1은 쉽게 바뀌지 않는 값이며, 프로그램 전체에서 고정된 값을 사용해야 한다.

```jsx
const TAX_RATE = 0.1;

let preTaxPrice = 100;

let afterTaxPrice = preTaxPrice + preTaxPrice * TAX_RATE;

console.log(afterTaxPrice); // 110
```

⇒ 상수는 전체 대문자 + 언더 스코어로 구분해서 스네이크 케이스로 표현하는 것이 일반적이다.

### 3-4. const 키워드와 객체

- const 키워드로 선언된 변수는 객체를 할당한 경우 값을 변경할 수 있다.

```jsx
const person = {
  name: 'Lee',
};

person.name = 'Kim';

console.log(person);
{
  name: 'Kim';
}
```

⇒ const 키워드는 재할당을 금지할 뿐 **_불변_**을 의미하지는 않는다.

### 4. var vs. let vs. const

- 변수 선언에는 기본적으로 const를 사용하고 let은 재할당이 필요한 경우에 한정해 사용하는 것이 좋다.
- const 키워드를 사용하면 의도치 않는 재할당을 방지하기 떄문에 좀 더 안전하다.
- ES6를 사용한다면 var 키워드는 사용하지 않는다.

⇒ 변수를 선언할때는 일단 const 키워드를 사용하자, 반드시 재할당이 필요하다면 그때 const 키워드를 let 키워드로 변경해도 결코 늦지 않는다.
