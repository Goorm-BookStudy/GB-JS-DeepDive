# 28장 Number

### 1. Number 생성자 함수

: 표준 빌트인 객체인 Number 객체는 생성자 함수 객체다. 따라서 new 연산자와 함께 호출하여 Number 인스턴스를 생성할 수 있다.

Number 생성자 함수에 인수를 전달하지 않고 new 연산자와 함께 호출하면 [[NumberData]] 내부 슬롯에 0을 할당한 Number 래퍼 객체를 생성한다.

```jsx
const numObj = new Number();
console.log(numObj); // Number {[[PrimitiveValue]]: 0}
```

⇒ [[PrimitiveValue]] 는 접근할수 없는 프로퍼티다.

⇒ 이는 [[NumberData]] 내부 슬롯을 가리킨다. ES5에서는 [[NumberData]]를 [[PrimitiveValue]]라 불렀다.

Number 생성자 함수에 인수로 숫자를 전달하면서 new 연산자와 함께 호출하면 [[NumberData]] 내부 슬롯에 인수로 전달받은 숫자를 할당한 Number 래퍼 객체를 생성한다.

```jsx
const numObj = new Number(10);
console.log(numObj); // Number {[[PrimitiveValue]]: 10}
```

Number 생성자 함수에 인수로 숫자가 아닌 인수를 전달하면 강제로 변환한 후, [[NumberData]] 내부 슬롯에 변환된 숫자를 할당한 Number 래퍼 객체를 생성한다. 인수를 숫자로 변환할 수 없다면 NaN를 [[NumberData]] 내부 슬롯에 할당한 Number 래퍼 객체를 생성한다.

```jsx
let numObj = new Number("10");
console.log(numObj); // Number {[[PrimitiveValue]]: 10}

let numObj = new Number("Hello");
console.log(numObj); // Number {[[PrimitiveValue]]: NaN}
```

```jsx
Number("0"); // 0
Number("-1"); // -1
Number("10.53"); // 10.53

Number(true); // 1
Number(false); // 0
```

### 2. Number 프로퍼티

### 2-1. Number.EPSILON

: ES6에서 도입된 Number.EPSILON은 1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이가 같다. Number.EPSILON은 약 2.2204460…. \* …. 이다.

- 부동소수점 산술 연산은 정확한 결과를 기대하기 어렵다.

```jsx
0.1 + 0.2; // 0.300000000000000004
0.1 + 0.2 === 0.3; // false
```

Number.EPSILON은 부동소수점으로 인해 발생하는 오차를 해결하기 위해 사용한다. 다음 예제는 Number.EPSILON를 사용하여 부동소수점을 비교하는 함수다.

```jsx
function isEqual(a, b) {
  // a와 b를 뺀 값의 절대값이 Number.EPSILON보다 작으면 같은 수로 인정한다.
  return Math.abs(a - b) < Number.EPSILON;
}

isEqual(0.1 + 0.2, 0.3); // true
```

### 2-2. Number.MAX_VALUE

: Number.Max_VALUE는 자바스크립트에서 표현할 수 있는 가장 큰 양수 값이다. Number.Max_VALUE 보다 큰 숫자는 Infinity다.

```jsx
Number.MAX_VALUE********************************************; //******************************************** 1.44392058392430e + 308
****************************************************************************************Infinity > Number.MAX_VALUE; // true
```

### 2-3. Number.MIN_VALUE

: Number.MIN_VALUE는 자바스크립트에서 표현할 수 있는 가장 작은 양수 값이다. Number.MIN_VALUE보다 작은 수는 0 이다.

```jsx
Number.MIN_VALUE;
Number.MIN_VALUE > 0; // true
```

### 2-4. Number.MAX_SAFE_INTEGER

: Number.MAX_SAFE_INTEGER는 자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값이다.

```jsx
Number.MAX_SAFE_INTEGER; // 90071939210892014
```

### 2-5. Number.MIN_SAFE_INTEGER

: Number.MIN_SAFE_INTEGER는 자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수값이다.

```jsx
Number.MIN_SAFE_INTEGER; // -9001390219305940
```

### 2-6. Number.POSITIVE_INFINITY

: Number.POSITIVE_INFINITY는 양의 무한대를 나타내는 숫자값 Infinity와 같다.

```jsx
Number.POSITIVE_INFINITY; // Infinity
```

### 2-7. Number.NEGATIVE_INFINITY

: Number.POSITIVE_INFINITY는 음의 무한대를 나타내는 숫자값 -Infinity와 같다.

```jsx
Number.NEGATIVE_INFINITY; // -Infinity
```

### 2-8. Number.NaN

: Number.POSITIVE_INFINITY는 음의 무한대를 나타내는 숫자값 -Infinity와 같다.

```jsx
Number.NaN; // NaN
```

### 3. Number 메서드

### 3-1. Number.isFinite

: ES6에서 도입된 Number.isFinite 정적 메서드는 인수로 전달된 숫자값이 정상적인 유한수, 즉 Infinity 또는 -Infinity가 아닌지 검사하여 그 결과를 불리언 값으로 반환한다.

만약 인수가 NaN이면 언제나 false를 반환한다.

```jsx
Number.isFinite(0); // true
Number.isFinite(Number.MAX_VALUE); // true
Number.isFinite(Number.MIN_VALUE); // true

Number.isFinite(Infinity); // false
Number.isFinite(-Infinity); // false

Number.isFinite(NaN); // false
```

