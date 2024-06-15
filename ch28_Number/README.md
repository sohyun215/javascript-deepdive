# 28.1 Number 생성자 함수

표준 빌트인 객체인 Number 객체는 생성자 함수 객체다. 따라서 new 연산자와 함께 호출하여 Number 인스턴스를 생성할 수 있다.

Number 생성자 함수에 인수를 전달하지 않고 new 연산자와 함께 호출하면 `[[NumberData]]` 내부 슬롯에 0을 할당한 Number 래퍼 객체를 생성한다.

```js
const numObj = new Number();
console.log(numObj); // Number {[[PrimitiveValue]]: 0}
```

[[PrimitiveValue]]는 [[NumberData]] 내부 슬롯을 가리킨다. ES5에서는 [[NumberData]]를 [[PrimitiveValue]]라 불렀다.

```js
const numObj = new Number(10);
console.log(numObj); // Number {[[PrimitiveValue]]: 10}
```

<br />

Number 생성자 함수의 인수로 숫자가 아닌 값을 전달하면 인수를 숫자로 강제 변환한 후, [[NumberData]] 내부 슬롯에 변환된 숫자를 할당한 Number 래퍼 객체를 생성한다.

```js
let numObj = new Number('10');
console.log(numObj); // Number {[[PrimitiveValue]]: 10}

numObj = new Number('Hello');
console.log(numObj); // Number {[[PrimitiveValue]]: NaN}
```

<br />

`new` 연산자를 사용하지 않고 Number 생성자 함수를 호출하면 Number 인스턴스가 아닌 숫자를 반환한다. 이를 이용해 명시적으로 타입을 변환하기도 한다.

```js
Number('0'); // 0
Number('10.53'); // 10.53
Number(true); // 1
Number(false); // 0
```

<br />
<br />

# 28.2 Number 프로퍼티

## `Number.EPSILON`

> ES6에서 도입된 `Number.EPSILON`은 **1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이**와 같다.
> 이는 약 2.220446049250313e-16 이다.

부동소수점 산술 연산은 정확한 결과를 기대하기 어렵다. 정수는 2진법으로 오차 없이 저장 가능하지만 부동소수점을 표현하기 위해 가장 널리 쓰이는 표준인 IEEE 754는 2진법으로 변환했을 때 무한소수가 되어 미세한 오차가 발생할 수 밖에 없는 구조적 한계가 있다.

```js
0.1 + 0.2; // 0.30000000000000004
0.1 + 0.2 === 0.3; // false
```

`Number.EPSILON`은 부동소수점으로 인해 발생하는 오차를 해결하기 위해 사용한다.

```js
function isEqual(a, b) {
  // a와 b를 뺀 값의 절대값이 Number.EPSILON보다 작으면 같은 수로 인정한다.
  return Math.abs(a - b) < Number.EPSILON;
}

isEqual(0.1 + 0.2, 0.3); // true
```

<br />

## `Number.MAX_VALUE`

> 자바스크립트에서 표현할 수 있는 가장 큰 양수 값(1.7976931348623157e+308)이다.

Number.MAX_VALUE보다 큰 숫자는 `Infinity`다.

## `Number.MIN_VALUE`

> 자바스크립트에서 표현할 수 있는 가장 작은 양수 값(5e-324)이다.

Number.MIN_VALUE보다 작은 숫자는 `0`이다.

<br />

## `Number.MAX_SAFE_INTEGER`

> 자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값(9007199254740991)이다.

## `Number.MIN_SAFE_INTEGER`

> 자바스크립트에서 가장 안전하게 표현할 수 있는 가장 작은 정수값(-9007199254740991)이다.

<br />

## `Number.POSITIVE_INFINITY`

> 양의 무한대를 나타내는 숫자값 `Infinity`와 같다.

## `Number.NEGATIVE_INFINITY`

> 음의 무한대를 나타내는 숫자값 `-Infinity`와 같다.

<br />

## `Number.NaN`

> 숫자가 아님(Not-a-Number)을 나타내는 숫자값이다.

Number.NaN은 window.NaN과 같다.

<br />
<br />

# 28.3 Number 메서드

## `Number.isFinite`

> 인수로 전달된 숫자값이 정상적인 유한수, 즉 Infinity 또는 -Infinity가 아닌지 검사하여 그 결과를 불리언 값으로 반환한다.

```js
Number.isFinite(0); // true
Number.isFinite(Number.MAX_VALUE); // true
Number.isFinite(Number.MIN_VALUE); // true

Number.isFinite(Infinity); // false
Number.isFinite(-Infinity); // false
```

만약 인수가 NaN이면 언제나 false를 반환한다.

```js
Number.isFinite(NaN); // false
```

`Number.isFinite` 메서드는 빌트인 전역 함수 `isFinite`와 차이가 있다.

빌트인 전역 함수 isFinite는 전달받은 인수를 숫자로 암묵적 타입 변환하여 검사를 수행하지만, Number.isFinite는 전달받은 인수를 숫자로 암묵적 타입 변환하지 않는다. 따라서 **숫자가 아닌 인수가 주어졌을 때 반환값은 언제나 false다.**

