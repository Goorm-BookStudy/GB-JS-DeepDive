# 33장 Symbol
## 심벌이란?
- 변경 불가능한 원시 타입의 값
- 심벌 값은 다른 값과 중복되지 않는 유일무이한 값
- 주로 이름의 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용

<br/>

## 심벌 값의 생성
### Symbol 함수
- 심벌 값은 Symbol 함수를 호출하여 생성
- 생성된 심벌 값은 외부로 노출되지 않아 확인할 수 없으며, 다른 값과 절대 중복되지 않는 유일무이한 값
- new 연산자로 호출되지 않는다. (타입 에러 발생)
- Symbol 함수에는 선택적으로 문자열을 인수로 전달할 수 있다. 디버깅 용도로만 사용되며, 심벌 값 생성에 어떠한 영향도 주지 않는다. 심벌 값에 대한 설명이 같더라도 생성된 심벌 값은 유일무이한 값

```
// Symbol 함수를 호출하여 유일무이한 심벌 값을 생성한다.
const mySymbol = Symbol();
console . log(typeof mySymbol); // symbol

// 심벌 값은 외부로 노출되지 않아 확인할 수 없다.
console . log(mySymbol); // Symbol()

// 심벌 값에 대한 설명이 같더라도 유일무이한 심벌 값을 생성한다.
const mySymbol1 = Symbol('mySymbol');
const mySymbol2 = Symbol('mySymbol');
console . log(mySymbol1 === mySymbol2); // false

// 심벌도 래퍼 객체를 생성한다.
console.log(mySymbol.description); // mySymbol 
console.log(mySymbol.toString()); // Symbol(mySymbol)

// 심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다.
console.log(mySymbol + ''); // TypeError: Cannot convert a Symbol value to a string 
console.log(+mySymbol);     // TypeError: Cannot convert a Symbol value to a number

//불리언 타입으로는 암묵적으로 타입 변환된다
console.log(!!mySymbol); // true

// if 문 등에서 존재 확인이 가능하다.
if (mySymbol) console.log('mySymbol is not empty.');
```

<br/>

### Symbol.for / Symbol.keyFor 메서드
- Symbol.for 메서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 전역 심벌 레지스트리 global symbol registry 에서 해당 키와 일치하는 심벌 값을 검색
- 검색에 성공하면 새로운 심벌 값을 생성하지 않고 검색된 심벌 값을 반환
- 검색에 실패하면 새로운 심벌 값을 생성하여 Symbol.for 메서드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장한 후, 생성된 심벌 값을 반환

```
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for('mySymbol');
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 있으면 해당 심벌 값을 반환
const s2 = Symbol.for('mySymbol');

console.log(s1 === s2); // true
```

- Symbol  함수는 호출될 때마다 유일무이한 심벌 값을 생성 이때 자바스크립트 엔진이 관리하는 심벌 값저장소인 전역 심벌 레지스트리에서 심벌 값을 검색할 수 있는 키를 지정할 수 없으므로 전역 심벌 레지스트 리에 등록되어 관리되지 않는다.
- 하지만 Symbol.for 메서드를 사용하면 애플리케이션 전역에서 중복되지 않는 유일무이한 상수인 심벌 값을 단 하나만 생성하여 전역 심벌 레지스트리를 통해 공유 가능
- Symbol.keyFor 메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출할 수 있다.

```
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for('mySymbol');

// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
Symbol.keyFor(s1); //  mySymbol

// Symbol 함수를 호출하여 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
const s2 = Symbol('foo');

// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출 
Symbol.keyFor(s2); // undefined
```

<br/>

## 심벌과 상수
- 변경/중복될 가능성이 있는 무의미한 상수 대신 중복될 가능성이 없는 유일무이한 심벌 값을 사용할 수 있다.

```
// 위, 아래, 왼쪽, 오른쪽을 나타내는 상수를 정의한다.
// 중복될 가능성이 없는 심벌 값으로 상수 값을 생성한다.
const Direction = { 
	UP: Symbol('up'),
	DOWN: Symbol('down'),
	LEFT: Symbol('left'),
	RIGHT: Symbol('right')
};

const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
	console.log('You are going UP.');
}
```

- enum - 명명된 숫자 상수집합으로 열거형 이라고 부른다. JS에서는 지원 X, TS에서 지원

<br/>

