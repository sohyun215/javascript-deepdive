# 타입 변환과 단축 평가

# 9.1 타입 변환이란?
> #### 기존 원시 값을 사용해 다른 타입의 새로운 원시 값을 생성하는 것


- `명시적 타입 변환` 또는 `타입 캐스팅`: 개발자가 의도적으로 값의 타입을 변환하는 것

- `암묵적 타입 변환` 또는 `타입 강제 변환`: 개발자의 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되는 것

<br>

- 명시적 타입 변환이나 암묵적 타입 변환이 기존 원시 값을 직접 변경하는 것은 아니다. 원시 값은 변경 불가능한 값이므로 변경할 수 없다. 

```jsx
var x = 10;

// 문자열 연결 연산자는 숫자 타입 x의 값을 바탕으로 새로운 문자열을 생성한다.
var str = x + '';
console.log(typeof str, str); // string 10

console.log(typeof x, x); // number 10
```

자바스크립트 엔진은 표현식 `x + ''` 을 평가하기 위해 x 변수의 숫자 값을 바탕으로 새로운 문자열 값 ‘10’을 생성하고 이것으로 ‘10’+ ‘’를 평가한다. 

⇒ 암묵적 타입 변환은 기존 변수 값을 재할당하여 변경하는 것이 아니다.    
자바스크립트 엔진은 표현식을 에러 없이 평가하기 위해 피연산자의 값을 암묵적 타입 변환해 새로운 타입의 값을 만들어 단 한 번 사용하고 버린다.

- 암묵적 타입 강제 변환은 자바스크립트 엔진에 의해 드러나지 않게 타입이 자동 변환되기 때문에 타입을 변경하겠다는 개발자의 의지가 코드에 명백히 나타나지 않는다.

- 따라서 작성한 코드에서 암묵적 타입 변환이 발생하는지, 어떤 타입의 어떤 값으로 변환되는지, 그리고 타입 변환된 값으로 표현식이 어떻게 평가될 것인지를 **예측 가능해야 한다.**

<br>
<br>

# 9.2 암묵적 타입 변환

> #### 자바스크립트 엔진은 표현식을 평가할 때 `코드의 문맥을 고려`해 암묵적으로 데이터 타입을 강제 변환한다.


## 1. 문자열 타입으로 변환

```jsx
'10' + 2  // -> '102'
```

- `+ 연산자` 는 피연산자 중 하나 이상이 문자열이므로 문자열 연결 연산자로 동작한다.

  - 문자열 연결 연산자의 역할은 문자열 값을 만드는 것 ⇒ 모든 피연산자는 코드의 문맥상 모두 문자열 타입이어야 한다.

  - 자바스크립트 엔진은 문자열 연결 연산자의`피연산자 중에서 문자열 타입이 아닌 피연산자를 문자열 타입으로 암묵적 타입 변환`한다.

- 연산자 표현식의 피연산자만이 타입 변환의 대상이 되는 것은 아니다.    
  예를 들어 템플릿 리터럴의 표현식 삽입은 표현식의 평가 결과를 문자열 타입으로 암묵적 타입 변환한다.

```jsx
`1 + 1 = ${1 + 1}`
```

```jsx
// 심벌 타입
(Symbol()) + ''  // TypeError: Cannot convert a Symbol value to a string

// 객체 타입
({}) + ''  // "[object Object]"
Math + ''  // "[object Math]"
[] + ''    // ""
[10, 20] + '' // "10, 20"
(function(){}) + '' // "function(){}"
Array + ''  // "function Array() { [native code] }"
```

<br>

## 2. 숫자 타입으로 변환

```jsx
1 - '1'  // 0
1 * '10' // 10
1 / 'one' // NaN
```

- 산술 연산자의 역할은 숫자 값을 만드는 것 ⇒ 모든 피연산자는 코드 문맥상 모두 숫자 타입이어야 한다.

  - 피연산자를 숫자 타입으로 변환할 수 없는 경우는 산술 연산을 수행할 수 없으므로 표현식의 평가 결과는 NaN이다.

- 비교 연산자도 피연산자를 숫자 타입으로 변환해야 한다.

