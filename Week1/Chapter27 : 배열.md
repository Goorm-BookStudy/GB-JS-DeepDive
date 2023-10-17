### 27. 배열

#### 27.1 배열이란? : 배열(array)는 여러 개의 값을 순차적으로 나열한 자료구조이다.

배열은 기본적으로 사용 빈도가 매우 높은 가장 기본적인 자료 구조이다. => 자바스크립트는 배열을 다루기 위한 유용한 메서드를 다수 제공한다.

```
const arr = ['apple', 'banana', 'orange'];
// 배열 리터럴
```

요소(element) 배열이 가지고 있는 값 

#### 자바스크립트의 모든 값은 배열의 요소가 될 수 있다. ex) 원시값, 객체, 함수, 배열 등등

#### 배열은 자신의 위치를 나타내는 0이상의 정수인 인덱스(index)를 갖는다.

#### => 인덱스는 배열의 요소에 접근할 때 사용한다. 대부분의 프로그래밍 언어에서 인덱스는 0부터 시작한다.

#### 요소에 접근할 경우에는 대괄호 표기법을 사용한다.

```
arr[0] // => apple 첫번째 예제 기준
arr[1] // => banana
arr[2] // => orange
```

#### 배열은 요소의 개수, 즉 배열의 길이를 나타내는 length 프로퍼티를 갖는다. => for문을 통해 순차적으로 요소에 접근할 수 있다.

```
for(let i = 0; i< arr.length; i++) {
  console.log(arr[i]); // 'apple' 'banana' 'orange'
}
```

#### 자바스크립트에 배열이라는 타입은 존재하지 않는다. 배열은 객체 타입이다.

```
typeof arr // => object
```

#### 배열의 생성 방법

- 배열 리터럴
- Array 생성자 함수
- Array.of 메서드
- Array.from 메서드

#### 배열의 생성자 함수 => Array / 배열의 프로토타입 객체 => Array.prototype : 배열을 위한 빌트인 메서드를 제공한다.

```
const arr = [1, 2, 3];

arr.constructor === Array // => true
Object.getPrototypeOf(arr) === Array.prototype // => true
```

#### 배열과 객체의 구분되는 특징

구분 | 객체 | 배열
|----|----|----|
구조 |프로퍼티 키와 프로퍼티 값 | 인덱스와 요소
값의 참조 | 프로퍼티 키 | 인덱스
length 프로퍼티 | x | O
값의 순서 | x | O

#### 일반 객체와 배열을 구분하는 가장 명확한 차이는 값의순서와 length 프로퍼티이다. 

#### 배열의 장점 : 처음부터 순차적으로 요소에 접근할 수도 있고, 마지막부터 역순으로 요소에 접근할 수도 있으며, 특정 위치부터 순차적으로 요소에 접근할 수도 있다. => 배열이 인덱스(값의 순서)와 length 프로퍼티를 갖기 때문이다.

#### 27.2 자바스크립트 배열은 배열이 아니다.

#### 자료구조에서 말하는 배열은 동일한 크기의 메모리 공간이 빈틈없이 연속적으로 나열된 자료구조를 말한다. 즉, 배열은 하나의 데이터 타입으로 통일되어 있으며 서로 연속적으로 인접해 있다. 이러한 배열을 밀집 배열 이라고 한다.

#### => 시간 복잡도 O(1) => 인덱스를 통해 단 한번의 연산으로 임의의 요소에 접근 할 수 있다. 이는 매우 효율적이며 고속으로 동작한다.

#### 이처럼 배열은 인덱스를 통해 효율적으로 요소에 접근할 수 있다는 장점이 있다. 하지만, 정렬되지 않은 배열에서 특정한 요소를 검색하는 경우 배열의 요소를 처음부터 특정요소를 발견할 때 까지 차례대로 검색(선형 검색) 해야해서 이때의 시간 복잡도는 O(n)이다.

```
function linearSearch(array, target) {
  const length = array.length;

  for (let i = 0; i < length; i++) {
    if (array[i] === target) return i;
  }

  return -1;
}

console.log(linearSearch([1, 2, 3, 4, 5, 6], 3)); // 2
console.log(linearSearch([1, 2, 3, 4, 5, 6], 0)); // -1

// 선형 검색을 통해 배열에 특정 요소가 존재하는지 확인한다.
// 배열에 특정 요소가 존재하면 요소의 인덱스를 반환하고, 존재하지 않으면 -1을 반환한다.
```

#### 또한 배열에 요소를 삽입하거나 삭제하는 경우, 배열의 요소를 연속적으로 유지하기 위해 요소를 이동시켜야하는 단점도 있다.

#### 그러나 자바스크립트의 배열은 지금까지 살펴본 자료구조에서 말하는 일반적인 의미의 배열과 다르다. 

#### 자바스크립트에서의 배열은 배열의 요소를 위한 각각의 메모리 공간은 동일한 크기를 갖지 않아도 되며, 연속적으로 이어져 있지 않을 수도 있다. 

#### 배열의 요소가 연속적으로 이어져 있지 않는 배열을 희소 배열 이라고 한다.

#### => 이처럼 자바스크립트의 배열은 엄밀히 말해 일반적 의미의 배열이 아니고 일반적인 배열의 동작을 흉내 낸 특수한 객체다.

```
// 16.2절 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체 참고

console.log(Object.getOwnPropertyDescriptors([1, 2, 3]));

/*
{
  '0' : {value: 1, writable: true, enumerable: true, configurable: true}
  '0' : {value: 2, writable: true, enumerable: true, configurable: true}
  '0' : {value: 3, writable: true, enumerable: true, configurable: true}
  length : {value: 3, writable: true, enumerable: false, configurable: false}
}
*/
```

