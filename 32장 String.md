# 32장 String
## String 생성자 함수
- 표준 빌트인 객체인 String 객체는 생성자 함수 객체
- new 연산자와 함께 호출하여 String 인스턴스를 생성할 수 있다.
- String 생성자 함수에 인수를 전달하지 않고 new 연산자와 함께 호출하면 [[StringData]] 내부 슬롯에 빈문자열을 할당한 String 래퍼 객체를 생성

```
const strObj = new String();
console.log(strObj); // String {length: 0, [[PrimitiveValue]]: ""}
```

- String 생성자 함수의 인수로 문자열을 전달하면서 new 연산자와 함께 호출하면 [[StringData]] 내부 슬롯에 인수로 전달받은 문자열을 할당한 String 래퍼 객체를 생성

```
const strObj = new String('Lee');
console . log(strObj);
// String {0: "L", 1: "e", 2: "e", length: 3, [[PrimitiveValue]]: "Lee"}
```

- length 프로퍼티와 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로, 각 문자를 프로퍼티 값으로 갖는 유사 배열 객체이 면서 이터러블이다.
- 배열과 유사하게 인덱스를 사용하여 각 문자에 접근할 수 있다.
- 단, 문자열은 원시 값이므로 변경할 수 없다. 이때 에러가 발생 X
    
```
const strObj = new String('Lee');
console . log(strObj[0]); // L
```     

- String 생성자 함수의 인수로 문자열이 아닌 값을 전달하면 인수를 문자열로 강제 변환한 후, [[StringData]] 내부 슬롯에 변환된 문자열을 할당한 String 래퍼 객체를 생성

```
String(1) // "1"
string(true) // "true"
```

- new 연산자를 사용하지 않고 String 생성자 함수를 호출하면 String 인스턴스가 아닌 문자열을 반환
- 이를 이용하여 명시적으로 타입을 변환하기도 한다.
    
```
let strObj = new String(123);
console . log(strObj);
// String {0: "1", 1: "2", 2: "3", length: 3, [[PrimitiveValue]]: "123"}
strObj = new String(null);
console . log(strObj);
// String {0: "n", 1: "u", 2: "l", : "l", length: 4, [[PrimitiveValue]]: "null"}
```

<br/>

## length 프로퍼티
- length 프로퍼티는 문자열의 문자 개수를 반환

```
'Hello'.length; // 5
'안녕하세요!'.length; //  6
```

<br/>

## String 메서드
- String 객체에는 원본 String 래퍼 객체를 직접 변경하는 메서드는 존재 X -> 언제나 새로운 문자열을 반환, 문자열은 변경 불가능한 원시값이기 때에 String 래퍼 객체도 읽기 전용 객체로 제공

<br/>

### indexOf
- 인수로 전달받은 문자열을 검색하여 첫 번째 인덱스를 반환, 검색에 실패하면 -1을 반환
- indexOf 메서드의 2번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.
- String.prototype.includes 메서드를 사용하면 가독성이 더 좋다.

```
const str = 'Hello World';

// 문자열 str에서 'l'을 검색하여 첫 번째 인덱스를 반환한다.
str . indexOf('l'); // 2

// 문자열 str에서 'x'를 검색하여 첫 번째 인덱스를 반환한다. 검색에 실패하면 -1을 반환한다.
str . indexOf('x'); // -1

str.indexOf('l', 3); // 3
```

<br/>

### search
- 정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스를 반환
- 검색 실패하면 -1을 반환

```
const str = 'Hello world';

// 문자열 str에서 정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스를 반환한다.
str . search(/o/); //  4
str . search(/x/); //  -1
```

<br/>

### includes
- 문자열이 포함되어 있는지 확인하여 그결과를 true 또는 false 로 반환

```
const str = 'Hello world';

str . includes('Hello'); // true 
str . includes(''); // true 
str . includes('x'); // false 
str . includes(); // false

// 2번째 인수로 검색을 시작할 인덱스 전달
str.includes('l' , 3); //  true 
str.includes('H' , 3); //  false
```

<br/>

## startsWith
- 전달받은 문자열로 시작하는지 확인하여 그 결과를 true 또는 false 로 반환

```
const str = 'Hello world';

// 문자열 str이 'He'로 시작하는지 확인
str.startsWith('He'); // true

// 문자열 str이 'x'로 시작하는지 확인
str.startsWith('x'); //  false

// 2번째 인수로 검색을 시작할 인덱스 전달
str.startsWith(' ' , 5); // true
```

<br/>

## endsWith
- 전달받은 문자열로 끝나는지 확인하여 그 결과를 true 또는 false 로 반환

```
const str = 'Hello world';

// 문자열 str이 'ld'로 끝나는지 확인
str.endsWith('ld'); // true
// 문자열 str이 'x'로 끝나는지 확인
str.endsWith('x'); // false

// 문자열 str의 처음부터 5자리까지('Hello')가 'lo'로 끝나는지 확인
str.endsWith('lo' , 5); // true
```

<br/>

## charAt
- 전달받은 인덱스에 위치한 문자를 검색하여 반환

```
const str = 'Hello';
for (let i = 0; i < str . length; i++) { 
	console . log(str . charAt(i)); // H e l l o 
}

// 인덱스가 문자열의 범위(0 ~ str.length-1)를 벗어난 경우 빈 문자열을 반환
str.charAt(5); // ''
```

<br/>

## subString
- 전달받은 인덱스에 위치하는 문자부터 두 번째 인수로 전달받은 인덱스에 위치하는 문자의 바로 이전 문자까지의 부분 문자열을 반환

