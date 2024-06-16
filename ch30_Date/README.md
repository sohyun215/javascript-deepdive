표준 빌트인 객체 `Date`는 날짜와 시간(연, 월, 일, 시, 분, 초, 밀리초)을 위한 메서드를 제공하는 빌트인 객체이면서 생성자 함수다.

UTC는 국제 표준시를 말한다. UTC는 GMT로 불리기도 한다. UTC와 GMT는 초의 소수점 단위에서만 차이가 나기 때문에 일상에서는 혼용되어 사용된다. 기술적인 표기에서는 UTC가 사용된다.

KST는 UTC에 9시간을 더한 시간이다. 즉, KST는 UTC보다 9시간이 빠르다. (UTC 00:00 AM은 KST 09:00 AM)

현재 날짜와 시간은 자바스크립트 코드가 실행된 시스템의 시계에 의해 결정된다.

<br />

# 30.1 Date 생성자 함수

Date 생성자 함수로 생성한 Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 갖는다.
이 값은 1970년 1월 1일 00:00:00(UTC)을 기점으로 Date 객체가 나타내는 날짜와 시간까지의 밀리초를 나타낸다.

현재 날짜와 시간이 아닌 다른 날짜와 시간을 다루고 싶은 경우 Date 생성자 함수에 명시적으로 해당 날짜와 시간 정보를 인수로 지정한다.

## `new Date()`

new 연산자와 함께 호출하면 현재 날짜와 시간을 가지는 Date 객체를 반환한다. 이 객체는 내부적으론 날짜와 시간을 나타내는 정수값을 갖지만 콘솔에 출력하면 기본적으로 날짜와 시간 정보를 출력한다.

```js
new Date(); // Sun Jun 16 2024 13:59:11 GMT+0900 (한국 표준시)
```

new 연산자 없이 호출하면 Date 객체를 반환하지 않고 날짜와 시간 정보를 나타내는 **문자열**을 반환한다.

```js
Date(); // 'Sun Jun 16 2024 13:59:21 GMT+0900 (한국 표준시)'
```

<br />

## `new Date(milliseconds)`

Date 생성자 함수에 숫자 타입의 밀리초를 인수로 전달하면 1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 밀리초만큼 경과한 날짜와 시간을 나타내는 Date 객체를 반환한다.

```js
new Date(0); // Thu Jan 01 1970 09:00:00 GMT+0900 (한국 표준시)
```

<br />

## `new Date(dateString)`

날짜와 시간을 나타내는 문자열을 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다.
이때 인수로 전달한 문자열은 `Date.parse` 메서드에 의해 해석 가능한 형식이어야 한다.

```js
new Date('Jun 16, 2024 10:00:00');

new Date('2024/06/16/10:00:00');
```

<br />

## `new Date(year, month[, day, hour, minute, second, millisecond])`

연, 월, 일, 시, 분, 초, 밀리초를 의미하는 숫자를 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다.

이때 연, 월은 반드시 지정해야 한다. 지정하지 않은 옵션 정보는 0 또는 1로 초기화된다.

- `year`: 연을 나타내는 1900년 이후의 정수. 0부터 99는 1900부터 1999로 처리된다.
- `month`: 월을 나타내는 **0 ~ 11**까지의 정수(**⚠️주의 : 0부터 시작(0 = 1월)**)

- `day`: 일을 나타내는 1 ~ 31까지의 정수

- `hour`: 시를 나타내는 0 ~ 23까지의 정수
- `minute`: 분을 나타내는 0 ~ 59까지의 정수
- `second`: 초를 나타내는 0 ~ 59까지의 정수
- `millisecond`: 밀리초를 나타내는 0 ~ 999까지의 정수

<br />

```js
new Date(2024, 6); // Mon Jul 01 2024 00:00:00 GMT+0900 (한국 표준시)

new Date(2024, 6, 16, 10, 00, 00, 0);
```

<br />
<br />

# 30.2 Date 메서드

## `Date.now`

1970년 1월 1일 00:00:00(UTC)을 기점으로 현재 시간까지 경과한 밀리초를 숫자로 반환한다.

```js
const new = Date.now(); // 1718514600331
```

## `Date.parse`

1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환한다.

```js
// UTC
Date.parse('Jun 16, 2024 00:00:00 UTC'); // 1718496000000

// KST
Date.parse('Jun 16, 2024 09:00:00');
Date.parse('2024/06/16/09:00:00');
```

<br />

## `Date.UTC`

Date.UTC 메서드는 `new Date(year, month[, day, hour, minute, second, millisecond])`와 같은 형식의 인수를 사용해야 한다.

Date.UTC 메서드의 인수는 로컬 타임이 아닌 **UTC**로 인식된다.

```js
Date.UTC(1970, 0, 2); // 86400000
```

<br />

## `Date.prototype.getFullYear`

Date 객체의 연도를 나타내는 정수를 반환한다.

```js
new Date('2024/06/16').getFullYear(); // 2024
```

<br />

## `Date.prototype.setFullYear`

Date 객체에 연도를 나타내는 정수를 설정한다. 연도 이외에 옵션으로 월, 일도 설정할 수 있다.

```js
const today = new Date();

today.setFullYear(2000);
today.getFullYear(); // 2000

today.setFullYear(2024, 1, 15);
today.getFullYear(); // 2024
```

<br />

## `Date.prototype.getMonth`

월을 나타내는 0 ~ 11사이의 정수를 반환한다. 1월은 0, 12월은 11이다.