#### 이처럼 자바스크립트의 배열은 인덱스를 나타내는 문자열을 프로퍼티 키로 가지며, length 프로퍼티를 가지는 특수한 객체다. => 자바스크립트의 배열의 요소는 사실 프로퍼티 값이다.

#### 자바스크립트에서 사용할 수 있는 모든 값은 객체의 프로퍼티 값이 될 수 있으므로 어떤 타입의 값이라도 배열의 요소가 될 수 있다.

#### 일반적인 배열과 자바스크립트 배열의 장/단점

- 일반적인 배열은 인덱스 요소로 빠르게 접근할 수 있다. / 하지만 요소를 삽입 또는 삭제하는 경우에는 효율적이지 않다.
- 자바스크립트 배열은 해시 테이블로 구현된 객체이므로 인덱스로 요소에 접근하는 경우 일반적인 배열보다 성능적인 면에서 느릴 수 밖에 없는 구조적인 단점이 있다. 하지만 요소를 삽입 또는 삭제하는 경우에는 일반적인 배열보다 빠른 성능을 기대할 수 있다.

#### 인덱스로 배열 요소에 접근할 때 일반적인 배열보다 느릴 수 밖에 없는 구조적 단점을 보완하기 위해 대부분의 모던 자바스크립트 엔진은 배열을 일반 객체와 구별하여 좀 더 배열처럼 동작하도록 최적화하여 구현 했다.

#### 27.3 length 프로퍼티와 희소 배열

- length 프로퍼티는 요소의 개수, 즉 배열의 길이를 나타내는 0이상의 정수를 값으로 가진다. length 프로퍼티의 값은 빈 배열일 경우 0이며, 빈 배열이 아닐 경우 가장 큰 인덱스에 1을 더한 것과 같다.

#### 현재 length 프로퍼티 값보다 작은 숫자 값을 할당하면 배열의 길이가 줄어든다.

```
const arr = [1, 2, 3, 4, 5];

// 현재 length 프로퍼티 값인 5보다 작은 숫자 값 3을 length 프로퍼티에 할당
arr.length = 3;

// 배열의 길이가 5에서 3으로 줄어든다.
console.log(arr); // [1, 2, 3]
```

#### length 프로퍼티 값보다 큰 숫자를 할당하는 경우 length 프로퍼티 값은 변경 되지만 실제로 배열의 길이가 늘어나지는 않는다.

```
const arr = [1];

// 현재 length 프로퍼티 값인 1보다 큰 숫자 값 3을 length 프로퍼티에 할당
arr.length = 3;

// length 프로퍼티 값은 변경되지만 실제로 배열의 길이가 늘어나지는 않는다.
console.log(arr.length); // 3
console.log(arr); // [1, empty × 2]
```

#### arr[1]과 arr[2]에는 값이 존재하지 않는다.

```
// 희소 배열
const sparse = [, 2, , 4];

// 희소 배열의 length 프로퍼티 값은 요소의 개수와 일치하지 않는다.
console.log(sparse.length); // 4
console.log(sparse); // [empty, 2, empty, 4]

// 배열 sparse에는 인덱스가 0, 2인 요소가 존재하지 않는다.
console.log(Object.getOwnPropertyDescriptors(sparse));
/*
{
  '1': { value: 2, writable: true, enumerable: true, configurable: true },
  '3': { value: 4, writable: true, enumerable: true, configurable: true },
  length: { value: 4, writable: true, enumerable: false, configurable: false }
}
*/
```

#### 희소 배열은 length와 배열 요소의 개수가 일치하지 않는다. 희소 배열의 length는 희소 배열의 실제 요소 개수보다 언제나 크다.


#### 27.4 배열 생성

객체와 마찬가지로 배열도 다양한 생성 방식이 있다.
가장 일반적이고 간편한 배열 생성 방식은 배열 리터럴을 사용하는 것이다.

배열 리터럴은 0개 이상의 요소를 쉼표로 구분하여 대괄호([])로 묶는다.
배열 리터럴은 객체 리터럴과 달리 프로퍼티 키가 없고 값만 존재한다.

```
const arr = [1, 2, 3];
console.log(arr.length); // 3
```

#### 배열 리터럴에 요소를 생략하면 희소 배열이 생성된다.

```
const arr = [1, , 3]; // 희소 배열
console.log(arr.length); // 3
console.log(arr);        // [1, empty, 3]
console.log(arr[1]);     // undefned
```

##### 1) Array.of
   
ES6에서 도입된 Array.of 메서드는 전달된 인수를 요소로 갖는 배열을 생성한다.
Array.of는 Array 생성자 함수와 다르게 전달된 인수가 1개이고 숫자이더라도 인수를 요소로 갖는 배열을 생성한다.

```
// 전달된 인수가 1개이고 숫자이더라도 인수를 요소로 갖는 배열을 생성한다.
Array.of(1); // -> [1]
Array.of(1, 2, 3); // -> [1,  2,  3]
Array.of('string'); // -> ['string']
```

##### 2) Array.from

ES6에서 도입된 Array.from 메서드는 유사 배열 객체 또는 이터러블 객체를 인수로 전달받아 배열로 변환하여 반환한다.

```
// 유사 배열 객체를 변환하여 배열을 생성한다.
Array.from({ length: 2, 0: 'a', 1: 'b' }); // -> ['a', 'b']

// 이터러블을 변환하여 배열을 생성한다. 문자열은 이터러블이다.
Array.from('Hello'); // -> ['H', 'e', 'l', 'l', 'o']
```