```jsx
'1' > 0 // true
```


- `+` 단항 연산자는 피연산자가 숫자 타입의 값이 아니면 숫자 타입의 값으로 암묵적 타입 변환을 수행한다.

  - `빈 문자열, 빈 배열, null, false`는 `0`으로, true는 1로 변환된다.

  - #### ❗️객체와 빈 배열이 아닌 배열, undefined는 변환되지 않아 NaN이 된다는 것에 주의

```jsx
+''  // 0
+'0' // 0
+'1' // 1
+'string' // NaN

+true // 1

+null // 0

+undefined // NaN

+Symbol() // TypeError

+{} // NaN
+[] // 0
+[10, 20] // NaN
+(function(){}) //NaN
```

<br>

## 3. 불리언 타입으로 변환

- 제어문 또는 삼항 조건 연산자의 조건식은 불리언 값으로 평가되어야 하는 표현식이다.

- 자바스크립트 엔진은 불리언 타입이 아닌 값을 Truthy 값(참으로 평가되는 값) 또는 Falsy 값(거짓으로 평가되는 값)으로 구분한다.

- ### 💡 false로 평가되는 Falsy 값
    - false
    - undefined
    - null
    - 0, -0
    - NaN
    - ‘’(빈 문자열)    
    
  <br>
  Falsy 값 외의 모든 값은 모두 Truthy 값이다.

<br>
<br>

# 9.3 명시적 타입 변환
> #### 개발자의 의도에 따라 명시적으로 타입을 변경하는 방법

1️⃣ 표준 빌트인 생성자 함수(String, Number, Boolean)를 new 연산자 없이 호출하는 방법

2️⃣ 빌트인 메서드를 사용하는 방법

3️⃣ 암묵적 타입 변환을 이용하는 방법

<br>

## 1. 문자열 타입으로 변환

1. String 생성자 함수를 new 연산자 없이 호출

2. Object.prototype.toString 메서드 사용
3. 문자열 연결 연산자 이용

```jsx
// 1. String 생성자 함수
String(1);        // "1"
String(NaN);      // "NaN"
String(Infinity); // "Infinity"

String(true);     // "true"

// 2. toString 메서드
(1).toString();
(NaN).toString();
(Infinity).toString();

(true).toString();

// 3. 문자열 연결 연산자
1 + '';
NaN + '';
Infinity + '';
```

<br>

## 2. 숫자 타입으로 변환

1. Number 생성자 함수를 new 연산자 없이 호출

2. parseInt, parseFloat 함수를 사용(문자열만 변환 가능)
3. `+` 단항 산술 연산자를 이용
4. `*` 산술 연산자를 이용

```jsx
// 1. Number 생성자 함수
Number('-1');     // -1 
Number('10.53');  // 10.53
Number(true);     // 1

// 2. parseInt, parseFloat
parseInt('-1');        // -1
parseFloat('10.53');   // 10.53

// 3. + 단항 산술 연산자 이용
+'-1';
+'10.53';  
+true;

// 4. * 산술 연산자 이용
'-1' * 1;
'10.53' * 1;
true * 1;
```

<br>

## 3. 불리언 타입으로 변환

1. Boolean 생성자 함수를 new 연산자 없이 호출
2. ! 부정 논리 연산자를 두 번 사용

```jsx
// 1. Boolean()
Boolean('x'); // true
Boolean('');  // false
Boolean('false'); // true

Boolean(NaN);  // false
Boolean(Infinity); // true

Boolean(null); // false

Booealn(undefined); // false

Boolean({}); // true 
Boolean([]); // true

// 2. ! 부정 논리 연산자 두 번 사용
!!'x' 
!!''; 
!'false';

!!NaN;
!!Infinity;

!!null;
!!undefined;

!!{};
!![];
```

<br>
<br>

# 9.4 단축 평가(short-circuit evaluation)
>  #### 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것

## 1. 논리 연산자를 사용한 단축 평가

- 논리곱 연산자(&&): 두 개의 피연산자가 모두 true로 평가될 때 true를 반환한다. (좌항→ 우항으로 평가 진행)

