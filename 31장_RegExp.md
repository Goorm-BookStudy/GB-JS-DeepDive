# 31장 RegExp

### 1-1. 정규 표현식이란?

: 정규 표현식을 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어다.

- 정규 표현식은 패턴 매칭 기능을 제공한다.
- 패턴 매칭 기능이란? 특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 수 있는 기능을 말한다.

```jsx
// 사용자로부터 입력받은 휴대폰 전화번호
const tel = "010-1234-567팔";

// 정규 표현식 리터럴로 휴대폰 전화번호 패턴을 정의한다.
const regExp = /^\d{3}-\d{4}-\d{4}$/;

// tel이 휴대폰 전화번호 패턴에 매칭하는지 테스트한다.
regExp.test(tel); // false
```

⇒ 정규 표현식은 주석이나 공백을 허용하지 않고 여러가지 기호를 혼합하여 사용하기 때문에 가독성이 좋지않다.

### 2. 정규 표현식의 생성

: 정규 표현식을 생성하기 위해서는 정규 표현식 리터럴과 RegExp 생성자 함수를 사용할 수 있다. 일반적인 방법은 정규 표현식 리터럴을 사용하는 것이다. 정규 표현식 리터럴은 다음과 같이 표현한다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/a5392071-e7f7-42c7-bf8a-95e33eeb1c8a/57f59fc0-6101-4f1c-a04b-d74b4452f9e6/Untitled.png)

```jsx
const target = "Is this all there is?";

// 패턴: is
// 플래그: i => 대소문자 구별하지 않고 검색한다.
const regexp = /is/i;

// test 메서드는 target 문자열에 대해 정규 표현식 regexp의 패턴을 검색하여 매칭 결과를
// 불리언 값으로 반환한다.
regexp.test(target); // true
```

Regexp 생성자 함수를 사용하여 Regexp 객체를 생성할 수도 있다.

```jsx
/*
 * pattern: 정규 표현식 패턴
 * flags: 정규 표현식의 플래그(g, i, m, u, y)
*/

new RegExp(pattern[, flags])
```

```jsx
const target = "Is this all there is?";

const regexp = new RegExp(/is/i); // ES6
// const regexp = new RegExp(/is/, 'i');
// const regexp = new RegExp('is', 'i');

regexp.test(target); // true
```

RegExp 생성자 함수를 사용하면 변수를 사용해 동적으로 RegExp 객체를 생성할 수 있다.

```jsx
const count = (str, cahr) => (str.match(new RegExpc(char, "gi")) ?? []).legth;

count("Is this all there is?", "is"); // 3
count("Is this all there is?", "xx"); // 0
```

### 3. RegExp 메서드

: 정규 표현식을 사용하는 메서드들을 알아보자

### 3-1. RegExp.prototype.exec

: exec 메서드는 인수로 전달받은 문자열에 대한 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환한다. 매칭 결과가 없는 경우 null을 반환한다.

```jsx
const target = "Is this all there is?";
const regExp = /is/;

regExp.exec(target);
// ["is", index: 5, input: "Is this all there is?", group: undefined]
```

⇒ exec 메서드는 문자열 내의 모든 패턴을 검색하는 g 플래그를 지정해도 첫 번째 매칭 결과만 반환하므로 주의하자

### 3-2. RegExp.prototype.test

: test 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.

```jsx
const target = "Is this all there is?";
const regExp = /is/;

regExp.test(target);
// true
```

### 3-3. String.prototype.match

: String 표준 빌트인 객체가 제공하는 match 메서드는 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환한다.

```jsx
const target = "Is this all there is?";
const regExp = /is/;

target.match(regExp);
// ["is", index: 5, input: "Is this all there is?", group: undefined]
```

⇒ exec 메서드는 문자열 내의 모든 패턴을 검색하는 g 플래그를 지정해도 첫 번째 매칭 결과만 반환한다. 하지만 String.prototype.match 메서드는 g 플래그가 지정되면 모든 매칭 결과를 배열로 반환한다.

```jsx
const target = "Is this all there is?";
const regExp = /is/g;

target.match(regExp);
// ["is", "is"]
```

### 4. 플래그

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/a5392071-e7f7-42c7-bf8a-95e33eeb1c8a/57f59fc0-6101-4f1c-a04b-d74b4452f9e6/Untitled.png)

: 패턴과 함께 정규 표현식을 구성하는 플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용한다. 플래그는 총 6개가 있다. 그중 중요한 3개를 살펴보자

| 플래그 | 의미        | 설명                                                            |
| ------ | ----------- | --------------------------------------------------------------- |
| i      | Ignore case | 대소문자를 구별하지 않고 패턴을 검색한다.                       |
| g      | Global      | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다. |
| m      | Multi line  | 문자열의 행이 바뀌더라도 패턴 검색을 계속한다.                  |

