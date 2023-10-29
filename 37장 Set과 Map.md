# 37장 Set과 Map
## Set 객체 - 중복되지 않는 유일한 값들의 집합
- 수학적 집합을 구현하기 위한 자료구조 교집합, 합집합, 차집합, 여집합 등을 구현 가능

|구분|배열|Set객체|
|:---:|:---:|:---:|
|동일한 값을 중복하여 포함할 수 있다.|O|X|
|요소 순서에 의미가 있다.|O|X|
|인덱스로 요소에 접근할 수 있다.|O|X|

<br/>

### Set 객체의 생성
- Set 생성자 함수로 생성 인수 전달 안 하면 빈 Set 객체가 생성
- 이터러블을 인수로 전달받아 Set 객체 생성 이 때 중복된 값은 Set객체 요소로 저장 x

```
// 배열의 중복 요소 제거
const uniq = array => array.filter((v, i, self) => self.indexOf(v) === i);
console.log(uniq([2,1,2,3,4,3,4])); // [2, 1, 3, 4]

// Set을 이용
const uniq = array => [...new, Set(array)];
console.log(uniq([2,1,2,3,4,3,4])); // [2,1,3,4]
```

<br/>

### 요소 개수 확인
- Set.prototype.size 프로퍼티 사용
- size 프로퍼티는 getter 함수만 존재하는 접근자 프로퍼티로 size 프로퍼티에 숫자를 할당해도 요소 개수 변경 불가

```
const { size } = new Set([1,2,3,3]);
console.log(size); // 3

set.size = 10; // 무시됨
console.log(size); // 3
```

<br/>

### 요소 추가
- Set.prototype.add 메서드 사용
- 새로운 요소가 추가된 Set 객체를 반환하기 때문에 연속적으로 add 호출 가능
- 모든 값을 요소로 저장 가능

```jsx
const set = new Set();
console.log(set); // set(0) {}

set.add(1);
console.log(set); // Set(1) {1}

// 연속 호출
set.add(1).add(2).add(2);

// NaN을 일치 비교 연산자로 비교하면 다르다고 평가하지만, Set 객체는 같다고 평가해 중복 허용x
console.log(NaN === NaN); // false
console.log(0 === -0); // true

set.add(NaN).add(NaN).add(0).add(-0);
console.log(set); // set(2) [NaN, 0]
```

<br/>

### 요소 존재 여부 확인
- Set.prototype.has 메서드 사용
- 특정 요소의 존재 여부를 나타내는 불리언 값 반환

```
const set = new Set([1,2,3]);

console.log(set.has(2)); // true
console.log(set.has(4)); // false
```

<br/>

### 요소 삭제
- Set.prototype.delete 메서드 사용
- 삭제 성공 여부를 나타내는 불리언 값 반환
- 인덱스가 아닌 삭제할 요소값을 인수로 전달해야 한다(Set 객체는 순서 의미 x -> 인덱스 의미 x)
- 삭제 성공 여부를 불리언 값을 반환하므로 연속 호출 불가

```
const set = new Set([1, 2, 3]);

set.delete(2);
console.log(set); // Set(2) {1,3}

// 존재하지 않는 Set 객체의 요소를 삭제하면 에러 없이 무시
set.delete(0);
console.log(set); // Set(2) {1,3}

```

<br/>

### 요소 일괄 삭제
- Set.prototype.clear 메서드 사용, 결과는 언제나 undefiend 반환

```
const set = new Set([1,2,3]);

set.clear();
console.log(set); // Set(0) {}
```

<br/>

### 요소 순회
- Set.prototype.forEach 메서드 사용
- 콜백 함수와 forEach 메서드이 콜백 함수 내부에서 this로 사용될 객체를 인수로 전달 (3개의 인수를 전달 각각 현재 순회 중인 요소값, 현재 순회 중인 요소값, 현재 순회 중인 Set 객체 자체)
- Set 객체는 이터러블로 for...of, 스프레드 문법, 배열 디스트럭처링의 대상이 된다.

