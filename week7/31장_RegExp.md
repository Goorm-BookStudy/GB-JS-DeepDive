# ✏️ 31장 RegExp

## 📌 31.1 정규 표현식이란?

- 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어
- JS만의 고유 문법은 아니며, 대부분의 프로그래밍 언어와 코드 에디터에 내장 되어 있음
- ES3부터 펄의 정규 표현식 문법을 도입함
- 정규 표현식을 사용하면 반복문과 조건문 없이 패턴을 정의, 테스트 해 간단히 체크할 수 있다.
- 다만 주석이나 공백을 허용하지 않고 여러 기호가 혼합되기 때문에 가독성이 좋지 않다.

## 📌 31.2 정규 표현식의 생성

- 정규 표현식 객체를 생성하기 위해서는 정규 표현식 리터럴과 RegExp 생성자 함수를 사용한다.
- 일반적으로 전자를 사용하며 패턴과 플래그로 구성된다.
- 후자를 사용하면 변수를 사용해 동적으로 RegExp 객체를 생성할 수 있다.

```
// 정규 표현식 리터럴
const target = 'Is this all there is?';
const regexp = /is/i;
regexp.test(target); // true

// RegExp 생성자 함수
const target = 'Is this all there is?';
const regexp = new RegExp(/is/i);
regexp.test(target); // true
```

## 📌 31.3 RegExp 메서드

#### 1. RegExp.prototype.exec

- 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색해 매칭 결과를 배열로 반환한다.
- 매칭 결과가 없는 경우 null을 반환한다.
- 문자열 내의 모든 패턴을 검색하는 g 플래그를 지정해도 첫 번째 매칭 결과만 반환한다.

```
const target = 'Is this all there is?';
const regExp = /is/;

regExp.exec(target);
// ['is', index: 5, input: 'Is this all there is?', groups: undefined]
```

#### 2. RegExp.prototype.test

- 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색해 매칭 결과를 불리언 값으로 반환한다.

```
const target = 'Is this all there is?';
const regExp = /is/;

regExp.test(target); // true
```

#### 3. String.prototype.match

- String 표준 빌트인 객체가 제공하는 match 메서드는 대상 문자열과 인수로 전달받은 정규 표현식과 매칭 결과를 배열로 반환한다.
- exec 메서드와 달리 g 플래그가 지정되면 모든 매칭 결과를 배열로 반화한다.

```
const target = 'Is this all there is?';
const regExp = /is/;

target.match(regExp); // ['is', index: 5, input: 'Is this all there is?', groups: undefined]

// g 플래그 설정
const target = 'Is this all there is?';
const regExp = /is/g;

target.match(regExp);  // ['is', 'is']
```

## 📌 31.4 플래그

- 정규 표현식의 검색 방식을 설정하기 위해 사용한다.
- 선택적으로 사용할 수 있고, 순서와 상관 없이 하나 이상의 플래그를 동시에 설정할 수 있다.
- 어떤 플래그도 사용하지 않는다면 대소문자를 구별해서 패턴을 검색하고, 첫 번째 매칭한 대상만 검색하고 종료한다.

| 플래그 | 의미        | 설명                                                              |
| ------ | ----------- | ----------------------------------------------------------------- |
| i      | Ignore case | 대소문자 구별 없이 패턴을 검색한다.                               |
| g      | Global      | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다.   |
| m      | Multi line  | 문자열의 행이 바뀌더라도 패턴 검색을 계속한다.                    |
| s      | dotAll      | '.' 메타 문자가 줄바꿈 문자를 포함해 모든 문자와 일치하도록 한다. |
| u      | unicode     | 패턴과 문자열이 유니코드 코드 포인트로 처리되도록 한다.           |
| y      | sticky      | 대상 문자열의 현재 위치에서만 검색을 수행한다.                    |

## 📌 31.5 패턴

- 정규 표현식은 패턴과 플래그로 구성된다.
- 패턴은 문자열의 일정한 규칙을 표현하기 위해 사용한다.
- 플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용한다.
- 패턴은 /로 열과 닫으며 문자열의 따옴표는 생략한다. 따옴표를 포함하면 따옴표까지 패턴에 포함되어 검색된다.
- 패턴은 특별한 의미를 가지는 메타문자 또는 기호로 표현할 수 있다.

#### 1. 문자열 검색

- 대소문자를 구별하지 않고 검색하려면 플래그 i를 사용한다.
- 검색 대상 문자열 내에서 패턴과 매치하는 모든 문자열을 전역 검색하려면 플래그 g를 사용한다.

#### 2. 임의의 문자열 검색

- .은 문자의 내용과 무관하게 임의의 문자 1개를 의미한다.

```
const target = 'Is this all there is?';
const regExp = /.../g;
target.match(regExp); // ['Is ', 'thi', 's a', 'll ', 'the', 're ', 'is?']
```

#### 3. 반복 검색