## 심벌과 프로퍼티 키
- 객체 프로퍼티 키는 빈 문자열을 포함하는 모든 문자열 또는 심벌 값으로 동적 생성 가능
- 심벌 값을 프로퍼티 키로 사용하려면 프로퍼티 키로 사용할 심벌 값에 대괄호를 사용
- 심벌 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 절대 충돌 X 기존, 미래에 추가될 어떤 프로퍼티 키와도 충돌할 위험이 X

```
const obj = {

// 심벌 값으로 프로퍼티 키를 생성
[Symbol.for('mySymbol')]: 1 
};

// 프로퍼티에 접근할 때도 마찬가지로 대괄호를 사용 해야 한다.
obj[Symbol.for('mySymbol')]; // 1
```

<br/>

## 심벌과 프로퍼티 은닉
- 프로퍼티 키로 사용하여 생성한 프로퍼티는 for...in 문이나 Object.keys, Object.getOwnPropertyNames 메서드로 찾을 수 없다.
- Object.getOwnPropertySymbols 메서드를 사용하면 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티를 찾을 수 있다.

```
const obj = {
// 심벌 값으로 프로퍼티 키를 생성
[Symbol('mySymbol')]: 1 };

for (const key in obj) { console.log(key); // 아무것도 출력되지 않는다.
}
console.log(Object.keys(obj)); // [] 
console.log(Object.getOwnPropertyNames(obj)); // []

const obj = {
// 심벌 값으로 프로퍼티 키를 생성
[Symbol('mySymbol')]: 1 
};

// getOwnPropertySymbols 메서드는 인수로 전달한 객체의 심벌 프로퍼티 키를 배열로 반환
console.log(Object.getOwnPropertySymbols(obj)); // [Symbol(mySymbol)]

// getOwnPropertySymbols 메서드로 심벌 값도 찾을 수 있다.
const symbolKey1 = Object.getOwnPropertySymbols(obj)[0];
console.log(obj[symbolKey1]);
```

<br/>

## 표준빌트인객체 확장
- 중복될 가능성이 없는 심벌 값으로 프로퍼티 키를 생성하여 표준 빌트인 객체를 확장하면 표준 빌트인 객체의 기존 프로퍼티 키와 충돌하지 않는 것은 물론, 추가될지 모르는 어떤 프로퍼티 키와도 충돌할 위험이 없어 안전하게 표준 빌트인 객체를 확장할 수 있다.

```
// 심벌 값으로 프로퍼티 키를 동적 생성하면 다른 프로퍼티 키와 절대 충돌하지 않아 안전하다.
Array.prototype[Symbol.for('sum')] = function () { 
	return this.reduce((acc, cur) => acc + cur, 0);
};
[1, 2][Symbol.for('sum')](); // 3
```

<br/>

## Well-known-Symbol
- 자바스크립트가 기본 제공하는 빌트인 심벌 값을 ECMAScript 사양에서는 Well-known-Symbol 이라고 부른다.
- JS 엔진의 내부 알고리즘에 사용
- 빌트인 이터러블은 Well-known Symbol인 Symbol.iterator 를 키로 갖는 메서드를 가지며, Symbol.iterator 메서드를 호출하면 이터레이터를 반환하도록 규정됨
- 만약 빌트인 이터러블이 아닌 일반 객체를 이터러블처럼 동작하도록 구현하고 싶다면 Symbol.iterator 를키로 갖는 메서드를 객체에 추가하고 이터레이터를 반환하도록 구현하면 그 객체는 이터러블이 된다.
- 심벌은 중복되지 않는 상수 값을 생성하는 것은 물론 기존에 작성된 코드에 영향을 주지 않고 새로운 프로퍼티를 추가하기 위해, 즉 하위 호환성을 보장하기 위해 도입

```
// 1 ~ 5 범위의 정수로 이루어진 이터러블
const iterable = {

// Symbol.iterator 메서드를 구현하여 이터러블 프로토콜을 준수
[Symbol.iterator]() { 
	let cur = 1;
	const max = 5;

// Symbol.iterator 메서드는 next 메서드를 소유한 이터레이터를 반환
	return { 
		next() { 
			return { value: cur++ , done: cur > max + 1 };
			} 	
		};
	}
};
for (const num of iterable) { 
	console . log(num); // 1 2 3 4 5
}
```
