# ✏️ 28장 배열

## 📌 28.1 Number 생성자 함수

- 표준 빌트인 객체인 Number는 생성자 함수 객체다.
- 따라서 new 연산자와 함께 호출해 Number 인스턴스를 생성할 수 있다.
- Number 생성자 함수에 인수를 전달하지 않고 new 연산자와 호출하면 [[NumberData]] 내부 슬롯에 0을 할당한 Number 래퍼 객체를 생성한다.
- 아래의 [[PrimitiveValue]]는 접근할 수 없는 프로퍼티이자 [[NumberData]]의 내부 슬롯이다.
- Number 생성자 함수에 숫자를 인수로 전달하면서 new 연산자와 호출하면 [[NumberData]] 내부 슬롯에 인수로 전달 받은 숫자를 할당한 Number 래퍼 객체를 생성한다.
- 숫자가 아닌 값을 인수로 전달하면, 인수를 숫자로 강제 변환한 후 변환된 숫자를 [[NumberData]]에 할당한 Number 래퍼 객체를 생성한다. 숫자로 변환할 수 없다면 NaN을 할당한다.

```
// 인수를 전달하지 않음
const numObj = new Number();
console.log(numObj); // Number {[[PrimitiveValue]]: 0}

// 인수를 전달함
const numObj = new Number(10);
console.log(numObj); // Number {[[PrimitiveValue]]: 10}

// 강제로 변환
const numObj = new Number('10');
console.log(numObj); // Number {[[PrimitiveValue]]: 10}

// 숫자로 변환할 수 없음
const numObj = new Number('Hello');
console.log(numObj); // Number {[[PrimitiveValue]]: NaN}
```

- new 연산자를 사용하지 않고 Number 생성자 함수를 호출하면 Number 인스턴스가 아닌 숫자를 반환하는데, 이를 이용해서 명시적으로 타입을 변환할 수 있다.

```
Number('0'); // 0
Number('-1'); // -1
Number('10.53'); // 10.53

Number(true); // 1
Number(false); // 0
```

## 📌 28.2 Number 프로퍼티

#### 1. Number.EPSILON

- ES6에서 도입된 Number.EPSILON은 1과 1보다 큰 숫자 중 가장 작은 숫자와의 차이와 같다.
- 0.1 + 0.2 !== 0.3 처럼 부동소수점으로 인해 발생하는 오차를 해결하기 위해 사용된다.
- JS 숫자 타입의 값은 IEEE 754의 부동소수점 표현 형식 중 배정밀도 64비트 부동소수점 형식을 따른다.
  - C, JAVA의 경우 정수와 실수를 구분해 int, long, float 등 정수와 실수를 타입으로 구분해 사용하지만 JS는 모든 수를 실수로 처리하기 때문에 하나의 숫자 타입만 존재한다.
  - JS는 10진수인 소수를 2진수로 변환하고, 소수점이 1이 나올 때까지 좌우로 옮긴 후 소수점 우측에 해당하는 수를 가수로 넣는데, 이 수가 표현 자리수를 넘으면 나머지 부분에서 반올림 처리를 하기에 근사값이 생기면서 오차가 발생하게 된다.
- 부동소수점 표현은 부호/지수/가수 3가지 부분으로 구성된다.
  - 부호: 양수면 0이고 음수면 1이 된다. (1비트)
  - 지수: 소수점을 이동 시킬 위치를 결정한다. 소수점 자리가 1.xxx가 되도록 좌우로 옮긴다. 지수 부분을 구하려면 2를 밑으로 좌우로 이동한 수에 Bias를 더하고 2진법으로 변환한다. (11비트)
  - 가수: 소수점을 옮긴 수에서 소수점의 오른쪽 부분이다. (52비트)
