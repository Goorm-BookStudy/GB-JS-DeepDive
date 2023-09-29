# this
- 동작을 나타내는 메서드는 자신이 속한 객체의 프로퍼티를 참조하고 변경할 수 있어야 한다. 이를 위해서는 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.
- 객체 리터럴 방식으로 생성한 객체의 경우 메서드 내부에서 자신이 속한 객체를 가리키는 식별자를 재귀적으로 참조할 수 있다. ( 호출되는 시점에 이미 객체 리터럴의 평가가 완료되어 객체가 생성되고 메서드 내부에서 식별자로 참조 가능하다.
- 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수
- this를 통해 자신이 속한 객체 or 자신이 생성할 인스턴스의 프로퍼티나 메서드 참조 가능
- JS 엔진에 의해 암묵적 생성된다 ( 함수를 호출하면 arguments 객체와 this가 암묵적으로 함수 내부에 전달)
- this 바인딩(식별자와 값을 연결하는 과정)은 함수 호출 방식에 의해 동적 결정

```
// 객체 리터럴
const circle = {
	radius: 5,
    getDinameter() {
    	// this는 메서드를 호출한 객체를 가리킨다.
        return 2 * this.radius;
    }
};
```

- 객체 리터럴의 메서드 내부에서 this는 메서드를 호출한 객체(circle)을 가리킨다.

```
function Circle(radius){
	// this는 생성자 함수가 생성할 인스턴스를 가리킨다.
	this.radius = radius;
}

Circle.prototype.getDiameter = function() {
	// this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    return 2 * this.radius;
};

// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter);
```

- 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다. 
- 자바, C++ 같은 클래스 기반 언어에서 this는 언제나 클래스가 생성하는 인스턴스를 가리킨다.
- strict mode(엄격 모드) 역시 this 바인딩에 영향을 준다. (적용된 일반 함수 내부에 this에는 undefined가 바인딩 된다.)
- 전역에서도 함수 내부에서도 참조 가능하다. (전역 this의 경우 전역 객체 window를 가리킨다.)

<br/>

### 함수 호출 방식과 this바인딩
- 렉시컬 스코프와 this 바인딩은 결정 시기가 다르다. (렉시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프 결정, this는 함수 호출 시점에 결정)

<br/>

## 일반 함수 호출
- 기본적으로 this에는 전역 객체가 바인딩 된다.
- 변수 객체를 생성하지 않는 일반 함수에서 this는 의미가 없다. 일반 함수 -> 전역 객체 바인딩
- var을 이용해 전역 변수로 선언할 경우 this로 사용 가능하다. ( 전역 객체에 바인딩 되기 때문에)

- 중첩 함수, 콜백 함수도 포함된지만 콜백 함수의 this 바인딩을 메서드의 this 바인딩과 일치시키는 방법

```
var value = 1;

const obj = {
	value: 100,
    foo() {
    	// this 바인딩(obj)을 변수 that에 할당
        const that = this;
        
        // 콜백 함수 내부에서 this 대신 that을 참조한다.
        setTimeout(function(){
        	console.log(that.value); // 100
        }, 100);
    }
};

obj.foo();
```

- Function.prototype.apply, Function.prototype.call, Function.prototype.bind 메서드를 사용해 명시적으로 this 바인딩도 가능하다.
- 화살표 함수를 사용할 경우 상위 스코프의 this를 가리키기 때문에 또다른 방법이 된다.

<br/>

## 메서드 호출
- 메서드 내부의 this에는 메서드를 호출할 때 메서드 이름 앞의 마침표(.) 연산자 앞에 기술한 객체가 바인딩 된다.

```
const person = {
	name: 'Lee',
    getName() {
    	// 메서드 내부의 this는 메서드를 호출한 객체에 바인딩
        return this.name;
    }
};

// 메서드 getName을 호출한 객체는 person이다.
console.log(person.getName()); // Lee
```
- getName 메서드의 경우 다른 객체의 프로퍼티에 할당하는 것으로 객체의 메서드가 될 수도 있고 일반 변수에 할당하여 일반 변수로 호출 될 수도 있다. person 객체에 포함된 것이 아닌 독립적으로 존재하는 별도의 객체이다. getName 프로퍼티가 함수 객체를 가리키고 있을 뿐이다.
- 메서드 내부의 this는 프로퍼티로 메서드를 가리키고 있는 객체와는 관계가 없고 메서드를 호출한 객체에 바인딩 된다.
- 프로토타입 메서드 내부에서 사용된 this도 일반 메서드와 마찬가지로 해당 메서드를 호출한 객체에 바인딩

<br/>

## 생성자 함수 호출
- 생성자 함수 내부의 this에는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩 된다.

```
// 생성자 함수
function Circle(radius) {
	// 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    this.radius = radius;
    this.getDiameter = function () {
    	return 2 * this.radius;
    };
}

const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

<br/>

## Function.prototype.apply/call/bind 메서드에 의한 간접 호출
- apply, call, bind 메서드는 Function.prototype의 메서드다. 이 메서드는 모든 함수가 상속받아 사용 가능하고 this로 사용할 객체와 인수 리스트를 인수로 전달 받아 함수를 호출한다.
- apply와 call 메서드의 본질적인 기능은 함수를 호출하는 것 함수를 호출하면 첫 번째 인수로 전달한 특정 객체를 호출한 함수의 this에 바인딩한다.
- apply 메서드는 호출할 함수의 인수를 배열로 묶어서 전달하고 call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다.
- bind 메서드는 apply와 call 메서드와 달리 함수를 호출하지 않고 첫 번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성한다.

```
function getThisBinding() {
	return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// bind 메서드의 this로 this 바인딩이 교체된 getThisBinding 함수를 새롭게 생성해 반환한다.
// bind 메서드는 함수를 호출하지 않으므로 명시적 호출이 필요하다.
console.log(getThisBinding.bind(thisArg)()); // {a: 1}
```

- bind 메서드는 메서드의 this와 메서드 내부의 중첩 함수 또는 콜백 함수의 this가 불일치하는 문제를 해결할 때 유용하다.


```
const person = {
	name: 'Lee',
    foo(callback) {
    	// bind 메서드로 callback 함수 내부의 this 바인딩을 전달
        setTimeout(callback.bind(this), 100);
    }
};

person.foo(function() {
	console.log(`hi my name is ${this.name}.`); // hi my name is Lee
}
```

- 일반함수로 호출되어 전역 객체에 바인딩 되어야 하지만 bind 메서드로 바인딩 시켜준다.
- bind 메서드를 사용하지 않았다면 foo 메서드를 호출했을 때 전역 객체에서의 window.name이 호출되게 된다.

<br/><br/>