Array.form을 사용하면 두 번째 인수로 전달한 콜백 함수를 통해 값을 만들면서 요소를 채울 수 있다.
Array.from 메서드는 두 번째 인수로 전달한 콜백 함수에 첫 번째 인수에 의해 생성된 배열의 요소값과 인덱스를 순차적으로 전달하면서 호출하고, 콜백 함수의 반환값으로
구성된 배열을 반환한다.

```
// Array.from에 length만 존재하는 유사 배열 객체를 전달하면 undefined를 요소로 채운다.
Array.from({ length: 3 }); // -> [undefined, undefined, undefined]

// Array.from은 두 번째 인수로 전달한 콜백 함수의 반환값으로 구성된 배열을 반환한다.
Array.from({ length: 3 }, (_, i) => i); // -> [0, 1, 2]
```

#### 유사 배열 객체와 이터러블 객체

```
유사 배열 객체는 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 잇고 length 프로퍼티를 갖는 객체를 말한다.
유사 배열 객체는 마치 배열처럼 for문으로 순회할 수도 있다.
   ```js
   // 유사 배열 객체
   const arrayLike = {
      "0": "apple",
      "1": "banana",
      "2": "orange",
      length: 3
   };
   
   // 유사 배열 객체는 마치 배열처럼 for문으로 순회할 수도 있다.
   for(let i = 0; i< arrayLike.length; i++) {
      console.log(arrayLike[i]); // apple banana orange
   }
```

이터러블 객체는 Symbol.iterator 메서드를 구현하여 for..of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 있는 객체를 말한다. ES6에서 제공하는 빌트인 이터러블은 Array.String, Map, Set, DOM 컬렉션(NodeList, HTMLCollection), arguments등이 있다. 이에 대해서는 34장 '이터러블'에 보충되어있다.

```
<br>

---

## 5. 배열 요소의 참조

배열 요소를 참조할 때에는 대괄호([]) 표기법을 사용한다.
   
대괄호 안에는 인덱스가 와야 한다.
    
정수로 평가되는 표현식이라면 인덱스 대신 사용할 수 있다.
    
인덱스는 값을 참조할 수 있다는 의미에서 객체의 프로퍼티 키와 같은 역할을 한다.   
    
```js
const arr = [1, 2];

// 인덱스가 0인 요소를 참조
console.log(arr[0]); // 1
// 인덱스가 1인 요소를 참조
console.log(arr[1]); // 2
```

##### 존재하지 않는 요소에 접근하면 undefined가 반환된다.(즉 에러 발생 x)

```
const arr = [1, 2];

// 인덱스가 2인 요소를 참조. 배열 arr에는 인덱스가 2인 요소가 존재하지 않는다.
console.log(arr[2]); // undefined
```

##### 같은 이유로 희소 배열의 존재하지 않는 요소를 참조해도 undefined가 반환된다.


#### 24.6 배열 요소의 추가와 갱신

객체에 프로퍼티를 동적으로 추가할 수 있는 것처럼 배열에도 요소를 동적으로 추가할 수 있다.

존재하지 않는 인덱스를 사용해 값을 할당하면 새로운 요소가 추가된다.

이때 length 프로퍼티 값은 자동 갱신된다.

```
const arr = [0];

// 배열 요소의 추가
arr[1] = 1;

console.log(arr); // [0, 1]
console.log(arr.length); // 2
```

만약 현재 배열의 length 프로퍼티 값보다 큰 인덱스로 새로운 요소를 추가하면 희소 배열이 된다.

```
arr[100] = 100;

console.log(arr); // [0, 1, empty × 98, 100]
console.log(arr.length); // 101
```

이때 인덱스로 요소에 접근하여 명시적으로 값을 할당하지 않은 요소는 생성되지 않는 것에 주의 하자.

```
// 명시적으로 값을 할당하지 않은 요소는 생성되지 않는다.
console.log(Object.getOwnPropertyDescriptors(arr));
/*
{
  '0': {value: 0, writable: true, enumerable: true, configurable: true},
  '1': {value: 1, writable: true, enumerable: true, configurable: true},
  '100': {value: 100, writable: true, enumerable: true, configurable: true},
  length: {value: 101, writable: true, enumerable: false, configurable: false}
*/
```

이미 요소가 존재하는 요소에 값을 재할당하면 요소값이 갱신된다.

```
// 요소값의 갱신
arr[1] = 10;

console.log(arr); // [0, 10, empty × 98, 100]
```

인덱스는 요소의 위치를 나타내므로 반드시 0이상의 정수(또는 정수 형태의 문자열)을 사용해야 한다.

만약 정수 이외의 값을 인덱스처럼 사용하면 요소가 생서되는 것이 아니라 프로퍼티가 생성된다. 이때 추가된 프로퍼티는 length프로퍼티 값에 영향을 주지 않는다.

```
const arr = [];

// 배열 요소의 추가
arr[0] = 1;
arr['1'] = 2;

// 프로퍼티 추가
arr['foo'] = 3;
arr.bar = 4;
arr[1.1] = 5;
arr[-1] = 6;

console.log(arr); // [1, 2, foo: 3, bar: 4, '1.1': 5, '-1': 6]

// 프로퍼티는 length에 영향을 주지 않는다.
console.log(arr.length); // 2
```

#### 24.7 배열 요소의 삭제

배열은 사실 객체이기 때문에 배열의 특정 요소를 삭제하기 위해 delete 연산자를 사용할 수 있다.

