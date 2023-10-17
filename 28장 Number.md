# 28장 Number
## Number 생성자 함수
- 표준 빌트인 객체인 Number 객체는 생성자 함수 객체다. (new 연산자와 함께 호출하여 Number 인스턴스 생성)
- Number 생성자 함수에 인수를 전달하지 않고 new 연산자와 함께 호출하면 [[NumberData]] 내부 슬롯에 0을 할당한 Number 래퍼 객체를 생성
- [[PrimitiveValue]] 프로퍼티 -> [[NumberData]] 내부 슬롯을 가리킨다.
- 인수로 숫자가 아닌 값을 전달하면 강제 변환한 후 [[NumberData]] 내부 슬롯에 변환된 숫자를 할당한 Number 객체를 생성한다. 인수로 변환 불가능하면 NaN을 [[NumberData]] 내부 슬롯에 할당한 Number 객체 생성

<br/>

## Number 프로퍼티
### Number.EPSILON
- 1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이와 같다
- 부동소수점을 표현하기 위해 가장 널리 쓰이는 표준 IEEE 754는 2진법으로 반환 시 무한소수가 되어 미세한 오차가 발생할 수밖에 없는 구조적 한계가 있다. EPSILON을 사용하면 오차 해결이 가능하다.

<br/>

### Number.MAX_VALUE
- 자바스크립트에서 표현할 수 있는 가장 큰 양수 값이다.
- 이 수보다 더 큰 숫자는 Infinity다.

<br/>

### Number.MinValue
- 자바스크립트에서 표현할 수 있는 가장 작은 양수 값이다.
- 이 수보다 더 작은 수는 0이다.

<br/>

### Number.MAX_SAFE.INTEGER
- JS에서 안전하게 표현할 수 있는 가장 큰 정수값이다.
- 9007199254740991

<br/>

### Number.MIN_SAFE.INTEGER
- JS에서 안전하게 표현할 수 있는 가장 작은 정수값이다
- -9007199254740991

<br/>

### Number.POSITIVE_INFINITY
- 양의 무한대를 나타내는 숫자값 Infinity와 같다.

<br/>

### Number.NAGATIVE_INFINITY
- dma의 무한대를 나타내는 숫자값 -Infinity와 같다.

<br/>

### Number.NaN
- 숫자가 아님을 나타내는 숫자값이다.
- window.NaN과 같다.

<br/>

## Number 메서드
### Number.isFinite
- 정적 메서드로 인수로 전달된 숫자값이 정상적인 유한수 (Infinity or -Infinity)인지 검사하고 결과를 불리언으로 반환
- 인수가 NaN이면 언제나 false
- isFinite와의 차이점은 isFinite은 타입을 변환하여 검사를 수행하지만 Number.isFinite은 암묵적 타입 변환 X

<br/>

### Number.isInteger
- 전달된 숫자값이 정수인지 검사하여 결과값을 불리언으로 반환
- 암묵적 타입변환 x

<br/>

### Number.isNaN
- 인수로 전달된 값이 NaN인지 검사하여 결과를 불리언으로 반환
- 빌트인 전역 함수 isNaN과 차이가 있다면 암묵적 타입 변환을 수행하지 않음

<br/>

### Number.isSafeInteger
- 전달된 숫자값이 안전한 정수인지 검사하여 결과를 불리언 값으로 반환
- 검사전에 인수를 숫자로 암묵적 타입 변환 x
- 안전한 정수값 -(2 ** 53 - 1)과 2 ** 53 - 1사이의 정수값

<br/>

Number.prototype.toExponential
- 숫자를 지수 표기법(매우 크거나 작은 숫자를 표기할 때 사용)으로 변환하여 문자열로 반환
- 인수로 소수점 이하로 표현할 자릿수를 전달할 수 있다.
- 숫자 리터럴과 함께 사용하면 에러 발생 ( JS 엔진이 소수점 . 인지 부동 소수점의 구분기호인지 구분을 못 한다.


```
(77.1234).toExponential(); // "7.71234e+1"

77.1234.toExponential(); // SyntaxError
```

<br/>

### Number.porototype.toFixed
- 숫자를 반올림하여 문자열로 변환
- 반올림하는 소수점 이하 자릿수를 나타내는 0~20 사이의 정수값을 인수로 전달 가능
- 인수 생략시 기본값 0

```
(12345.6789).toFixed() // "12346
```

<br/>

### Number.prototype.toPrecision
- 인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환
- 인수로 전달받은 전체 자릿수로 표현할 수 없는 경우 지수 표기법으로 결과 반환
- 인수 생략 시 기본값 0, 0~21 사이의 정수값을 인수로 전달

```
(12345.6789).toPrecision(); // "12345.6789"
```

<br/>

### Number.prototype.toString
- 숫자를 문자열로 변환하여 반환
- 진법을 나타내는 2~36 사이의 정수값을 인수로 전달 가능
- 인수 생략 시 기본값 10진법

```
(10).toString() // "10"
```

<br/>
