# 30장 Date

: 표준 빌트인 객체인 Date는 날짜와 시간 (연, 월, 일, 시, 분, 초, 밀리초)을 위한 메서드를 제공하는 빌트인 객체이면서 생성자 함수다.

### 1. Date 생성자 함수

: Date 생성자 함수로 생성한 Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 갖는다.

⇒ 현재 날짜와 시간이 아닌 다른 날짜와 시간을 다루고 싶은 경우 Date 생성자 함수에 명시적으로 해당 날짜와 시간 정보를 인수로 지정한다. Date 생성자 함수로 객체를 생성하는 방법은 다음과 같이 4가지가 있다.

### 1-1. new Date()

```jsx
new Date(); // Tue Oct 17 2023 17:10:03 GMT+0900 (한국 표준시)

// new 연산자 없이 호출하면 날짜와 시간 정보를 나타내는 문자열을 반환한다.
Date(); // "Tue Oct 17 2023 17:10:03 GMT+0900 (한국 표준시)"
```

### 1-2. new Date(milliseconds)

```jsx
// 1970년 1월 1일 00:00:00을 반환한다.
new Date(0); // Thu Jan 01 1970 09:00:00 GMT+0900 (한국 표준시)

// 86400000ms는 1day를 뜻한다.
// 1970년 1월 1일 00:00:00을 기점으로 86400000ms 만큼 경과한 날짜를 반환한다.
new Date(86400000); // Fri Jan 02 1970 09:00:00 GMT+0900 (한국 표준시)
```

### 1-3. new Date(dateString)

: new Date 생성자 함수에 날짜와 시간을 나타내는 문자열을 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다.

단, 인수로 전달한 문자열은 Date.parse 메서드에 의해 해석 가능한 형식이어야 한다.

```jsx
new Date("May 26, 2020 10:00:00"); // Tue May 26 2020 10:00:00 GMT+0900 (한국 표준시)

new Date("2020/03/26/10:00:00"); // Tue May 26 2020 10:00:00 GMT+0900 (한국 표준시)
```

### 1-4. new Date(year, month[, day, hour, minute, second, millisecond])

```jsx
// 월을 나타내는 2는 3월을 의미한다. 2020/3/1/00:00:00:00
new Date(2020, 2);
// Sun Mar 01 2020 00:00:00 GMT+0900 (한국 표준시)

// 2020/3/26/10:00:00:00
new Date(2020, 2, 26, 10, 00, 00, 0);
// Thu Mar 26 2020 10:00:00 GMT+0900 (한국 표준시)

// 다음처럼 표현하면 가독성이 훨씬 좋다.
new Date("2020/3/26/10:00:00");
// Tue May 26 2020 10:00:00 GMT+0900 (한국 표준시)
```

### 2. Date 메서드

### 2-1. Date.now

: 1970년 1월 1일 00:00:00(UTC)을 기점으로 현재 시간까지 경과한 밀리초를 숫자로 반환한다.

```jsx
const now = Date.now(); // 1697530997516

// Date 생성자 함수에 숫자 타입의 밀리토를 인수로 전달하면 1970년 1월 1일 00:00:00
// 을 기점으로 인수로 전달된 밀리초만큼 경과한 날짜와 시간을 나타내는 Date 객체를 반환한다.
new Date(now); // Tue Oct 17 2023 17:10:03 GMT+0900 (한국 표준시)
```

### 2-2. Date.parse

: 1970년 1월 1일 00:00:00(UTC)을 기점으로 전달된 지정 시간의 인수와 동일한 형식까지의 밀리초를 숫자로 반환

```jsx
Date.parse("Jan 2, 1970, 00:00:00:00 UTC"); // 86400000

Date.parse("Jan 2, 1970, 09:00:00:00"); // 86400000

Date.parse("1970/01/02/09:00:00"); // 86400000
```

### Date.prototype.get연도, 월, 일, 요일, 시간 관련 메서드

```jsx
new Date("2020/07/24").getFullYear(); // -> 2020
```

```jsx
// 0월 부터 시작 0 = 1월 11 = 12월
new Date("2020/07/24").getMonth(); // -> 6
```

```jsx
new Date("2020/07/24").getDate(); // -> 24
```

