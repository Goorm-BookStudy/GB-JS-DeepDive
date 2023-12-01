# ✏️ 46장 제너레이터와 async/await

## 📌 46.1 제너레이터란?

- ES6에서 도입된 제너레이터는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개하는 특수한 함수다.
- 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.
  - 일반 함수를 호출하면 제어권이 함수에게 넘어가고 함수 코드를 일괄 실행한다.
  - 제너레이터 함수는 함수 실행을 함수 호출자가 제어한다.
  - 함수의 제어권을 함수 호출자에게도 양도할 수 있음을 의미한다.
- 제너레이터 함수는 함수 호출자와 함수의 상태를 주고 받을 수 있다.
  - 일반 함수를 호출하면 함수가 실행되는 동안 함수 외부에서 함수 내부로 값을 전달, 함수의 상태를 바꿀 수 없다.
  - 제너레이터 함수는 함수 호출자에게 상태를 전달할 수 있고, 함수 호출자로부터 상태를 전달 받을 수도 있다.
- 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
  - 일반 함수를 호출하면 함수 코드를 일괄 실행 후 값을 반환한다.
  - 제너레이터 함수를 호출하면 이터러블이면서 동시에 이터레이터인 제너레이터 객체를 반환한다.

## 📌 46.2 제너레이터 함수의 정의

- function\* 키워드로 선언한다.
- 하나 이상의 yield 표현식을 포함한다.
- \*의 위치는 function 키워드와 함수 이름 사이라면 어디든 상관 없다.
- 하지만 일관성을 위해 function 키워드 바로 뒤에 붙이는 것을 권장한다.
- 제너레이터 함수는 화살표 함수로 정의할 수 없다.
- 또한 new 연산자와 함께 생성자 함수로 호출할 수 없다.

```
// 제너레이터 함수 선언문
function* getDecFunc() {
    yield 1
}

// 제너레이터 함수 표현식
const genExpFunc = function* () {
    yield 1
}

// 제너레이터 매서드
const obj = {
    * getObjMethod(){
        yield 1
    }
};

// 제너레이터 클래스 메서드
class MyClass {
    *getClsMethod(){
        yield 1
    }
}
```

## 📌 46.3 제너레이터 객체

- 제널이터 함수가 반환하는 제너레이터 객체는 이터러블이면서 동시에 이터레이터다.
- Symbol.iterator 메서드를 상속 받는 이터러블이면서, value/done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환하는 next 메서드를 소유하는 이터레이터다.
- 제너레이터 객체는 next 메서드를 가지는 이터레이터이므로 Symbol.iterator 메서드를 호출해 별도 이터레이터를 만들 필요가 없다.

```
function* getFunc(){
    yield 1
    yield 2
    yield 3
}

const generator = getFunc();

console.log(Symbol.iterator in generator); // true (이터러블이면서 동시에 이터레이터다.)
console.log('next' in generator); //true (next 메서드를 갖는다.)
```

- 제너레이터 객체는 next 메서드를 갖는 이터레이터지만 이터레이터에 없는 return, throw 메서드를 갖는다.
- next 메서드 호출 시 제너레이터 함수의 yield 표현식까지 코드 블록 실행 후 yield된 값을 value 프로퍼티 값으로, false를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
- return 메서드를 호출하면 인수로 전달 받은 값을 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.

```
function* getFunc(){
    try{
        yield 1
        yield 2
        yield 3
    }catch(e){
        console.error(e);
    }
}
const generator = getFunc();
console.log(generator.next()); // {value: 1, done: false}
console.log(generator.return('끝!')) // {value: '끝!', done: true}
```

- throw 메서드를 호출하면 인수로 전달받은 에러를 발생시키고 undefined를 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.

```
function* getFunc(){
    try{
        yield 1
        yield 2
        yield 3
    }catch(e){
        console.error(e);
    }
}
const generator = getFunc();
console.log(generator.next()); // {value: 1, done: false}
console.log(generator.throw('에러')) // {value: undefined, done: true}
```

## 📌 46.4 제너레이터의 일시 중지와 재개

