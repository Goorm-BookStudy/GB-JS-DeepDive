# ✏️ 32장 String

## 📌 32.1 String 생성자 함수

- 표준 빌트인 객체인 String 객체는 생성자 함수 객체다.
- new 연산자와 함께 호출하여 String 인스턴스를 생성할 수 있다.
- String 생성자 함수에 인수를 전달하지 않고 new 연산자와 함께 호출하면 [[StringData]] 내부 슬롯에 빈 문자열을 할당한 String 래퍼 객체를 생성한다.

```
const strObj = new String();
console.log(strObj);
// String
// {length: 0 [[Prototype]]: String [[PrimitiveValue]]: ""}
```

- [[PrimitiveValue]]는 접근할 수 없는 프로퍼티인데, 이는 [[StringData]] 내부 슬롯을 가리킨다.
- String 생성자 함수의 인수로 문자열을 전달하면서 new 연산자와 함께 호출하면 내부 슬롯에 인수로 전달받은 문자열을 할당한 String 래퍼 객체를 생성한다.

```
const strObj = new String('Hwang');
console.log(strObj);
// String {'Hwang'} 0: "H" 1: "w" 2: "a" 3: "n" 4: "g"
// length: 5 [[Prototype]]: String [[PrimitiveValue]]: "Hwang"
```

- String 래퍼 객체는 배열처럼 length 프로퍼티와 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로, 각 문자를 프로퍼티 값으로 갖는 유사 배열 객체이면서 이터러블이다.

```
console.log(strObj[0]); // H
```

- 단, 문자열은 원시 값이므로 변경할 수 없다.
- String 생성자 함수의 인수로 문자열이 아닌 값을 전달하면 인수를 문자열로 강제 변환 후 String 래퍼 객체를 생성한다.

```
const strObj = new String('123');
console.log(strObj);
// String {'123'} 0: "1" 1: "2" 2: "3"
// length: 3 [[Prototype]]: String [[PrimitiveValue]]: "123"
```

- new 연산자를 사용하지 않고 String 생성자 함수를 호출하면 문자열을 반환한다. 이를 이용해 명시적으로 타입을 변환할 수 있다.

## 📌 32.2 length 프로퍼티

- 문자열의 문자 개수를 반환한다.
- String 래퍼 객체는 length 프로퍼티와 인덱스를 나타내는 숫자를 프로퍼티 키로, 각 문자를 프로퍼티 값으로 가지므로 유사 배열 객체다.

## 📌 32.3 String 메서드

- String 객체에는 원본 String 래퍼 객체를 직접 변경하는 메서드는 존재하지 않는다.
- 즉 언제나 새로운 문자열을 반환한다.
- 문자열은 변경 불가능한 원시 값이기 때문에 String 래퍼 객체도 읽기 전용 객체로 제공된다.
- 읽기 전용 객체가 아니라면 변경된 String 래퍼 객체를 문자열로 되돌릴 때 문자열이 변경된다.
- 따라서 String 객체의 모든 메서드는 String 래퍼 객체를 직접 수정할 수 없고, String 객체의 메서드는 언제나 새로운 문자열을 생성해 반환한다.

#### 1. String.prototype.indexOf

- 대상 문자열에서 인수로 전달받은 문자열을 검색해 첫 번째 인덱스를 반환한다.
- 검색에 실패하면 -1을 반환한다.
- 2번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.
- 대상 문자열에 특정 문자열이 존재하는지 확인할 때 유용하나 includes 메서드를 사용하면 가독성이 더 좋다.

#### 2. String.prototype.search

- 대상 문자열에서 인수로 전달받은 정규 표현식과 매치하는 문자열을 검색해 일치하는 문자열의 인덱스를 반환한다.
- 검색에 실패하면 -1을 반환한다.

```
const str = 'Hello world';
str.search(/o/); // 4
str.search(/x/); // -1
```

#### 3. String.prototype.includes

- ES6에서 도입된 메서드로 대상 문자열에 인수로 전달받은 문자열이 포함되어 있는지 확인해 그 결과를 불리언 값으로 반환한다.
- 2번째 인수로 검색을 시작할 메서드를 전달할 수 있다.

```
const str = 'Hello world';

str.includes(''); // true
str.includes('l', 3); // true
str.includes('H', 3); // false
```

#### 4. String.prototype.startsWith

- ES6에서 도입된 메서드로 대상 문자열이 인수로 전달받은 문자열로 시작하는지 확인해 그 결과를 불리언 값으로 반환한다.
- 2번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.

```
const str = 'Hello world';
str.startsWith('He'); // true
str.startsWith('x'); // false
str.startsWith(' ', 5); // true
```

#### 5. String.prototype.endsWith

- ES6에서 도입된 메서드로 대상 문자열이 인수로 전달받은 문자열로 끝나는지 확인해 그 결과를 불리언 값으로 반환한다.
- 2번째 인수로 검색할 문자열의 길이를 전달할 수 있다.

```
const str = 'Hello world';
str.endsWith('ld'); // true
str.endsWith('x'); // false
str.endsWith('lo', 5); // true
```

#### 6. String.prototype.charAt

- 대상 문자열에서 인수로 전달받은 인덱스에 위치한 문자를 검색해 반환한다.
- 인덱스는 문자열의 범위 사이의 정수여야 하며 벗어날 경우 빈 문자열을 반환한다.

