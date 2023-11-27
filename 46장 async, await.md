# 46장 제너레이터와 async/await
## 제네레이터
- 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수

<br/>

## 일반 함수와의 차이 
제네레이터 함수는 호출자에게 함수 실행의 제어권 양도 가능 <br/>
- 일반 함수의 경우 함수 호출자는 호출 이후로 제어가 불가능 하지만 제네레이터 함수는 제어 가능
- 함수 제어권을 독점하는 것이 아닌 양도할 수 있다는 것

<br/>

제네레이터 함수는 함수 호출자와 함수의 상태를 주고 받을 수 있다. <br/>
- 일반 함수는 값을 전달받고 반환하는 역할만 하지만 제네레이터 함수는 함수 호출자에게 상태를 전달하거나 호출자로 부터 상태를 전달받을 수도 있다.

<br/>

제네레이터 함수를 호출하면 제네레이터 객체를 반환한다. <br/>
- 일반 함수는 호출 시 함수 코드를 일괄 실행하고 값을 반환
- 제네레이터 함수는 호출 시 함수 코드 실행이 아닌 이터러블이면서 동시에 이터레이터인 제네레이터 객체 반환

<br/>

## 제네레이터 함수의 정의
- function 키워드로 선언
- 하나 이상의 yield 표현식을 포함한다.

```
// 제너레이터 함수 선언문
function* genDecFunc() {
  yield 1;
}

// 제너레이터 함수 표현식
const genExpFunc = function* () {
  yield 1;
}

// 제너레이터 메서드
const obj = {
  * genObjMethod() {
    yield 1;
  }
}

// 제너레이터 클래스 메서드
class MyClass {
  * genClsMethod() {
    yield 1;
  }
}
```

- 애스터리스크(*)의 위치는 function 키워드와 함수 이름 사이 어디든지 가능
- 일관성 유지를 위해 function 키워드 바로 뒤에 붙이는 것을 권장
- 화살표 함수로 저으이 불가
- new 연산자와 함께 생성자 함수로 호출 불가

<br/>

## 제네레이터 객체
- 제네레이터 함수 호출 시 코드 블록 실행이 아닌 제네레이터 객체를 생성해 반환
- Symbol.iterator 메서드를 상속받는 이터러블이면서 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환하는 next 메서드를 소유하는 이터레이터이다.

```
// 제너레이터 함수
function* genFunc() {
  yield 1;
  yield 2;
  yield 3;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환
const generator = genFunc();

// 제너레이터 객체는 이터러블이면서 동시에 이터레이터
// 이터러블은 Symbol.iterator 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체다.
console.log(Symbol.iterator in generator); // true
// 이터레이터는 next 메서드를 갖는다.
console.log('next' in generator) // true
```

- 제너레이터 객체는 next 메서드를 갖는 이터레이터이지만 이터레이터에는 없는 return, throw 메서드를 갖는다.

- 제너레이터 객체의 세 개의 메서드 호출 시 동작 과정
1. next 메서드를 호출하면 제너레이터 함수의 yield 표현식까지 코드 블록을 실행하고 yield된 값을 value 프로퍼티 값으로, false를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
2. return 메서드를 호출하면 인수로 전달받은 값을 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.

```
function genFunc() {
  try {
    yield 1;
    yield 2;
    yield 3;
  } catch (e) {
    console.error(e);
  }
}

const generator = genFunc();

console.log(generator.next()); // {value: 1, done: false}
console.log(generator.return('End!')); // {value: 'End!', done: return}
console.log(generator.throw('Error')); // {value: undefined, done: true}
```

- throw 메서드를 호출하면 인수로 전달받은 에러를 발생시키고 undefined를 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.

<br/>

## 제네레이터의 일시 중지와 재개
- yield 키워드와 next 메서드를 통해 실행을 일시 중지했다가 필요한 시점에 다시 재개 가능
- yield 키워드는 제네레이터 함수를 일시 중지시키거나 yield 키워드 뒤에 오는 표현식 평과 결과를 제네레이터 함수 호출자에게 반환한다.