- 플래그는 옵션이므로 선택적으로 사용할 수 있으면, 순서와 상관없이 하나 이상의 플래그를 동시에 설정할 수도 있다.
- 어떠한 플래그도 사용하지 않은 경우 대소문자를 구별해서 패턴을 검색한다. 그리고 문자열에 패턴 검색 매칭 대상이 1개 이상 존재해도 첫 번째 대상만 검색하고 종료한다.

```jsx
const target = "Is this all there is?";

target.match(/is/);
// ["is", index: 5, input: "Is this all there is?", group: undefined]

target.match(/is/i);
// ["Is", index: 0, input: "Is this all there is?", group: undefined]

target.match(/is/g);
// ["is", "is"]

target.match(/is/gi);
// ["Is", "is", "is"]
```

### 5. 패턴

: 정규 표현식은 *“일정한 규칙(패턴)을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어”*다.

- 정규 표현식의 패턴은 문자열의 일정한 규칙을 표현하기 위해 사용하며, 플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용한다.

### 패턴의 표현하는 방법들

### 5-1. 문자열 검색

```jsx
const target = "Is this all there is?";

const regExp = /is/;

regExp.test(target); // true

target.match(regExp);
// ["is", index: 5, input: "Is this all there is?", group: undefined]

// 대소문자 구별하지 않고 검색하려면 플래그 i를 사용한다.
regExp = /is/i;

target.match(regExp);
// ["is", index: 5, input: "Is this all there is?", group: undefined]

// 대소문자 구별 x + 전역으로 검색 하면 i와 g를 함께 사용한다.
regExp = /is/gi;

target.match(regExp);
// ["Is", "is", "is"]
```

### 5-2. 임의의 문자열 검색

: **.**은 임의의 문자 한 개를 의미한다. 문자의 내용은 무엇이든 상관없다. 다음 예제의 경우 **.**을 3개 연속하여 패턴을 생성했으므로 문자의 내용과 상관없이 3자리 문자열과 매치한다.

```jsx
const target = "Is this all there is?";

const regExp = /.../g;

target.match(regExp);
// ["Is ", "thi", "s a", "ll ", "the", "re ", "is?"]
```

### 5-3. 반복 검색

: {m, n}은 앞선 패턴(다음 예제의 경우 A)이 최소 m번, 최대 n번 반복되는 문자열을 의미한다. 콤마 뒤에 공백이 있으면 정상 동자하지 않으므로 주의하기 바란다.

```jsx
const target = "A AA B BB Aa Bb AAA";

const regExp = /A{1, 2}/g;

target.match(regExp);
// ["A", "AA", "A", "AA", "A"]
```

⇒ {n}은 앞선 패턴이 n번 반복되는 문자열을 의미한다. 즉, {n} 은 {n, n}과 같다.

```jsx
const target = "A AA B BB Aa Bb AAA";

const regExp = /A{2}/g;

target.match(regExp);
// ["AA", "AA"]
```

⇒ {n,}은 앞선 패턴이 최소 n번 이상 반복되는 문자열을 의미한다.

```jsx
const target = "A AA B BB Aa Bb AAA";

const regExp = /A{2,}/g;

target.match(regExp);
// ["AA", "AAA"]
```

⇒ **+**는 앞선 패턴이 최소 한번 이상 반복되는 문자열을 의미한다. 즉, **+**는 {1,}과 같다. 다음 예제의 경우 A+는 앞선 패턴 “A”가 한번 이상 반복되는 문자열, 즉 “A”만으로 이루어진 문자열 “A”, “AA”, “AAA”, … 와 매치한다.

```jsx
const target = "A AA B BB Aa Bb AAA";

const regExp = /A+/g;

target.match(regExp);
// ["A", "AA", "A", "AAA"]
```

⇒ **?**는 앞선 패턴이 최대 한번(0번 포함)이상 반복되는 문자열을 의미한다. 즉, **?**는 {0, 1}과 같다. 다음 예제의 경우 /colou?r/는 ‘colo’ 다음 ‘u’가 최대 한 번(0번 포함) 이상 반복되고 ‘r’이 이어지는 문자열 ‘color’, ‘colour’와 매치한다.

```jsx
const target = "color colour";

const regExp = /colou**?**r/g;

target.match(regExp);
// ["color", "colour"]
```

### 5-4. OR 검색

: |는 or의 의미를 갖는다. 다음 예제의 /A|B/는 “A”또는 “B”를 의미한다.

```jsx
const target = "A AA B BB Aa Bb";

const regExp = /A|B/g;

target.match(regExp);
// ["A", "A", "A", "B", "B", "B", "A", "B"]
```