- 예시: 0.1(10)을 2진법으로 바꾸면 0.00011001100...(2)로 순환 소수가 나온다.
  - 부호: 양수이므로 0이 된다.
  - 지수: 소수점의 자리가 1.xxx가 되도록 오른쪽으로 4칸 이동한다. 1.1001100...(2) \* 2^-4로 표현할 수 있고, 2^-4의 지수인 -4에 Bias를 더하고 2진법으로 변환한다.
    - Bias란 지수 편향을 말하는데, 이를 더하는 이유는 양수는 2진법으로 표현할 수 있지만 음수는 표현할 수 없기 때문이다.
    - Bias는 2^(지수 부분의 총 비트 수 -1) -1이므로, 64비트의 지수 부분은 11비트이기에 2^(11-1)-1로 2^10-1이 되어 1023이다. 따라서 0~1023에 해당하면 음수, 1024~2047에 해당하면 양수가 저장된다.
    - -4 + 1023 = 1019를 2진법으로 변환하면 1111111011인데, 지수 부분은 총 11비트이기 때문에 앞에 0을 붙여서 01111111011이 들어가게 된다.
  - 가수: 1001100 부분으로 계속 순환되기 때문에 52비트에 넣으면 52번째 이후는 반올림을 해야 한다. 10011001100110011001100110011001100110011001100110011로 나타낼 수 있다.
  - 따라서 0.1(10)을 64비트 부동소수점 형식으로 표현하면 0 01111111011 10011001100110011001100110011001100110011001100110011과 같다.

```
function isEqual(a, b) {
    // a-b의 절대값이 Number.EPSILON보다 작으면 같은 수로 인정
    return Math.abs(a - b) < Number.EPSILON;
}
isEqual(0.1 + 0.2, 0.3); // true
```

#### 2. Number.MAX_VALUE

- JS에서 표현할 수 있는 가장 큰 양수 값으로 1.7976931348623157 \* 10^308이다.
- 부동소수점 형식에서 표현 가능한 가장 큰 수로, 이보다 더 큰 숫자는 Infinity다.

#### 3. Number.MIN_VALUE

- JS에서 표현할 수 있는 가장 작은 양수 값으로 5 \* 10-324다.
- 부동소수점 형식에서 표현 가능한 가장 작은 수로, 이보다 더 작은 숫자는 0이다.

#### 4. Number.MAX_SAFE_INTEGER

- JS에서 안전하게 표현할 수 있는 가장 큰 정수값으로 2^53-1이다.
- 64비트 부동소수점 형식에서 가수 부분은 52비트, 부호 비트는 1비트로 정수는 총 53비트로 표현될 수 있다. 2^52까지의 정수는 정확하게 표현할 수 있으므로 2^53에서 1을 뺀 값이 가장 안전한 정수값이 된다.

#### 5. Number.MIN_SAFE_INTEGER

- JS에서 안전하게 표현할 수 있는 가장 작은 정수값으로 -2^53+1이다.
- 동일하게 가수 부분은 52비트, 부호 비트는 1비트로 정수는 총 53비트로 표현될 수 있는데 여기서 1을 더한 값이 가장 안전한 정수값이 된다.

#### 6. Number.POSITIVE_INFINITY

- 양의 무한대를 나타내는 숫자값 Infinity와 같다.

#### 7. Number.NEGATIVE_INFINITU

-음의 무한대를 나타내는 숫자값 -Infinity와 같다.

#### 8. Number.NaN

- 숫자가 아님을 나타낸다.
- window.NaN과 같다.

## 📌 28.3 Number 메서드

#### 1. Number.isFinite

- ES6에서 도입된 Number.isFinite는 정적 메서드다.
- 인수로 전달된 숫자값이 정상적인 유한수인지 무한수인지 검사해 그 결과를 불리언 값으로 반환한다.
- 인수가 NaN이라면 언제나 false를 반환한다.
- 빌트인 전역 함수 isFinite는 암묵적 타입 변환 후 검사를 수행하지만 Number.isFinite는 변환하지 않는다. 따라서 숫자가 아닌 인수에 대한 반환값은 언제나 false다.

```
// 인수가 정상적인 유한수일 때
Number.isFinite(0); // true
Number.isFinite(Number.MAX_VALUE); // true
Number.isFinite(Number.MIN_VALUE); // true

// 인수가 무한수일 때
Number.isFinite(Infinity); // false
Number.isFinite(-Infinity); // false

isFinite(null); // true (0)
Number.isFinite(null); // false
```

#### 2. Number.isInteger

- ES6에서 도입된 Number.isInteger는 정적 메서드다.
- 인수로 전달된 숫자값이 정수인지 검사해 그 결과를 불리언 값으로 반환한다.
- 암묵적 타입 변환을 하지 않는다.

```
// 인수가 정수일 때
Number.isInteger(0); // true
Number.isInteger(123); // true
Number.isInteger(-123); // true

// 인수가 정수가 아닐 때
Number.isInteger(0.5); // false
Number.isInteger('123'); // false
Number.isInteger(false); // false
Number.isInteger(Infinity); // false
Number.isInteger(-Infinity); // false
```

