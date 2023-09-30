# ✏️ 22장 this

## 📌 22.1 this 키워드

- 객체란 상태를 나타내는 프로퍼티와 동작을 나타내는 메서드를 하나의 논리적인 단위로 묶은 복합적 자료구조다.
- 동작을 나타내는 메서드는 자신이 속한 객체의 상태(프로퍼티)를 참조하고 변경할 수 있어야 한다.
- 메서드가 자신이 속한 객체의 상태(프로퍼티)를 참조하려면 먼저 **자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.**

```
const circle = {
    radius: 5,
    getDiameter() {
        return 2 * circle.radius;
    }
};

console.log(circle.getDiameter()); // 10
```

- getDiameter 메서드 내에서 메서드 자신이 속한 객체를 가리키는 식별자 circle을 참조하고 있다.
- 이 참조 표현식이 평가되는 시점은 getDiameter 메서드가 호출돼 함수 몸체가 실행되는 시점이다.
- 객체 리터럴은 circle 변수에 할당되기 직전에 평가된다. 따라서 getDiameter 메서드가 호출되는 시점에는 이미 객체 리터럴의 평가가 완료돼 객체가 생성되었고, circle 식별자에 생성된 객체가 할당된 이후다.
- 따라서 메서드 내부에서 circle 식별자를 참조할 수 있다.
- 하지만 자기 자신이 속한 객체를 재귀적으로 참조하는 방식은 일반적이지도, 바람직하지도 않다.
- 생성자 함수 내부에서는 프로퍼티 또는 메서드를 추가하기 위해 자신이 생성할 인스턴스를 참조할 수 있어야 한다.
- 하지만 생성자 함수에 의한 객체 생성 방식은 먼저 생성자 함수를 정의한 후 new 연산자와 함께 생성자 함수를 호출하는 단계가 필요하다. 즉 생성자 함수로 인스턴스를 생성하려면 생성자 함수가 존재해야 한다.
- 생성자 함수를 정의하는 시점에는 아직 인스턴스를 생성하기 전이라 생성자 함수가 생성할 인스턴스를 가리키는 식별자를 알 수 없다. 따라서 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 식별자가 필요한데, JS는 this라는 특수한 식별자를 제공한다.

this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다.

- this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.
- JS 엔진에 의해 암묵적으로 생성되며 코드 어디에서든 참조할 수 있다.
- 함수를 호출하면 arguments 객체와 this가 암묵적으로 함수 내부에 전달되어 this도 지역 변수처럼 사용할 수 있다.
- this가 가리키는 값, this 바인딩은 **함수 호출 방식에 의해 동적으로 결정된다.**

```
// this를 사용한 객체 리터럴
const circle = {
    radius: 5,
    getDiameter() {
        return 2 * this.radius;
    }
};

console.log(circle.getDiameter()); // 10
```

여기서 this는 메서드를 호출한 객체, circle을 가리킨다.

```
// this를 사용한 생성자 함수
function Circle(radius) {
    this.radius = radius;
}

Circle.prototype.getDiameter = function() {
    return 2 * this.radius;
};

const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```

여기서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.

this는 코드 어디에서든 참조 가능하다. 전역에서도 함수 내부에서도 참조할 수 있다.

- 전역에서 this는 전역 객체 window를 가리킨다.
- 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
- 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
- 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
- 하지만 this는 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이므로 일반적으로 객체의 메서드 내부 또는 생성자 함수 내부에서만 의미가 있다.
- 엄격 모드가 적용된 일반 함수 내부의 this에는 undefined가 바인딩된다. 일반 함수 내부에서 this를 사용할 필요가 없기 때문이다.

## 📌 22.2 함수 호출 방식과 this 바인딩

this 바인딩은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.

- 렉시컬 스코프와 this 바인딩은 결정 시기가 다르다.
- 함수의 상위 스코프를 결정하는 방식인 렉시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프를 결정한다.
- this 바인딩은 함수 호출 시점에 결정된다.

this 바인딩에서 함수를 호출하는 방식으로 다음 4가지가 있다.

- 일반 함수 호출
- 메서드 호출
- 생성자 함수 호출
- Function.prototype.apply/call/bind 메서드에 의한 간접 호출

#### 1. 일반 함수 호출

this에는 전역 객체가 바인딩된다.

- p.347의 예제처럼 전역 함수는 물론, 중첩 함수를 일반 함수로 호출하면 함수 내부의 this에는 전역 객체가 바인딩된다.
- this는 객체의 프로퍼티 또는 메서드를 참조하기 위한 자기 참조 변수로, 객체를 생성하지 않는 일반 함수에서 this는 무의미하다.
- 따라서 위에서 말한 것처럼 엄격 모드에서는 동일한 경우에 this에 undefined가 바인딩된다.
- 콜백 함수가 일반 함수로 호출된다면, 콜백 함수 내부의 this에도 전역 객체가 바인딩된다.
- 즉 어떤 함수라도 일반 함수로 호출되면 this에 전역 함수가 바인딩된다.

```
var value = 1;
const obj = {
    value: 100,
    foo() {
        console.log("foo's this: ", this); // {value: 100, foo: f}
        setTimeout(function () {
            console.log("callback's this: ", this); // window
            console.log("callback's this.value: ", this.value); // 1
        }, 100);
    }
};

obj.foo();
```

