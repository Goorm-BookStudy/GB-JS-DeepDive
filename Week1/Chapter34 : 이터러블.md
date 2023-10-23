# 34. 이터러블

## 1. 이터레이션 프로토콜

순회 가능한 iterable 데이터 컬렉션(자료구조)을 만들기 위해 ECMAScript 사양에 정의하여 미리 약속한 규칙

순회 가능한 데이터 컬렉션 (배열, 문자열, 유사 배열 객체, DOM 컬렉션 등)은

- ES6 이전
    - **통일된 규약 없이** for 문, for...in 문, forEach 메서드 등 다양한 방법으로 순회할 수 있었다.
- ES6
    - **이터레이션 프로토콜을 준수하는 이터러블로 통일**하여 for...of문, 스프레드 문법, 배열 디스트럭처링 할당의 대상으로 사용할 수 있도록 일원화했다.

이터레이션 프로토콜에는 이터러블 프로토콜과 이터레이터 프로토콜이 있다.

1. 이터러블 프로토콜
    
    ```
     * 조건
       1. Symbol 중 Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현
       2. 프로토타입 체인을 통해 상속 받은 Symbol.iterator 메서드를 호출
    
     * 결과
       1. 이터레이터 프로토콜을 준수한 이터레이터를 반환한다.
    
    ```
    
- 이러한 규약을 이터러블 프로토콜이라 하며, 이터러블 프로토콜을 준수한 객체를 `이터러블`이라 한다.
- 이터러블은 for...of 문으로 순회할 수 있으며 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.
1. 이터레이터 프로토콜
    
    ```
     * 조건
       1. 이터러블의 Symbol.iterator 메서드를 호출
    
     * 결과
      1. 이터레이터 프로토콜을 준수한 이터레이터를 반환한다.
      2. next 메서드를 소유하며 next 메서드를 호출하면 이터러블을 순회하며 value와 done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다.
    
    ```
    
- 이러한 규약을 이터레이터 프로토콜이라 하며, 이터레이터 프로토콜을 준수한 객체를 `이터레이터`라 한다.
- 이터레이터는 **이터러블의 요소를 탐색**하기 위한 포인터 역할을 한다.
- <img width="711" alt="image" src="https://github.com/Javascript-sc/javascript-deepdive/assets/98210662/4a06fbeb-d692-400b-9c76-464b62deec49">

<aside>
💡 iterator == 순회 도우미

이 단어는 iterator의 역할이 **데이터 구조나 컬렉션의 순회를 도와주는 것**이기 때문에 적절하게 표현할 수 있습니다.
'순회 도우미'는 데이터를 반복적으로 접근하고 처리하는 데 도움을 주는 객체 또는 개념으로 사용될 수 있습니다.
이러한 도우미는 순회 로직을 추상화하고 반복적인 작업을 효율적으로 수행할 수 있도록 합니다.

</aside>

### 1-1. 이터러블

`이터러블 프로토콜`을 준수한 객체를 이터러블이라 한다. 

즉, 이터러블은 Symbol.iterator 를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체를 말한다.

- 이터러블인지 확인하는 함수 구현하기
    
    ```jsx
    const isIterable = v => v !== null && typeof v[Symbol . iterator] === 'function'; // 배열, 문자열, Map, Set 등은 이터러블이다.
    isIterable([]); //true
    isIterable(''); //true
    isIterable(new Map()); //true
    isIterable(new Set()); //true
    isIterable({}); //false
    ```
    
    배열 이터러블 확인
    
    ```jsx
    const array = [1 , 2 , 3];
    
    // 배열은 Array.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다.
    console . log(Symbol . iterator in array); // true
    
    // 이터러블인 배열은 for...of 문으로 순회 가능하다.
    for (const item of array) { 
    console . log(item);
    }
    
    // 이터러블인 배열은 스프레드 문법의 대상으로 사용할 수 있다.
    console . log([...array]); // [1, 2, 3]
    
    // 이터러블인 배열은 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.
    const [a , ...rest] = array;
    console . log(a , rest); // 1, [2, 3]
    ```
    

### 1-2. 이터레이터

이터러블의  Symbol . iterator  메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다. 

