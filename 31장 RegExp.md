# 31장 RegExp
## 31.1 정규 표현식이란
- 정규 표현식regular expression은 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어formal language
- 자바스크립트의 고유 문법 X
- 문자열을 대상으로 특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 수 있는 패턴 매칭 기능을 제공
- 정규 표현식을 사용하면 반복문이나 조건문 없이 패턴을 정의하고 테스트하는 것으로 간단히 체크할 수 있지만, 가독성이 좋지 않다는 문제점이 있다.

#### 31.2 정규 표현식의 생성
1. 일반적인 - 방법 정규 표현식 리터럴 사용
- 패턴 - 문자열의 일정한 규칙을 표현
- 플래그 - 정규 표현식의 검색 방식을 설정

```
const testString = 'super duper ultra happy';

// 패턴: is
// 플래그 i: 대소문자 구별 없이 검색
const regexp = /is/i;

// test - target 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환
regexp.test(testString);
```

2. RegExp 생성자 함수 사용
- 변수를 사용해 동적으로 RegExp 객체 생성 가능

```
/**
 * pattern: 정규 표현식의 패턴
 * flags: 정규 표현식의 플래그(g, i, m, u, y)
 */
new RegExp(pattern[, flags])

const regexp = new RegExp(/is/i); // ES6
// new RegExp(/is/, 'i');
// new RegExp('is', 'i');

regexp.test(target); // true

// 동적으로 RegExp 객체 생성
const count = (str, char) => (str.match(new RegExp(char, 'gi')) ?? []).length;

count('Is this all there is?', 'is'); //3
count('Is this all there is?', 'xx'); //0
```

<br/>

## RegExp 메서드
### RegExp.prototype.exec
- 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환, 매칭 결과가 없는 경우 null을 반환
- g 플래그(문자열 내의 모든 패턴 검색)를 지정해도 첫 번째 매칭 결과만 반환

```
const target = 'Is this all there is';
const regexp = /is/;

regexp.exec(target);
// [ 'is', index: 5, input: 'Is this all there is', groups: undefined ]
```

<br/>

### RegExp.prototype.test

- 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환

```
const target = '\Is this all there is';
const regexp = /is/;

regexp.test(target); // true
```

### String.prototype.match
- 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환
- exec 메서드와는 다르게 g 플래그가 지정되면 모든 매칭 결과를 배열로 반환

```
// g플래그 적용
const target = 'Is this all there is';
const regexp = /is/g;

target.match(regexp);
// ['is', 'is']
```

## 플래그

|플래그|의미|설명|
|:---:|:---:|:----:|
|i|Ignore case|대소문자를 구별하지 않고 패턴을 검색한다.|
|g|Global|대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다.|
|m|Multi line|문자열의 행이 바뀌더라도 패턴 검색을 계속한다.|

<br/>

- 플래그는 옵션이라 선택적으로 사용
- 사용하지 않은 경우 대소문자를 구별해서 패턴을 검색하고 첫 번째 매칭한 대상만 검색하고 종료
- 순서와 상관없이 하나 이상의 플래그를 동시에 설정 가능

## 패턴
- /로 열고 닫으며 문자열의 따옴표는 생략
- 특별한 의미를 가지는 메타문자 또는 기호로 표현할 수 있다.
- 어떤 문자열 내에 패턴과 일치하는 문자열이 존재할 때 '정규 표현식과 매치한다'고 표현한다.

<br/>

##### 임의의 문자열 검색
- .은 임의의 문자 한 개를 의미

```
const target = 'Is this all there is';

// 임의의 3자리 문자열을 대소문자를 구별하여 전역으로 검색
const regexp = /.../g;

target.match(regexp);
// [ 'Is ", "thi", "s a", "ll ", "the ", "re ", "is?"]
```

<br/>

### 반복 검색
- {m,n} - 앞선 패턴이 최소 m번, 최대 n번 반복되는 문자열dmf dmlal
- 콤마 뒤에 공백이 있으면 정상 동작하지 않으므로 주의

```
const target = 'A AA B BB Bb AAA';

// 'Z'가 최소 1번, 최대 2번 반복되는 문자열 전역 검색
const regexp = /A{1,2}/g;

target.match(regexp); // [ 'A', 'A', 'B', 'BB', 'A' ]
```