#### 3. Number.isNaN

- ES6에서 도입된 Number.isNaN은 정적 메서드다.
- 인수로 전달된 숫자값이 NaN인지 검사해 그 결과를 불리언 값으로 반환한다.
- 빌트인 전역 함수 isNaN 암묵적 타입 변환 후 검사를 수행하지만 Number.isNaN는 변환하지 않는다. 따라서 숫자가 아닌 인수에 대한 반환값은 언제나 false다.

```
// 인수가 NaN일 때
Number.isNaN(NaN); // true

isNaN(undefined); // true (null, '', 문자열)
Number.isNaN(undefined); // false
```

#### 4. Number.isSafeInteger

- ES6에서 도입된 Number.isSafeInteger는 정적 메서드다.
- 인수로 전달된 숫자값이 안전한 정수인지 검사해 그 결과를 불리언 값으로 반환한다.
- 안전한 정수값은 -(2^53-1) ~ 2^53-1 사이의 정수값이다.
- 암묵적 타입 변환을 하지 않는다.

```
// 안전한 정수
Number.isSafeInteger(0); // true
Number.isSafeInteger(1000000000000000); // true

// 안전한 정수가 아니거나 정수가 아님
Number.isSafeInteger(1000000000000001); // false
Number.isSafeInteger(0.5); // false
Number.isSafeInteger('123'); // false
Number.isSafeInteger(Infinity); // false
```

#### 5. Number.prototype.toExponential

- 숫자를 지수 표기법으로 변환해 문자열로 반환한다.
- 지수 표기법: 매우 크거나 작은 숫자를 표기할 때 주로 사용하며 e 앞에 있는 숫자에 10의 n승을 곱하는 형식으로 수를 나타내는 방식
- 인수로 소수점 이하로 표현할 자릿수를 전달할 수 있다.
- 숫자 리터럴과 Number 프로토타입 메서드를 사용할 경우 에러가 발생한다.
  - 숫자 리터럴은 원시 자료형으로 숫자 값 자체에는 메서드가 없기 때문에 직접 호출이 불가하다.
  - Number는 원시 자료형이 아닌 래퍼 객체로, Number 객체는 Number.prototype을 상속해 여러 메서드를 사용할 수 있기 때문에 다르다.
- 숫자 뒤의 .은 부동 소수점 숫자의 소수 구분 기호일 수도 있고 프로퍼티 접근 연산자일 수도 있다.
- JS는 숫자 뒤의 .을 전자로 해석하지만 77.toExponential()의 77은 Number 래퍼 객체라 프로퍼티로 해석할 수 없으므로 에러가 발생한다. 따라서 그룹 연산자로 감싸주거나 공백을 추가해야 한다.

```
(77.1234).toExponential(); // "7.71234e+1"
(77.1234).toExponential(4); // "7.7123e+1"
(77.1234).toExponential(2); // "7.71e+1"

77.toExponential(); // 에러 발생
77 .toExponential(); // "7.7e+1"
```

#### 6. Number.prototype.toFixed

- 숫자를 반올림해 문자열로 반환한다.
- 반올림하는 소수점 이하 자릿수를 나타내는 0~20 사이의 정수값을 인수로 전달할 수 있다.
- 인수를 생략하면 기본값 0이 지정된다.

#### 7. Number.prototype.toPrecision

- 인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림해 문자열로 반환한다.
- 인수로 전달받은 전체 자릿수로 표현할 수 없는 경우 지수 표기법으로 결과를 반환한다.
- 전체 자릿수를 나타내는 0~21 사이의 정수값을 인수로 전달할 수 있다.
- 인수를 생략하면 기본값 0이 지정된다.

```
(12345.6789).toPrecision(); // "12345.6789"
(12345.6789).toPrecision(1); // "1e+4"
(12345.6789).toPrecision(2); // "1.2e+4"
(12345.6789).toPrecision(6); // "12345.7"
```

#### 8. Number.prototype.toString

- 숫자를 문자열로 변환해 반환한다.
- 진법을 나타내는 2 ~ 36 사이의 정수값을 인수로 전달할 수 있다.
- 인수를 생략하면 기본값 10진법이 지정된다.

```
(10).toString(); // "10"
(16).toString(2); // "10000"
(16).toString(8); // "20"
(16).toString(16); // "10"
```