```
const set = new Set([1,2,3]);

console.log(Symbol.iterator in set); // true

for(const value of set){
	console.log(value); // 1 2 3
}

console.log([...set]); // [1,2,3]

const [a, ...rest] = set;
console.log(a, rest); // 1, [2,3]
```

- Set 객체는 순서 의미가 없지만 Set 객체를 순회하는 순서 요소가 추가된 순서를 따른다 -> ECMAScrip 사양에 규정되어있지는 않지만 다른 이터러블의 순회와 호환성을 유지하기 위해

<br/>

### 집합 연산
#### 교집합
- A와 B의 공통 요소로 구성

```
Set.prototype.intersection = function (set) {
	const result = new Set();
    
    for (const value of set) {
    	if(this.has(value)) result.add(value);
    }
    
	return result;
};

const setA = new Set([1,2,3,4]);
const setB = new Set([2, 4]);

console.log(setA.intersection(setB)); // set(2) {2,4} 

// 또 다른 방법
Set.prototype.intersection = function (set) {
	return new Set([...this].filter(v=>set.has(v)));
};

const setA = new Set([1,2,3,4]);
const setB = new Set([2, 4]);

console.log(setA.intersection(setB)); // set(2) {2,4} 
```

<br/>

#### 합집합

```
Set.prototype.union = function (set) {
	const result = new Set(this);
    
    for (const value of set) {
		result.add(value);
    }
    
	return result;
};

const setA = new Set([1,2,3,4]);
const setB = new Set([2, 4]);

console.log(setA.union(setB)); // set(2) {1,2,3,4} 

// 다른 방법
Set.prototype.union = function (set) {
	return new Set([...this].set);
};

const setA = new Set([1,2,3,4]);
const setB = new Set([2, 4]);

console.log(setA.union(setB)); // set(2) {1,2,3,4} 
```

<br/>

#### 차집합

```
Set.prototype.difference = function (set) {
	const result = new Set();
    
    for (const value of set) {
    	result.delete(value);
    }
    
	return result;
};

const setA = new Set([1,2,3,4]);
const setB = new Set([2, 4]);

console.log(setA.difference(setB)); // set(2) {1, 3} 

// 다른 방법
Set.prototype.difference = function (set) {
	return new Set([...this].filter(v=> !set.has(v)));
};

const setA = new Set([1,2,3,4]);
const setB = new Set([2, 4]);

console.log(setA.difference(setB)); // set(2) {1,3} 
```

<br/>

#### 부분 집합과 상위 집합
- 집합 A가 B에 포함되면 집합 A는 집합 B의 부분 집합 이며, 집합 B는 집합 A의 상위 집합이다.

```
Set.prototype.isSuperset = function (subset) {
    for (const value of subset) {
		if(!this.has(value)) return false;
    }
    
	return result;
};

const setA = new Set([1,2,3,4]);
const setB = new Set([2, 4]);

console.log(setA.isSuperset(setB)); // true

// 다른 방법
Set.prototype.isSuperset = function (subset) {
	const supersetArr = [... this];
    return [... subset].every(v => supersetArr.includes(v));
};

const setA = new Set([1,2,3,4]);
const setB = new Set([2, 4]);

console.log(setA.isSuperset(setB)); // true
```

<br/>


## Map

|구분|배열|Map 객체|
|:---:|:---:|:---:|
|키로 사용할 수 있는 값|문자열 또는 심벌 값|객체를 포함한 모든 값|
|이터러블|X|O|
|요소 개수 확인|Object.keys(obj).length|map.size|

- Map 객체는 Map 생성자 함수로 생성, 인수를 전달하지 안흥면 빈 Map 객체 생성
- Map 생성자 함수는 이터러블을 인수로 Map 객체 생성, 이때 인수로 전달되는 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성되어야 함
- 중복된 키를 갖는 요소가 존재하면 값을 덮어쓰게 된다.