```js
Number.isFinite(null); // false

// null은 0으로 암묵적 타입 변환된다.
isFinite(null); // true
```

<br />

## `Number.isInteger`

> 인수로 전달된 숫자값이 정수인지 검사하여 그 결과를 불리언 값으로 반환한다. 검사하기 전에 인수를 숫자로 암묵적 타입 변환하지 않는다.

```js
Number.isInteger(123); // true
Number.isInteger(0.5); // false
Number.isInteger('123'); // false
Number.isInteger(false); // false
Number.isInteger(Infinity); // false
```

<br />

## `Number.isNaN`

> 인수로 전달된 숫자값이 `NaN`인지 검사하여 그 결과를 불리언 값으로 반환한다.

빌트인 전역 함수 isNaN과 차이가 있다. 빌트인 전역 함수 isNaN은 전달받은 인수를 숫자로 암묵적 타입 변환하여 검사를 수행하지만 Number.isNaN은 전달받은 인수를 암묵적 타입 변환하지 않는다. 따라서 **숫자가 아닌 인수가 주어졌을 때 반환값은 언제나 false이다.**

```js
Number.isNaN(undefined); // false

// undefined는 NaN으로 암묵적 타입 변환된다.
isNaN(undefined); // true
```

<br />

## `Number.isSafeInteger`

> 인수로 전달된 숫자값이 안전한 정수인지 검사하여 그 결과를 불리언 값으로 반환한다.

> 안전한 정수값은 **-(2<sup>53</sup> - 1)과 2<sup>53</sup> - 1 사이의 정수값**이다.
>
> 검사전에 인수를 숫자로 암묵적 타입 변환하지 않는다.

```js
Number.isSafeInteger(0); // true
Number.isSafeInteger(1000000000000000); // true
Number.isSafeInteger(10000000000000001); // false

Number.isSafeInteger(0.5); // false
Number.isSafeInteger('123'); // false
```

<br />

## `Number.prototype.toExponential`

> 숫자를 지수 표기법으로 변환하여 문자열로 반환한다. 지수 표기법이란 매우 크거나 작은 숫자를 표기할 때 주로 사용하며 `e`앞에 있는 숫자에 10의 n승을 곱하는 형식으로 수를 나타내는 방식이다.
> 인수로 소수점 이하로 표현할 자릿수를 전달할 수 있다.

```js
(77.1234).toExponential(); // '7.71234e+1'
(77.1234).toExponential(4); // '7.7123e+1'
(77.1234).toExponential(2); // '7.71e+1'
```

<br />

숫자 리터럴과 함께 Number 프로토타입 메서드를 사용할 경우 에러가 발생한다.

```js
77.toExponential(); // SyntaxError: Invalid or unexpected token
```

숫자 뒤의 `.`은 의미가 모호하다. 부동 소수점 숫자의 소수 구분 기호일 수도 있고 객체 프로퍼티에 접근하기 위한 프로퍼티 접근 연산자일 수도 있다.

자바스크립트 엔진은 숫자 뒤의 .을 부동 소수점 숫자의 소수 구분 기호로 해석한다.

따라서 77 뒤의 .을 소수 구분 기호로 해석하여 뒤에 이어지는 toExponential을 프로퍼티로 해석할 수 없으므로 에러가 발생한 것이다.

> 숫자 리터럴과 함께 메서드를 사용할 경우 혼란을 방지하기 위해 그룹 연산자를 사용하는 것을 권장한다.

<br />

## `Number.prototype.toFixed`

> 숫자를 반올림하여 문자열로 반환한다. 반올림하는 **소수점 이하 자릿수**를 나타내는 0~20 사이의 정수값을 인수로 전달할 수 있다.
>
> 인수를 생략하면 기본값 0이 지정된다.

```js
(12345.6789).toFixed(); // '12346'
(12345.6789).toFixed(1); // '12345.7'
(12345.6789).toFixed(2); // '12345.68'
(12345.6789).toFixed(3); // '12345.679'
```

<br />

## `Number.prototype.toPrecision`

> 인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환한다.
> 인수로 전달받은 전체 자릿수로 표현할 수 없는 경우 지수 표기법으로 결과를 반환한다.

**전체 자릿수**를 나타내는 0~21 사이의 정수값을 인수로 전달할 수 있다. 인수를 생략하면 기본값 0이 지정된다.

```js
(12345.6789).toPrecision(); // '12346.6789'
(12345.6789).toPrecision(1); // '1e+4'
(12345.6789).toPrecision(2); // '1.2e+4'
(12345.6789).toPrecision(6); // '12345.7'
```

<br />

## `Number.prototype.toString`

> 숫자를 문자열로 변환하여 반환한다. **진법**을 나타내는 2~36 사이의 정수값을 인수로 전달할 수 있다. 인수를 생략하면 기본값 10진법이 지정된다.

```js
(10).toString(); // '10'
(16).toString(2); // '10000'
(16).toString(8); // '20'
(16).toString(16); // '10'
```