```
const arr = [1, 2, 3];

// 배열 요소의 삭제
delete arr[1];
console.log(arr); // [1, empty, 3]

// length 프로퍼티에 영향을 주지 않는다. 즉, 희소 배열이 된다.
console.log(arr.length); // 3
```

delete 연산자는 객체의 프로퍼티를 삭제한다.

따라서 위 예제의 delete arr[1]은 arr에서 프로퍼티 키가 '1'인 프로퍼티를 삭제한다.

이때 배열은 희소 배열이 되며 length 프로퍼티 값은 변하지 않는다.

따라서 희소 배열을 만드는 delete 연산자는 사용하지 않는 것이 좋다.

희소 배열을 만들지 않으면서 배열의 특정 요소를 완전히 삭제하려면 Array.prototype.splice 메서드를 사용한다.

```
const arr = [1, 2, 3];

// Array.prototype.splice(삭제를 시작할 인덱스, 삭제할 요소 수)
// arr[1]부터 1개의 요소를 제거
arr.splice(1, 1);
console.log(arr); // [1, 3]

// length 프로퍼티가 자동 갱신된다.
console.log(arr.length); // 2
```

#### 24.8 배열 메서드

자바스크립트는 배열을 다룰 떼 유용한 빌트인 메서드를 제공한다.

Array 생성자 함수는 정적 메소드를 제공

배열 객체의 프로토타입인 Array.prototype은 프로토타입 메서드 제공

#### 배열에는 원본 배열(배열메서드를 호출한 배열, 즉 배열 메서드의 구현체 내부에서 this가 가리키는 객체)을 직접 변경하는 메서드와 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드가 있다.

#### Array.isArray 메서드

> Array 생성자 함수의 정적 메서드

- 전달된 인수가 배열이면 `true` , 배열이 아니면 `false` 를 반환

  ```jsx
  console.log(Array.isArray([])); // true
  console.log(Array.isArray([1, 2])); // true
  console.log(Array.isArray(new Array())); // true

  console.log(Array.isArray(null)); // false
  console.log(Array.isArray(1)); // false
  console.log(Array.isArray("string")); // false
  console.log(Array.isArray(undefined)); // false
  console.log(Array.isArray(true)); // false
  console.log(Array.isArray({})); // false
  ```

<br />

#### Array.prototype.indexOf 메서드

- 원본 배열에서 인수로 전달한 `요소를 검색하여 인덱스를 반환`

  - 검색되는 요소가 중복되어 여러 개일 경우 `첫 번째 검색된 요소의 인덱스를 반환`
  - 원본 배열에 검색할 요소가 존재하지 않으면 `-1 반환`
  - `배열에 특정 요소가 존재하는지 확인할 때 유용`

  ```jsx
  const arr = [1, 2, 2, 3];

  console.log(arr.indexOf(2)); // 1  ( 2를 검색 )
  console.log(arr.indexOf(2, 2)); // 2  ( 2번 째 인덱스 2를 검색)
  console.log(arr.indexOf(-1)); // -1 ( 존재하지 않는 요소 검색 )
  ```

<br />

#### Array.prototype.push 메서드

- 인수로 전달받은 `모든 값을 원본 배열 마지막 요소로 추가` , `변경된 length 프로퍼티 값을 반환`
- `mutator method`
  - 부수 효과가 있으므로, `ES6의 스프레드 문법을 사용하는 편이 좋다.`
- 성능 측면에서 배열에 추가할 요소가 하나라면 마지막 배열 요소를 직접 추가하는 방법이 더 빠르다.

  ```jsx
  const arr = [1, 2];

  arr.push([3, 4]);
  console.log(arr); // [ 1, 2, [ 3, 4 ] ]

  arr.push("a", "b");
  console.log(arr); // [ 1, 2, [ 3, 4 ], 'a', 'b' ]

  const arr2 = [...arr, true];
  console.log(arr2); // [ 1, 2, [ 3, 4 ], 'a', 'b', true ]
  ```

<br />

#### Array.prototype.pop 메서드

- 원본 배열에서 `마지막 요소를 제거하고 제거한 요소를 반환`
  - 원본 배열이 `빈 배열이면 undefined 반환`
- `mutator method`

  ```jsx
  const arr = [1, 2];

  let pop = arr.pop();
  console.log(arr); // [ 1 ]
  console.log(pop); // 2
  ```

- push 메서드와 혼합해서 `스택(stack) 자료구조` 를 구현할 수 있다.

  ```jsx
  // 클래스로 구현한 push와 pop 메서드를 활용한 "스택 자료구조"
  class Stack {
    #array;

    constructor(array = []) {
      if (!Array.isArray(array)) {
        throw new TypeError(`${array} is not an array !`);
      }
      this.#array = array;
    }

    push(value) {
      return this.#array.push(value);
    }

    pop() {
      return this.#array.pop();
    }

    entries() {
      return [...this.#array];
    }
  }

  const stack = new Stack([1, 2]);
  console.log(stack.entries()); // [ 1, 2 ]

  stack.push(3);
  console.log(stack.entries()); // [ 1, 2, 3 ]

  let pop = stack.pop();
  console.log(stack.entries(), pop); // [ 1, 2 ] 3
  ```

<br />

#### Array.prototype.unshift 메서드