- 제너레이터 함수를 호출하면 이터러블 + 이터레이터면서 next 메서드를 갖는 제너레이터 객체를 반환한다.
- 제너레이터 객체의 next 메서드를 호출하면 제너레이터 함수의 코드 블록을 실행한다.
- yield 키워드는 제너레이터 함수의 실행을 일시 중지 시키거나 뒤에 오는 표현식의 평가 결과를 함수 호출자에게 반환한다.

```
function* genFunc() {
  yield 1;
  yield 2;
  yield 3;
}

const generator = genFunc();

console.log(generator.next()); // {value: 1, done: false}
console.log(generator.next()); // {value: 2, done: false}
console.log(generator.next()); // {value: 3, done: false}
console.log(generator.next()); // {value: undefined, done: true}
```

- 제너레이터 객체의 next 메서드는 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다.
- 이 객체의 value 프로퍼티에는 yield 표현식에서 yield된 값이 할당, done 프로퍼티에는 끝까지 함수가 실행되었는지 불리언 값이 할당된다.
- 제너레이터 객체의 next 메서드에는 인수를 전달할 수 있다. 전달된 인수는 yield 표현식을 할당받는 변수에 할당된다.

```
function* genFunc() {
  const x = yield 1;
  const y = yield (x + 10);
  return x + y;
}

const generator = genFunc(0);

let res = generator.next();
console.log(res); {value: 1, done: false}

let res = generator.next(10);
console.log(res); {value: 20, done: false}

let res = generator.next(20);
console.log(res); {value: 30, done: true}
```

## 📌 46.5 제너레이터의 활용

#### 1. 이터러블의 구현

- 제너레이터 함수로 간단하게 이터러블을 구현할 수 있다.

```
// 이터레이션 프로토콜 준수
const infiniteFibonacci = (function () {
  let [pre, cur] = [0, 1];

  return {
    [Symbol.iterator]() { return this; },
    next() {
      [pre, cur] = [cur, pre + cur];

      return {value: cur};
    }
  };
}());

for (const num of infiniteFibonacci) {
  if (num > 10000) break;
  console.log(num); // 1 2 3 5 8...
}
```

```
// 제너레이터로 구현
const infiniteFibonacci = (function* () {
  let [pre, cur] = [0, 1];

  while (true) {
    [pre, cur] = [cur, pre + cur];
    yield cur;
  }
}());

for (const num of infiniteFibonacci) {
  if (num > 10000) break;
  console.log(num); // 1 2 3 5 8...
}
```

#### 2. 비동기 처리

- 함수 호출자와 함수 상태를 주고 받는 특성을 활용하면 프로미스를 사용한 비동기 처리를 동기 처리처럼 구현할 수 있다.
- 프로미스의 후속 처리 메서드 없이 비동기 결과 처리를 반환하도록 구현할 수 있다.

```
const fetch = require('node-fetch');

const async = generatorFunc => {
    const generator = generatorFunc(); (2)

    const onResolved = arg => {
        const result = generator.next(arg); (5)

        return result.done
            ?result.value :
            result.value.then(res => onResolved(res)); (7)

    }
    return onResolved; (3)
}

(async(function * fetchTodo(){ (1)
    const url = 'https://jsonplaceholder.typicode.com/post/1';

    const res = yield fetch(url); (6)
    const todo = yield response.json(); (8)
    console.log(todo);
})()) (4)
```

## 📌 46.6 async/await

- 위 예제를 async/await을 사용하면 더 간단하게 다시 구현할 수 있다.

```
const fetch = require('node-fetch');

async function fetchTodo() {
  const url = 'https://jsonplaceholder.typicode.com/todos/1';
  const response = await fetch(url);
  const todo = await response.json();
  console.log(todo);
}

fetchTodo();
```

#### 1. async 함수

- await 키워드는 반드시 async 함수 내부에서 사용해야 한다.
- asncy 함수는 해당 키워드를 정의해 언제나 프로미스를 반환한다.
- 명시적으로 프로미스를 반환하지 않아도 암묵적으로 반환값을 resolve하는 프로미스를 반환한다.
- 클래스의 constructor 메서드는 async 메서드가 될 수 없다. (클래스의 메서드는 인스턴스를 반환 / async 함수는 늘 프로미스 반환)

