# 31.1 정규 표현식이란?

> 정규 표현식(regular expression)은 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어다.

정규 표현식은 문자열을 대상으로 **패턴 매칭 기능**을 제공한다. 패턴 매칭 기능은 특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 수 있는 기능을 말한다.

예를 들어. 휴대폰 전화번호는 `숫자 3개 + '-' + 숫자 4개 + '-' + 숫자 4개`라는 일정한 패턴이 있다.

```js
const tel = '010-1234-567팔';

const regExp = /^\d{3}-\d{4}-\d{4}/;

regExp.test(tel); // false
```

정규 표현식을 사용하지 않는다면 반복문과 조건문을 통해 한 문자씩 연속해서 체크해야 한다.

정규 표현식을 사용하면 패턴을 정의하고 테스트하는 것으로 간단히 체크할 수 있지만 주석이나 공백을 허용하지 않고 여러가지 기호를 혼합하여 사용하기 때문에 가독성이 좋지 않다.

<br />

# 31.2 정규 표현식의 생성

정규 표현식 객체(`RegExp` 객체)를 생성하기 위해서는 정규 표현식 리터럴과 RegExp 생성자 함수를 사용할 수 있다.

일반적인 방법은 정규 표현식 리터럴을 사용하는 것이다.

`/regexp/i`

여기서 `/`는 각각 `시작, 종료 기호`를 나타내고, regexp자리에 오는 것이 **`패턴`**, i자리에 **`플래그`** 가 오게 된다.

```js
const target = 'Is this all there is?';

// 패턴: is
// 플래그 : i -> 대소문자를 구별하지 않고 검색한다.
const regexp = /is/i;

regexp.test(target); // true
```

<br />

RegExp 생성자 함수를 사용하여 RegExp 객체를 생성할 수도 있다.

`new RegExp(pattern[, flags])`

- pattern: 정규 표현식의 패턴
- flags: 정규 표현식의 플래그(g, i, m, u, y)

```js
const regexp = new RegExp(/is/i);
// const regexp = new RegExp(/is/, 'i');
// const regexp = new RegExp('is', 'i');
```

<br />

생성자 함수를 사용하면 변수를 사용해 동적으로 RegExp 객체를 생성할 수도 있다.

```js
const count = (str, char) => (str.match(new RegExp(char, 'gi')) ?? []).length;

count('Is this all there is?', 'is'); // 3
count('Is this all there is?', 'xx'); // 0
```

<br />
<br />

# 31.3 RegExp 메서드

## `RegExp.prototype.exec`

> 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 `배열`로 반환한다.
>
> 매칭 결과가 없는 경우 `null`을 반환한다.

```js
const target = 'Is this all there is?';
const regExp = /is/;

regExp.exec(target);
// ['is', index: 5, input: 'Is this all there is?', groups: undefined]
```

exec 메서드는 문자열 내의 모든 패턴을 검색하는 g 플래그를 지정해도 **첫 번째 매칭 결과만 반환**하므로 주의하자.

<br />

## `RegExp.prototype.test`

> 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 `불리언 값`으로 반환한다.

<br />

## `String.prototype.match`

> String 표준 빌트인 객체가 제공하는 match 메서드는 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환한다.

```js
const target = 'Is this all there is?';
const regExp = /is/;

target.match(regExp);
// ['is', index: 5, input: 'Is this all there is?', groups: undefined]
```

match 메서드는 exec 메서드와는 달리 g 플래그가 지정되면 **모든 매칭 결과를 배열로 반환한다.**

```js
const target = 'Is this all there is?';
const regExp = /is/g;

target.match(regExp); // ['is', 'is']
```

<br />
<br />

# 31.4 플래그

플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용한다.

중요한 3개의 플래그는 다음과 같다.

| 플래그 | 의미        | 설명                                                            |
| ------ | ----------- | --------------------------------------------------------------- |
| i      | Ignore case | 대소문자를 구별하지 않고 패턴을 검색한다.                       |
| g      | Global      | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다. |
| m      | Multi line  | 문자열의 행이 바뀌더라도 패턴 검색을 계속한다.                  |

<br />

플래그는 옵션이므로 선택적으로 사용할 수 있으며, 순서와 상관없이 하나 이상의 플래그를 동시에 설정할 수도 있다.

어떠한 플래그를 사용하지 않은 경우 대소문자를 구별해서 패턴을 검색한다.
그리고 문자열에 패턴 검색 매칭 대상이 1개 이상 존재해도 첫 번째 매칭한 대상만 검색하고 종료한다.