- 인수로 전달 받은 `모든 값을 원본 배열의 선두에 추가`하고 `변경된 length 프로퍼티 값을 반환`
- `mutator mehtod`

  - 부수 효과가 있으므로, `ES6의 스프레드 문법을 사용하는 편이 좋다.`

  ```jsx
  const arr = [1, 2];

  let result = arr.unshift(3, 4);
  console.log(result); // 4
  console.log(arr); // [ 3, 4, 1, 2 ]

  const newArr = [100, ...arr];
  console.log(newArr); // [ 100, 3, 4, 1, 2 ]
  ```

<br />

#### Array.prototype.shift 메서드

- 원본 배열에서 `첫 번째 요소를 제거하고 제거한 요소를 반환`
  - 원본 배열이 빈 배열이면 `undefined 반환`
- `mutator method`

  ```jsx
  const arr = [1, 2];

  let shift = arr.shift();
  console.log(shift); // 1
  console.log(arr); // [ 2 ]
  ```

- unshift 메서드와 혼합해서 `큐(queue) 자료구조` 를 구현할 수 있다.

  ```jsx
  class Queue {
    #array;

    constructor(array = []) {
      if (!Array.isArray(array)) {
        throw new TypeError(`${array} is not an array !`);
      }
      this.#array = array;
    }

    enqueue(value) {
      return this.#array.push(value);
    }

    dequeue() {
      return this.#array.shift();
    }

    entries() {
      return [...this.#array];
    }
  }

  const queue = new Queue([1, 2]);
  console.log(queue.entries()); // [ 1, 2 ]

  queue.enqueue(3);
  console.log(queue.entries()); // [ 1, 2, 3 ]

  let dequeue = queue.dequeue();
  console.log(queue.entries(), dequeue); // [ 2, 3 ] 1
  ```

<br />

#### Array.prototype.concat 메서드

- 인수로 전달된 값들(배열 or 원시값)을 `원본 배열의 마지막 요소로 추가한 새로운 배열을 반환`
  - 인수로 전달한 값이 배열인 경우, 배열을 해체하여 새로운 배열의 요소로 추가
- push 메서드와 unshift 메서드는 concat 메서드로 대체 가능
  - 다만, 차이점은 concat 메서드는 원본 배열을 직접 변경하지 않고, 새로운 배열을 반환하는 것
  - 따라서, push 와 unshift 메서드의 경우 원본 배열은 다른 변수에 복사해놓고 사용해야 안전
- `ES6의 스프레드 문법으로 대체 가능하다.`

  ```jsx
  const arr1 = [1, 2];
  const arr2 = [3, 4];

  const arr3 = arr1.concat(arr2);
  console.log(arr3); // [ 1, 2, 3, 4 ]
  console.log(arr1, arr2); // [ 1, 2 ] [ 3, 4 ]

  const arr4 = arr3.concat("a", true);
  console.log(arr4); // [ 1, 2, 3, 4, 'a', true ]
  ```

<br />

#### Array.prototype.splice 메서드

- 원본 배열의 `중간에 요소를 추가`하거나 `중간에 있는 요소를 제거`하는 경우 사용
- 3개의 매개변수를 가진다.

  - `start` : 삭제 시작 인덱스
  - `deleteCount` : 시작 인덱스로부터 삭제할 요소의 개수
  - `items` : 요소를 삭제 후, 삭제한 인덱스로부터 추가할 데이터

  ```jsx
  const arr = [1, 2, 3, 4];

  const result = arr.splice(2, 1, 300);

  console.log(result); // [ 3 ]
  console.log(arr); // [ 1, 2, 300, 4 ]
  ```

- 배열에서 특정 요소를 제거하려면 Array.prototype.indexOf 와 혼합해서 구현할 수 있다.

  ```jsx
  const arr = [1, 2, 3, 1, 2];

  function remove(array, item) {
    const index = array.indexOf(item);

    if (index !== -1) array.splice(index, 1);

    return array;
  }

  console.log(remove(arr, 2)); // [ 1, 3, 1, 2 ] << 1번째 인덱스에 요소 2가 삭제된 후의 배열을 반환
  console.log(remove(arr, 100)); // [ 1, 3, 1, 2 ] << 100은 존재하지 않으므로 삭제된 요소는 없음
  ```

<br />

#### Array.prototype.slice 메서드

- 인수로 전달된 `범위의 요소들을 복사하여 배열로 반환`
- `accessor method`
- 2개의 매개변수를 가진다.

  - `start` : 복사 시작할 인덱스
  - `end` : 복사 끝 인덱스

  ```jsx
  const arr = [1, 2, 3];

  console.log(arr.slice(1, 3)); // [ 2, 3 ]
  console.log(arr); // [ 1, 2, 3 ]
  ```

- `얕은 복사(shallow copy)를 통해 새로운 배열을 생성`

  ```jsx
  const arr = [1, 2, 3];
  const shallowCopy = arr.slice();

  shallowCopy.splice(0, 1); // 복사본 배열 첫 번째 요소 삭제
  console.log(shallowCopy); // [ 2, 3 ]
  console.log(arr); // [ 1, 2, 3 ]
  ```

<br />

#### Array.prototype.join 메서드

- 원본 배열의 `모든 요소를 문자열로 변환한 후`, 인수로 전달받은 문자열, 즉 `구분자(separator)로 연결한 문자열을 반환`

  - 구분자는 `생략 가능`하며 `default separator 는 콤마(,)다.`

  ```jsx
  const arr = [1, 2, 3, 4];

  console.log(arr.join()); // 1,2,3,4
  console.log(arr.join(":")); // 1:2:3:4
  console.log(arr.join("")); // 1234
  ```

<br />

#### Array.prototype.reverse 메서드