- 위 예시처럼 메서드 내에서 정의한 중첩 함수 또는 메서드에게 전달한 콜백 함수(보조 함수)가 일반 함수로 호출될 때, 메서드 내의 중첩 함수 또는 콜백 함수의 this가 전역 객체를 바인딩하는 것은 문제가 있다.
- 중첩 함수 또는 콜백 함수는 외부 함수를 돕는 헬퍼 함수로 외부 함수의 일부 로직을 대신하는 경우가 대부분인데, 외부 함수와 헬퍼 함수의 this가 일치하지 않으면 헬퍼 함수가 제대로 동작하기 어려워진다.
- 따라서 foo() 내부에서 'const that = this'로 this 바인딩을 변수 that에 할당한 후, 콜백 함수 내부에서 this 대신 that을 참조하게 하면 this를 일치시킬 수 있다.
- 이 외에도 Function.prototype.apply/call/bind 메서드로 this를 명시적으로 바인딩하거나 화살표 함수로 this 바인딩을 일치 시킬 수 있다.

#### 2. 메서드 호출

메서드 내부의 this에는 메서드를 호출한 객체, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표 연산자 앞에 기술한 객체가 바인딩된다.
메서드 내부의 this는 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩된다.

```
const person = {
    name: 'Hwang',
    getName() {
        return this.name;
    }
};

consoel.log(person.getName()); // Hwang
```

- getName 메서드는 person 객체의 메서드로 정의되었다. 메서드는 프로퍼티에 바인딩된 함수다.
- person 객체의 getName 프로퍼티가 가리키는 함수 객체는 person 객체에 포함된 게 아니라 독립적으로 존재하는 별도의 객체다. (**이해 안됨**)
- getName 프로퍼티가 가리키는 함수 객체, 즉 getName 메서드는 다른 객체의 프로퍼티에 할당하는 것으로, 다른 객체의 메서드가 될 수도 있고 일반 변수에 할당해 일반 함수로 호출될 수도 있다. (p.351 참고)
- 메서드 내부의 this는 프로퍼티로 메서드를 가리키고 있는 객체와는 관련이 없고, 메서드를 호출한 객체에 바인딩된다.
- 프로토타입 메서드 내부에서 사용된 this도 일반 메서드와 동일하게 해당 메서드를 호출한 객체에 바인딩된다.

#### 3. 생성자 함수 호출

생성자 함수 내부의 this에는 생성자 함수가 미래에 생성할 인스턴스가 바인딩된다.

- 생성자 함수란 객체를 생성하는 함수다.
- 일반 함수와 동일하게 생성자 함수를 정의하고, new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다. 만약 new 연산자와 함께 생성자 함수를 호출하지 않으면 일반 함수로 동작한다. (p.353 참고)

#### 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출

apply, call, bind 메서드는 Function.prototype의 메서드로, 모든 함수가 상속 받아 사용할 수 있다.

- 해당 메서드들은 this로 사용할 객체와 인수 리스트를 인수로 전달 받아 함수를 호출한다.
- apply 메서드의 사용법은 아래와 같다.
  - 파라미터: 1) this로 사용할 객체 2) 함수에게 전달할 인수 리스트의 배열 또는 유사 배열 객체
  - 리턴값: 호출된 함수의 반환 값
- call 메서드의 사용법은 아래와 같다.
  - 파라미터: 1) this로 사용할 객체 2) 함수에게 전달할 리스트 (쉼표로 구분)
  - 리턴값: 호출된 함수의 반환 값
- 두 메서드의 본질적인 기능은 함수를 호출하는 것으로, 함수를 호출하면서 첫 번째 인수로 전달할 특정 객체를 호출한 함수의 this에 바인딩한다.
- 부가적으로 argument 같은 유사 배열 객체에 배열 메서드를 사용할 때에도 사용할 수 있다.

```
function getThisBindings() {
    return this;
}

const thisArg = {a: 1};
console.log(getThisBindings()); // window

console.log(getThisBindings.apply(thisArg)); // {a: 1}
console.log(getThisBindings.call(thisArg)); // {a: 1}
```

- bind 메서드는 함수를 호출하지 않는다.
- 첫 번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성해 반환한다.
- 따라서 새롭게 생성된 함수를 명시적으로 호출해야 한다.

```
function getThisBinding() {
    return this;
}

const thisArg = {a: 1};
console.log(getThisBinding.bind(thisArg)); // getThisBinding
console.log(getThisBinding.bind(thisArg)()); // {a: 1}
```

```
const person = {
    name: 'Hwang',
    foo(callback) { // *1*
        setTimeout(callback, 100);
    }
};

person.foo(function() {
    console.log(`Hi! my name is ${this.name}.`); // Hi! my name is .(전역 객체 window의 name로 *2*)
});
```

- person.foo의 콜백 함수가 호출되기 전인 1의 시점에서 this는 person 객체를 가리킨다.
- 그러나 2의 시점에서 this는 전역 객체 window를 가리킨다.
- 외부 함수 person.foo를 돕는 헬퍼 함수인 콜백 함수와 this가 상이하여 문제가 발생할 수 있기 때문에 아래와 같이 bind 메서드로 this를 일치 시킨다.

```
const person = {
    name: 'Hwang',
    foo(callback) { // *1*
        setTimeout(callback.bind(this), 100);
    }
};

person.foo(function() {
    console.log(`Hi! my name is ${this.name}.`); // Hi! my name is Hwang.
});
```