⇒ Number.isFinite 정적 메서드는 빌트인 전역 함수 isFinite와 차이가 있다. 빌트인 전역 함수 isFinite는 전달받은 인수를 숫자로 암묵적 타입 변환하여 검사를 수행하지만 Number.isFinite는 전달받은 인수를 숫자로 암묵적 타입 변환하지 않는다. 따라서 숫자가 아닌 인수가 주어졌을 때 반환값은 언제나 false다

```jsx
Number.isFinite(null); // false

// null이 0으로 암묵적 타입변환됨
isFinite(null); // true
```

### 3-2. Number.isInteger

: ES6에서 도입된 Number.isInteger 정적 메서드는 인수로 전달된 숫자값이 정수인지 검사하여 그 결과를 불리언 값으로 반환한다. 검사하기 전에 인수를 숫자로 암묵적 타입 변환하지 않는다.

```jsx
Number.isInteger(0); // true
Number.isInteger(123); // true
Number.isInteger(-123); // true

Number.isInteger(0.5); // false
Number.isInteger("123"); // false
Number.isInteger(false); // false
Number.isInteger(Infinity); // false
Number.isInteger(-Infinity); // false
```

### 3-3. Number.isNaN

: ES6에서 도입된 Number.isNaN 정적 메서드는 인수로 전달된 숫자값이 NaN인지 검사하여 그 결과를 불리언 값으로 반환한다.

```jsx
Number.isNaN(NaN); // true
```

- Number.isNaN는 빌트인 전역 함수인 isNaN과 차이가 있다. 빌트인 전역 함수 isNaN은 전달받은 인수를 숫자로 암묵적 타입 변환하여 검사를 수행하지만 Number.isNaN 메서드는 전달받은 인수를 숫자로 암묵적 타입 변환하지 않는다. 따라서 숫자가 아닌 인수가 주어졌을 때 반환값은 언제나 false다.

```jsx
Number.isNaN(undefined); // false

// isNaN는 undefined를 NaN로 암묵적 타입변환한다.
isNaN(undefined); // true
```

### 3-4. Number.isSafeInteger

: ES6에서 도입된 Number.isSafeInteger 정적 메서드는 인수로 전달된 숫자값이 안전한 정수인지 검사하여 그 결과를 불리언 값으로 반환한다. 검사전에 인수를 숫자로 암묵적 타입 변환하지 않는다.

```jsx
Number.isSafeInteger(0); // true
Number.isSafeInteger(100000000000000); // true
Number.isSafeInteger(100000000000001); // false

Number.isSafeInteger(0.5); // false
Number.isSafeInteger("123"); // false
Number.isSafeInteger(false); // false
Number.isSafeInteger(Infinity); // false
```

### 3-5. Number.prototype.toExponential

: toExponential 메서드는 숫자를 지수 표기법으로 변환하여 문자열로 반환한다. 지수 표기법이란 매우 크거나 작은 숫자를 표기할 때 주로 사용되는 e(Exponent) 앞에 있는 숫자에 10의 n승을 곱하는 형식으로 수를 나타내는 방식이다. 인수로 소수점 이하로 표현할 자릿수를 전달할 수 있다.

```jsx
(77.1234).toExponential(); // "7.71234e+1"
(77.1234).toExponential(4); // "7.7123e+1"
(77.1234).toExponential(2); // "7.714e+1"
```

참고로 다음과 같이 숫자 리터럴과 함께 Number 프로토타입 메서드를 사용할 경우 에러가 발생한다.

```jsx
77.toExponential(); // SyntaxError....
```

⇒ 숫자뒤에 .의 의미가 모호하기 때문이다.

```jsx
(77.1234).toExponential(); // "7.71234e+1"
(77).toExponential(); // "7.7e+1"
(77).toExponential(); // "7.7e+1"
```

### 3-6. Number.prototype.toFixed

: toFixed 메서드는 숫자를 반올림하여 문자열로 반환한다. 반올림하는 소수점 이하 자릿수를 나타내는 0~20사이의 정수값을 인수로 전달할 수 있다. 인수를 생략하면 기본값이 0이 지정된다.

```jsx
(12345.6789).toFixed(); // 12346
(12345.6789).toFixed(1); // 12345.7
(12345.6789).toFixed(2); // 12345.68
(12345.6789).toFixed(3); // 12345.679
```

### 3-7. Number.prototype.toPercision

: toPercision 메서드는 인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환한다. 인수로 전달받은 전체 자연수로 표현할 수 없는 경우 지수 표기법으로 결과를 반환한다.

전체 자릿수를 나타내는 0~21 사이의 정수값을 인수로 전달할 수 있다. 인수를 생략하면 기본값은 0이 지정된다.

```jsx
(12345.6789).toPercision(); // 12345.6789
(12345.6789).toPercision(1); // 1e+4
(12345.6789).toPercision(2); // 12e+4
(12345.6789).toPercision(6); // 12345.7
```

### 3-8. Number.prototype.toString

: toString 메서드는 숫자를 문자열로 변환하여 반환한다. 진법을 나타내는 2~36 사이의 정수값을 인수로 전달할 수 있다. 인수를 생략하면 기본값 10진법이 지정된다.

```jsx
(10).toString(); // "10"
(16).toString(2); // "10000"
(16).toString(8); // "20"
(16).toString(16); // "10"
```
