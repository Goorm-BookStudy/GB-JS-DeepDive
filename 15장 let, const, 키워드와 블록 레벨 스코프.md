## 15장 let, const, 키워드와 블록 레벨 스코프
### var 키워드의 문제점
1. 변수 중복 선언 허용
2. 함수 레벨 스코프
3. 변수 호이스팅

<br/>

> 📌 공통적으로 에러를 발생시키지는 않지만 가독성과 오류를 발생시킬 여지를 남긴다

<br/>

### let 키워드
#### 변수 중복 선언 금지
```
let name = '하민';

let name = '박하'; // SyntaxError Identifier 'name' has already been declared
```

<br/>

#### 블록 레벨 스코프
- let 키워드로 선언된 변수는 모든 코드 블록(함수, if 문, for 문, while 문, try/catch 문 등)을 지역 스코프로 인정
- 전역에 생성된 변수와 지역 변수 안에 선언된 변수의 이름이 같아도 별개의 변수다.
```
let foo = 1; // 전역변수

{
	let foo = 2; // 지역 변수
    let bar = 3; // 지역 변수
}

conosle.log(foo); // 1
console.log(bar); // ReferenceError: bar is not defined
```

<br/>

#### 변수 호이스팅
```
console.log(foo); // ReferenceError: foo is not defined

let foo; // 변수 선언문에서 초기화 단계가 실행
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행된다.
console.log(foo); // 1
```
- var 키워드의 경우 선언과 초기화 단계가 한번에 진행된다. ( <=> let은 선언과 초기화가 분리되어 진행)
- let 키워드는 변수 호이스팅이 발생하지 않는 것처럼 동작하지만 런타임 전에 JS 엔진에 의해 암묵적으로 선언 단계가 되어 실행되지만 초기화 단계는 변수 선언문에 도달했을 때 실행된다. -> 초기화 이전에 변수 접근하려고 하면 참조 에러 발생(호이스팅은 발생하기 때문에 참조 에러가 발생한다.)
- let 키워드 선언 변수는 스코프 시작 지점부터 초기화 단계 시작 지점(변수 선언문)까지 변수를 참조할 수 없다 이 구간을 ***일시적 사각지대(TDZ)***라고 부른다.

|선언단계|ReferenceError|
|:----:|:----:|
|일시적 사각지대(TDZ)|ReferenceError|
|초기화 단계|foo === undefined|
|할당 단계|foo === 1|

<br/>


### const 키워드
- 보통 상수 선언을 위해 사용(상수는 상태 유지와 가독성. 유지보수의 편의를 위해 적극 사용 권장, 상수 이름은 대문자로 선언해 상수임을 명확히 나타내고 여러 단어로 이뤄진 경우 언더스코어(_)로 구분해 스네이크 케이스로 표현하는 것이 일반적)

- 선언과 동시에 초기화가 필요하고 재할당이 불가능하다.
```
const foo = 1;

const foo; // SyntaxError: Missing initializer in const declaration

foo = 2; // TypeError: Assignment to constant variable.
```
- 변수 호이스팅이 발생하지 않는 것처럼 동작한다.
- const 키워드로 선언한 변수에 원시 값을 할당한 경우 값 변경이 불가능하지만 객체를 할당한 경우 값 변경이 가능하다. -> 객체는 재할당 없이도 직접 변경이 가능하다. const 키워드는 재할당을 금지하지만 ***"불변"을 의미하지는 않는다.***

<br/>

> #### 💡 var vs let vs const
- ES6 사용 시 var 키워드는 사용하지 않는다.
- 재할당 필요 여부를 모를 경우 const를 우선 사용하고 상황을 지켜보는 편이 좋다.



<br/><br/>