- {n} - 앞선 패턴이 n번 반복되는 문자열 {n,n}과 같다.
- {n,}: 앞선 패턴이 최소 n번 이상 반복되는 문자열
- + - 앞선 패턴이 최소 한번 이상 반복되는 문자열 (= {1,})

```
const target = 'A AA B BB Aa Bb AAA';

// 'Z'가 최소 1번 이상 반복되는 문자열 전역 검색
const regexp = /A+/g;

target.match(regexp);
// [ 'A', 'AA', 'A', 'AAA' ]
```

- ? - 앞선 패턴이 최대 한 번(0번 포함) 이상 반복되는 문자열 {0,1}과 같다.

```
const target = 'color colour';

// 'colo' 다음 'u'가 최대 한 번(0번 포함) 이상 반복되고,
// 'r'이 이어지는 문자열 color, colour 전역 검색
const regexp = /colou?r/g;

target.match(regexp); // [ 'color', 'colour' ]
```

<br/>

### OR 검색
- |은 or의 의미를 갖는다. 

```
const target = 'A AA B BB Aa Bb';

// 'A' 또는 'B'를 전역 검색
const regExp = /A|B/g;

target.match(regExp); // ['A', 'A', 'A', 'B' 'B', 'B', 'A', 'B']

// 'A' ~ 'Z' 또는 'a' ~ 'z'가 한 번 이상 반복되는 문자열을 전역 검색
const regExp = /[A-Za-z]+/g;

target.match(regExp); // ['AA', 'BB', 'Aa', 'Bb']

// '0' ~ '9' 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색
const regExp = /[0-9,]+/g;

target.match(regExp); // ['12,345]
```

<br/>

### NOT 검색
- [...] 내의 ^은 not의 의미

```
const target = 'AA BB 12 Aa Bb';

// 숫자를 제외한 문자열을 전역 검색
const regExp = /[^0-9]+/g;

target.match(regExp) // ['AA BB', ' Aa Bb']
```

<br/>

### 시작 위치로 검색
- [...] 밖의 ^은 문자열의 시작을 의미

```
const regExp = 'https://poiemaweb.com;

// 'https'로 시작하는지 검사한다.
const regExp = /^https/;
target.match(regExp) // true
```

<br/>

### 마지막 위치로 검색
- $은 문자열의 마지막을 의미

```
// 'com'으로 끝나는지 검사
const regExp = /com$/;

regExp.test(target) // true
```

<br/>

## 자주 사용하는 정규 표현식
### 특정 단어로 시작하는지 검사

```
const regExp = 'https://poiemaweb.com;

// 'http://' 또는 'https://'로 시작하는지 검사
/^https?:\/\//.test(url) // true

다른 방법
/^(http|https)\/\//.test(url)
```
<br/>

### 특정 단어로 끝나는지 검사
- $는 문자열의 마지막 의미

```
const fileName = 'index.html';

// html로 끝나는지 검사
/html$/.test(fileName); // true
```

<br/>

### 숫자로만 이루어진 문자열인지 검사

```
// 처음과 끝이 숫자이고, 최소 한 번 이상 반복되는 문자열인지 검사
const target = '12345';

/^\d+$/.test(target); // true
```

<br/>

### 하나 이상의 공백으로 시작하는지 검사

```
const target = 'Hi';

// 하나 이상의 공백으로 시작하는지 검사
/^[\s]+/.test(target) // true
```

<br/>

### 아이디로 사용 가능한지 검사

```
const id = 'abc123';

// 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4~10자리인지 검사
/^[A-Za-z0-9]{4,10}$/.test(id) // true
```

<br/>

### 메일 주소 형식에 맞는지 검사

```
const email = 'hamin0320@naver.com';
/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(email) // true
```

<br/>

### 핸드폰 번호 형식에 맞는지 검사

```
const cellphone = 000-000-0000;

/^\d{3}-\d{3,4}-\d{4}$/.test(cellphone); // true
```

<br/>

### 특수 문자 포함 여부 검사

```
const target = 'abc#123';
(/[^A-Za-z0-9]/gi).test(target); // true
```

<br/>

```
// 특수 문자 선택적 검사
(/[\{\}\[\]\/?.,;:|\)*~`!^\-_+<>@\#$%&\\\=\(\'\"]/gi).test(target); // true
```

<br/>

- 특수 문자를 제거할 때는 String.prototype.replace 메서드 사용

```
// 특수 문자 제거
target.replace(/[^A-Za-z0-9]/gi, ''); // abc123
```