```js
new Date('2024/06/16').getMonth(); // 5
```

<br />

## `Date.prototype.setMonth`

Date 객체에 월을 나타내는 0 ~ 11의 정수를 설정한다.
월 이외 옵션으로 일도 설정할 수 있다.

```js
const today = new Date();

today.setMonth(0);
today.getMonth(); // 0
```

<br />

## `Date.prototype.getDate`

Date 객체의 날짜를 나타내는 정수를 반환한다.

```js
new Date('2024/06/16').getDate(); // 16
```

## `Date.prototype.setDate`

Date 객체에 날짜를 나타내는 정수를 설정한다.

```js
const today = new Date();
today.setDate(15);
today.getDate(); // 15
```

<br />

## `Date.prototype.getDay`

Date 객체의 요일(0 ~ 6)을 나타내는 정수를 반환한다.

- `일`: 0
- `월`: 1
- `화`: 2
- `수`: 3
- `목`: 4
- `금`: 5
- `토`: 6

```js
new Date('2024/06/16').getDay(); // 0
```

<br />

## `Date.prototype.getHours`

Date 객체의 시간을 나타내는 정수를 반환한다.

```js
new Date('2024/06/16/12:00').getHours(); // 12
```

## `Date.prototype.setHours`

시간을 나타내는 정수를 설정한다. 시간 이외에 옵션으로 분, 초, 밀리초도 설정할 수 있다.

```js
const today = new Date();

today.setHours(7);
today.getHours(); // 7

today.setHours(0, 0, 0, 0); // 00:00:00:00
```

<br />

## `Date.prototype.getMinutes`

Date 객체의 분을 나타내는 정수를 반환한다.

```js
new Date('2024/06/16/12:30').getMinutes(); // 30
```

## `Date.prototype.setMinutes`

Date 객체에 분을 나타내는 정수를 설정한다. 옵션으로 초, 밀리초도 설정할 수 있다.

```js
const today = new Date();

today.setMinutes(10);
today.getMinutes(); // 10

today.setHours(20, 30, 0); // HH:20:30:00
```

<br />

## `Date.prototype.getSeconds`

Date 객체의 초를 나타내는 정수를 반환한다.

```js
new Date('2024/06/16/12:00:40').getSeconds(); // 40
```

## `Date.prototype.setSeconds`

Date 객체에 초를 나타내는 정수를 설정한다. 옵션으로 밀리초도 설정할 수 있다.

<br />

## `Date.prototype.getMilliseconds`

Date 객체의 밀리초(0 ~ 999)를 나타내는 정수를 반환한다.

## `Date.prototype.setMilliseconds`

Date 객체에 밀리초를 나타내는 정수를 설정한다.

<br />

## `Date.prototype.getTime`

1970년 1월 1일 00:00:00(UTC)를 기점으로 Date 객체의 시간까지 경과된 밀리초를 반환한다.

```js
new Date('2024/06/16/14:40').getTime(); // 1718516400000
```

## `Date.prototype.setTime`

Date 객체에 1970년 1월 1일 00:00:00를 기점으로 경과된 밀리초를 설정한다.

```js
const today = new Date();
today.setTime(86400000);
console.log(today); // Fri Jan 02 1970 09:00:00 GMT+0900 (한국 표준시)
```

<br />

## `Date.prototype.getTimezoneOffset`

UTC와 Date 객체에 지정된 로캘(locale)시간과의 차이를 분 단위로 반환한다.

KST는 UTC에 9시간을 더한 시간이다. 즉, UTC = KST - 9h다.

```js
const today = new Date();

today.getTimezoneOffset() / 60; // -9
```

<br />

## `Date.prototype.toDateString`

사람이 읽을 수 있는 형식의 문자열로 Date 객체의 날짜를 반환한다.

```js
const today = new Date();

today.toString(); // 'Sun Jun 16 2024 14:46:46 GMT+0900 (한국 표준시)'
today.toDateString(); // 'Sun Jun 16 2024'
```

<br />

## `Date.prototype.toTimeString`

사람이 읽을 수 있는 형식으로 Date 객체의 시간을 표현한 문자열을 반환한다.

```js
today.toTimeString(); // '14:46:57 GMT+0900 (한국 표준시)'
```

<br />

## `Date.prototype.toISOString`

ISO 8601 형식으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다.

```js
const today = new Date('2024/06/16/12:30');

today.toISOString(); // '2024-06-16T03:30:00.000Z'
today.toISOString().slice(0, 10); // '2024-06-16'
today.toISOString().slice(0, 10).replace(/-/g, ''); // '20240616'
```

<br />

## `Date.prototype.toLocaleString`

인수로 전달한 로캘을 기준으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다.

인수를 생략한 경우 브라우저가 동작 중인 시스템의 로캘을 적용한다.

```js
const today = new Date('2024/06/16/12:30');

today.toLocaleString(); // '2024. 6. 16. 오후 12:30:00'
today.toLocaleString('ko-KR');
today.toLocaleString('en-US'); // '6/16/2024, 12:30:00 PM'
today.toLocaleString('ja-JP'); // '2024/6/16 12:30:00'
```

<br />

## `Date.prototype.toLocaleTimeString`

인수로 전달한 로캘을 기준으로 Date 객체의 시간을 표현한 문자열을 반환한다.

```js
today.toLocaleTimeString(); // '오후 12:30:00'
today.toLocaleTimeString('en-US'); // '12:30:00 PM'
today.toLocaleTimeString('ja-JP'); // '12:30:00'
```
