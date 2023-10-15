# ✏️ 29장 Math

표준 빌트인 객체인 Math는 수학적 상수와 함수를 위한 프로퍼티와 메서드를 제공한다.
Math는 생성자 함수가 아니기 때문에 정적 프로퍼티와 정적 메서드만 제공한다.

## 📌 29.1 Math 프로퍼티

#### 1. Math.PI

- 원주율 PI 값을 반환한다.

```
Math.PI; // 3.141592...
```

## 📌 29.2 Math 메서드

#### 1. Math.abs

- 인수로 전달된 숫자의 절대값을 반환한다.
- 절대값은 반드시 0 또는 양수여야 한다.

```
Math.abs(-1); // 1
Math.abs('-1'); // 1

Math.abs(''); // 0
Math.abs([]); // 0
Math.abs(null); // 0

Math.abs(undefined); // NaN
Math.abs({}); // NaN
Math.abs('string'); // NaN
Math.abs(); // NaN
```

#### 2. Math.round

- 인수로 전달된 숫자의 소수점 이하를 반올림한 정수를 반환한다.

```
Math.round(1.4); // 1
Math.round(1.6); // 2
Math.round(-1.4); // -1
Math.round(-1.6); // -2
Math.round(1); // 1
Math.round(); // NaN
```

#### 3. Math.ceil

- 인수로 전달된 숫자의 소수점 이하를 올림한 정수를 반환한다.

```
Math.ceil(1.4); // 2
Math.ceil(1.6); // 2
Math.ceil(-1.4); // -1
Math.ceil(-1.6); // -1
Math.ceil(1); // 1
Math.ceil(); // NaN
```

#### 4. Math.floor

- 인수로 전달된 숫자의 소수점 이하를 내림한 정수를 반환한다.

```
Math.floor(1.9); // 1
Math.floor(9.1); // 9
Math.floor(-1.9); // -2
Math.floor(-9.1); // -10
Math.floor(1); // 1
Math.floor(); // NaN
```

#### 5. Math.sqrt

- 인수로 전달된 숫자의 제곱근을 반환한다.

```
Math.sqrt(9); // 3
Math.sqrt(-9); // NaN
Math.sqrt(2); // 1.41421...
Math.sqrt(1); // 1
Math.sqrt(0); // 0
Math.sqrt(); // NaN
```

#### 6. Math.random

- 임의의 난수를 반환한다. 반환한 난수는 0부터 1미만의 실수다.

#### 7. Math.pow

- 첫 번째 인수를 밑으로, 두 번째 인수를 지수로 거듭제곱한 결과를 반환한다.
- ES7에서 도입된 지수 연산자를 사용하는 게 더 가독성이 좋다.

```
Math.pow(2, 4); // 16
2 ** 2 ** 2; // 16
```

#### 8. Math.max

- 전달받은 인수 중 가장 큰 수를 반환한다.
- 인수가 전달되지 않으면 -Infinity를 반환한다.
- 배열을 인수로 전달 받아 배열의 요소 중 최대값을 구하려면 apply 메서드 또는 스프레드 문법을 사용해야 한다.

```
Math.max(1); // 1
Math.max(1, 2); // 2
Math.max(); // -Infinity

// 배열로 전달
Math.max.apply(null, [1, 2, 3]); // 3
Math.max(...[1, 2, 3]); // 3
```

#### 9. Math.min

- 전달받은 인수 중 가장 작은 수를 반환한다.
- 인수가 전달되지 않으면 Infinity를 반환한다.
- 배열을 인수로 전달 받아 배열의 요소 중 최소값을 구하려면 apply 메서드 또는 스프레드 문법을 사용해야 한다.

```
Math.min(1); // 1
Math.min(1, 2); // 1
Math.min(); // Infinity

// 배열로 전달
Math.min.apply(null, [1, 2, 3]); // 1
Math.min(...[1, 2, 3]); // 1
```
