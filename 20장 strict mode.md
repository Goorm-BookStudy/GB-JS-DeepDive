## 20장 strict mode


```
function foo () {
	x = 10;
}
foo();

console.log(x) // ??
```
- JS 엔진에 의해 상위 스코프로 x 변수 선언 검사가 시작된다. 에러가 발생할 것 같지만 JS 엔진이 암묵적으로 x 프로퍼티를 동적 생산해 마치 전역 변수처럼 사용되는 ***암묵적 전역***이 발생한다.
- 안정적인 코드를 위해 실수를 방지하고 오류를 줄이기 위해 ES5부터 strict mode(엄격 모드)가 추가됨
- 전역의 선두 또는 함수 몸체의 선두에 'use strict';를 추가하면 스크립트 전체에 strict mode가 적용된다. ( 반드시 선두에 지정해야한다. )

<br/>

### 전역에 strict mode 적용을 피하자.

```
<!DOCTYPE html>
<html>
~
~
<script>
'use strict';
</script>
```
- 다른 스크립트에 영향을 주지 않고 해당 스크립트에만 한정 적용된다.
- strict mode 스크립트와 non-strict mode 스크립트를 혼용하는 것은 오류를 발생시킬 가능성이 높다. ( 서드파티 라이브러리 사용하는 경우 라이브러리가 non-strict mode인 경우도 있기 때문에 전역은 피하자 )

<br/>

#### 즉시 실행 함수에 적용하는 방법

```
(function () {
	'use strict';
    
    ~~
}
```

<br/>

### 함수 단위로 strict mode 적용하는 것도 피하자
- 함수 단위로 적용할 경우 어떤 함수만 적용하고 어떤 함수는 적용하지 않는 것은 바람직 하지 않고 일일이 다 적용하는것은 번거로운 일이다.
- strict mode는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.

<br/>

### strict mode가 발생시키는 에러
1. 암묵적 전역
- 선언하지 않은 변수를 참조하면 ReferenceError 발생

<br/>

2. 변수, 함수, 매개변수의 삭제
- delete 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError 발생

<br/>

3. 매개변수 이름의 중복
- 중복된 매개변수 이름을 사용하면 SyntaxError 발생

<br/>

4. with 문의 사용
- with 문을 사용하면 SyntaxError가 발생한다. (with 문은 전달된 객체를 스코프 체인에 추가한다.)
- with 문은 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름 생략 가능(코드가 간단해지는 효과가 있지만 가독성이 나빠진다.)
- 사용하지 않는 것이 좋다.

```
(function() {
	'use strict'
    
    // SyntaxError
    with({ x: 1 }) {
    	console.log(x);
    }
}());
```

<br/>

### strict mode 적용에 의한 변화
1. 일반 함수의 this
- strict mode에서 함수를 일반 호출하면 this에 undefiend가 바인딩 된다. (생성자 함수가 아닌 일반 함수에서 this를 사용할 필요가 없기 때문에)
- 에러는 발생하지 않는다.

<br/>

2. arguments 객체
- strict mode에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영 x

```
(function(a){
	'use strict';
    // 매개변수에 전달된 인수를 재할당하여 변경
    a = 2;
    
    // 변경된 인수가 arguments 객체에 반영 x
    console.log(arguments) // { 0: 1, length: 1 }
}(1));
```

<br/><br/>