```js
const target = 'Is this all there is?';

// is 문자열을 대소문자를 구별하여 한 번만 검색한다.
target.match(/is/);
// ['is', index: 5, input: 'Is this all there is?', groups: undefined]

// 대소문자를 구별하지 않고 한 번만 검색한다.
target.match(/is/i);
// ['is', index: 0, input: 'Is this all there is?', groups: undefined]

// 대소문자를 구별하여 전역 검색한다.
target.match(/is/g); // ['is', 'is']

// 대소문자를 구별하지 않고 전역 검색한다.
target.match(/is/gi); // ['Is', 'is', 'is']
```

<br />
<br />

# 31.5 패턴

패턴은 /로 열고 닫으며 문자열의 따옴표는 생략한다. 따옴표를 포함하면 따옴표까지도 패턴에 포함되어 검색된다.

또한 패턴은 특별한 의미를 가지는 메타문자 또는 기호로 표현할 수 있다.

어떤 문자열 내에 패턴과 일치하는 문자열이 존재할 때 '정규 표현식과 매치한다'고 표현한다.

## 문자열 검색

RegExp 메서드를 사용해 검색 대상 문자열과 정규 표현식의 매칭 결과를 구하면 검색이 수행된다.

검색 대상 문자열과 플래그를 생략한 정규 표현식의 매칭 결과를 구하면 대소문자를 구별하여 정규 표현식과 매치한 첫 번째 결과만 반환한다.

```js
const target = 'Is this all there is?';

const regExp = /is/;

target.match(regExp);
```

대소문자를 구별하지 않고 검색하려면 플래그 i를 사용, 패턴과 매치하는 모든 문자열을 전역 검색하려면 플래그 g를 사용하면 된다.

<br />

## 임의의 문자열 검색

`.`은 임의의 문자 한 개를 의미한다. 문자의 내용은 무엇이든 상관없다.

```js
const target = 'Is this all there is?';

// 임의의 3자리 문자열을 대소문자를 구별하여 전역 검색한다.
const regExp = /.../g;

target.match(regExp);
// ['Is ', 'thi', 's a', 'll ', 'the', 're ', 'is?']
```

<br />

## 반복 검색

`{m,n}`은 앞선 패턴이 **최소 m번, 최대 n번** 반복되는 문자열을 의미한다.

⚠️ 콤마 뒤에 공백이 있으면 정상 동작하지 않는다.

```js
const target = 'A AA B BB Aa Bb AAA';

const regExp = /A{1,2}/g;

target.match(regExp); // ['A', 'AA', 'A', 'AA', 'A']
```

<br />

`{n}`은 앞선 패턴이 n번 반복되는 문자열을 의미한다. {n}은 {n,n}과 같다.

```js
const regExp = /A{2}/g;

target.match(regExp); // ['AA', 'AA']
```

<br />

`{n,}`은 앞선 패턴이 최소 n번 이상 반복되는 문자열을 의미한다.

```js
const regExp = /A{2,}/g;

target.match(regExp); // ['AA', 'AAA']
```

<br />

`+`는 앞선 패턴이 최소 한 번 이상 반복되는 문자열을 의미한다. 즉, +는 {1,}과 같다.

```js
const regExp = /A+/g;

target.match(regExp); // ['A', 'AA', 'A', 'AAA']
```

<br />

`?`는 앞선 패턴이 최대 한 번(0번 포함) 이상 반복되는 문자열을 의미한다. 즉, ?는 {0,1}과 같다.

```js
const target = 'color colour';

// colo 다음 'u'가 최대 한 번(0번 포함) 이상 반복되고 'r'이 이어지는 문자열을 검색한다.
const regExp = /colou?r/g;

target.match(regExp); // ['color', 'colour']
```

<br />

## OR 검색

`|`는 or의 의미를 갖는다.

```js
const target = 'A AA B BB Aa Bb';

// A 또는 B를 전역 검색한다.
const regExp = /A|B/g;

target.match(regExp); // ['A', 'A', 'A', 'B', 'B', 'B', 'A', 'B']
```

분해되지 않은 단어 레벨로 검색하기 위해서는 `+`를 함께 사용한다.

```js
// A 또는 B가 한 번 이상 반복되는 문자열을 전역 검색
const regExp = /A+|B+/g;

target.match(regExp); // ['A', 'AA', 'B', 'BB', 'A', 'B']
```

위 예제는 패턴을 or로 한 번 이상 반복하는 것인데 이를 간단히 표현하면 다음과 같다. `[]`내의 문자는 or로 동작한다.
그 뒤에 +를 사용하면 앞선 패턴을 한 번 이상 반복한다.

```js
// A 또는 B가 한 번 이상 반복되는 문자열을 전역 검색
const regExp = /[AB]+/g;

target.match(regExp); // ['A', 'AA', 'B', 'BB', 'A', 'B']
```

<br />

범위를 지정하려면 [] 내에 `-`를 사용한다.

```js
const target = 'A AA BB ZZ Aa Bb';

// 'A' ~ 'Z'가 한 번 이상 반복되는 문자열 전역 검색
const regExp = /[A-Z]+/g;
target.match(regExp); // ['A', 'AA', 'BB', 'ZZ', 'A', 'B']
```

