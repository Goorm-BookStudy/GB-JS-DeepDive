# ✏️ 30장 Date

표준 빌트인 객체인 Date는 날짜와 시간을 위한 메서드를 제공하는 빌트인 객체면서 생성자 함수다.

- UTC(협정 세계시)는 국제 표준시를 말하며 GMT(그리니치 평균시)로 불리기도 한다. 둘은 초의 소수점 단위에서만 차이가 나 일상에서는 혼용되며 기술적 표기에서는 UTC를 사용한다.
- KST(한국 표준시)는 UTC + 9시간으로 UTC보다 9시간 빠르다.
- 현재 날짜와 시간은 JS가 실행된 시스템의 시계에 의해 결정된다.

## 📌 30.1 Date 생성자 함수

- Date 생성자 함수로 생성한 Date 객체는 내부적으로 현재 날짜와 시간을 나타내는 정수값을 갖는다.
- 1970년 1월 1일 00:00:00(UTC)를 기점으로 Date 객체가 나타내는 날짜와 시간까지의 밀리초를 나타낸다.
- 현재 날짜와 시간이 아닌 다른 날짜와 시간을 다루고 싶다면 Date 생성자 함수에 명시적으로 해당 날짜와 시간 정보를 인수로 지정한다.

#### 1. new Date()

- 인수 없이 new 연산자와 함께 호출하면 현재 날짜와 시간을 가진 Date 객체를 반환한다.
- Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 갖는다.
- Date 객체를 콘솔에 출력하면 기본적으로 날짜와 시간 정보를 출력한다.
- new 연산자 없이 호출하면 Date 객체를 반환하지 않고, 날짜와 시간 정보를 나타내는 **문자열**을 반환한다.

#### 2. new Date(milliseconds)

- 숫자 타입의 밀리초를 인수로 전달하면 1970년 1월 1일 00:00:00(UTC)를 기점으로 전달된 밀리초만큼 경과한 날짜와 시간을 나타내는 Date 객체를 반환한다.

```
new Date(0); // 1970년 1월 1일 9시
new Date(86400000); // 1970년 1월 2일 9시
```

#### 3. new Date(dateString)

- 날짜와 시간을 나타내는 문자열을 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다.
- 인수로 전달한 문자열은 아래를 비롯해 Date.parse 메서드로 해석 가능한 형식이어야 한다.
  - ISO 형식 (YYYY-MM-DD)
  - "Month Day, Year Hours:Minutes:Seconds" (예: "January 1, 2000 00:00:00")
  - "Month Day, Year" (예: "January 1, 2000")
  - "Day Month Year Hours:Minutes:Seconds Timezone" (예: "1 January 2000 00:00:00 GMT-0700")
  - "Day Month Year" (예: "1 January 2000")

#### 4. new Date(year, month[, day, hour, minute, second, millisecond])

- 연, 월, 일, 시, 분, 초, 밀리초를 의미하는 숫자를 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다.
- 이 때 연/월은 반드시 지정해야 하며 지정하지 않은 옵션 정보는 0 또는 1로 초기화 된다.
- 월은 0부터 시작한다.

```
new Date('2020/3/26/10:00:00:00')
```

## 📌 30.2 Date 메서드

#### 1. Date.now

- 1970년 1월 1일 00:00:00(UTC)를 기점으로 현재까지 경과한 밀리초를 숫자로 반환한다.

#### 2. Date.parse

- 1970년 1월 1일 00:00:00(UTC)를 기점으로 전달된 지정 시간까지의 밀리초를 숫자로 반환한다.

```
Date.parse('1970/01/02/09:00:00:00'); // 86400000
```

#### 3. Date.UTC

- 1970년 1월 1일 00:00:00(UTC)를 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환한다.
- 인수는 로컬 타임이 아닌 UTC로 인식되며 월은 0부터 시작한다.

```
Date.UTC(1970, 0, 2); // 86400000
Date.UTC('1970/1/2'); // NaN (이런 형식으로 보내면 안됨)
```

#### 4. Date.prototype.getFullYear

- Date 객체의 연도를 나타내는 정수를 반환한다.

```
new Date('2020/07/24').getFullYear(); // 2020
```

#### 5. Date.prototype.setFullYear

- Date 객체의 연도를 나타내는 정수를 설정한다.
- 연도 이외에 옵션으로 월/일도 설정할 수 있다.

```
const today = new Date();

today.setFullYear(2023, 10, 15);
today.getFullYear(); // 2023
```

#### 6. Date.prototype.getMonth

