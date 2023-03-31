# 일급 객체

### 📌 일급 객체의 조건

1.  무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.

2.  변수나 자료구조에 저장할 수 있다.
3.  함수의 매개변수에 전달할 수 있다.
4.  함수의 반환값으로 사용할 수 있다.

```jsx
// 런타임에 함수 리터럴이 평가되어 함수 객체가 생성되고 변수에 할당된다.
const increase = function (num) {
  return ++num;
};

const decrease = function (num) {
  return --num;
};

// 함수는 객체에 저장할 수 있다.
const auxs = { increase, decrease };

// 함수의 매개변수에 전달할 수 있다.
function makeCounter(aux) {
  let num = 0;
  // 함수의 반환값으로 사용할 수 있다.
  return function () {
    num = aux(num);
    return num;
  };
}
```

함수는 조건을 모두 만족하므로 일급 객체이다. 따라서 함수는 값으로 사용할 수 있는 곳이라면 어디서든지 리터럴로 정의할 수 있으며 런타임에 함수 객체로 평가된다.

일반 객체는 호출할 수 없지만 함수 객체는 호출할 수 있다. 그리고 함수 객체는 일반 객체에는 없는 함수 고유의 프로퍼티를 소유한다.

<br>

# 함수 객체의 프로퍼티

```jsx
function square(number) {
  return number * number;
}
console.dir(square);
```

<img src="https://user-images.githubusercontent.com/79398566/229028220-5ad21537-a2fd-4d1b-b1c9-38f57d7e4581.png" width=500px>

<br>

square 함수의 모든 프로퍼티의 프로퍼티 어트리뷰트를 `Object.getOwnPropertyDescriptors` 메서드로 확인하면 다음과 같이 나온다.

```jsx
console.log(Object.getOwnPropertyDescriptors(square));
```

