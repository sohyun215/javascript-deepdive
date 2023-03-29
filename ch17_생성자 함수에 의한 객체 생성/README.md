# Object 생성자 함수

생성자 함수(constructor)란 new 연산자와 함께 호출해 객체를 생성하는 함수이고, 생성자 함수에 의해 생성된 객체를 인스턴스라 한다.

```jsx
// 빈 객체 생성
const person = new Object();
```

<br>

# 생성자 함수

## 객체 리터럴에 의한 객체 생성 방식의 문제점

객체 리터럴에 의한 객체 생성 방식은 **단 하나의 객체만 생성**한다.  
따라서 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 매번 같은 프로퍼티를 기술해야 하기 때문에 비효율적이다.

<br>

## 생성자 함수에 의한 객체 생성 방식의 장점

객체를 생성하기 위한 템플릿처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다.

```jsx
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 인스턴스 생성
const circle1 = new Circle(5);
const circle2 = new Circle(10);
```

### ✔️ this

> 객체 자신의 프로퍼티나 메서드를 참조하기 위한 **자기 참조 변수**

this가 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.

- 일반 함수로서 호출: `전역 객체`

- 메서드로서 호출: `메서드를 호출한 객체(마침표 앞의 객체)`
- 생성자 함수로서 호출: 생성자 함수가 (미래에) 생성할 `인스턴스`

```jsx
function foo() {
  console.log(this);
}

foo(); // window (전역 객체: 브라우저-window, Node.js-global)

const obj = { foo };
obj.foo(); // obj

const inst = new foo(); // inst
```

new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다. → 만약 new 연산자와 함께 호출하지 않으면 일반 함수로 동작한다.

<br>

## 생성자 함수의 인스턴스 생성 과정

생성자 함수의 역할: 프로퍼티 구조가 동일한 인스턴스를 생성하기 위한 템플릿으로서 동작하여 `인스턴스를 생성` 하는 것과 `생성된 인스턴스를 초기화`(인스턴스 프로퍼티 추가 및 초기값 할당) 하는 것

```jsx
function Circle(radius) {
  // 인스턴스 초기화
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 인스턴스 생성
const circle1 = new Circle(5);
```

자바스크립트 엔진은 암묵적인 처리를 통해 인스턴스를 생성하고 반환한다.

new 연산자와 함께 생성자 함수를 호출하면 자바스크립트 엔진은 다음과 같은 과정을 거친다.

<br>

### 1. 인스턴스 생성과 this 바인딩

암묵적으로 빈 객체가 생성된다. 이 빈 객체가 생성자 함수가 생성한 인스턴스이고, this에 바인딩된다.

이 처리는 함수 몸체의 코드가 한 줄씩 실행되는 런타임 이전에 실행된다.

<br>

### 2. 인스턴스 초기화

생성자 함수에 기술되어 있는 코드가 한 줄씩 실행되어 this에 바인딩되어 있는 인스턴스를 초기화한다.

<br>

### 3. 인스턴스 반환

생성자 함수 내부에서 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 `this`를 암묵적으로 반환한다.

```jsx
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

  // 2. this에 바인딩되어 있는 인스턴스 초기화
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };

  // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
}
```

만약 this가 아닌 다른 `객체`를 명시적으로 반환하면 return 문에 명시한 객체가 반환된다. (암묵적인 this 반환 무시)

하지만 `원시 값`을 반환하면 원시 값 반환은 무시되고 암묵적으로 this가 반환된다.

💡 이처럼 생성자 함수 내부에서 명시적으로 this가 아닌 다른 값을 반환하는 것은 생성자 함수의 기본 동작을 훼손하기 때문에 생성자 함수 내부에서 return 문을 반드시 생략해야 한다.

<br>

## 내부 메서드 [[Call]]과 [[Construct]]

함수는 객체이지만 일반 객체와는 다르다. 일반 객체는 호출할 수 없지만 함수는 호출할 수 있다.

따라서 함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드는 물론, 함수로서 동작하기 위한 [[Environment]], [[FormalParameters]] 등의 내부 슬롯과 [[Call]], [[Construct]] 같은 내부 메서드를 추가로 가지고 있다.

함수가 일반 함수로서 호출되면 내부 메서드 [[Call]]이 호출되고, new 연산자와 함께 생성자 함수로서 호출되면 내부 메서드 [[Construct]]가 호출된다.

- `callable`: 내부 메서드 [[Call]]을 갖는 함수 객체, 함수를 의미

- `constructor`: 내부 메서드 [[Construct]]를 갖는 함수 객체, 생성자 함수로서 호출할 수 있는 함수

- `non-constructor`: 내부 메서드 [[Construct]]를 갖지 않는 함수 객체, 객체를 생성자 함수로서 호출할 수 없는 함수

> 모든 함수 객체는 내부 메서드 [[Call]]을 갖고 있지만 모든 함수 객체가 [[Construct]]를 갖는 것은 아니다.  
> → 모든 함수 객체는 호출할 수 있지만 모든 함수 객체를 생성자 함수로서 호출할 수 있는 것은 아니다.