```
function* genFunc() {
  const x = yield 1;
  
  const y = yield (x + 10);
  
  // 일반적으로 제너레이터의 반환값은 의미가 없다.
  // 따라서 제너레이터에서는 값을 반환할 필요가 없고 return은 종료의 의미로만 사용해야 한다.
  return x + y;
}

const generator = genFunc(0);
let res = generator.next();
console.log(res); // {value: 1, done: false}

res = generator.next(10);
console.log(res); // {value: 20, done: true}

res = generator.next(20);
console.log(res); // {value: 30, done: true}
```

- next 메서드를 호출 시 yield 표현식까지 실행되고 일시 중지 된다. 이후 함수의 제어권이 호출자로 양도된다. 이때 제네레이터 객체의 리절트 객체를 반환한다.
- 제네레이터의 next 메서드는 이터레이터의 next 메서드와 달리 인수 전달이 가능하다. 그리고 이 인수는 제네레이터 함수의 yield 표현식을 할당받는 변수에 할당된다.

<br/>

## 제네레이터의 활용
### 이터러블의 구현

- 무한 피보나치 수열을 생성하는 함수

```
const infiniteFibonacci = (function () {
  let [pre, cur] = [0, 1];
  
  return {
    [Symbol.iterator]() { return this; },
    next() {
      [pre, cur] = [cur, pre + cur];
      // 무한 이터러블이므로 done 프로퍼티 생략
      return { value: cur };
    }
  }
}());

for (const num of infiniteFibonacci) {
  if (num > 10000) break;
  console.log(num); // 1 2 3 5 8 --
}
```

<br/>

- 위의 함수를 아래처럼 사용할 수 있다.

```
const infiniteFibonacci = (function* () {
  let [pre, cur] = [0, 1];
  
  while(true) {
    [pre, cur] = [cur, pre + cur];
    yield cur;
  }
}());
```

<br/>

### 비동기 처리
- 프로미스의 후속 처리 메서드 then/catch/finally 없이 비동기 처리 결과를 반환하도록 구현할 수 있다.

```
const async = generatorFunc => {
  const generator = generatorFunc(); // 2
  
  const onResolved = arg => {
    const result = generator.next(arg); // 5
    
    return result.done ? result.value : result.value.then(res -> onResolved(res));
  }
  return onResolved;
}
```

- 제네레이터 실행기가 필요하다면 직접 구현 보다는 co 라이브러리 사용하기

<br/>

## async/await
- ES8에 도입됨
- 프로미스 기반으로 동작(프로미스 후속 처리 메서드 없이 마치 동기 처리처럼 프로미스가 처리 결과를 반환

```
async function foo(n) { return n; }
foo(1).then(v => console.log(v)); // 1

const obj = {
  async foo(n) { return n; }
}
obj.foo(4).then(v => console.log(v)); // 4
```

### async 함수
- await 키워드는 반드시 async 함수 내부에서 사용해야 한다.
- async 함수는 async 키워드를 사용해 정의하며 언제나 프로미스를 반환
- 명시적으로 프로미스를 반환하지 않아도 암묵적으로 반환값을 resolve하는 프로미스 반환
- class의 construector 메서드는 async 메서드가 될 수 없다.(인스턴스를 반환해서)

<br/>

```
class MyClass {
async constructor() {}
	// SyntaxError
}

const myClass = new MyClass();
```

<br/>

### await 키워드
- 프로미스가 settled 상태가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환
- 반드시 프로미스 앞에 사용해야 한다.
- 여러 개의 프로미스에 await 키워드를 사용하는 것은 주의가 필요하다. 비동기 처리를 앞의 결과가 끝날 때까지 대기하므로 직렬적인 처리가 진행된다.

<br/>

### 에러 처리
- 비동기 처리를 위한 콜백 패턴의 단점 중 가장 심각한 것은 에러 처리가 곤란하다는 것이다. 즉, 콜 스택의 아래방향(실행 중인 컨텍스트가 푸시되기 직전에 푸시된 실행 컨텍스트 방향)으로 전파된다.
- 하지만 비동기 함수의 콜백함수를 호출한 것은 비동기 함수가 아니기 때문에 try ... catch 문을 사용해 에러를 캐치할 수 없다. 하지만 async/await에서 에러 처리는 try ... catch 문을 사용할 수 있다. 콜백
- async 함수 내에서 catch 문을 사용해서 에러 처리를 하지 않으면 async 함수는 발생한 에러를 reject하는 프로미스를 반환한다.