![image](https://user-images.githubusercontent.com/79398566/229028440-780ab0ae-04c7-4cee-8bc9-21c15bbf7558.png)

arguments, caller, length, name, prototype 프로퍼티는 일반 객체에는 없는 함수 객체 고유의 프로퍼티다.
<br>

## `arguments` 프로퍼티

함수 객체의 arguments 프로퍼티 값은 arguments 객체다. 이는 함수 호출 시 전달된 인수들의 정보를 담고 있는 `순회 가능한 유사 배열 객체`이며, 함수 내부에서 지역 변수처럼 사용된다.

함수 객체의 arguments 프로퍼티는 현재 일부 브라우저에서 지원하고 있지만 ES3부터 표준에서 폐지되었다. 따라서 Function.arguments와 같은 사용법은 권장되지 않으며 함수 내부에서 지역 변수처럼 사용할 수 있는 arguments 객체를 참조하도록 한다.

```jsx
function multiply(x, y) {
  console.log(arguments);
  return x * y;
}

console.log(multiply()); // NaN
console.log(multiply(1)); // NaN
console.log(multiply(1, 2, 3)); // 2
// 자바스크립트는 함수의 매개변수와 인수의 개수가 일치하는지 확인하지 않는다.
```

함수를 정의할 때 선언한 매개변수는 함수 몸체 내부에서 변수와 동일하게 취급된다. 즉, 함수가 호출되면 함수 몸체 내에서 암묵적으로 매개변수가 선언되고 undefined로 초기화된 이후 인수가 할당된다.

선언된 매개변수의 개수보다 인수를 적게 전달했을 경우 인수가 전달되지 않은 매개변수는 undefined로 초기화된 상태를 유지하고, 인수를 더 많이 전달한 경우 초과된 인수는 무시된다.

multiply 함수에서 `console.log(arguments)` 의 결과이다.

![image](https://user-images.githubusercontent.com/79398566/229028748-576168ed-5d02-4373-857f-b15a10962a8c.png)

- arguments 객체는 인수를 프로퍼티 값으로 소유하며 프로퍼티 키는 인수의 순서를 나타낸다.

- callee 프로퍼티: 호출되어 arguments 객체를 생성한 함수, 즉 함수 자신

- length 프로퍼티: 인수의 개수

- Symbol(Symbol.iterator) 프로퍼티: arguments 객체를 순회 가능한 자료구조인 이터러블로 만들기 위한 프로퍼티, Symbol.iterator를 프로퍼티 키로 사용한 메서드를 구현하는 것에 의해 이터러블이 된다.

  ```jsx
  function multiply(x, y) {
    const iterator = arguments[Symbol.iterator]();

    console.log(iterator.next()); // { value: 1, done: false };
    console.log(iterator.next()); // { value: 2, done: false };
    console.log(iterator.next()); // { value: 3, done: false };
    console.log(iterator.next()); // { value: undefined, done: true };
    return x * y;
  }
  multiply(1, 2, 3);
  ```

선언된 매개변수의 개수와 함수를 호출할 때 전달하는 인수의 개수를 확인하지 않는 자바스크립트의 특성 때문에 함수가 호출되면 인수 개수를 확인하고 이에 따라 함수의 동작을 달리 정의할 필요가 있을 수 있다.

arguments 객체는 매개변수 개수를 확정할 수 없는 `가변 인자 함수`를 구현할 때 유용하다.

```jsx
function sum() {
  let res = 0;
  for (let i = 0; i < arguments.length; i++) {
    res += arguments[i];
  }
  return res;
}

console.log(sum()); // 0
console.log(sum(1, 2)); // 3
```

arguments 객체는 배열 형태로 인자 정보를 담고 있지만 실제 배열이 아닌 `유사 배열 객체` 다.

✔️ 유사 배열 객체

> length 프로퍼티를 가진 객체로 for 문으로 순회할 수 있는 객체

ES6에서 도입된 이터레이션 프로포콜을 준수하면 순회 가능한 자료구조인 이터러블이 된다. 이터러블의 개념이 없었던 ES5에서 arguments 객체는 유사 배열 객체로 구분되었다. 하지만 ES6부터 arguments 객체는 유사 배열 객체이면서 동시에 이터러블이다.

유사 배열 객체는 배열이 아니므로 배열 메서드를 사용할 경우 에러가 발생한다. 따라서 배열 메서드를 사용하려면 `Function.prototype.call`, `Function.prototype.apply` 를 사용해 간접 호출해야 하는 번거로움이 있다.

```jsx
function sum() {
  // arguments 객체를 배열로 변환
  const array = Array.prototype.slice.call(arguments);
  return array.reduce(function (pre, cur) {
    return pre + cur;
  }, 0);
}
```

이러한 번거로움을 해결하기 위해 ES6에서는 `Rest 파라미터`를 도입했다.

```jsx
function sum(...args) {
  return args.reduce((pre, cur) => pre + cur, 0);
}
```

<br>

## `caller` 프로퍼티

> 함수 자신을 호출한 함수를 가리킨다.

caller 프로퍼티는 ECMAScript 사양에 포함되지 않은 프로퍼티다. 이후 표준화될 예정도 없는 프로퍼티이므로 사용하지 말고 참고로만 알아두자.

<br>

## `length` 프로퍼티

> 함수를 정의할 때 선언한 매개변수의 개수를 가리킨다.

arguments 객체의 length 프로퍼티는 `인자의 개수`를 가리키고, 함수 객체의 length 프로퍼티는 `매개변수의 개수`를 가리킨다는 점을 주의하자.

<br>

## `name` 프로퍼티

> 함수 이름

name 프로퍼티는 ES6 이전까지는 비표준이었다가 ES6에서 정식 표준이 되었다.

익명 함수 표현식의 경우 ES5에서 name 프로퍼티는 빈 문자열을 값으로 갖는다. 하지만 ES6에서는 함수 객체를 가리키는 식별자를 값으로 갖는다.

<br>

## `prototype` 프로퍼티

> 함수가 객체를 생성하는 생성자 함수로 호출될 때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킨다.

생성자 함수로 호출할 수 있는 함수 객체, 즉 `constructor`만이 소유하는 프로퍼티다.

```jsx
(function () {}).hasOwnProperty("prototype"); // true
({}).hasOwnProperty("prototype"); // false
```
