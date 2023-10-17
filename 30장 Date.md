# 30장 Date
- 날짜와 시간을 위한 메서드를 제공하는 빌트인 객체이면서 생성자 함수
- UTC는 국제 표준시를 말하고 GMT로 불리기도 한다. KST(한국 표준시)는 UTC에 9시간을 더한 시간
- 현재 날짜와 시간은 JS 코드가 실행된 시스템 시계에 의해 결정

<br/>

## Date 생성자 함수
- 생성자 함수이고 내부적으로 날짜와 시간을 나타내는 정수값을 갖는다. 이 값은 1970년 1월 1일 00:00:00을 기점으로 Date 객체가 나타내는 날짜와 시간까지의 밀리초를 나타낸다.
- Date 생성자 함수로 생성한 Date 객체는 기본적으로 현재 날짜와 시간을 나타내는 정수값을 가진다. 

<br/>

## Date 생성자 함수로 객체를 생성하는 방법
### new Date()
- 인수 없이 new 연산자와 함께 호출하면 현재 날짜와 시간을 가지는 Date 객체를 반환한다.
- 내부적으로는 정수의 형태고 콘솔에 출력하면 기본적으로 날짜와 시간 정보를 출력한다.
- new 연산자 없이 호출하면 Date 객체가 아닌 날짜와 시간 정보를 나타내는 문자열을 반환한다.

<br/>

### new Date(milliseconds)
- 밀리초를 인수로 전달하면 1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 밀리초만큼 경과한 날짜와 시간을 나타내는 객체로 반환한다.

<br/>

### new Date(dateString)
- 날짜와 시간을 나타내는 문자열을 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환
- 이때 인수로 전달한 문자열은 Date.parse 메서드에 의해 해석 가능한 형식이어야 한다.

```
new Date('May 26 , 2020 10:00:00);
// Tue May 26 2020 10:00:00 GMT_0900 (대한민국 표준시)
```

<br/>

### new Date(year, month[, day, hour, minute, second, millisecond])

|인수|내용|
|:-----:|:------:|
|year|연을 나타내는 1900년 이후의 정수 0 부터 99는 1900부터 1999로 처리된다.|
|month|월을 나타내는 0 ~ 11까지의 정수|
|day|일을 나타내는 1 ~ 31까지의 정수|
|hour|시를 나타내는 0 ~ 23까지의 정수|
|minute|분을 나타내는 0 ~ 59까지의 정수|
|second|초를 나타내는 0 ~ 59까지의 정수|
|millisecond | 밀리초를 나타내는 0 ~ 999까지의 정수|

```
new Date(2020, 2);
// Sun Mar 01, 2020, 00:00:00 GMT+900
```

<br/>

## Date 메서드
### Date.now
- 1970년 1월 1일 00:00:00을 기점으로 현재 시간까지 밀리초를 숫자로 반환한다.

```
const now = Date.now(); // 1593971539112
```

<br/>

### Date.parse
- 1970년 1월 1일 00:00:00을 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환

```
Date.parse(' Jan 2, 1970 00:00:00 UTC'); // 86400000
```

<br/>

### Date.UTC
- 1970년 1월 1일 00:00:00을 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환

```
Date.UTC(1970, 0, 2); // 86400000
Date.UTC('1970/1/2'); // NaN
```

<br/>

### Date.prototype.getFullYear
- Date 객체의 연도를 나타내는 정수를 반환

```
new Date('2020/07/24').getFullYear(); // 2020
```

<br/>

### Date.prototype.setFullYear
- Date 객체의 연도를 나타내는 정수를 설정 연도 이외에 옵션으로 월, 일도 가능

```
const today = new Date();

today.setFullYear(2000);
today.getFullYear(2000); // 2000
```

<br/>

### Date.prototype.getMonth
- Date 객체의 월을 나타내는 0 ~ 11의 정수를 반환

```
new Date('2020/07/24').getMonth(); // 6
```

<br/>

### Date.prototype.setMonth
- Date 객체의 월을 나타내는 0 ~ 11의 정수를 설정한다. 1월은 0, 12월은 11이고 월 이외에 옵션으로 일도 설정 가능

```
const today = new Date();

today.setMonth(0); // 1월
today.getMonth(); // 0

```

<br/>

### Date.prototype.getDate
- Date 객체의 날짜(1 ~ 31)를 나타내는 정수를 반환

```
new Date('2020/07/24').getDate(); // 24
```

<br/>

### Date.prototype.setDate
- Date 객체의 날짜(1 ~ 31)를 나타내는 정수를 설정

```
const today = new Date();

today.setDate(1);
today.getDate(); // 1
```

<br/>

### Date.prototype.getDay
- Date 객체의 요일(0~6)을 나타내는 정수를 반환한다.

