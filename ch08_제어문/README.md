# 제어문

# 8.1 블록문

- 0개 이상의 문을 중괄호로 묶은 것으로, 코드 블록 또는 블록이라고 부르기도 한다.

- 자바스크립트는 블록문을 하나의 실행 단위로 취급한다. 블록문은 단독으로 사용할 수도 있으나 일반적으로 제어문이나 함수를 정의할 때 사용한다.

- 블록문은 언제나 문의 종료를 의미하는 자체 종결성을 갖기 때문에 블록문 끝에는 세미콜론을 붙이지 않는다.

<br><br>

# 8.2 조건문
> 주어진 조건식(불리언 값으로 평가될 수 있는 표현식)의 평가 결과에 따라 코드 블록의 실행을 결정한다.
- if...else문, switch 문

## 8.2.1 if…else 문
```jsx
if (조건식) {
  // 조건식1이 참일 때 실행
} else if (조건식2) {
  // 조건식2가 참일 때 실행
}
else {
  // 조건식1, 2가 모두 거짓일 때 실행
}
```

- 조건식이 불리언 값이 아닌 값으로 평가되면 자바스크립트 엔진에 의해 암묵적 타입 변환에 의해 실행할 코드 블록을 결정한다.

- 코드 블록 내의 문이 하나뿐이면 중괄호를 생략 가능하다.
- 대부분의 if…else문은 삼항 조건 연산자로 바꿔쓸 수 있다.

<br>

## 8.2.2 switch 문

- 주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case 문으로 실행 흐름을 옮긴다. switch문의 표현식과 일치하는 case 문이 없다면 실행 순서는 default 문으로 이동한다.

- switch 문은 논리적 참, 거짓보다는 다양한 상황에 따라 실행할 코드 블록을 결정할 때 사용한다.
- default 문은 선택사항이다.

- if…else문으로 해결할 수 있다면 switch 문보다 if…else 문을 사용하는 편이 좋고, 만약 조건이 너무 많아서 switch문을 사용했을 때 가독성이 더 좋다면 switch문을 사용하자.

```jsx
switch (표현식) {
  case 표현식1:
    // switch 문의 표현식과 표현식1이 일치하면 실행될 문;
    break;
  case 표현식2:
    // 표현식과 표현식2가 일치;
    break;
  default:
    // switch 문의 표현식과 일치하는 case 문이 없을 때 실행;
}
```


- `폴스루(fall through)`: break 문이 없다면 case 문의 표현식과 일치하지 않더라도 실행 흐름이 다음 case 문으로 연이어 이동한다.

폴스루가 유용한 경우도 있다. 폴스루를 활용해 여러 개의 case 문을 하나의 조건으로 사용할 수도 있다.

#### 윤년인지 판별해서 2월의 일수를 계산하는 예제

```jsx
윤년 계산 알고리즘
1. 연도가 4로 나누어떨어지는 해는 윤년이다.
2. 연도가 4로 나누어떨어지더라도 연도가 100으로 나누어떨어지는 해는 평년이다.
3. 연도가 400으로 나누어떨어지는 해는 윤년이다.
```

```jsx
var year = 2000;
var month = 2;
var days = 0;

switch (month) {
  case 1: case 3: case 5: case 7: case 8: case 10: case 12:
	  days = 31;
	  break;
  case 4: case 6: case 9: case 11:
    days = 30;
    break;
  case 2:
    days = ((year % 4 === 0 && year % 100 !== 0) || (year % 400 === 0)) ? 29 : 28;
    break;
  default: 
    console.log('Invalid month');
}

console.log(days); // 29
```

<br>
<br>

# 8.3 반복문
> 조건식의 평가 결과가 참인 경우 코드 블록 실행, 조건식이 거짓일 때까지 반복한다.


## 8.3.1 for 문

```jsx
for ( 변수 선언문 또는 할당문; 조건식; 증감식) {
	조건식이 참인 경우 반복 실행될 문;
}
```

- 변수 선언문은 단 한 번만 실행된다.
- 변수 선언문, 조건식, 증감식은 모두 옵션이다. 단, 어떤 식도 선언하지 않으면 무한루프가 된다.
<br>

## 8.3.2 while 문
- 주어진 조건식의 평가 결과가 참이면 코드 블록을 계속해서 반복 실행한다.
- `for 문은 반복 횟수가 명확할 때 주로 사용`하고 `while 문은 반복 횟수가 불명확할 때 주로 사용`한다.

```jsx
var count = 0;

// count가 3보다 작을 때까지 코드 블록 반복 실행
while (count < 3) {
  console.log(count); // 0 1 2
  count++;
}

// 무한루프
while (true) { ... }
```

<br>

## 8.3.3 do … while 문

- 코드 블록을 먼저 실행하고 조건식을 평가한다. → 코드 블록은 `무조건 한 번 이상 실행`된다.

```jsx
var count = 0;
do {
  console.log(count); // 0 1 2
  count++;
} while (count < 3);
```

<br>
<br>

# 8.4 break 문

> 레이블 문, 반복문, switch 문의 코드 블록을 탈출한다. 그 외에 break 문을 사용하면 SyntaxError 발생
>
- 반복문을 더 이상 진행하지 않아도 될 때 불필요한 반복을 회피할 수 있어 유용하다.
- `레이블 문`: 식별자가 붙은 문
    - 프로그램의 실행 순서를 제어하는 데 사용한다.

    - switch문의 case 문과 default 문도 레이블 문이다.
    - 레이블 문을 탈출하려면 `break 문에 레이블 식별자를 지정`한다.
    
    ```jsx
    foo: {
      console.log(1);
      break foo; // foo 레이블 블록문을 탈출한다.
      console.log(2);
    }
    
    console.log('Done!');
    ```
    
    ```jsx
    outer: for (var i = 0; i < 3; i++) {
      for(var j = 0; j < 3; j++) {
    	  if(i + j === 3) break outer;
    	  console.log(`inner [${i}, ${j}]`);
      }
    }
    ```
    
    - 중첩된 for 문 외부로 탈출할 때 유용하지만 그 밖의 경우에는 권장하지 않는다.
    - 레이블 문의 사용은 프로그램의 흐름이 복잡해져서 가독성이 나빠지고 오류를 발생시킬 가능성이 높아진다.
    
<br>
<br>

# 8.5 continue 문
> 반복문의 코드 블록 실행을 현 시점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동시킨다.

#### 문자열에서 특정 문자의 개수를 세는 예

- if 문 내에서 실행해야 할 코드가 길다면 continue 문을 사용하는 편이 가독성이 더 좋다.

```jsx
// 문자열에서 l의 개수를 세는 예
var string = 'Hello Wolrd.';
var search = 'l';
var count = 0;

for(var i = 0; i < string.length; i++) {

  if (string[i] !== search) continue;

  count++; // continue 문이 실행되면 이 문부터는 실행되지 않는다. 
  // code
}

for(var i = 0; i < string.length; i++) {

  if(string[i] === search) {
    count++;
    // code 
  }
}

// 참고) String.prototype.match 메서드 사용
const regexp = new RegExp(search, 'g');
console.log(string.match(regexp).length); // 3
```