# ✏️ 33장 7번째 데이터 타입 Symbol

## 📌 33.1 심벌이란?

- ES6에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입의 값
- 심벌 값은 다른 값과 중복되지 않는 유일무이한 값
- 이름의 충돌 위험이 없는 프로퍼티 키를 만들기 위해 사용된다.

## 📌 33.2 심벌 값의 생성

#### 1. Symbol 함수

- 리터럴 표기법으로 값을 생성하는 다른 타입과 달리 심벌 값은 Symbol 함수를 호출해 생성한다.
- 생성된 심벌 값은 외부로 노출되지 않아 확인할 수 없고, 다른 값과 절대 중복되지 않는다.
- new 연산자와 함께 호출하지 않는다.
- new 연산자와 함께 생성자 함수 또는 클래스를 호출하면 인스턴스가 생성되지만 심벌은 변경 불가능한 원시 값이므로 new 연산자와 함께 호출할 수 없다.

```
// Symbol 함수를 호출해 심벌 값 생성
const mySymbol = Symbol();
console.log(typeof mySymbol); // symbol

// 값이 노출되지 않는다.
console.log(mySymbol); // Symbol()

// new 연산자와 같이 호출하지 않는다.
new Symbol(); // 타입 에러 (Symbol is not a constructor)
```

- Symbol 함수에 선택적으로 문자열을 인수로 전달할 수 있다.
- 이 문자열은 생성된 심벌 값에 대한 **설명**으로 디버깅 용으로만 사용되며 값에는 아무 영향도 주지 않는다. 심벌 값에 대한 설명이 같더라도 심벌 값은 절대 같지 않다.

```
const mySymbol1 = Symbol('mySymbol');
const mySymbol2 = Symbol('mySymbol');

console.log(mySymbol1 === mySymbol2); // false
```

- 심벌 값도 객체처럼 접근하면 암묵적으로 래퍼 객체를 생성한다.
- description 프로퍼티와 toString 메서드는 Symbol.prototype의 프로퍼티다.

```
const mySymbol = Symbol('mySymbol');

console.log(mySymbol.description); // mySymbol
console.log(mySymbol.toString()); // Symbol(mySymbol)
```

- 심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다.
- 하지만 불리언 타입으로는 암묵적 타입 변환이 가능하다.

```
// 문자열이나 숫자 타입으로 암묵적 타입 변환 불가
const mySymbol = Symbol();
console.log(mySymbol + ''); // 타입 에러 (Cannot convert a Symbol value to a string)
console.log(+mySymbol); // 타입 에러 (Cannot convert a Symbol value to a number)

// 불리언 타입으로 암묵적 타입 변환 가능
const mySymbol = Symbol();
console.log(!!mySymbol); // true
```

#### 2. Symbol.for / Symbol.keyFor 메서드

- Symbol.for 메서드는 인수로 전달받은 문자열을 키로 사용해 키와 심벌 값의 쌍들이 저장되어 있는 전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색한다.
  - 검색에 성공하면 새로운 심벌 값을 생성하지 않고 검색된 심벌 값을 반환한다.
  - 검색에 실패하면 새로운 심벌 값을 생성해 Symbol.for 메서드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장한 후 생성된 심벌 값을 반환한다.

```
// 전역 심벌 레지스트리에 mySymbol 이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값 생성
const s1 = Symbol.for('mySymbol');

// 전역 심벌 레지스트리에 mySymbol 이라는 키로 저장된 심벌 값이 있으면 생성된 심벌 값 반환
const s2 = Symbol.for('mySymbol');

// 동일한 전역 심볼을 참조
console.log(s1 === s2); // true
```

- Symbol 함수는 호출될 때마다 유일무이한 심벌 값을 생성한다.
- 이때 JS 엔진이 관리하는 심벌 값 저장소인 전역 심벌 레지스트리에서 심벌 값을 검색할 수 있는 키를 지정할 수 없어, 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
- 반면 Symbol.for 메서드를 쓰면 전역 심벌 레지스트리를 통해 공유할 수 있다.
- Symbol.Keyfor 메서드로 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출할 수 있다.

```
const s1 = Symbol.for('mySymbol');
Symbol.keyFor(s1); // mySymbol

const s2 = Symbol('foo');
Symbol.keyFor(s2); // undefined
```

## 📌 33.3 심벌과 상수

- p.608 예제처럼 상수 이름 자체에 의미가 있을 경우, 상수 값이 변경되거나 다른 변수 값과 중복될 수도 있다. 이런 경우를 방지하기 위해 중복될 가능성이 없는 유일무이한 심벌 값을 사용한다.

```
const Direction = {
    up: Symbol('up'),
    down: Symbol('down'),
    left: Symbol('left'),
    right: Symbol('right')
};
```

- p.609의 enum은 명명된 숫자 상수의 집합으로 열거형이라고 부른다.
- JS에서는 지원하지 않기 때문에 구현 시 Object.freeze 메서드와 심벌 값으로 구현할 수 있다.