- 논리합 연산자(||): 두 개의 피연산자 중 하나만 true로 평가되어도 true를 반환한다. 

- `단축평가`: 논리곱과 논리합 연산자는 `논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그대로 반환한다.`


  - true || anything  => `true`

  - false || anything => `anything`
  - true && anything  =>  `anything`
  - false && anything =>  `false`

```jsx
'Cat' || 'Dog';  // "Cat"
false || 'Dog';  // "Dog"
'Cat' || false;  // "Cat"

'Cat' && 'Dog';  // "Dog"
false && 'Dog';  // false
'Cat' && false;  // false
```

- 조건이 `Truthy` 값일 때 무언가를 해야 한다면 `논리곱` 연산자 표현식으로 if문 대체 가능
    
    ```jsx
    var done = true;
    var message = '';
    
    if(done) message = '완료';
    
    // 단축 평가로 대체 가능
    message = done && '완료';
    ```
    
- 조건이 `Falsy` 값일 때 무언가를 해야 한다면 `논리합` 연산자 표현식으로 if문 대체 가능
    
    ```jsx
    done = false;
    if(!done) message = '미완료';
    
    // done이 false라면 '미완료'할당
    message = done || '미완료';
    ```
    
<br>

## 단축 평가가 유용하게 사용되는 상황
### 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때

```jsx
var elem = null;
var value = elem.value; // TypeError
```


```jsx
var elem = null;
var value = elem && elem.value; // null
```

- elem이 Falsy 값이면 elem으로 평가되고, Truthy 값이면 elem.value로 평가

<br>

### 함수 매개변수에 기본값을 설정할 때
- 함수를 호출할 때 인수를 전달하지 않으면 매개변수에는 undefined가 할당된다.

단축 평가를 사용해 매개변수의 기본값을 설정하면 에러를 방지할 수 있다.

```jsx
// 단축 평가를 사용한 매개변수의 기본값 설정
function getStringLength(str) {
  str = str || ''; 
  return str.length;
}

getStringLength(); // 0
getStringLength('hi'); // 2

// ES6의 매개변수의 기본값 설정
function getStringLength(str = '') {
  return str.length;
}
```

<br>

## 2. 옵셔널 체이닝 연산자(?.)
> #### 좌항의 피연산자가 `null` 또는 `undefined`인 경우 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.

- ES11에서 도입

```jsx
var elem = null;

var value = elem?.value;
console.log(value); // undefined;
```

- **객체가 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때 유용하다.**

- 옵셔널 체이닝 연산자가 도입되기 이전에는 논리연산자 &&를 사용한 단축평가를 통해 확인했다.

&&는 좌항 피연산자가 Falsy 값이면 그대로 좌항 피연산자를 반환한다. 하지만 0이나 ‘’은 객체로 평가될 때도 있다. 

```jsx
var str = '';
var length = str && str.length;

console.log(length); // ''
```

하지만 ?.는 좌항 피연산자가 `Falsy 값이라도 null 또는 undefined가 아니면 우항의 프로퍼티 참조를 이어간다.`

```jsx
var str = '';
var length = str?.length;

console.log(length); // 0
```

<br>

## 3. null 병합 연산자(??)
> #### 좌항의 피연산자가 `null` 또는 `undefined`인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.

- ES11에서 도입

- **변수에 기본값을 설정할 때 유용하다.**

```jsx
var foo = null ?? 'default string';
console.log(foo); // "default string"
```

- null 병합 연산자가 도입되기 이전에는 논리 연산자 ||를 사용한 단축 평가를 통해 변수에 기본값을 설정했다.

||를 사용한 단축 평가의 경우 좌항의 피연산자가 Falsy 값이면 우항의 피연산자를 반환한다. 만약 Falsy값인 0이나 ‘’도 기본값으로서 유효하다면 예기치 않은 동작이 발생할 수 있다.

```jsx
var foo = '' || 'default string';
console.log(foo); // "default string"
```

??는 좌항의 피연산자가 `Falsy 값이라도 null 또는 undefined가 아니면 피연산자를 그대로 반환한다.`

```jsx
var foo = '' ?? 'default string';
console.log(foo); // ""
```