- `원본 배열의 순서를 반대로 뒤집는다.`
- `mutator method`
- 반환 값은 변경된 배열

  ```jsx
  const arr = [1, 2, 3];
  const reversed = arr.reverse();

  console.log(arr); // [ 3, 2, 1 ] << 원본 데이터 파괴 (mutator method)
  console.log(reversed); // [ 3, 2, 1 ]
  ```

<br />

#### Array.prototype.fill 메서드

- `ES6에 도입`
- 인수로 전달받은 값을 `배열의 처음부터 끝까지 요소로 채운다.`
- `mutator method`
- 3개의 파라미터를 가진다.

  - `initialValue` : 초기화 시킬 값
  - `start` : 시작 인덱스 값
  - `end` : 끝 인덱스 값

  ```jsx
  const arr = new Array(3);
  console.log(arr); // [ <3 empty items> ]

  arr.fill(1);
  console.log(arr); // [ 1, 1, 1 ]

  arr.fill(100, 1, 2);
  console.log(arr); // [ 1, 100, 1 ] << 원본 데이터 파괴 (mutator method)
  ```

<br />

#### Array.prototype.includes 메서드

- `ES7에 도입`
- 배열 내에 `특정 요소가 포함되어 있는지 확인하여 true OR false 를 반환`
- 2개의 매개변수를 가진다.

  - `serachValue` : 검색할 값
  - `start` : 시작 인덱스

  ```jsx
  const arr = [1, 2, 3];

  console.log(arr.includes(3)); // true
  console.log(arr.includes(100)); // false
  ```

- Array.prototype.indexOf 메서드와 차이점은 `indexOf 메서드는 없으면 -1 임을 확인해야하며, 배열에 NaN 이 있다면 판별할 수 없다.`
  ```jsx
  console.log([NaN].indexOf(NaN)); // -1 🔍
  console.log([NaN].includes(NaN)); // true
  ```

<br />

#### Array.prototype.flat 메서드