이터러블의  Symbol . iterator  메서드가 반환한 이터레이터는 next 메서드를 갖는다.

```jsx
// 배열은 이터러블 프로토콜을 준수한 이터러블이다.
const array = [1 , 2 , 3];

// Symbol.iterator 메서드는 이터레이터를 반환한다.
const iterator = array[Symbol . iterator]();

// Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 갖는다.
console . log('next' in iterator); // true
```

이터레이터의 next 메서드는 이터러블의 각 요소를 순회하기 위한 포인터의 역할을 한다. 

즉, next 메서드를 호출하면 이터러블을 순차적으로 한 단계씩 순회하며 순회 결과를 나타내는 이터레이터 리절트 객체를 반환한다.

```jsx
// 배열은 이터러블 프로토콜을 준수한 이터러블이다.
const array = [1 , 2 , 3];

// Symbol.iterator 메서드는 이터레이터를 반환한다. 이터레이터는 next 메서드를 갖는다.
const iterator = array[Symbol . iterator]();

// next 메서드를 호출하면 이터러블을 순회하며 순회 결과를 나타내는 이터레이터 리절트 객체를 반환한다.
// 이터레이터 리절트 객체는 value와 done 프로퍼티를 갖는 객체다.
console . log(iterator . next()); // { value: 1, done: false } 
console . log(iterator . next()); // { value: 2, done: false } 
console . log(iterator . next()); // { value: 3, done: false } 
console . log(iterator . next()); // { value: undefined, done: true }
```

- value 프로퍼티는 현재 순회 중인 이터러블의 값
- done 프로퍼티는 이터러블의 순회 완료 여부

## 2. 빌트인 이터러블

<img width="469" alt="image" src="https://github.com/Javascript-sc/javascript-deepdive/assets/98210662/85e57ab6-0a8a-4c57-b2c9-fef781413a8a">

<img width="480" alt="image" src="https://github.com/Javascript-sc/javascript-deepdive/assets/98210662/0f919634-121d-4652-93d0-fda65b991457">


## 3. for…of 문

`for (변수선언문 of 이터러블) { ... }`

- for...in 문
    - 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 [[Enumerable]] 의 값이 true 인 프로퍼티를 순회하며 열거 enumeration 한다. 이때 프로퍼티 키가 심벌인 프로퍼티는 열거하지 않는다.
- for...of 문
    - 내부적으로 이터레이터의 next 메서드를 호출하여 이터러블을 순회하며 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티 값을 for...of 문의 변수에 할당한다.
    - 그리고 이터레이터 리절트 객체의 done 프로퍼티 값이 false 이면 이터러블의 순회를 계속하고 true 이면 이터러블의 순회를 중단한다.

```jsx
for (const item of [1 , 2 , 3]) {
// item 변수에 순차적으로 1, 2, 3이 할당된다.
console . log(item); // 1 2 3
}
```

- 내부 동작 설명
    
    ```jsx
    // 이터러블
    const iterable = [1 , 2 , 3];
    
    // 이터러블의 Symbol.iterator 메서드를 호출하여 이터레이터를 생성한다.
    const iterator = iterable[Symbol . iterator]();
    
    for (;;) { // 이터레이터의 next 메서드를 호출하여 이터러블을 순회한다.
    
    // 이때 next 메서드는 이터레이터 리절트 객체를 반환한다.
    const res = iterator . next();
    
    // next 메서드가 반환한 이터레이터 리절트 객체의 done 프로퍼티 값이 true이면 이터러블의 순회를 중단한다.
    if (res . done) break;
    
    // 이터레이터 리절트 객체의 value 프로퍼티 값을 item 변수에 할당한다.
    const item = res . value;
    
    console . log(item); // 1 2 3
    }
    ```
    

## 4. 유사 배열 객체

마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고 length 프로퍼티를 갖는 객체를 말한다. 

유사 배열 객체는 length 프로퍼티를 갖기 때문에 for 문으로 순회할 수 있고, 

인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로 가지므로 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있다.

```jsx
// 유사 배열 객체
const arrayLike = { 
0: 1 ,
1: 2 ,
2: 3 ,
length: 3 
};

// 유사 배열 객체는 length 프로퍼티를 갖기 때문에 for 문으로 순회할 수 있다.
for (let i = 0; i < arrayLike . length; i++) {
// 유사 배열 객체는 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있다.
console . log(arrayLike[i]); // 1 2 3
}
```