|요일|반환값|
|:-----:|:-----:|
|일|0|
|월|1|
|화|2|
|수|3|
|목|4|
|금|5|
|토|6|

```
new Date('2020/07/24').getDate(); // 5
```

<br/>

### Date.prototype.getHours
- Date 객체의 시간 (0~23)을 나타내는 정수를 반환

```
new Date('2020/07/24/12:00').getHours(); // 12
```

<br/>

### Date.prototype.setHours
- Date 객체의 시간 (0~23)을 나타내는 정수를 설정 시간 이외에 옵션으로 분, 초, 밀리초도 설정 가능

```
const today = new Date();

today.setHours(7);
today.getHours(); // 7
```

<br/>

### Date.prototype.getMinutes
- Date 객체의 분(0~59)을 나타내는 정수 반환

```
new Date('2020/07/24/12:30').getMinutes(); // 30
```

<br/>

### Date.prototype.setMinutes
- Date 객체의 분(0~59)을 나타내는 정수를 설정 시간 이외에 옵션으로 초, 밀리초도 설정 가능

```
const today = new Date();

today.setMinutes(50);
today.getMinutes(); // 50
```

<br/>

### Date.prototype.getSeconds
- Date 객체의 초(0~59)을 나타내는 정수 반환

```
new Date('2020/07/24/12:30:10').getSeconds(); // 10
```

<br/>

### Date.prototype.setSeconds
- Date 객체의 초(0~59)를 나타내는 정수를 설정 시간 이외에 옵션으로 밀리초도 설정 가능

```
const today = new Date();

today.setSeconds(30);
today.getSeconds(); // 30
```

<br/>

### Date.prototype.getMilliSeconds
- Date 객체의 밀리초(0~999)을 나타내는 정수 반환

```
new Date('2020/07/24/12:30:10:150').getMilliSeconds(); // 150
```

<br/>

### Date.prototype.setMilliSeconds
- Date 객체의 밀리초(0~999)를 나타내는 정수를 설정

```
const today = new Date();

today.setMilliSeconds(123);
today.getMilliSeconds(); // 123
```

<br/>

### Date.prototype.getTime
- 1970년 1월 1일 00:00:00(UTC)를 기점으로 Date 객체의 시간까지 경과된 밀리초를 반환

```
new Date('2020/07/24/12:30').getTime(); // 1595561400000
```

<br/>

### Date.prototype.setMilliSeconds
- Date 객체의 1970년 1월 1일 00:00:00(UTC)를 기점으로 경과된 밀리초 설정

```
const today = new Date();

today.setTime(86400000);
console.log(today); // Fri Jan 02 1970 09:00:00 GMT+0900 ( 대한민국 표준시)
```

<br/>

### Date.prototype.getTimezoneOffset
- UTC와 Date 객체에 지정된 로캘 시간과의 차이를 분 단위로 반환 KST는 UTC + 9시간을 더한 시간이다.

```
const today = new Date();

today.getTimezoneOffset() / 60 // -9
```

<br/>

### Date.prototype.toDateString
- 사람이 읽을 수 있는 형식의 문자열로 Date 객체의 날짜 반환

```
const today = new Date('2020/7/24/12/12:30');

today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 ( 대한민국 표준시)
today.toDateString(); // Fri Jul 24 2020
```

<br/>

### Date.prototype.toTimeString
- 사람이 읽을 수 있는 형식으로 시간을 표현한 문자열 반환

```
const today = new Date('2020/7/24/12/12:30');

today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 ( 대한민국 표준시)
today.toTimeString(); // 12:30:00 GMT+0900 ( 대한민국 표준시)
```

<br/>

### Date.prototype.toTimeISOString
- ISO 8601 형식으로 Date 객체의 날짜와 시간을 표현한 문자열 반환

```
const today = new Date('2020/7/24/12/12:30');

today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 ( 대한민국 표준시)
today.toTimeISOString(); // 2020-07-24T03:30:00.000z
```

<br/>

### Date.prototype.toLocalString
- 인수로 전달한 로캘을 기준으로 Date 객체의 날짜와 시간을 표현한 문자열 반환
- 인수 생략 시 브라우저가 동작 중인 시스템의 로캘 적용

```
const today = new Date('2020/7/24/12/12:30');

today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 ( 대한민국 표준시)
today.toLocalString('Ko-KR'); // 2020. 7. 24 오후 12:30:00
```

<br/>

### Date.prototype.toLocalTimeString
- 인수로 전달한 로캘을 기준으로 Date 객체의 시간을 표현한 문자열 반환
- 인수 생략 시 브라우저가 동작 중인 시스템의 로캘 적용

```
const today = new Date('2020/7/24/12/12:30');

today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 ( 대한민국 표준시)
today.toLocalTimeString('Ko-KR'); // 오후 12:30:00
```

<br/>
