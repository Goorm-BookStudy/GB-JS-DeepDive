# 스프레드 문법

하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 목록으로 만든다.

- 스프레드 문법을 사용할 수 있는 대상
    - Array , String , Map , Set , DOM 컬렉션( NodeList , HTMLCollection ), arguments
- 스프레드 문법의 결과는 변수에 할당할 수 없다.

## 1. 함수 호출문 인수에서 사용

```jsx
const arr = [1 ,  2 ,  3];
// 스프레드 문법을 사용하여 배열 arr을 1, 2, 3으로 펼쳐서 Math.max에 전달한다.
// Math.max(...[1, 2, 3])은 Math.max(1, 2, 3)과 같다.
const max = Math . max(...arr); // 3
```

<aside>
⚠️ Rest 파라미터와 형태가 동일하여 혼동할 수 있으므로 주의

`Rest 파라미터`는 함수에 전달된 인수들의 목록을 배열로 전달받기 위해 매개변수 이름 앞에 ... 을 붙이는 것이다. 

`스프레드 문법`은 여러 개의 값이 하나로 뭉쳐 있는 배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 만드는 것이다. 

따라서 Rest 파라미터와 스프레드 문법은 서로 반대의 개념이다.

</aside>

```jsx
// Rest 파라미터는 인수들의 목록을 배열로 전달받는다.
function foo(...rest) { 
   console . log(rest); // 1, 2, 3 ➡️ [ 1, 2, 3 ] 
}
// 스프레드 문법은 배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 만든다.
// [1, 2, 3] ➡️ 1, 2, 3 
foo(...[1 , 2 , 3]);
```

## 2. 배열 리터럴 내부에서 사용

### 2-1. concat

 ES5

```jsx
var arr = [1 ,  2] . concat([3 ,  4]);
console . log(arr); // [1, 2, 3, 4]
```

ES6

```jsx
const arr = [...[1 , 2] , ...[3 , 4]];
console . log(arr); // [1, 2, 3, 4]
```

### 2-2. splice

ES5

```jsx
var arr1 = [1 , 4];
var arr2 = [2 , 3];
// 세 번째 인수 arr2를 해체하여 전달해야 한다.
// 그렇지 않으면 arr1에 arr2 배열 자체가 추가된다.
arr1 . splice(1 , 0 , arr2);
console . log(arr1); // [1, [2, 3], 4]
```

ES6

```jsx
const arr1 = [1 ,  4];
const arr2 = [2 ,  3];
arr1 . splice(1 ,  0 ,  ...arr2);
console . log(arr1); // [1, 2, 3, 4]
```

### 2-3. 배열 복사 (slice)

ES5

```jsx
var origin = [1 ,  2];
var copy = origin . slice();
console . log(copy); // [1, 2] 
console . log(copy === origin); // false
```

ES6

```jsx
const origin = [1 ,  2];
const copy = [...origin];
console . log(copy); // [1, 2] 
console . log(copy === origin); // false
```

### 2-4. 이터러블을 배열로 변환

```jsx
function sum() {
// 이터러블이면서 유사 배열 객체인 arguments를 배열로 변환
return [...arguments] . reduce((pre , cur) => pre + cur , 0);
}
console . log(sum(1 , 2 , 3)); // 6
```

<aside>
💡 이터러블이 아닌 유사 배열 객체를 배열로 변경하려면 `Array . from` 메서드를 사용한다.

</aside>

```jsx
// Array.from은 유사 배열 객체 또는 이터러블을 배열로 변환한다.
Array . from(arrayLike); // [1, 2, 3]
```

## 3. 객체 리터럴 내부에서 사용

```jsx
// 스프레드 프로퍼티 // 객체 복사(얕은 복사)
const obj = { x: 1 , y: 2 };
const copy = { ...obj };
console . log(copy); // { x: 1, y: 2 } 
console . log(obj === copy); // false
// 객체 병합
const merged = { x: 1 , y: 2 , ...{ a: 3 , b: 4 } };
console . log(merged); // { x: 1, y: 2, a: 3, b: 4 }
```

```jsx
// 객체 병합. 프로퍼티가 중복되는 경우 뒤에 위치한 프로퍼티가 우선권을 갖는다.
const merged = { ...{ x: 1 ,  y: 2 } ,  ...{ y: 10 ,  z: 3 } };
console . log(merged); // { x: 1, y: 10, z: 3 }
// 특정 프로퍼티 변경
const changed = { ...{ x: 1 ,  y: 2 } ,  y: 100 };
// changed = { ...{ x: 1, y: 2 }, ...{ y: 100 } }
console . log(changed); // { x: 1, y: 100 }

// 프로퍼티 추가 const added = { ...{ x: 1 ,  y: 2 } ,  z: 0 };
// added = { ...{ x: 1, y: 2 }, ...{ z: 0 } }
console . log(added); // { x: 1, y: 2, z: 0 }
```