<img src="https://user-images.githubusercontent.com/79398566/228589888-0adcb513-8a3e-4fc9-854c-70d81f9b7fdb.png" width=450px>

<br>

## constructor와 non-constructor의 구분

자바스크립트 엔진은 함수 정의를 평가하여 함수 객체를 생성할 때 함수 정의 방식에 따라 구분한다.

- constructor: 함수 선언문, 함수 표현식, 클래스

- non-constructor: 메서드(ES6 메서드 축약 표현), 화살표 함수

```jsx
const baz = {
  x: function () {},
};

new baz.x(); // x {}

const arrow = () => {};
new arrow(); // TypeError: arrow is not a constructor

const obj = {
  x() {},
};

new obj.x(); // TypeError: obj.x is not a constructor
```

ECMAScript사양에서 메서드란 [`ES6의 메서드 축약 표현`](https://github.com/sohyun215/javascript-deepdive/tree/main/ch10_%EA%B0%9D%EC%B2%B4%20%EB%A6%AC%ED%84%B0%EB%9F%B4#1093-%EB%A9%94%EC%84%9C%EB%93%9C-%EC%B6%95%EC%95%BD-%ED%91%9C%ED%98%84)만을 의미한다.

<br>

## new 연산자

new 연산자와 함께 함수를 호출하면 해당 함수는 생성자 함수로 동작한다.

→ 함수 객체의 내부 메서드 [[Call]]이 호출되는 것이 아니라 [[Construct]]가 호출된다.

```jsx
function add(x, y) {
  return x + y;
}

let inst = new add();

// 함수가 객체를 반환하지 않았으므로 반환문이 무시됨 -> 빈 객체가 반환된다.
console.log(inst); // {}
```

```jsx
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function() {
    return 2 * this.radius;
  };
}

// 일반 함수로서 호출
const circle = Circle(5);

console.log(circle); // undefined

console.log(radius); // 5
console..log(getDiameter()); // 10
```

Circle 함수는 일반 함수로서 호출되었기 때문에 Circle 함수 내부의 this는 전역 객체 window를 가리킨다. 따라서 radius 프로퍼티와 getDiameter 메서드는 전역 객체의 프로퍼티와 메서드가 된다.

일반 함수와 생성자 함수는 특별한 형식적 차이가 없기 때문에 생성자 함수는 일반적으로 파스칼 케이스로 명명하여 구별하도록 하자

<br>

## new.target

생성자 함수가 new 연산자 없이 호출되는 것을 방지하기 위해 파스칼 케이스 컨벤션을 사용한다해도 실수는 언제나 발생할 수 있다 .이러한 위험성을 회피하기 위해 ES6에서 `new.target` 을 지원한다.

this와 유사하게 constructor인 모든 함수 내부에서 암묵적인 지역 변수와 같이 사용되며 메타 프로퍼티라고 부른다.

함수 내부에서 new.target을 사용하면 생성자 함수로서 호출되었는지 확인할 수 있다.

> new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target은 **`함수 자신`** 을 가리킨다. 일반 함수로서 호출된 함수 내부의 new.target은 **`undefined`** 이다.

```jsx
function Circle(radius) {
  if (!new.target) {
    // new 연산자와 함께 생성자 함수를 재귀 호출
    return new Circle(raius);
  }
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

const circle = Circle(5);
console.log(circle.getDiameter());
```

### 스코프 세이프 생성자 패턴

> new.target을 사용할 수 없는 상황에 사용

```jsx
function Circle(radius) {
  // 이 함수가 new 연산자와 호출되지 않았다면 이 시점의 this는 전역 객체 window를 가리킨다.
  // 즉, this와 Circle은 프로토타입에 의해 연결되지 않는다.
  if (!(this instanceof Circle)) {
    return new Circle(radius);
  }
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}
```

new 연산자와 함께 생성자 함수에 의해 생성된 객체는 프로토타입에 의해 생성자 함수와 연결된다. 이를 통해 new 연산자와 함께 호출되었는지 확인할 수 있다.

<br>

대부분의 빌트인 생성자 함수는 new 연산자와 함께 호출되었는지 확인한 후 적절한 값을 반환한다.

예를 들어 `Object` 와 `Function` 생성자 함수는 new 연산자 없이 호출해도 new 연산자와 함께 호출했을 때와 동일하게 동작한다.

```jsx
let obj = new Object();
console.log(obj); // {}

obj = Object();
console.log(obj); // {}
```

하지만 `String` , `Number`, `Boolean` 생성자 함수는 new 연산자 없이 호출하면 문자열, 숫자, 불리언 값을 반환한다. 이를 통해 데이터 타입을 변환하기도 한다.

```jsx
const str = String(123);
console.log(str, typeof str); // 123 string

const strObj = new String("Kim");
console.log(strObj, typeof strObj); // String {"Kim"} object
```