<br />

대소문자를 구별하지 않고 알파벳을 검색하는 방법은 다음과 같다.

```js
const regExp = /[A-Za-z]+/g;
```

숫자를 검색하는 방법은 다음과 같다.

```js
const target = 'AA BB 12,345';

const regExp = /[0-9]+/g;

target.match(regExp); // ['12', '345']
```

```js
const target = 'AA BB 12,345';

const regExp = /[0-9,]+/g;

target.match(regExp); // ['12,345']
```

위 예제를 간단히 표현하면 다음과 같다. `\d`는 숫자를 의미한다. 즉, \d는 [0-9]와 같다.

`\D`는 \d와 반대로 동작한다. 즉 \D는 숫자가 아닌 문자를 의미한다.

```js
const target = 'AA BB 12,345';

let regExp = /[\d,]+/g;

target.match(regExp); // ['12,345']

// '0'~'9'가 아닌 문자(숫자가 아닌 문자) 또는 ','가 한 번 이상 반복되는 문자열 전역 검색
regExp = /[\D,]+/g;

target.match(regExp); // ['AA BB ', ',']
```

<br />

`\w`는 알파벳, 숫자, 언더스코어를 의미한다. 즉, \w는 [A-Za-z0-9_]와 같다.

\W는 \w와 반대로 동작한다. 즉, 알파벳, 숫자, 언더스코어가 아닌 문자를 의미한다.

```js
const target = 'Aa Bb 12,345 _$&&';

let regExp = /[\w,]+/g;

target.match(regExp); // ['Aa', 'Bb', '12,345', '_']

regExp = /[\W,]+/g;

target.match(regExp); // [' ', ' ', ',', ' $&&']
```

<br />

## NOT 검색

[ ... ] **내**의 `^`은 not의 의미를 갖는다.

예를 들어 [^0-9]는 숫자를 제외한 문자를 의미하고, 이것은 \D와 같은 것이다.

```js
const target = 'AA BB 12 Aa Bb';

const regExp = /[^0-9]+/g;

target.match(regExp); // ['AA BB ', ' Aa Bb']
```

<br />

## 시작 위치로 검색

[ ... ] **밖**의 `^`은 문자열의 시작을 의미한다.

```js
const target = 'https://github.com/sohyun215';

// 'https'로 시작하는지 검사한다.
const regExp = /^https/;

regExp.test(target);
```

<br />

## 마지막 위치로 검색

`$`는 문자열의 마지막을 의미한다.

```js
// 'com'으로 끝나는지 검사한다.
const regExp = /com$/;
```

<br />
<br />

# 31.6 자주 사용하는 정규표현식

## 특정 단어로 시작하는지 검사

```js
const regExp = /^https?:\/\//;

// 위와 동일
const regExp = /^(http|https):\/\//;
```

위 예제는 문자열이 'http://'또는 'https://'로 시작하는지 검사하는 정규표현식이다.
^은 문자열의 시작을 의미하고 ?은 앞선 패턴(여기선 's')이 최대 한 번 이상 반복되는지를 의미한다.

<br />

## 특정 단어로 끝나는지 검사

`$`를 사용하여 검사한다.

```js
// 'html로 끝나는지 검사'
const regExp = /html$/;
```

<br />

## 숫자로만 이루어진 문자열인지 검사

```js
const regExp = /^\d+$/;
```

처음과 끝이 숫자이고 최소 한 번 이상 반복되는 문자열인지 검사한다.

## 하나 이상의 공백으로 시작하는지 검사

`\s`는 여러 가지 공백 문자(스페이스, 탭 등)를 의미한다.

즉 \s는 [\t\r\n\v\f]와 같은 의미다.

```js
const regExp = /^[\s]+/;
```

<br />

## 아이디로 사용 가능한지 검사

다음 예제는 검색 대상 문자열이 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4~10자리인지 검사한다.

```js
const id = 'abc123';

const regExp = /^[A-Za-z0-9]{4,10}$/;

regExp.test(id);
```

## 메일 주소 형식에 맞는지 검사

```js
const regExp =
  /^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/;
```

<br />

## 특수 문자 포함 여부 검사

```js
const target = 'abc#123';

const regExp = /[^A-Za-z0-9]/gi;

regExp.test(target); // true
```

다음 방식으로 대체해 사용할 수도 있다. 이 방식은 특수 문자를 선택적으로 검사할 수 있다는 장점이 있다.

```js
/[\{\}\[\]\/?.,;:|\)*~`!^\-_+<>@\#$%&\\\=\(\'\"]/gi.test(target);
```

특수 문자를 제거할 때는 String.prototype.replace 메서드를 사용한다.

```js
target.replace(/[^A-Za-z0-9]/gi, '');
```
