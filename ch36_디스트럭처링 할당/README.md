> 디스트럭처링 할당(구조 분해 할당)은 **구조화된 배열과 같은 이터러블 또는 객체를 destructuring(비구조화, 구조 파괴)하여 1개 이상의 변수에 개별적으로 할당**하는 것을 말한다.
>
> 배열과 같은 이터러블 또는 객체 리터럴에서 필요한 값만 추출하여 변수에 할당할 때 유용하다.

<br />

# 36.1 배열 디스트럭처링 할당

ES5에서 구조화된 배열을 디스트럭처링하여 1개 이상의 변수에 할당하는 방법은 다음과 같다.

```js
var arr = [1, 2, 3];

var one = arr[0];
var two = arr[1];
var three = arr[2];
```

ES6의 배열 디스트럭처링 할당은 배열의 각 요소를 배열로부터 추출하여 1개 이상의 변수에 할당한다.

이때 **배열 디스트럭처링 할당의 대상(할당문의 우변)은 이터러블이어야 하며, 할당 기준은 배열의 인덱스다.** 즉, 순서대로 할당된다.

```js
const arr = [1, 2, 3];

const [one, two, three] = arr;
```

배열 디스트럭처링 할당을 위해서는 할당 연산자 왼쪽에 값을 할당받을 변수를 선언해야 한다. 이때 변수를 리터럴 형태로 선언한다.

```js
const [x, y] = [1, 2];
```

<br />

이때 우변에 이터러블을 할당하지 않으면 에러가 발생한다.

```js
const [x, y]; // SyntaxError: Missing initializer in destructuring declaration

const [a, b] = {}; // TypeError: {} is not iterable
```

배열 디스트럭처링 할당의 변수 선언문은 다음처럼 선언과 할당을 분리할 수도 있다. 단, 이 경우 const 키워드로 변수를 선언할 수 없으므로 지양하자.

```js
let x, y;
[x, y] = [1, 2];
```

<br />

배열 디스트럭처링 할당의 기준은 배열의 인덱스다. 이때 변수의 개수와 이터러블의 개수가 반드시 일치할 필요는 없다.

```js
const [c, d] = [1];
console.log(c, d); // 1 undefined

const [e, f] = [1, 2, 3];
console.log(e, f); // 1

const [g, , h] = [1, 2, 3];
console.log(g, h); // 1 3
```

<br />

배열 디스트럭처링 할당을 위한 변수에 기본값을 설정할 수 있다.

```js
const [a, b, c = 3] = [1, 2];
console.log(a, b, c); // 1 2 3

// 기본값보다 할당된 값이 우선
const [e, f = 10, g = 3] = [1, 2];
console.log(e, f, g); // 1 2 3
```

<br />

배열 디스트럭처링 할당을 위한 변수에 Rest 파라미터와 유사하게 **`Rest 요소 ...`** 을 사용할 수 있다. Rest 요소는 Rest 파라미터와 마찬가지로 반드시 마지막에 위치해야 한다.

```js
const [x, ...y] = [1, 2, 3];
console.log(x, y); // 1 [2, 3]
```

<br />
<br />

# 36.2 객체 디스트럭처링 할당

ES5에서 객체의 각 프로퍼티를 객체로부터 디스트럭처링하여 변수에 할당하기 위해서는 프로퍼티 키를 사용해야 한다.

```js
var user = { firstName: 'So', lastName: 'Kim' };

var firstName = user.firstName;
var lastName = user.lastName;
```

ES6의 객체 디스트럭처링 할당은 객체의 각 프로퍼티를 객체로부터 추출하여 1개 이상의 변수에 할당한다.

이때 객체 디스트럭처링 할당의 대상(할당문의 우변)은 객체이어야 하며, **할당 기준은 `프로퍼티 키`다.** 즉, 순서는 의미가 없으며 선언된 변수 이름과 프로퍼티 키가 일치하면 할당된다.

```js
const { lastName, firstName } = user;
```

<br />

객체 디스트럭처링 할당을 위해서는 할당 연산자 왼쪽에 프로퍼티 값을 할당받을 변수를 선언해야 한다. 이때 변수를 객체 리터럴 형태로 선언한다.

```js
const { lastName, firstName } = { firstName: 'So', lastName: 'Kim' };
```

<br />

우변에 객체 또는 객체로 평가될 수 있는 표현식(문자열, 숫자, 배열 등)을 할당하지 않으면 에러가 발생한다.

```js
const { lastName, firstName }; // SyntaxError: Missing initializer in destructuring declaration

const { lastName, firstName } = null; // TypeError: Cannot destructure property 'lastName' of 'null' as it is null.
```

<br />

위 예제에서 객체 리터럴 형태로 선언한 변수는 lastName, firstName 이다. 이는 프로퍼티 축약 표현을 통해 선언한 것이다.

```js
const { lastName, firstName } = user;
// 위와 아래는 동치
const { lastName: lastName, firstName: firstName } = user;
```

<br />

따라서 객체의 프로퍼티 키와 다른 변수 이름으로 값을 할당받으려면 다음과 같이 변수를 선언한다.

```js
const { lastName: ln, firstName: fn } = user;
```

<br />

객체 디스트럭처링 할당을 위한 변수에 기본값을 설정할 수 있다.

```js
const { firstName = 'Bori', lastName } = { lastName: 'Kim' };

const { firstName: fn = 'Sohyun', lastName: ln } = { lastName: 'Kim' };
console.log(fn, ln); // Sohyun Kim
```

<br />

객체에서 프로퍼티 키로 필요한 프로퍼티 값만 추출하여 변수에 할당하고 싶을 때 유용하다.

```js
const str = 'Hello';
const { length } = str;
console.log(length); // 5

const todo = { id: 1, content: 'HTML', completed: true };
const { id } = todo;
console.log(id); // 1
```

<br />

객체 디스트럭처링 할당은 객체를 인수로 전달받는 함수의 매개변수에도 사용할 수 있다.

```js
function printTodo({ content, completed }) {
  console.log(`할일 ${content}은 ${completed ? '완료' : '비완료'} 상태입니다.`);
}

printTodo({ id: 1, content: 'HTML', completed: true });
```

<br />

배열의 요소가 객체인 경우 배열 디스트럭처링 할당과 객체 디스트럭처링 할당을 혼용할 수 있다.

```js
const todos = [
  { id: 1, content: 'HTML', completed: true },
  { id: 2, content: 'CSS', completed: true },
  { id: 3, content: 'JS', completed: false },
];

const [, { id }] = todos;
console.log(id); // 2
```

중첩 객체의 경우는 다음과 같이 사용한다.

```js
const user = {
  name: 'Sohyun',
  address: {
    zipCode: '12345',
    city: 'Seoul',
  },
};

const {
  address: { city },
} = user;
console.log(city); // Seoul
```

<br />

객체 디스트럭처링 할당을 위한 변수에 Rest 파라미터나 Rest 요소와 유사하게 **`Rest 프로퍼티 ...`** 을 사용할 수 있다. Rest 프로퍼티는 Rest 파라미터나 Rest 요소와 마찬가지로 반드시 마지막에 위치해야 한다.

```js
const { x, ...rest } = { x: 1, y: 2, z: 3 };
console.log(x, rest); // 1 {y: 2, z: 3}
```

<br />