유사 배열 객체는 이터러블이 아닌 **일반 객체**다. 

따라서 유사 배열 객체에는 Symbol . iterator 메서드가 없기 때문에 for...of 문으로 순회할 수 없다.

```jsx
// 유사 배열 객체는 이터러블이 아니기 때문에 for...of 문으로 순회할 수 없다.
for (const item of arrayLike) { 
console . log(item); // 1 2 3
}
// TypeError: arrayLike is not iterable
```

<aside>
💡 ES6 에서 이터러블이 도입되면서 유사 배열 객체인 arguments , NodeList , HTMLCollection 객체에 Symbol.iterator 메서드를 구현하여 이터러블이 되었다.

하지만 이터러블이 된 이후에도 length 프로퍼티를 가지며 인덱스로 접근할 수 있는 것에는 변함이 없으므로 유사 배열 객체이면서 이터러블인 것이다.

배열도 마찬가지로 ES6에서 이터러블이 도입되면서 Symbol . iterator 메서드를 구현하여 이터러블이 되었다.

</aside>

`Array . from` 메서드는 유사 배열 객체 또는 이터러블을 인수로 전달받아 배열로 변환하여 반환한다.

```jsx
// 유사 배열 객체
const arrayLike = { 
0: 1 ,
1: 2 ,
2: 3 ,
length: 3 
};

// Array.from은 유사 배열 객체 또는 이터러블을 배열로 변환한다.
const arr = Array . from(arrayLike);
console . log(arr); // [1, 2, 3]
```

## 이터레이션 프로토콜의 필요성

만약 다양한 데이터 공급자가 각자의 순회 방식을 갖는다면 데이터 소비자는 다양한 데이터 공급자의 순회 방식을 모두 지원해야 한다. 이는 효율적이지 않다. 

이터러블을 지원하는 데이터 소비자는 내부에서 Symbol . iterator 메서드를 호출해 이터레이터를 생성하고 이터레이터의 next 메서드를 호출하여 이터러블을 순회하며 이터레이터 리절트 객체를 반환한다. 그리고 이터레이터 리절트 객체의 value/done 프로퍼티 값을 취득한다.

이처럼 이터레이션 프로토콜은 다양한 **데이터 공급자가 하나의 순회 방식을 갖도록 규정**하여 데이터 소비자가 효율적으로 **다양한 데이터 공급자를 사용할 수 있도록** 데이터 소비자와 데이터 공급자를 연결하는 인터페이스의 역할을 한다.

## 6. 사용자 정의 이터러블

### 6-1.  구현

이터레이션 프로토콜을 준수하지 않는 일반 객체도 이터레이션 프로토콜을 준수하도록 구현하면 사용자 정의 이터러블이 된다.

```jsx
// 피보나치 수열을 구현한 사용자 정의 이터러블
const fibonacci = {

// Symbol.iterator 메서드를 구현하여 이터러블 프로토콜을 준수한다.
	[Symbol . iterator]() { 
		let [pre , cur] = [0 , 1]
		const max = 10; // 수열의 최대값

		// Symbol.iterator 메서드는 next 메서드를 소유한 이터레이터를 반환해야 하고 
		// next 메서드는 이터레이터 리절트 객체를 반환해야 한다.
		return { 
			next() { 
			[pre , cur] = [cur , pre + cur];
			// 이터레이터 리절트 객체를 반환한다.
			return { value: cur , done: cur >= max };
			} 
		};
	} 
};

// 이터러블인 fibonacci 객체를 순회할 때마다 next 메서드가 호출된다.
for (const num of fibonacci) { 
console . log(num); // 1 2 3 5 8
}

// 이터러블은 스프레드 문법의 대상이 될 수 있다.
const arr = [...fibonacci];
console . log(arr); // [ 1, 2, 3, 5, 8 ]

// 이터러블은 배열 디스트럭처링 할당의 대상이 될 수 있다.
const [first , second , ...rest] = fibonacci;
console . log(first , second , rest); // 1 2 [ 3, 5, 8 ]
```

