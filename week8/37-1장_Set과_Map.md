# ✏️ 37장 Set과 Map

## 📌 37.1 Set

- 중복되지 않는 유일한 값들의 집합이다.
- 동일한 값을 중복해 포함할 수 없다.
- 요소 순서에 의미가 없다.
- 인덱스로 요소에 접근할 수 없다.
- 수학적 집합의 특성과 일치하여 교집합, 합집합, 차집합, 여집합 등을 구현할 수 있다.

#### 1. Set 객체의 생성

- Set 생성자 함수로 생성하며 인수를 전달하지 않을 경우 빈 Set 객체가 생성된다.

```
const set = new Set();
console.log(set); // Set(0) {}
```

- Set 생성자 함수는 이터러블을 인수로 전달받아 Set 객체를 생성한다.
- 이터러블의 중복된 값은 Set 객체에 요소로 저장되지 않는다.

```
const set1 = new Set([1, 2, 3, 3]);
console.log(set1); // Set(3) {1, 2, 3}

const set2 = new Set('hello');
console.log(set2); // Set(4) {"h", "e", "l", "o"}
```

- Set을 이용해 배열의 중복된 요소를 제거할 수도 있다.

```
const uniq = array  => array.filter((v, i, self) => self.indexOf(v) === i);
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]

const uniq = array => [...new Set(array)];
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]
```

#### 2. 요소 개수 확인

- Set 객체의 요소 개수를 확인 할 때는 Set.prototype.size 프로퍼티를 사용한다.
- size 프로퍼티는 getter 함수만 존재하는 접근자 프로퍼티로 숫자를 할당해 요소 개수를 변경할 수 없다.

```
const { size } = new Set([1, 2, 3, 3]);
console.log(size); // 3
```

#### 3. 요소 추가

- Set 객체에 요소를 추가할 때는 Set.prototype.add 메서드를 사용한다.
- add 메서드는 새로운 요소가 추가된 Set 객체를 반환한다.
- 따라서 메서드 체이닝이 가능하다.
- 중복된 요소의 추가는 허용되지 않으며 에러 없이 무시된다.

```
const set = new Set();
console.log(set); // Set(0) {}

set.add(1);
console.log(set); // Set(1) {1}

set.add(2).add(3).add(3);
console.log(set); // Set(3) {1, 2, 3}
```

- 일치 비교 연산자 ===를 사용하면 NaN과 NaN를 다르다고 평가한다. (자기 자신과 일치하지 않는 유일한 값)
- 하지만 Set 객체는 같다고 평가하여 중복 추가를 허용하지 않는다.
- +0, -0은 일치 비교 연산자와 동일하게 같다고 평가하여 중복 추가를 허용하지 않는다.
- Set 객체는 객체나 배열처럼 JS의 모든 값을 요소로 저장할 수 있다.

```
const set = new Set();

console.log(NaN === NaN); // false
console.log(0 === -0); // true

set.add(NaN).add(NaN);
console.log(set); // Set(1) {NaN}

set.add(0).add(-0);
console.log(set); // Set(2) {NaN, 0}
```

#### 4. 요소 존재 여부 확인

- Set 객체에 특정 요소의 존재 여부를 확인하려면 Set.prototype.has 메서드를 사용한다.
- has 메서드는 특정 요소의 존재 여부를 나타내는 불리언 값을 반환한다.

```
const set = new Set([1, 2, 3]);
console.log(Set.has(2)); // true
console.log(Set.has(4)); // false
```

#### 5. 요소 삭제

- Set 객체의 특정 요소를 삭제하려면 Set.prototype.delete 메서드를 사용한다.
- delete 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환한다.
- 해당 메서드에는 인덱스가 아니라 삭제하려는 **요소 값**을 인수로 전달해야 한다.
- Set 객체에는 순서에 의미가 없다. (인덱스가 없다.)
- 존재하지 않는 요소를 삭제하려 하면 에러 없이 무시한다.
- 불리언 값을 반환하므로 메서드 체이닝을 사용할 수 없다.

```
const set = new Set([1, 2, 3]);

set.delete(2);
console.log(set); // Set(2) {1, 3}

set.delete(1);
console.log(set); // Set(1) {3}
```

#### 6. 요소 일괄 삭제

