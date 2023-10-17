# 29장 Math
- 표준 빌트인 객체이며 수학적인 상수와 함수를 위한 프로퍼티와 메서드를 제공
- 생성자 함수가 아니다. 따라서 정적 프로퍼티와 정적 메서드만 제공

<br/>

## Math 프로퍼티
### Math.PI
- 원주율 PI 값 (3.141592...)

<br/>

## Math 메서드
### Math.abs
- 인수로 전달된 숫자의 절대값을 반환
- 절대값은 반드시 0 or 양수이어야 한다.

```
Math.abs(-1); // 1
```

<br/>

### Math.round
- 인수로 전달된 숫자의 소수점 이하를 반올림한 정수 반환

```
Math.round(1.4); // 1
```

### Math.ceil
- 인수로 전달된 숫자의 소수점 이하를 올림한 정수 반환

```
Math.ceil(1.4); // 2
```

<br/>

### Math.floor
- 인수로 전달된 숫자의 소수점 이하를 내림한 정수 반환

```
Math.floor(1.9); // 1
```

<br/>

### Math.sqrt
- 인수로 전달된 숫자의 제곱근을 반환

```
Math.sqrt(9); // 3
```

<br/>

### Math.random
- 임의의 난수를 반환 (0~1 미만의 실수)
- 0은 포함하나 1은 포함 x

```
Math.random();

const random = Math.floor((Math.random() * 10) + 1);
console.log(random); // 1~10 범위 정수
```

<br/>

### Math.pow
- 첫 번째 인수를 밑으로, 두 번째 인수를 지수로 거듭제곱한 결과 반환
- ES7에서 도입된 지수 연산자 ( ** )를 사용하면 가독성이 더 좋다.

```
Math.pow(2, 8); // 256
```

<br/>

### Math.max
- 전달받은 인수 중 가장 큰 수 반환 인수 전달 안 되면 -Infinity 반환
- 배열을 인수로 전달받은 경우 Function.porototype.apply 메서드 또는 스프레드 문법을 사용해야 한다.

```
Math.max(1, 2); // 2

Math.max.apply(null, [1,2,3]); // 3

Math.max(...[1,2,3]_; // 3
```

<br/>

### Math.min
- 전달받은 인수 중에서 가장 작은 수를 반환 인수가 전달되지 않으면 Infinity 반환
- 배열을 인수로 전달받은 경우 Function.porototype.apply 메서드 또는 스프레드 문법을 사용해야 한다.
 
```
Math.min(1, 2); // 1

Math.min.apply(null, [1,2,3]); // 1

Math.min(...[1,2,3]_; // 1

```

<br/>