```
const map1 = new Map([['key', 'value1'], ['key2', 'valye2']]);
console.log(map1); // Map(2) {'key1'; => 'value1', 'key2' => 'value2'}

const map2 = new Map([1,2]); // TypeError
```

<br/>

### 요소 개수 확인
- Map.prototype.size 프로퍼티 사용
- size 프로퍼티는 getter 함수만 존재하는 접근자 프로퍼티다. -> size 프로퍼티에 값 할당해도 요소 개수 변경 불가

```
const {size} = new Map([['key', 'value1'], ['key2', 'valye2']]);
console.log(size); // 2
```

<br/>

### 요소 추가
- Map.prototype.set 메서드 추가
- 연속 호출 가능하다.
- 중복된 키의 경우 에러를 발생시키지 않고 덮어써진다.
- 객체는 문자열 또는 심벌만 키로 사용 가능하지만 Map은 키 타입에 제한 x

```
const map = new Map();
console.log(map); // Map(0) {}

map.set('key1', 'value1');
console.log(map); // Map(1) {'key1' => 'value1'}

// NaN을 일치 비교 연산자로 비교하면 다르다고 평가하지만, Set 객체는 같다고 평가해 중복 허용x
console.log(NaN === NaN); // false
console.log(0 === -0); // true

map.add(NaN).add(NaN).add(0).add(-0);
console.log(map); // map(2) [NaN, 0]
```

<br/>

### 요소 취득
- Map.prototype.get 메서드 사용
- 인수로 키를 전달하면 Map 객체에서 인수로 전달한 키를 갖는 값을 반환 존재 안 하면 undefiend 반환


```
const map = new Map();

const lee = {name: 'Lee'};

map.set(lee, 'hamin');

console.log(map.get(lee)); // hamin
console.log(map.get(key)); // undefiend
```

<br/>

### 요소 존재 여부 확인
- Map.prototype.has 사용 특정 요소 존재 여부를 불리언 값으로 반환

```
const lee = {name: 'Lee'};

const map = new Map([lee, 'hamin']);

console.log(map.has(lee)); // true
console.log(map.has(key)); // false
```

<br/>

### 요소 삭제
- Map.prototype.delete 메서드 사용 성공 여부를 불리언으로 반환
- 존재하지 않는 키로 삭제하면 에러 없이 무시
- 연속적으로 호출 불가

```
const lee = {name: 'Lee'};

const map = new Map([lee, 'hamin']);

map.delete(lee);
console.log(map); // Map(0) {}
```

<br/>

### 요소 일괄 삭제
- Map.prototype.clear 사용, 언제나 undefined 반환

```
const lee = {name: 'Lee'};

const map = new Map([lee, 'hamin']);

map.clear();
console.log(map); // Map(0) {}
```

<br/>

### 요소 순회
- Map.prototype.forEach 사용
- Set과 동일하게 3개의 인수를 전달받음
- Map 객체는 이터러블이므로 for...of, 스프레드 문법, 배열 디스트럭처링 할당의 대상이 된다.
- Map 객체는 이터러블이면서 동시에 이터러블인 객체를 변환하는 메서드 제공

```
const lee = {name: 'Lee'};
const Kim = {name: 'Kim'};

const map = new Map([[lee, 'apple'], [kim, 'banana']]);

// Map 객체에서 요소키를 값으로 갖는 이터레이터 반환
for (const key of map.keys()){
	console.log(key); // {name: 'Lee'} {name: 'Kim'}
}

// Map 객체에서 요소값을 값으로 갖는 이터레이터 반환
for (const key of map.values()){
	console.log(key); // apple, banana
}

// Map 객체에서 요소값을 값과 요소키를 갖는 이터레이터 반환
for (const key of map.entries()){
	console.log(key); // [{name: 'Lee'}, 'apple'] [{name: 'Kim'}, 'banana']
}
```