- Set 객체의 모든 요소를 일괄 삭제하려면 Set.prototype.clear 메서드를 사용한다.
- 언제나 undefined를 반환한다.

```
const set = new Set([1, 2, 3]);
set.clear();
console.log(set); // Set(0) {}
```

#### 7. 요소 순회

- Set 객체의 요소를 순회하려면 Set.prototype.forEach 메서드를 사용한다.
- 해당 메서드는 콜백 함수와 forEach 메서드의 콜백 함수 내부에서 this로 사용될 객체를 인수로 전달한다. 이 때 콜백 함수는 1번째 인수(현재 순회중인 요소 값), 2번째 인수(현재 순회중인 요소 값), 3번째 인수(현재 순회중인 Set 객체 자체)를 전달 받는다.
- Array.prototype.forEach 메서드와 인터페이스를 통일하기 위해 1번째 인수와 2번째 인수를 같은 값으로 사용하는 것이다. (여기서 2번째 인수는 순회 중인 요소의 인덱스를 전달 받지만 Set 객체는 인덱스를 갖지 않는다.)

```
const set = new Set([1, 2, 3]);

set.forEach((v, v2, set) => console.log(v, v2, set));
/*
1 1 set(3) {1, 2, 3}
2 2 set(3) {1, 2, 3}
3 3 set(3) {1, 2, 3}
*/
```

- Set객체는 이터러블이므로 for...of 문으로 순회할 수 있다.
- 스프레드 문법과 배열 구조 분해 할당의 대상이 될 수도 있다.
- 요소의 순서에 의미를 갖진 않지만 다른 이터러블의 순회와 호환성 유지를 위해 순회하는 순서는 요소가 추가된 순서를 따른다.

```
const set = new Set([1, 2, 3]);

console.log(Symbol.iterator in set); // true (이터러블)

for (v of set) {
    console.log(v); // 1 2 3
}

console.log([...set]); // [1, 2, 3]

const [a, ...rest] = set;
console.log(a, rest); // 1, [2, 3]
```

#### 8. 집합 연산

##### 1. 교집합

- 1번째 방식

```
Set.prototype.intersection = function (set) {
    const result = new Set();

    for (const v of set) {
        if (this.has(v)) result.add(v);
    }
    return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.intersection(setB)); // Set(2) {2, 4}
console.log(setB.intersection(setA)); // Set(2) {2, 4}
```

- 2번째 방식

```
Set.prototype.intersection = function (set) {
    return new Set([...this].filter(v => set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.intersection(setB)); // Set(2) {2, 4}
console.log(setB.intersection(setA)); // Set(2) {2, 4}
```

##### 2. 합집합

- 1번째 방식

```
Set.prototype.union = function (set) {
    const result = new Set(this);

    for (const v of set) {
        result.add(v);
    }
    return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
console.log(setB.union(setA)); // Set(4) {2, 4, 1, 3}
```

- 2번째 방식

```
Set.prototype.union = function (set) {
    return new Set([...this, ...set])
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
console.log(setB.union(setA)); // Set(4) {2, 4, 1, 3}
```

##### 3. 차집합

- 1번째 방식

```
Set.prototype.difference = function (set) {
    const result = new Set(this);

    for (const v of set) {
        result.delete(v);
    }
    return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.difference(setB)); // Set(2) {1, 3}
console.log(setB.difference(setA)); // Set(0) {}
```

- 2번째 방식

```
Set.prototype.difference = function (set) {
    return new Set([...this].filter(v => !set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.difference(setB)); // Set(2) {1, 3}
console.log(setB.difference(setA)); // Set(0) {}
```

##### 5. 부분 집합과 상위 집합

- 1번째 방식

```
Set.prototype.isSuperset = function (subset) {
    for (const v of subset) {
        if (!this.has(v)) return false;
    }
    return true;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.isSuperset(setB)); // true
console.log(setB.isSuperset(setA)); // false
```

- 2번째 방식 (every 메서드는 배열의 모든 요소가 주어진 조건을 만족하는지 확인하는 데 사용된다.)

```
Set.prototype.isSuperset = function (subset) {
    const supersetArr = [...this];
    return [...subset].every(v => supersetArr.includes(v));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.isSuperset(setB)); // true
console.log(setB.isSuperset(setA)); // false
```
