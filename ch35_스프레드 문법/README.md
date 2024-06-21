> ES6에서 도입된 스프레드 문법(전개 문법) `...`은 **하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 목록으로 만든다.**

스프레드 문법을 사용할 수 있는 대상은 Array, String, Map, Set, DOM 컬렉션(NodeList, HTMLCollection), arguments와 같이 for ... of 문으로 순회할 수 있는 **이터러블**에 한정된다.

```js
console.log(...[1, 2, 3]); // 1 2 3

console.log(...'Hello'); // H e l l o

console.log(
  ...new Map([
    ['a', '1'],
    ['b', '2'],
  ]),
); //  ['a', '1'] ['b', '2']

console.log(...new Set([1, 2, 3])); // 1 2 3

console.log(...{ a: 1, b: 2 }); // TypeError: Spread syntax requires ...iterable[Symbol.iterator] to be a function
```

위 예제에서 `...[1, 2, 3]`은 이터러블인 배열을 펼쳐서 요소들을 개별적인 값들의 목록 1 2 3으로 만든다. 이때 1 2 3은 값이 아니라 **값들의 목록이다.**

즉, 스프레드 문법의 결과는 값이 아니다. 이는 스프레드 문법이 피연산자를 연산하여 값을 생성하는 연산자가 아님을 의미한다. 따라서 스프레드 문법의 결과는 변수에 할당할 수 없다.

```js
const list = ...[1, 2, 3]; // SyntaxError: Unexpected token '...'
```

<br />

스프레드 문법은 **쉼표로 구분한 값의 목록을 사용하는 문맥**에서만 사용할 수 있다.

- 함수 호출문의 인수 목록
- 배열 리터럴의 요소 목록

- 객체 리터럴의 프로퍼티 목록

<br />
<br />

# 35.1 함수 호출문의 인수 목록에서 사용하는 경우

요소들의 집합인 배열을 펼쳐서 개별적인 값들의 목록으로 만든 후, 이를 함수의 인수 목록으로 전달해야 하는 경우가 있다.

```js
const arr = [1, 2, 3];

const max = Math.max(arr); // NaN
```

Math.max 메서드는 매개변수 개수를 확정할 수 없는 가변 인자 함수다. 개수가 정해져 있지 않은 여러 개의 숫자를 인수로 전달받아 인수 중에서 최대값을 반환한다.

만약 메서드에 숫자가 아닌 배열을 인수로 전달하면 최대값을 구할 수 없으므로 NaN을 반환한다.

<br />

이 같은 문제를 해결하기 위해 배열을 펼쳐서 요소들을 개별적인 값들의 목록으로 만든 후 인수로 전달해야 한다.

스프레드 문법이 제공되기 이전에는 `Function.prototype.apply`를 사용했다.

```js
// apply 함수의 두 번째 인수(배열)는 apply 함수가 호출하는 함수의 인수 목록이다.
Math.max.apply(null, arr);
```

<br />

스프레드 문법을 사용하면 더 간결하고 가독성이 좋다.

```js
Math.max(...arr);
```

<br />

스프레드 문법은 Rest 파라미터와 형태가 동일하여 혼동할 수 있어서 주의할 필요가 있다.

Rest 파라미터는 **함수에 전달된 인수들의 목록을 배열로 전달받기 위해** 매개변수 이름 앞에 ...을 붙이는 것이다.

스프레드 문법은 여러 개의 값이 하나로 뭉쳐 있는 배열과 같은 **이터러블을 펼쳐서 개별적인 값들의 목록을 만드는 것이**다. 따라서 Rest 파라미터와 스프레드 문법은 서로 반대의 개념이다.

<br />

```js
function foo(...rest) {
  console.log(rest); // 1, 2, 3 -> [1, 2, 3]
}

foo(...[1, 2, 3]); // [1, 2, 3] -> 1, 2, 3
```

<br />
<br />

# 35.2 배열 리터럴 내부에서 사용하는 경우

스프레드 문법을 배열 리터럴에서 사용하면 ES5에서 사용하던 기존의 방식보다 더욱 간결하고 가독성 좋게 표현할 수 있다.

## `concat`

ES5에서 2개의 배열을 1개의 배열로 결합하고 싶은 경우 concat 메서드를 사용해야 한다.

```js
// ES5
var arr = [1, 2].concat([3, 4]);
console.log(arr); // [1, 2, 3, 4]
```

스프레드 문법을 사용하면 별도의 메서드를 사용하지 않고 배열을 결합할 수 있다.

```js
const arr = [...[1, 2], ...[3, 4]];
```

<br />

## `splice`

ES5에서 어떤 배열 중간에 다른 배열의 요소들을 추가하거나 제거하려면 splice 메서드를 사용한다. 메서드의 세 번째 인수로 배열을 전달하면 배열 자체가 추가된다.