⇒ 분해되지 않은 단어 레벨로 검색하기 위해서는 +를 함께 사용한다.

```jsx
const target = "A AA B BB Aa Bb";

const regExp = /A+|B+/g;

target.match(regExp);
// ["A", "AA", "B", "BB", "A", "B"]
```

⇒ 위 예제는 패턴을 or로 한번 이상 반복하는 것인데 이를 간단히 표현하면 다음과 같다. []내의 문자는 or로 동작한다. 그 뒤에 +를 사용하면 앞선 패턴을 한 번 이상 반복한다.

```jsx
const target = "A AA B BB Aa Bb";

const regExp = /[AB]+/g;

target.match(regExp);
// ["A", "AA", "B", "BB", "A", "B"]
```

⇒ 범위를 지정하려면 []내에 -를 사용한다.

```jsx
const target = "A AA BB ZZ Aa Bb";

const regExp = /[A-Z]+/g;

target.match(regExp);
// ["A", "AA", "BB", "ZZ", "A", "B"]
```

⇒ 대소문자를 구별하지 않고 알파벳을 검색하는 방법은 다음과 같다.

```jsx
const target = "AA BB Aa Bb 12";

const regExp = /[A-Za-z]+/g;

target.match(regExp);
// ["AA", "BB", "Aa", "Bb"]
```

⇒ 숫자를 검색하는 방법은 다음과 같다.

```jsx
const target = "AA BB 12,345";

// 0~9가 한 번 이상 반복되는 문자열을 전역 검색 // ,로 매칭 결과가 분리됨
const regExp = /[0-9]+/g;

target.match(regExp);
// ["12", "345"]
```

```jsx
const target = "AA BB 12,345";

// 0~9또는 ,가 한 번 이상 반복되는 문자열을 전역 검색
const regExp = /[0-9,]+/g;

target.match(regExp);
// ["12,345"]
```

⇒ 위 예제를 간단히 표현하면 다음과 같다.

```jsx
const target = "AA BB 12,345";

// **0~9**또는 ,가 한 번 이상 반복되는 문자열을 전역 검색
const regExp = /[**\d**,]+/g;

target.match(regExp);
// ["12,345"]

// **숫자가 아닌 문자**
const regExp = /[**\D**,]+/g;

target.match(regExp);
// ["AA BB ", ","]
```

- \w는 알파벳, 숫자, 언더스코어를 의미한다. 즉 \w는 [A-Za-z0-9_]와 같다. \W는 \w와 반대로 동작한다. 즉 \W는 알파벳, 숫자, 언더스코어가 아닌 문자를 의미한다.

```jsx
const target = "Aa Bb 12,345 _$%&";

// **알파벳, 숫자, 언더스코어** 또는 ,가 한 번 이상 반복되는 문자열을 전역 검색
const regExp = /[**\w**,]+/g;

target.match(regExp);
// ["Aa", "Bb", "12,345", "_"]

// **알파벳, 숫자, 언더스코어 아닌 문자** 또는 ,가 한 번 이상 반복되는 문자열을 전역 검색
const regExp = /[**\W**,]+/g;

target.match(regExp);
// [" ", " ", ",", " $%&"]
```

### 5-5. NOT 검색

: […]내의 ^는 not의 의미를 갖는다. 예를 들어 [^0-9]는 숫자를 제외한 문자를 의미한다. 따라서 [0-9]와 같은 의미의 \d와 잔대로 동작하는 \D는 [^0-9]와 같고, [A-Za-z0-9_]와 같은 의미의 \w와 반대로 동작하는 \W는 [^A-Za-z0-9_]와 같다.

```jsx
const target = "AA BB 12 Aa Bb";

const regExp = /[^0-9]+/g;

target.match(regExp);
// ["AA BB ", " Aa Bb"]
```

### 5-6. 시작 위치로 검색

: […] 밖의 ^은 문자열의 시작을 의미한다. 단, […]내의 ^은 not의 의미를 가지므로 주의하기 바란다.

```jsx
const target = "https://poiemaweb.com";

const regExp = /^http/;

target.test(regExp); // true
```

### 5-7. 마지막 위치로 검색

: $는 문자열의 마지막을 의미한다.

```jsx
const target = "http:poiemaweb.com";

const regExp = /com$/;

target.test(regExp); // true
```

### 6. 자주 사용하는 정규 표현식

### 6-1. 특정 단어로 시작하는 검사

```jsx
const url = "https://example.com";

// 'http://' 또는 'https://'로 시작하는지 검사한다.
/^https?:\/\//.test(url); // true
```