```
// 함수 선언문
async function foo(n) {return n;}
foo(1).then(v=> console.log(v)) // 1

// async 함수 표현식
const bar = async function(n) {return n};
bar(2).then(v => console.log(v)); // 2

// async 화살표 함수
const baz = async n => n;
baz(3).then(v=>console.log(v)) // 3

// async 메서드
const obj = {
    async foo(n) {return n;}
};
obj.foo(4).then(v => console.log(v)); // 4

// async 클래스 메서드
class MyClass {
    async bar(n) {return n;}
}
const myClass = new MyClass();
myClass.bar(5).then(v => console.log(v)) // 5
```

#### 2. await 키워드

- await 키워드는 프로미스가 settled 상태 (비동기 처리가 수행된 상태)가 될 때까지 대기하다 settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환한다.
- 반드시 프로미스 앞에서 사용해야 한다.

```
const getGitUserName = async id => {
    const res = await fetch(`https://api.github.com/users/${id}`); (1)
    const { name } = await res.json(); (2)
    console.log(name); // Ungmo Lee
};

getGitUserName('ungmo2');
```

- 프로미스가 settled 상태가 되면 프로미스가 resolve한 처리 결과가 res 변수에 할당된다.
- await 키워드는 다음 실행을 일시 중지했다가 프로미스의 상태에 따라 재개한다.
- 아래의 경우 약 6초 소요된다.

```
async function foo() {
    const a = await new Promise(resolve => setTimeout(() => resolve(1), 3000));
    const b = await new Promise(resolve => setTimeout(() => resolve(2), 2000));
    const c = await new Promise(resolve => setTimeout(() = >resolve(3), 1000));

    console.log([a, b, c]); // [1, 2, 3]
}

foo();
```

- 위 예제의 경우 3개의 비동기 처리가 서로 연관이 없기에 아래와 같이 수정한다.

```
async function foo() {
    const res = await Promise.all([
        new Promise(resolve => setTimeout(() => resolve(1), 3000)),
        new Promise(resolve => setTimeout(() => resolve(2), 2000)),
        new Promise(resolve => setTimeout(() => resolve(3), 1000))
    ])

    console.log(res); // [1, 2, 3]
}

foo();
```

- 반면 아래의 예제는 비동기 처리의 처리 순서가 보장되어야 하므로 모든 프로미스에 await 키워드를 넣어 순차적으로 처리한다.

```
async function foo() {
    const a = await new Promise(resolve => setTimeout(() => resolve(n), 3000));
    const b = await new Promise(resolve => setTimeout(() => resolve(a+1), 2000));
    const c = await new Promise(resolve => setTimeout(() => resolve(b+1), 1000))
    console.log([a, b, c]); // [1, 2, 3]
}

bar(1);
```

#### 3. 에러 처리

- try... catch문으로 에러를 처리한다.
- 콜백 함수를 인수로 전달 받는 비동기 함수(호출자가 없음)와 달리 프로미스를 반환하는 비동기 함수는 명시적으로 호출할 수 있기에 호출자가 명확하다.
  - 비동기 함수의 콜백 함수를 호출한 것은 비동기 함수가 아니라서 에러를 캐치할 수 없었음

```
const fetch = require('node-fetch');
const foo = async () => {
    try {
        const wrongUrl = 'https://잘못된url';

        const response = await fetch(wrongUrl);
        const data = await response.json();
        console.log(data);
    } catch(err){
        console.error(err); // 에러 메세지
    }
};

foo();
```

- async 함수 내에서 catch문으로 에러 처리를 하지 않으면 발생한 에러를 reject하는 프로미스를 반환한다.

```
const fetch = require('node-fetch');

const foo = async() => {
    const wrongUrl = 'https://잘못된url';

    const response = await fetch(wrongUrl);
    const data = await response.json();
    return data;
};

foo()
.then(console.log)
.catch(console.error) // TyepError : Failed to fetch
```