```js
var arr1 = [1, 4];
var arr2 = [2, 3];

arr1.splice(1, 0, arr2);
console.log(arr1); // [1, [2, 3], 4 ]
```

위 예제의 경우 세 번째 인수 arr2를 2, 3으로 해체하여 전달해야 한다. 그렇지 않으면 arr1에 arr2 배열 자체가 추가된다.

이러한 경우 Function.prototype.`apply` 메서드를 사용하여 splice 메서드를 호출해야 한다.

```js
/*
  apply 메서드의 2번째 인수는 apply 메서드가 호출한 splice 메서드의 인수 목록
  apply 메서드의 2번째 인수 [1, 0].concat(arr2)는 [1, 0, 2, 3]으로 평가된다.
  따라서 splice 메서드에 [1, 0, 2, 3]이 해체되어 전달된다.
  즉, arr1[1]부터 0개의 요소를 제거하고 그 자리(arr1[1])에 새로운 요소 (2, 3)을 삽입한다.
*/
Array.prototype.splice.apply(arr1, [1, 0].concat(arr2));
console.log(arr1); // [1, 2, 3, 4]
```

스프레드 문법을 사용하면 간결하고 가독성이 좋다.

```js
const arr1 = [1, 4];
const arr2 = [2, 3];

arr1.splice(1, 0, ...arr2);
```

<br />

## 배열 복사

ES5에서 배열을 복사하려면 `slice` 메서드를 사용한다.

```js
var origin = [1, 2];
var copy = origin.slice();

console.log(copy); // [1, 2]
console.log(copy === origin); // false
```

```js
const origin = [1, 2];
const copy = [...origin];

console.log(copy); // [1, 2]
console.log(copy === origin); // false
```

이때 원본 배열의 각 요소를 얕은 복사하여 새로운 복사본을 생성한다. 이는 slice 메서드도 마찬가지다.

<br />

## 이터러블을 배열로 변환

ES5에서 이터러블을 배열로 변환하려면 apply 또는 call 메서드를 사용하여 slice 메서드를 호출해야 한다.

```js
function sum() {
  // 이터러블이면서 유사 배열 객체인 arguments를 배열로 변환
  var args = Array.prototype.slice.call(arguments);

  return args.reduce(function (pre, cur) {
    return pre + cur;
  }, 0);
}

console.log(sum(1, 2, 3)); // 6
```

스프레드 문법을 사용하면 좀 더 간편하게 이터러블을 배열로 변환할 수 있다.

```js
function sum() {
  return [...arguments].reduce((pre, cur) => pre + cur, 0);
}
```

위 예제보다 나은 방법은 Rest 파라미터를 사용하는 것이다.

```js
const sum = (...args) => args.reduce((pre, cur) => pre + cur, 0);
```

<br />

단, 이터러블이 아닌 유사 배열 객체는 스프레드 문법의 대상이 될 수 없다.

```js
const arrayLike = {
  0: 1,
  1: 2,
  2: 3,
  length: 3,
};

// const arr = Array.prototype.slice.call(arrayLike);
// [1, 2, 3]

const arr = [...arrayLike]; // TypeError: arrayLike is not iterable
```

이터러블이 아닌 유사 배열 객체를 배열로 변경하려면 ES6에서 도입된 `Array.from` 메서드를 사용한다.

```js
Array.from(arrayLike);
```

<br />
<br />

# 35.3 객체 리터럴 내부에서 사용하는 경우

객체 리터럴의 프로퍼티 목록에서도 스프레드 문법을 사용할 수 있다.

```js
const obj = { x: 1, y: 2 };
const copy = { ...obj };

console.log(copy); // {x: 1, y: 2}
console.log(obj === copy); // false

const merged = { x: 1, y: 2, ...{ a: 3, b: 4 } };
console.log(merged); // {x: 1, y: 2, a: 3, b: 4}
```

<br />

이전에는 ES6에서 도입된 `Object.assign` 메서드를 사용하여 여러 개의 객체를 병합하거나 특정 프로퍼티를 변경 또는 추가했다.

```js
// 프로퍼티가 중복되는 경우 뒤에 위치한 프로퍼티가 우선권을 갖는다.
const merged = Object.assign({}, { x: 1, y: 2 }, { y: 10, z: 3 });
console.log(merged); // {x: 1, y: 10, z: 3}

const changed = Object.assign({}, { x: 1, y: 2 }, { y: 100 });
console.log(changed); // {x: 1, y: 100}

const added = Object.assign({}, { x: 1, y: 2 }, { z: 0 });
console.log(added); // {x: 1, y: 2, z: 0}
```

스프레드 프로퍼티는 Object.assign 메서드를 대체할 수 있는 간편한 문법이다.

```js
const merged = { ...{ x: 1, y: 2 }, ...{ y: 10, z: 3 } };

const changed = { ...{ x: 1, y: 2 }, y: 100 };

const added = { ...{ x: 1, y: 2 }, z: 0 };
```

<br />