⇒ […] 바깥의 ^문자열은 시작을 의미하고, ?는 앞선 패턴이 최대한 한번(0번 포함)이상 반복되는지를 의미한다. 다시 말해, 검색 대상 문자열에 앞선 패턴이 있어도 없어도 매치된다.

```jsx
/^(http|https):\/\//.test(url);
```

⇒ 위 방법과 동일하게 동작한다.

### 6-2. 특정 단어로 끝나는지 검사

```jsx
const fileName = "index.html";

//'html'로 끝나는지 검사한다.
/html$/.test(fileName); // true
```

### 6-3. 숫자로만 이루어진 문자열인지 검사

```jsx
const target = '12345'

/^\d+$/.test(target); // true
```

⇒ \d는 숫자를 희미하고 +는 앞선 패턴이 최소 한 번 이상 반복되는 문자열을 의미한다. 즉, 처음과 끝이 숫자이고 최소 한 번 이상 반복되는 문자열과 매치한다.

### 6-4. 하나 이상의 공백으로 시작하는지 검사

```jsx
const target = " Hi!";

/^[\s]+/.test(target); // true
```

⇒ \s는 여러 가지 공백 문자(스페이스, 탭 등)를 의미한다. 즉, \s는 [\t\r\n\v\f]와 같은 의미다.

<aside>
💡 `**[\t\r\n\v\f]**`

1. **`\t`** - 탭 (Tab): 문자나 문자열 사이에 탭 공간을 생성합니다.
2. **`\r`** - 캐리지 리턴 (Carriage Return, CR): 대부분의 오래된 프린터와 같은 하드웨어에서 사용되며, 커서를 같은 줄의 시작 위치로 옮깁니다. 윈도우 환경에서는 줄바꿈을 나타내기 위해 **`\r\n`**을 사용합니다.
3. **`\n`** - 줄 바꿈 문자 (Newline, LF): 텍스트에서 새로운 줄을 시작하도록 커서를 다음 줄로 이동시킵니다. Unix 기반 시스템에서는 줄바꿈을 나타내기 위해 **`\n`**만 사용합니다.
4. **`\v`** - 수직 탭 (Vertical Tab): 몇몇 환경에서 사용되며, 일반적으로 다음 줄로의 일정한 수직 이동을 나타냅니다. 오늘날 많이 사용되지 않습니다.
5. **`\f`** - 폼 피드 (Form Feed): 과거의 프린터에서 새 페이지로 이동하라는 명령으로 사용되었습니다. 프로그래밍에서는 이제 거의 사용되지 않습니다.
</aside>

### 6-5. 아이디로 사용 가능한지 검사

: 다음 예제는 검색 대상 문자열이 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4~10자리인지 검사한다.

```jsx
const id = "abc123";

/^[A-Za-z0-9]{4,10}$/.test(id); // true
```

⇒ {4, 10}은 앞선 패턴이 최소 4번, 최대 10번 반복되는 문자열을 의미한다. 즉, 4~10자리로 이루어진 알파벳 대소문자 또는 숫자를 의미한다.

### 6-6. 메일 주소 형식에 맞는지 검사

: 다음 예제는 검색 대상 문자열이 메일 주소 형식에 맞는지 검사한다.

```jsx
const email = "ungmo2@gmail.com";

/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(
  email
); // true
```

⇒ 참고로 인터넷 메시지 형식 규약이 RFC 5322에 맞는 정교한 패턴 패칭이 필요하다면 다음과 같이 무척이나 복잡한 패턴을 사용할 필요가 있다.

```jsx
(?:[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*" +
  '|"(?:[\\x01-\\x08\\x0b\\x0c\\x0e-\\x1f\\x21\\x23-\\x5b\\x5d-\\x7f]' +
  '|\\\\[\\x01-\\x09\\x0b\\x0c\\x0e-\\x7f])*")@(?:(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?' +
  '|\\[(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)' +
  "|[a-z0-9-]*[a-z0-9]:(?:[\\x01-\\x08\\x0b\\x0c\\x0e-\\x1f\\x21-\\x5a\\x53-\\x7f]|\\\\[\\x01-\\x09\\x0b\\x0c\\x0e-\\x7f])+)\\])
```

### 6-7. 핸드폰 번호 형식에 맞는지 검사

```jsx
const cellphone = "010-1234-1234";

/^\d{3}-\d{3,4}-\d{4}$/.test(cellphone); // true
```

### 6-8. 특수 문자 포함 여부 검사

```jsx
const target = "abc#123";

/[^A-Za-z0-9]/gi.test(target); // true
```

```jsx
target.replace(/[^A-Za-z0-9]/gi, ""); // abc123
```

⇒ 특수 문자를 제거할때 String.prototype.replace 메서드를 사용한다.