### 6-2. 이터러블을 생성하는 함수

피보나치 수열을 구현한 사용자 정의 **이터러블을 반환**하는 함수

```jsx
// 수열의 최대값을 인수로 전달받는다.
const fibonacciFunc = function (max) { 
	let [pre , cur] = [0 , 1];
	// Symbol.iterator 메서드를 구현한 이터러블을 반환한다.
	return { 
		[Symbol . iterator]() { 
			return { 
				next() { 
					[pre , cur] = [cur , pre + cur];
					return { value: cur , done: cur >= max };
				} 
			};
		}
	};
};

// 이터러블을 반환하는 함수에 수열의 최대값을 인수로 전달하면서 호출한다.
// fibonacciFunc(10)은 이터러블을 반환한다.
for (const num of fibonacciFunc(10)) { 
	console . log(num); // 1 2 3 5 8
}
```

### 6-3. 이터러블이면서 이터레이터인 객체를 생성하는 함수

- 형식

```jsx
// 이터레이터를 반환하는 Symbol.iterator 메서드와 이터레이션 리절트 객체를 반환하는 next 메서드를 소유한다.
{ 
	[Symbol . iterator]() { return this; } ,
	next() { 
		return { value: any , done: boolean };
	} 
}
```

```jsx
// 이터러블이면서 이터레이터인 객체를 반환하는 함수
const fibonacciFunc = function (max) { 
	let [pre , cur] = [0 , 1];
	// Symbol.iterator 메서드와 next 메서드를 소유한 이터러블이면서 이터레이터인 객체를 반환
	return { 
		[Symbol . iterator]() { return this; } ,
		// next 메서드는 이터레이터 리절트 객체를 반환
		next() { [pre , cur] = [cur , pre + cur];
			return { value: cur , done: cur >= max };
		} 
	};
};

// iter는 이터러블이면서 이터레이터다.
let iter = fibonacciFunc(10);

// iter는 이터러블이므로 for...of 문으로 순회할 수 있다.
for (const num of iter) { 
console . log(num); // 1 2 3 5 8
}

// iter는 이터러블이면서 이터레이터다.
iter = fibonacciFunc(10);
// iter는 이터레이터이므로 이터레이션 리절트 객체를 반환하는 next 메서드를 소유한다.
console . log(iter . next()); // { value: 1, done: false } 
console . log(iter . next()); // { value: 2, done: false } 
console . log(iter . next()); // { value: 3, done: false } 
console . log(iter . next()); // { value: 5, done: false } 
console . log(iter . next()); // { value: 8, done: false } 
console . log(iter . next()); // { value: 13, done: true } 
```

### 6-4 무한 이터러블과 지연평가

done 프로퍼티를 생략하여 무한 이터러블 생성

```jsx
// 무한 이터러블을 생성하는 함수
const fibonacciFunc = function () { 
	let [pre , cur] = [0 , 1];
	return { 
		[Symbol . iterator]() { return this; } ,
		next() { 
			[pre , cur] = [cur , pre + cur];
			// 무한을 구현해야 하므로 done 프로퍼티를 생략한다.
			return { value: cur };
		} 
	};
};

// fibonacciFunc 함수는 무한 이터러블을 생성한다.
for (const num of fibonacciFunc()) { if (num > 10000) break;
console . log(num); // 1 2 3 5 8...4181 6765
}
// 배열 디스트럭처링 할당을 통해 무한 이터러블에서 3개의 요소만 취득한다.
const [f1 , f2 , f3] = fibonacciFunc();
console . log(f1 , f2 , f3); // 1 2 3
```



- 지연평가
    - 데이터가 필요한 시점 이전까지는 미리 데이터를 생성하지 않다가 데이터가 필요한 시점이 되면 그때야 비로소 데이터를 생성하는 기법
    - 평가 결과가 필요할 때까지 평가를 늦추는 기법
    - 불필요한 데이터를 미리 생성하지 않고 필요한 데이터를 필요한 순간에 생성하 므로 빠른 실행 속도를 기대할 수 있고 불필요한 메모리를 소비하지 않으며 무한도 표현할 수 있다는 장점이 있다.