```
const str = 'Hello World';

// 인덱스 1부터 인덱스 4 이전까지의 부분 문자열을 반환한다.
str.substring(1 , 4); // ell

// 인덱스 1부터 마지막 문자까지 부분 문자열을 반환한다.
str.substring(1); // 'ello World'
```

### 정상 동작
- 첫 번째 인수 > 두 번째 인수인 경우 두 인수는 교환된다.
- 인수 < 0 또는 NaN인 경우 0으로 취급된다.
- 인수 > 문자열의 길이(str . length)인 경우 인수는 문자열의 길이(str . length)로 취급된다.

<br/>

### slice
- substring 메서드와 동일하게 동작
- slice 메서드에는 음수인 인수를 전달할 수 가능 음수인 인수를 전달하면 대상 문자열의 가장 뒤에서부터 시작하여 문자열을 잘라내어 반환

```
const str = 'Hello World';

// 인수 < 0 또는 NaN인 경우 0으로 취급된다.
str.substring(-5); // 'hello world'

// slice 메서드는 음수인 인수를 전달할 수 있다. 뒤에서 5자리를 잘라내어 반환한다.
str.slice(-5); // 'world'
```

<br/>


### toUpperCase
- 대상 문자열을 모두 대문자로 변경한 문자열을 반환

```
const str = 'Hello World!';
str.toUpperCase(); // 'HELLO WORLD!'
```

<br/>


### toLowerCase
- 대상 문자열을 모두 소문자로 변경한 문자열을 반환

```
const str = 'Hello World!';
str.toLowerCase(); // 'hello world!'
```

<br/>


### trim
- 대상 문자열 앞뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환
- trimStart, trimEnd를사용하면 대상 문자열 앞 또는 뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환

```
const str = ' foo ';
str.trim(); // 'foo'

const str = ' foo ';
str.trimStart(); // 'foo ' 
str.trimEnd(); // ' foo'
```

### repeat
- 문자열을 인수로 전달받은 정수만큼 반복해 연결한 새로운 문자열을 반환
- 인수로 전달받은 정수가 0이면 빈 문자열을 반환하고, 음수이면 RangeError
- 인수를 생략하면 기본값 0이 설정

```
const str = 'abc';

str.repeat(); // '' 
str.repeat(0); // '' 
str.repeat(1); //  'abc' 
str.repeat(2); //  'abcabc' 
str.repeat(2 . 5); //  'abcabc' (2.5 → 2) 
str.repeat(-1); //  RangeError: Invalid count value
```

<br/>

### replace
- 첫 번째 인수로 전달받은 문자열 또는 정규표현식을 검색하여 두 번째 인수로 전달한 문자열로 치환한 문자열을 반환
- 검색된 문자열이 여럿 존재하면 첫 번째 문자만 치환

```
const str = 'Hello world';

// str에서 첫 번째 인수 'world'를 검색하여 두 번째 인수 'Lee'로 치환한다.
str.replace('world' , 'Lee'); //  'Hello Lee'
```

- 특수한 교체 패턴

```
const str = 'Hello world';

// 특수한 교체 패턴을 사용할 수 있다. ($& => 검색된 문자열)
str.replace('world' , '<strong>$&</strong>');
```

- 첫번째 인수에 정규표현식 검색

```
const str = 'Hello Hello';

// 'hello'를 대소문자를 구별하지 않고 전역 검색한다.
str.replace(/hello/gi , 'Lee'); // 'Lee Lee'
```

- 첫 번째 인수로 전달한 문자열 또는 정규 표현식에 매치한 두 번째 인수로 전달한 치환 함수의 인수로 전달하면서 호출하고 치환 함수가 반환한 결과와 매치 결과를 치환

```
// 카멜 케이스를 스네이크 케이스로 변환하는 함수
function camelToSnake(camelCase) {

return camelCase.replace(/ . [A-Z]/g , match => { 
console.log(match); // 'oW' 
return match[0] + '_' + match[1].toLowerCase();
});
}

const camelCase = 'helloWorld';

camelToSnake(camelCase); // 'hello_world'

// 스네이크 케이스를 카멜 케이스로 변환하는 함수
function snakeToCamel(snakeCase) {
	return snakeCase.replace(/_[a-z]]/g , match => { 
		console.log(match); // '_w'
		return match[1].toUpperCase();
	});
}

const snakeCase = 'hello_world';
snakeToCamel(snakeCase); // 'helloWorld'
```

<br/>

### split
- 첫 번째 인수로 전달한 문자열 또는 정규 표현식을 검색하여 문자열을 구분한 후 분리된 각 문자열로 이루어진 배열을 반환
- 인수로 빈 문자열을 전달하면 각 문자를 모두 분리하고, 인수를 생략하면 대상 문자열 전체를 단일 요소로 하는 배열을 반환
- 두 번째 인수로 배열의 길이 지정 가능
- reverse, join 메서드와 함께 사용하면 문자열을 역순으로 뒤집을 수 있다.

```
const str = 'How are you doing?';

// 공백으로 구분(단어로 구분)하여 배열로 반환한다.
str.split(' '); // ["How", "are", "you", "doing?"]

// \s는 여러 가지 공백 문자(스페이스, 탭 등)를 의미한다. 즉, [\t\r\n\v\f]와 같은 의미다.
str.split(/\s/); //  ["How", "are", "you", "doing?"]

// 인수로 빈 문자열을 전달하면 각 문자를 모두 분리한다.
str.split(''); //  ["H", "o", "w", " ", "a", "r" ... "?"]

// 인수를 생략하면 대상 문자열 전체를 단일 요소로 하는 배열을 반환한다.
str.split(); //  ["How are you doing?"]
```