- Date 객체의 월을 나타내는 0 ~ 11의 정수를 반환한다.

#### 7. Date.prototype.setMonth

- Date 객체의 월을 나타내는 0 ~ 11의 정수를 설정한다.
- 월 이외에 옵션으로 일도 설정할 수 있다.

#### 8. Date.prototype.getDate

- Date 객체의 일을 나타내는 1 ~ 31의 정수를 반환한다.

#### 9. Date.prototype.setDate

- Date 객체의 일을 나타내는 1 ~ 31의 정수를 설정한다.

#### 10. Date.prototype.getDay

- Date 객체의 요일을 나타내는 정수를 반환한다.
- 일요일부터 0으로 토요일까지 6이다.

#### 11. Date.prototype.getHours

- Date 객체의 시간을 나타내는 0 ~ 23의 정수를 반환한다.

#### 12. Date.prototype.setHours

- Date 객체의 시간을 나타내는 0 ~ 23의 정수를 설정한다.
- 시간 이외에 옵션으로 분/초/밀리초도 설정할 수 있다.

#### 13. Date.prototype.getMinutes

- Date 객체의 분을 나타내는 0 ~ 59의 정수를 반환한다.

#### 14. Date.prototype.setMinutes

- Date 객체의 분을 나타내는 0 ~ 59의 정수를 설정한다.
- 분 이외에 옵션으로 초/밀리초도 설정할 수 있다.

#### 15. Date.prototype.getSeconds

- Date 객체의 초를 나타내는 0 ~ 59의 정수를 반환한다.

#### 16. Date.prototype.setSeconds

- Date 객체의 초를 나타내는 0 ~ 59의 정수를 설정한다.
- 초 이외에 옵션으로 밀리초도 설정할 수 있다.

#### 17. Date.prototype.getMilliseconds

- Date 객체의 밀리초를 나타내는 0 ~ 999의 정수를 반환한다.

#### 18. Date.prototype.setMilliseconds

- Date 객체의 초를 나타내는 0 ~ 999의 정수를 설정한다.

#### 19. Date.prototype.getTime

- 1970년 1월 1일 00:00:00(UTC)를 기점으로 Date 객체의 시간까지 경과된 밀리초를 반환한다.

#### 20. Date.prototype.setTime

- 1970년 1월 1일 00:00:00(UTC)를 기점으로 경과된 밀리초를 설정한다.

#### 21. Date.prototype.getTimezoneOffset

- UTC와 Date 객체에 설정된 로캘 시간과의 차이를 분 단위로 반환한다.
- KST는 UTC보다 9시간 빠르다.

#### 22. Date.prototype.toDateString

- 사람이 읽을 수 있는 형식의 문자열로 Date 객체의 날짜를 반환한다.

```
const today = new Date('2020/7/24/12:30');
today.toDateString(); // Fri Jul 24 2020
```

#### 23. Date.prototype.toTimeString

- 사람이 읽을 수 있는 형식으로 Date 객체의 시간을 표현한 문자열을 반환한다.

```
const today = new Date('2020/7/24/12:30');
today.toTimeString(); // 12:30:00 GMT+0900
```

#### 24. Date.prototype.toISOString

- ISO 8601 형식으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다.

```
const today = new Date('2020/7/24/12:30');
today.toISOString(); // 2020-07-24T03:30:00.000Z

today.toISOString.slice(0, 10); // 2020-07-24
today.toISOString.slice(0, 10).replace(/-/g, ''); // 20200724
```

#### 25. Date.prototype.toLocaleString

- 인수로 전달한 로캘을 기준으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다.
- 인수를 생략하면 브라우저가 동작중인 시스템의 로캘을 적용한다.

```
const today = new Date('2020/7/24/12:30');

today.toLocaleString('ko-KR'); // 2020. 7. 24. 오후 12:30:00
today.toLocaleString('en-US'); // 7/24/2020, 12:30:00 PM
today.toLocaleString('ja-JP'); // 2020/7/24 12:30:00
```

#### 26. Date.prototype.toLocaleTimeString

- 인수로 전달한 로캘을 기준으로 Date 객체의 시간을 표현한 문자열을 반환한다.
- 인수를 생략하면 브라우저가 동작중인 시스템의 로캘을 적용한다.

```
const today = new Date('2020/7/24/12:30');

today.toLocaleString('ko-KR'); // 오후 12:30:00
today.toLocaleString('en-US'); // 12:30:00 PM
today.toLocaleString('ja-JP'); // 12:30:00
```