```jsx
new Date("2020/07/24").getDay(); // -> 5
```

```jsx
new Date("2020/07/24/12:00").getHours(); // -> 12
```

```jsx
new Date("2020/07/24/12:30").getMinutes(); // -> 30
```

```jsx
new Date("2020/07/24/12:30:10").getSeconds(); // -> 10
```

⇒ set 메서드를 이용하여 설정도 할 수 있다.

### 2-21. **Date.prototype.getTimezoneOffset**

: UTC와 Date 객체에 지정된 로캘(locale) 시간과의 차이를 분 단위로 반환한다. KST는 UTC에 9시간을 더한 시간이다. 즉, UTC = KST - 9h다.

```jsx
const today = new Date(); // today의 지정 로캘은 KST다.

//UTC와 today의 지정 로캘 KST와의 차이는 -9시간이다.
today.getTimezoneOffset() / 60; // -9
```

### 2-22 ~ 2-24.

Date.prototype.toDateString
Date.prototype.toTimeString
**Date.prototype.toISOString**

```jsx
const today = new Date("2020/7/24/12:30");

today.toString(); // -> Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toTimeString(); // 12:30:00 GMT+0900
today.toISOString(); // -> 2020-07-24T03:30:00.000Z

today.toISOString().slice(0, 10); // -> 2020-07-24
today.toISOString().slice(0, 10).replace(/-/g, ""); // -> 20200724
```

### 2-25. Date.prototype.toLocalString

: 인수로 전달한 로캘을 기준으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다. 인수를 생략한 경우 브라우저가 동작 중인 시스템의 로캘을 적용한다.

```jsx
const today = new Date("2020/7/24/12:30");

today.toString(); // -> Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toLocaleString(); // -> 2020. 7. 24. 오후 12:30:00
today.toLocaleString("ko-KR"); // -> 2020. 7. 24. 오후 12:30:00
today.toLocaleString("en-US"); // -> 7/24/2020, 12:30:00 PM
today.toLocaleString("ja-JP"); // -> 2020/7/24 12:30:00
```

### 2-26. **Date.prototype.toLocalTimeString**

: 인수로 전달한 로캘을 기준으로 Date 객체의 시간을 표현한 문자열을 반환한다. 인수를 생략한 경우 브라우저가 동작 중인 시스템의 로캘을 적용한다.

```jsx
const today = new Date("2020/7/24/12:30");

today.toString(); // -> Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toLocaleTimeString(); // -> 오후 12:30:00
today.toLocaleTimeString("ko-KR"); // -> 오후 12:30:00
today.toLocaleTimeString("en-US"); // -> 12:30:00 PM
today.toLocaleTimeString("ja-JP"); // -> 12:30:00
```

### 3. Date를 활요한 시계 예제

```jsx
(function printNow() {
  const today = new Date();

  const dayNames = [
    "(일요일)",
    "(월요일)",
    "(화요일)",
    "(수요일)",
    "(목요일)",
    "(금요일)",
    "(토요일)",
  ];
  // getDay 메서드는 해당 요일(0 ~ 6)을 나타내는 정수를 반환한다.
  const day = dayNames[today.getDay()];

  const year = today.getFullYear();
  const month = today.getMonth() + 1;
  const date = today.getDate();
  let hour = today.getHours();
  let minute = today.getMinutes();
  let second = today.getSeconds();
  const ampm = hour >= 12 ? "PM" : "AM";

  // 12시간제로 변경
  hour %= 12;
  hour = hour || 12; // hour가 0이면 12를 재할당

  // 10미만인 분과 초를 2자리로 변경
  minute = minute < 10 ? "0" + minute : minute;
  second = second < 10 ? "0" + second : second;

  const now = `${year}년 ${month}월 ${date}일 ${day} ${hour}:${minute}:${second} ${ampm}`;

  console.log(now);

  // 1초마다 printNow 함수를 재귀 호출한다. 41.2.1절 "setTimeout / clearTimeout" 참고
  setTimeout(printNow, 1000);
})();
```

⇒ 위 코드를 활요하면 Date 각종 예시에서 연도, 월, 일, 요일, 시간 등을 쉽게 사용할 수 있다.