```
const str = 'Hello';
for (let i = 0; i < str.length; i++) {
    console.log(str.charAt(i)); // H e l l o
}
```

#### 7. String.prototype.substring

- 대상 문자열에서 첫 번째 인수로 전달받은 인덱스에 위치하는 문자부터 두 번째 인수로 전달받은 인덱스에 위치하는 문자의 바로 이전 문자까지의 부분 문자열을 반환한다.
- 두 번째 인수는 생략할 수 있으며 이 경우 첫 번째 인수부터 마지막 문자까지 부분 문자열을 반환한다.
- 첫 번째 인수는 두 번째 인수보다 작은 정수여야 하지만 아래의 경우에도 정상 동작한다.
  - 첫 번째 인수 > 두 번째 인수인 경우 두 인수는 교환된다.
  - 인수 < 0 또는 NaN인 경우 0으로 취급된다.
  - 인수 > 문자열의 길이인 경우 인수는 문자열의 길이로 취급한다.
- indexOf 메서드와 함께 사용하면 특정 문자열을 기준으로 앞뒤에 위치한 부분 문자열을 취득할 수 있다.

```
const str = 'Hello world';
str.substring(1, 4); // ell
str.substring(1); // ello world

// indexOf와 같이 쓰기
const str = 'Hello world';
str.substring(0, str.indexOf(' ')); // 'Hello'
```

#### 8. String.prototype.slice

- substring과 동일하게 동작하나 음수인 인수를 전달할 수 있다.
- 음수인 인수를 전달하면 대상 문자열의 가장 뒤에서부터 시작해 문자열을 잘라내 반환한다.

```
const str = 'Hello world';
str.substring(-5); // 'Hello world'
str.slice(-5); // 'world'
```

#### 9. String.prototype.toUpperCase

- 대상 문자열을 모두 대문자로 변경한 문자열을 반환한다.

#### 10. String.prototype.toLowerCase

- 대상 문자열을 모두 소문자로 변경한 문자열을 반환한다.

#### 11. String.prototype.trim

- 대상 문자열 앞뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환한다.
- trimStart, trimEnd를 사용하면 대상 문자열 앞 또는 뒤의 공백 문자를 제거한 문자열을 반환한다.
- replace 메서드에 정규 표현식을 인수로 젇날해 공백 문자를 제거할 수도 있다.

#### 12. String.prototype.repeat

- ES6에서 도입된 메서드로 대상 문자열을 인수로 전달받은 정수만큼 반복해 연결한 새로운 문자열을 반환한다.
- 인수로 전달받은 정수가 0이면 빈 문자열을 반환하고, 음수면 범위 오류를 발생시킨다.
- 인수를 생략하면 기본값 0이 설정된다.
- 2.5 같은 수를 주면 내림하여 2로 설정한다.

#### 13. String.prototype.replace

- 대상 문자열에서 첫 번째 인수로 전달받은 문자열 또는 정규 표현식을 검색해 두 번째 인수로 전달한 문자열로 치환한 문자열을 반환한다.

```
const str = 'Hello world';
str.replace('world', 'Hwang'); // 'Hello Hwang'
```

- 검색한 문자열이 여럿 존재할 경우 첫 번째로 검색한 문자열만 치환된다.

```
const str = 'Hello world world';
str.replace('world', 'Hwang'); // 'Hello Hwang world'
```

- $&(검색된 문자열)과 같은 특수한 교체 패턴을 사용할 수 있다.
- 첫 번째 인수로 정규 표현식을 전달할 수도 있다.
- 두 번째 인수로 치환 함수를 전달할 수 있다.
  - 첫 번째 인수로 전달한 문자열 또는 정규 표현식에 매치한 결과를 두 번째 인수로 전달한 치환 함수의 인수로 전달하면서 호출하고 치환 함수가 반환한 결과와 매치 결과를 치환한다.
  - p.603 참고

#### 14. String.prototype.split

- 대상 문자열에서 첫 번째 인수로 전달한 문자열 또는 정규 표현식을 검색해 문자열을 구분한 후 분리된 각 문자열로 이루어진 배열을 반환한다.
- 인수로 빈 문자열을 전달하면 각 문자를 모두 분리하고, 인수를 생략하면 대상 문자열 전체를 단일 요소로 하는 배열을 반환한다.
- 두 번째 인수로 배열의 길이를 지정할 수 있다.

```
const str = 'How are you doing?';

str.split(' '); // ['How', 'are', 'you', 'doing?']
str.split(/\s/); // ['How', 'are', 'you', 'doing?']
str.split(''); // ['H', 'o', 'w', ' ', 'a', 'r', 'e', ' ', 'y', 'o', 'u', ' ', 'd', 'o', 'i', 'n', 'g', '?']
str.split(); // ['How are you doing?']

str.split(' ', 3); // ['How', 'are', 'you']

```

- 해당 메서드는 배열을 반환하므로 reverse, join 메서드와 함께 사용하면 문자열을 역순으로 뒤집을 수 있다.

```
function reverseString(str) {
    return str.split('').reverse().join('');
}

reverseString('Hello world!'); // '!dlrow olleH'
```