```
const Direction = Object.freeze({
    up: Symbol('up'),
    down: Symbol('down'),
    left: Symbol('left'),
    right: Symbol('right')
});
```

## 📌 33.4 심벌과 프로퍼티 키

- 객체의 프로퍼티 키는 문자열 & 심벌 값으로 만들 수 있고, 동적으로 생성할 수도 있다.
- 심벌 값을 프로퍼티 키로 사용하려면 사용할 심벌 값에 대괄호를 사용한다.
- 프로퍼티에 접근할 때도 대괄호를 사용한다.
- 심벌 값은 유일무이하므로 한 번 만들면 다른 프로퍼티 키와 절대 충돌하지 않는다.

```
const obj = {
    [Symbol.for('mySymbol')]: 1
};

obj[Symbol.for('mySymbol')]; // 1
```

## 📌 33.5 심벌과 프로퍼티 은닉

- 프로퍼티 키로 심벌 값을 사용해 생성한 경우 for...in 문이나 Object.keys, Object.getOwnPropertyNames 같은 메서드로 찾을 수 없다.

```
const obj = {
    [Symbol.for('mySymbol')]: 1
};

for (const key in obj) {
    console.log(key); // 조회 불가
}

console.log(Object.keys(obj)); // []
console.log(Object.getOwnpropertyNames); // []
```

- 은닉할 수 있지만 ES6에서 도입한 Object.getOwnPropertySymbols 메서드로 찾을 수 있기 때문에 완벽하게 숨길 수 있는 것은 아니다.

```
const obj = {
    [Symbol.for('mySymbol')]: 1
};

console.log(Object.getOwnPropertySymbols(obj)); // [Symbol(mySymbol)]

const symbolKey1 = Object.getOwnPropertySymbols(obj)[0];
console.log(obb[symbolKey1]); // 1
```

## 📌 33.6 심벌과 표준 빌트인 객체 확장

- 일반적으로 표준 빌트인 객체에 사용자 정의 메서드를 직접 추가해 확장하는 것은 지양한다.
  - 개발자가 직접 추가한 메서드와 미래에 표준 사양으로 추가될 메서드의 이름이 중복될 가능성이 있기 때문
  - 따라서 표준 빌트인 객체는 읽기 전용으로 사용하는 것이 좋다.
  - 또는 중복될 가능성이 없는 심벌 값으로 프로퍼티 키를 생성해 표준 빌트인 객체를 확장하면 중복될 일이 없으므로 안전하게 표준 빌트인 객체를 확장할 수 있다.

```
// 표준 빌트인 객체 확장 (지양)
Array.prototype.sum = function() {
    return this.reduce((acc, cur) => acc + cur, 0);
};
[1, 2].sum(); // 3

// 심벌 값을 사용
Array.prototype.[Symbol.for('sum')] = function() {
    return this.reduce((acc, cur) => acc + cur, 0);
};
[1, 2][Symbol.for('sum')](); // 3
```

## 📌 33.7 Well-Known Symbol

- JS가 기본 제공하는 빌트인 심벌 값
- JS 엔진의 내부 알고리즘에 사용되며 Symbol 함수의 프로퍼티에 할당되어 있다.
- Array, String, Map, Set처럼 for...of문으로 순회 가능한 빌트인 이터러블은 Well-Known Symbol인 Symbol.iterator를 키로 갖는 메서드를 가진다.
- 이 메서드를 호출하면 이터레이터를 반환하도록 규정되어 있으며, 빌트인 이터러블은 이터레이션 프로토콜을 준수한다.
- 빌트인 이터러블이 아닌 일반 객체를 이터러블처럼 동작하도록 구현하려면 이터레이션 프로토콜을 준수하면 된다.
  - Symbol.iterator를 키로 갖는 메서드를 객체에 추가하고 이터레이터를 반환하도록 구현하면 해당 객체는 이터러블이 된다.
- 즉 이터레이션 프로토콜은 이터러블을 만드는 규약이고, 빌트인 이터러블은 이 규약을 준수하는 것

```
const iterable = {
    [Symbol.iterator]() {
        let cur = 1;
        const max = 5;

        return {
            next() {
                return { value: cur++, done: cur > max + 1};
            }
        };
    }
};

for (const num of iterable) {
    console.log(num); // 1 2 3 4 5
}
```

- 이터레이터는 JS에서 값을 순회하는데 사용되는 객체로 value, done 프로퍼티를 가진 객체를 반환하는 next() 메서드를 포함한다.
  - value: 현재 순회중인 요소의 값
  - done: 순회가 끝났는지 나타내는 불리언 값
  - 보통 for...of 문과 같은 순회 구조와 함께 사용된다. (이터러블 객체의 요소들을 반복적으로 접근하기 위해 이터레이터를 사용함)