- {m, n}은 앞선 패턴이 최소 m번, 최대 n번 반복되는 문자열을 의미한다.
- 콤마 뒤에 공백이 있으면 정상 작동하지 않는다.

```
const target = 'A AA B BB Aa Bb AAA';
const regExp = /A{1,2}/g;
target.match(regExp); // ['A', 'AA', 'A', 'AA', 'A']
```

- {n}은 앞선 패턴이 n번 반복되는 문자열을 의미한다.

```
const target = 'A AA B BB Aa Bb AAA';
const regExp = /A{2}/g;
target.match(regExp); // ['AA', 'AA']
```

- {n,}은 앞선 패턴이 최소 n번 이상 반복되는 문자열을 의미한다.

```
const target = 'A AA B BB Aa Bb AAA';
const regExp = /A{2,}/g;
target.match(regExp); // ['AA', 'AAA']
```

- +는 앞선 패턴이 최소 1번 이상 반복되는 문자열을 의미한다. (= {1,})

```
const target = 'A AA B BB Aa Bb AAA';
const regExp = /A+/g;
target.match(regExp); // ['A', 'AA', 'A', 'AAA']
```

- ?는 앞선 패턴이 최대 한 번(0번 포함) 이상 반복되는 문자열을 의미한다. (= {0,1})

```
const target = 'color colour';
const regExp = /colou?r/g; (colo 다음 u가 0번 또는 1번 포함되는)
target.match(regExp); // ['color', 'colour']
```

#### 4. OR 검색

- |는 or의 의미를 갖는다.
- 분해되지 않는 단어 레벨로 검색하려면 +를 함께 사용한다.
- [] 내의 문자는 or로 동작한다.
- 범위를 지정하려면 []내에 -를 사용한다.
  - A-Za-z(대소문자 구별없는 알파벳) 나 0-9로 함께 검사할 수 있다.

```
// 그냥 쓰기
const target = 'A AA B BB Aa Bb';
const regExp = /A|B/g;
target.match(regExp); // ['A', 'A', 'A', 'B', 'B', 'B', 'A', 'B']

// + 사용하기
const target = 'A AA B BB Aa Bb';
const regExp = /A+|B+/g;
target.match(regExp); // ['A', 'AA', 'B', 'BB', 'A', 'B']

// +와 [] 사용하기
const target = 'A AA B BB Aa Bb';
const regExp = /[AB]+/g;
target.match(regExp); // ['A', 'AA', 'B', 'BB', 'A', 'B']
```

- \d는 숫자를 의미한다. \d는 [0-9]를 의미한다. (digit)
- \D는 숫자가 아닌 문자를 의미한다.
- \w는 알파벳, 숫자, 언더스코어를 의미한다. \w는 [A-Za-z0-9_]를 의미한다. (word)
- \W는 알파벳, 숫자, 언더스코어가 아닌 문자를 의미한다. ($%^ 같은 문자 또는 공백, 컴마 등)

#### 5. NOT 검색

- [] 내의 ^는 not을 의미한다.

```
const target = 'AA BB 12 Aa Bb';
const regExp = /[^0-9]+/g;
target.match(regExp); // ['AA BB ', ' Aa Bb']
```

#### 6. 시작 위치로 검색

- [] 밖의 ^는 문자열의 시작을 의미한다.

```
const target = 'aabbb';
const regExp = /^a/;
regExp.test(target); // true
```

#### 7. 마지막 위치로 검색

- $는 문자열의 마지막을 의미한다.

```
const target = 'aabbb';
const regExp = /b$/;
regExp.test(target); // true
```

## 📌 31.6 자주 사용하는 정규 표현식

#### 1. 특정 단어로 시작하는지 검사

- [] 바깥의 ^는 문자열의 시작을 의미한다.
- ?는 앞선 패턴이 최대 한 번 이상 반복되는지 의미한다.

```
const url = 'https://www.naver.com';
/^https?:\/\//.test(url); // true
/^(http|https):\/\//.test(url); // true
```

#### 2. 특정 단어로 끝나는지 검사

- $는 문자열의 마지막을 의미한다.

```
const fileName = 'index.html';
/html$/.test(fileName); // true
```

#### 3. 숫자로만 이루어진 문자열인지 검사

- [] 바깥의 ^는 문자열의 시작을, $는 문자열의 마지막을 의미한다.
- \d는 숫자를 의미하고 +는 앞선 패턴이 최소 한 번 이상 반복되는 문자열을 의미한다.

```
const target = '12345';
/^\d+$/.test(target); // true
```

#### 4. 하나 이상의 공백으로 시작하는지 검사

- \s는 여러 가지 공백 문자를 의미한다.
- 즉 [\t\r\n\v\f]와 같은 의미다.
  - 수평 탭, 캐리지 리턴(텍스트의 줄의 시작 위치로 커서를 옮김), 줄 바꿈, 수직 탭, 페이지 넘김

```
const target = ' Hi!';
/^[\s]+/.test(target); // true
```
