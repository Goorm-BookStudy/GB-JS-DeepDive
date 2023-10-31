# ✏️ 37장 Set과 Map

## 📌 37.2 Map

- 키와 값의 쌍으로 일워진 컬렉션으로 객체와 유사하지만 차이점이 있다.
- Map 객체는 객체를 포함한 모든 값을 키로 사용할 수 있다. (객체는 문자열 & 심벌 값만 가능)
- Map 객체는 이터러블하다. (객체는 이터러블하지 않다.)
- Map 객체는 map.size로 요소의 개수를 확인할 수 있다. (객체는 Object.keys(obj).length로 확인)

#### 1. Map 객체의 생성

- Map 생성자 함수로 생성하며 인수를 전달하지 않을 경우 빈 Map 객체가 생성된다.

```
const map = new Map();
console.log(map); // Map(0) {}
```

- Map 생성자 함수는 이터러블을 인수로 전달받아 Map 객체를 생성한다.
- 인수로 전달되는 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성되어야 한다.

```
const map1 = new Map([['key1', 'value1'], ['key2', 'value2']]);
console.log(map1); // Map(2) {"key1" => "value1", "key2" => "value2"}

const map2 = new Map([1, 2]); // 타입 에러
```

- Map 생성자 함수의 인수로 전달한 이터러블에 중복된 키를 갖는 요소가 존재하면 값이 갱신된다.
- Map 객체에는 중복된 키를 갖는 요소가 존재할 수 없다. (Set이랑 비슷한 듯 다름)

```
const map = new Map([['key1', 'value1'], ['key1', 'value2']]);
console.log(map); // Map(1) {"key1" => "value2"}
```

#### 2. 요소 개수 확인

- Map 객체의 요소 개수를 확인할 때는 Map.prototype.size 프로퍼티를 사용한다.
- size 프로퍼티는 getter 함수만 존재하는 접근자 프로퍼티로 숫자를 할당해 요소 개수를 변경할 수 없다.

```
const { size } = new Map([['key1', 'value1'], ['key1', 'value2']]);
console.log(size); // 2
```

#### 3. 요소 추가

- Map 객체에 요소를 추가할 때는 Map.prototype.set 메서드를 사용한다.
- set 메서드는 새로운 요소가 추가된 Map 객체를 반환한다.
- 따라서 메서드 체이닝이 가능하다.
- 중복된 키를 갖는 요소가 존재할 수 없기에 중복된 키를 갖는 요소를 추가하면 에러 없이 값이 갱신된다.

```
const map = new Map();
map.set('k1', 'v1').set('k1', 'v2');
console.log(map); // Map(1) {"k1" => "v2"}
```

- 일치 비교 연산자 ===를 사용하면 NaN과 NaN를 다르다고 평가한다. (자기 자신과 일치하지 않는 유일한 값)
- 하지만 Map 객체는 같다고 평가하여 중복 추가를 허용하지 않는다.
- +0, -0은 일치 비교 연산자와 동일하게 같다고 평가하여 중복 추가를 허용하지 않는다.
- Map 객체는 키 타입에 제한이 없어 모든 값을 키로 설정할 수 있다.

```
const map = new Map();

console.log(NaN === NaN); // false
console.log(0 === -0); // true

map.set(NaN, 'v1').set(NaN, 'v2');
console.log(map); // Map(1) {NaN => 'v2'}

map.set(0, 'v1').set(-0, 'v2');
console.log(map); // Map(1) {NaN => 'v2', 0 => 'v2'}

// 모든 키 타입
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

map.set(lee, 'developer').set(kim, 'designer');
console.log(map); // Map(2) { {name: "Lee"} => "developer", {name: "Kim"} => "designer" }
```

#### 4. 요소 취득

- Map 객체에서 특정 요소를 취득하려면 Map.prototype.get 메서드를 사용한다.
- get 메서드의 인수로 키를 전달하면 Map 객체에서 인수로 전달한 키를 갖는 값을 반환한다.
- Map 객체에서 인수로 전달한 키를 갖는 요소가 존재하지 않으면 undefined를 반환한다.

```
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

map.set(lee, 'developer').set(kim, 'designer');

console.log(map.get(lee)); // developer
console.log(map.get('key')); // undefined
```

#### 5. 요소 존재 여부 확인

- Map 객체에 특정 요소의 존재 여부를 확인할 때는 Map.prototype.has 메서드를 사용한다.
- 특정 요소의 존재 여부를 나타내는 불리언 값을 반환한다.

```
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

console.log(map.has(lee)); // true
console.log(map.has('key')); // false
```

#### 6. 요소 삭제

- Map 객체의 요소를 삭제하려면 Map.prototype.delete 메서드를 사용한다.
- 존재하지 않는 키로 요소를 삭제하려 하면 에러 없이 무시된다.
- 삭제 성공 여부를 나타내는 불리언 값을 반환하므로 메서드 체이닝이 불가하다.

```
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.delete(kim);
console.log(map); // Map(1) { {name: "Kim"} => "designer" }

// 메서드 체이닝 실패
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.delete(lee).delete(kim); // 타입 에러
```

#### 7. 요소 일괄 삭제

- Map 객체의 요소를 일괄 삭제하려면 Map.prototype.clear 메서드를 사용한다.
- 언제나 undefined를 반환한다.

```
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.clear();
console.log(map); // Map(0) {}
```

#### 8. 요소 순회

- Map 객체의 요소를 순회하려면 Map.prototype.forEach 메서드를 사용한다.
- 해당 메서드는 콜백 함수와 forEach 메서드의 콜백 함수 내부에서 this로 사용될 객체를 인수로 전달한다. 이 때 콜백 함수는 1번째 인수(현재 순회중인 요소 값), 2번째 인수(현재 순회중인 요소 키), 3번째 인수(현재 순회중인 Map 객체 자체)를 전달 받는다.

```
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.forEach((v, k, map) => console.log(v, k, map));
/*
developer {name: "Lee"} Map(2) {
    {name: "Lee"} => "developer",
    {name: "Kim"} => "designer"
}
designer {name: "Kim"} Map(2) {
    {name: "Lee"} => "developer",
    {name: "Kim"} => "designer"
}
*/
```

- Map 객체는 이터러블이므로 for...of 문으로 순회할 수 있다.
- 스프레드 문법과 배열 구조 분해 할당의 대상이 될 수도 있다.

```
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

console.log(Symbol.iterator in map); // true

for (const entry of map) {
    console.log(entry); // [[{name: 'Lee'}, "developer"] [{name: 'Kim'}, "designer"]]
}

console.log([...map]);
// [[{name: 'Lee'}, "developer"] [{name: 'Kim'}, "designer"]]

const [a, b] = map;
console.log(a, b); // [{name: 'Lee'}, "developer"] [{name: 'Kim'}, "designer"]
```

- Map 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드를 제공한다.
  - Map.prototype.keys: Map 객체에서 요소키를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체 반환
  - Map.prototype.values: Map 객체에서 요소값를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체 반환
  - Map.prototype.entries: Map 객체에서 요소키와 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체 반환
- 요소의 순서에 의미를 갖진 않지만 다른 이터러블의 순회화 호환성 유지를 위해 순서는 요소가 추가된 순서를 따른다.

```
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

for (const k of map.keys()) {
    console.log(k); // {name: 'Lee'} {name: 'Kim'}
}

for (const v of map.values()) {
    console.log(v); // developer designer
}

for (const entry of map.entris()) {
    console.log(entry); // [{name: "Lee"}, "developer", {name: "Kim"}, "designer"]
}
```