- `ES10에 도입`
- 인수로 전달한 `깊이만큼 재귀적으로 배열을 평탄화(flat)한다.`
- defaultValue 는 1이다.

  - 인수를 `Infinity` 로 넘기면, `아무리 깊은 중첩 배열도 모두 평탄화한다.`

  ```jsx
  const dupArr = [1, [2, 3, 4, 5]];
  console.log(dupArr.flat()); // [ 1, 2, 3, 4, 5 ]

#### 배열 고차 함수

> `고차 함수(Higher-Order Function, HOF)` : 함수를 인수로 전달받거나 함수를 반환하는 함수를 의미

- 자바스크립트에서 함수는 `객체`다.
  - 정확히는 `일급 객체`다.
  - 따라서, 함수를 값처럼 인수로 전달할 수 있으며 반환할 수도 있는 것이다.
- 고차 함수는 외부 상태의 변경이나 `가변 데이터(mutable data)를 피하고 불변성(immutability)을 지향하는 함수형 프로그래밍에 기반을 둔다.`
  - 함수형 프로그래밍은 `순수 함수(pure function)`와 `보조 함수의 조합`을 통해 로직 내에 존재하는 `조건문과 반복문을 제거하여 복잡성을 해결하고 변수의 사용을 억제`하여 상태 변경을 피하려는 `프로그래밍 패러타임을 의미`
- 자바스크립트 배열은 매우 유용한 고차 함수를 제공

<br />

#### Array.prototype.sort 메서드

- `배열의 요소를 정렬, 정렬된 배열을 반환`

  - default 로는 오름차순 정렬이다.

  ```jsx
  const fruits = ["Banana", "Orange", "Apple"];

  fruits.sort();
  console.log(fruits); // [ 'Apple', 'Banana', 'Orange' ]
  ```

- `mutator method`
- 기본적인 정렬 순서는 `유니코드 코드 포인트의 순서를 따른다.`

  - 배열의 요소들을 정렬 시, `숫자 타입이어도 암묵적으로 문자열 타입으로 변환 후 유니코드 코드 포인트 순서에 따라 정렬`
  - 따라서, 숫자 요소를 정렬 시에는 정렬 순서를 정의하고 `비교 함수(compare function)를 인수로 전달해야 한다.`
  - 비교 함수는 `양수 or 음수 or 0을 반환해야 한다.`
    - `양수` : 비교 함수의 두 번째 인수를 우선 정렬
    - `음수` : 비교 함수의 첫 번째 인수를 우선 정렬
    - `0` : 정렬하지 않음

  ```jsx
  const numbers = [40, 100, 1, 5, 2, 25, 10];

  numbers.sort();
  console.log(numbers); // [1, 10, 100, 2, 25, 40, 5] << 제대로 된 오름차순 정렬이 아님

  numbers.sort((a, b) => a - b);
  console.log(numbers); // [1, 2, 5, 10, 25, 40, 100]
  ```

- 초기에 sort 메서드는 quicksort 알고리즘을 기반으로 구현되었지만, ES10 이후부터는 `timsort 알고리즘 기반으로 변경`되었다고 한다.

<br />

#### Array.prototype.forEach 메서드

- `for문을 대체할 수 있는 고차 함수`
  ```jsx
  [1, 2, 3].forEach((item, idx, arr) => {
    console.log(`요소 값 : ${item}, 인덱스 : ${idx}, this : ${arr}`);
  });
  // 요소 값 : 1, 인덱스 : 0, this : 1,2,3
  // 요소 값 : 2, 인덱스 : 1, this : 1,2,3
  // 요소 값 : 3, 인덱스 : 2, this : 1,2,3
  ```
- 자신의 내부에서 반복문을 실행

  - 즉, 반복문을 추상화한 고차 함수로서 내부에서 반복문을 통해 `자신을 호출한 배열을 순회하면서 수행해야 할 처리를 콜백 함수로 전달받아 반복 호출한다.`
  - `각 배열의 요소에 대해 한 번씩 콜백 함수가 호출된다.`
  - `콜백 함수는 일반 함수로 적용`되기 때문에, `this의 참조는 undefined가 바인딩된다.` ( 고차함수에서는 strict mode 가 반영되기 때문, 아닐 경우 전역 객체를 가리킨다. )

  ```jsx
  class Numbers {
    numberArray = [];

    mul(arr) {
      arr.forEach(function (item) {
        // forEach 의 인수로 전달하는 콜백함수가 "일반함수"인 경우, this 참조는 undefined 를 바인딩
        // TypeError: Cannot read property 'numberArray' of undefined
        this.numberArray.push(item * item);
      });
    }
  }

  const numbers = new Numbers();
  numbers.mul([1, 2, 3]);
  ```

  - 콜백 함수 내부의 this와 호출한 메서드 내부의 this를 일치시키려면 `화살표 함수나, this로 사용할 객체를 바인딩해야 한다.`

  ```jsx
  class Numbers {
    numberArray = [];

    mul(arr) {
      arr.forEach((item) => {
        // forEach 의 인수로 전달하는 콜백함수가 "화살표 함수"인 경우, this 참조는 Lexical this 참조를 따른다. 즉 this 호출의 상위 스코프를 참조한다.
        this.numberArray.push(item * item);
      });
    }
  }

  const numbers = new Numbers();
  numbers.mul([1, 2, 3]);
  console.log(numbers.numberArray); // [ 1, 4, 9 ]
  ```

- `accessor method`
- 반환값은 undefined 다.
- `forEach 문 내부에서는 break, continue 문을 사용할 수 없다.` 즉, 중간에 순회를 중단할 수 없다.

  ```jsx
  [1,2,3].forEach((item, idx) => {
    console.log(item);
    if(idx > 0) break;
    // SyntaxError: Illegal break statement
  })

  [1,2,3].forEach((item, idx) => {
    console.log(item);
    if(idx > 0) continue;
    // SyntaxError: Illegal continue statement: no surrounding iteration statement
  })
  ```

- for 문에 비해 기본적인 성능은 좋지 않지만 `가독성은 좋다.`
  - 따라서, 대용량 데이터를 순회하거나 시간이 많이 걸리는 작업이 아니라면 for문 보단 forEach문을 사용하는 것을 권장

<br />

#### [Array.prototype.map](http://Array.prototype.map) 메서드

- `콜백 함수의 반환값들로 구성된 새로운 배열을 반환`
- `accessor method`
- 원본 배열의 요소값을 다른 값으로 `매핑(mapping)`한 `새로운 배열을 생성하기 위한 고차 함수`

  ```jsx
  const arr = [1, 2, 3];
  const mappingArr = arr.map((item) => Math.pow(item, 2));

  console.log(arr); // [ 1, 2, 3 ]
  console.log(mappingArr); // [ 1, 4, 9 ]
  ```

<br />

#### Array.prototype.filter 메서드

- `콜백 함수의 반환값이 true인 요소로만 구성된 새로운 배열을 반환`
- `accessor mehtod`
- `특정 조건에 맞는 배열의 요소들을 추출할 때 유용하게 사용된다.`

  ```jsx
  const arr = [1, 2, 3, 4, 5];
  const filteredArr = arr.filter((item) => item < 4);

  console.log(arr); // [ 1, 2, 3, 4, 5 ]
  console.log(filteredArr); // [ 1, 2, 3 ]
  ```

<br />

#### Array.prototype.reduce 메서드

- 콜백 함수를 호출하여 `하나의 결과값을 만들어 반환`
  - 매 차례마다, 콜백 함수의 반환값과 두 번째 요소값을 콜백 함수의 인수로 전달하면서 호출한다.
- `accessor method`
- 4개의 매개변수를 가진다.

  - `accumulator` : 누적될 값
  - `currentValue` : 현재 조회하는 값
  - `index` : 현재 조회하는 값의 인덱스
  - `array` : 원본 배열 참조(this)

  ```jsx
  const arr = [1, 2, 3, 4, 5];

  arr.reduce((acc, cur, idx, arr) => {
    console.log(acc, cur, idx, arr);
    return acc + cur;
  }, 0);
  // 0 1 0 [ 1, 2, 3, 4, 5 ]
  // 1 2 1 [ 1, 2, 3, 4, 5 ]
  // 3 3 2 [ 1, 2, 3, 4, 5 ]
  // 6 4 3 [ 1, 2, 3, 4, 5 ]
  // 10 5 4 [ 1, 2, 3, 4, 5 ]
  ```

- 여러 용도로 사용한다.

  - `평균 구하기`

  ```jsx
  const arr = [1, 2, 3, 4, 5];

  const average = arr.reduce((acc, cur, idx, { length }) => {
    // 마지막 index 원소라면 마지막 원소까지 누적한 수의 arr 길이만큼 나눠서 평균을 반환, 그게 아니라면 누적한 수에 현재 요소를 더한 값을 다음 콜백함수의 사용할 반환값으로 반환
    return idx === length - 1 ? (acc + cur) / length : acc + cur;
  }, 0);

  console.log(average); // 3  = (1+2+3+4+5) / 5
  ```

  - `최대값 구하기`

  ```jsx
  const arr = [1, 2, 3, 4, 5];

  const max = arr.reduce((acc, cur) => (acc > cur ? acc : cur), 0);
  console.log(max); // 5

  // const max = Math.max(...arr);  // Math.max 를 사용하는 것이 사실 훨씬 더 직관적
  // console.log(max);              // 5
  ```

  - `요소의 중복 횟수 구하기`

  ```jsx
  const arr = ["banana", "apple", "apple", "orange", "apple"];

  const dupArr = arr.reduce((acc, cur) => {
    acc[cur] = (acc[cur] || 0) + 1;
    return acc;
  }, {});

  console.log(dupArr); // { banana: 1, apple: 3, orange: 1 }
  ```

  - `중복 요소 제거`

  ```jsx
  const arr = [1, 2, 1, 3, 5, 4, 5, 3, 4, 4];

  const removeDupArr = arr.reduce((acc, cur, idx, arr) => {
    if (arr.indexOf(cur) === idx) acc.push(cur);
    return acc;
  }, []);

  console.log(removeDupArr); // [ 1, 2, 3, 5, 4 ]

  // 사실, 배열 내 중복된 요소를 제거한 배열을 추출하는 것은 Array.prototype.from 메서드와 Set 자료형을 활용하면 쉽게 구할 수 있다.
  // const removeDupArr = Array.from(new Set(arr));
  // console.log(removeDupArr1);   // [ 1, 2, 3, 5, 4 ]
  ```

<br />

#### Array.prototype.some 메서드

- 콜백 함수의 반환값이 `단 한 번이라도 참이면 true, 모두 거짓이면 false를 반환`
- 즉, `배열의 요소 중에 콜백 함수를 통해 정의한 조건을 만족하는 요소가 1개 이상 존재하는지 확인`하여 그 결과를 boolean 타입으로 반환

  - 단, `빈 배열인 경우 언제나 false를 반환`

  ```jsx
  const odds = [1, 3, 5, 7];

  // odds 배열 내에는 조건인 짝수가 하나도 없으므로, false 반환
  console.log(
    odds.some((item) => {
      console.log(item);
      return item % 2 === 0;
    }),
  );
  // 1
  // 3
  // 5
  // 7
  // false

  // odds 배열 내에는 첫 요소 1부터 홀수 이므로, 하나라도 조건에 만족하는 요소가 있으니 true 반환
  console.log(
    odds.some((item) => {
      console.log(item);
      return item % 2 !== 0;
    }),
  );
  // 1
  // true
  ```

<br />

#### Array.prototype.every 메서드

- 콜백 함수의 반환값이 `모두 참이면 true, 단 한 번이라도 거짓이면 false를 반환`
- 즉, `배열의 모든 요소가 콜백 함수를 통해 정의한 조건을 모두 만족하는지 확인`하여 그 결과를 boolean 타입으로 반환

  - 단, `빈 배열인 경우 언제나 true를 반환`

  ```jsx
  const odds = [1, 3, 5, 7];

  // odds 배열 내에는 조건인 짝수가 하나도 없으므로, false 반환
  console.log(
    odds.every((item) => {
      console.log(item);
      return item % 2 === 0;
    }),
  );
  // 1
  // false

  // odds 배열 내에는 첫 요소 1부터 홀수 이므로, 하나라도 조건에 만족하는 요소가 있으니 true 반환
  console.log(
    odds.every((item) => {
      console.log(item);
      return item % 2 !== 0;
    }),
  );
  // 1
  // 3
  // 5
  // 7
  // true
  ```

<br />

#### Array.prototype.find 메서드

- `ES6에 도입`
- 자신을 호출한 `배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 true를 반환하는 첫 번째 요소를 반환`

  - true를 반환하는 요소가 존재하지 않는 경우 `undefined 를 반환`

  ```jsx
  const users = [
    {
      id: 1,
      name: "YI",
    },
    {
      id: 2,
      name: "KI",
    },
    {
      id: 3,
      name: "WI",
    },
    {
      id: 4,
      name: "MI",
    },
  ];

  console.log(users.find((item) => item.id === 3)); // { id: 3, name: 'WI' }
  console.log(users.find((item) => item.name === "YOUNG")); // undefined
  ```

<br />

#### Array.prototype.findIndex 메서드

- `ES6에 도입`
- findIndex 메서드는 `콜백 함수에 정의한 조건에 대해 true를 반환하는 요소의 인덱스를 반환`

  - true를 반환하는 요소가 존재하지 않는 경우 `-1을 반환`

  ```jsx
  const users = [
    {
      id: 1,
      name: "YI",
    },
    {
      id: 2,
      name: "KI",
    },
    {
      id: 3,
      name: "WI",
    },
    {
      id: 4,
      name: "MI",
    },
  ];

  console.log(users.findIndex((item) => item.id === 3)); // 2;
  console.log(users.findIndex((item) => item.name === "YOUNG")); // -1
  ```

<br />

#### Array.prototype.flatMap 메서드

- `ES10에 도입`
- [`Array.prototype.map](http://Array.prototype.map) 메서드를 통해 생성된 배열을 평탄화한다.`

  - 즉, map 함수 호출한 결과 → flat 메서드 호출한 결과와 같다.
  - 단, `flat 과정의 깊이를 1로만 적용할 수 있다.`

  ```jsx
  const arr = ["hello", "world"];

  console.log(arr.map((item) => item.split("")).flat()); // ["h", "e", "l", "l", "o", "w", "o", "r"]
  console.log(arr.flatMap((item) => item.split(""))); // ["h", "e", "l", "l", "o", "w", "o", "r"]
  ```







