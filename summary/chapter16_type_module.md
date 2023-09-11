# 16.1 모듈이란?
* 프로그래밍 관점에서 특정 기능을 갖는 작은 단위의 코드를 의미한다.
* 애플리케이션 크기가 작다면 모듈이라는 개념이 필요 없지만, 수십 개의 파일과 많은 수의 코드 라인을 갖고 있다면 코드도 역할과 목적에 따라 구분되어 있는 것이 좋다.
```ts
// 숫자 연산과 문자열 형식 정리하는 목적으로 모듈을 각각 분리하고 해당 모듈과 관련 있는 기능들을 분류하였다.

// Math.js
sum(), substract()

// FormatString.js
capitalize(), splitWords()
```

# 16.2 자바스크립트 모듈
## 자바스크립트의 태생적 한계
* 자바스크립트는 모듈이라는 개념이 없던 프로그래밍 언어이며 파일 단위로 변수나 함수가 구분되지 않아 문제점이 많았다.
```js
<body>
    <script src="a.js"></script>
    <script src="b.js"></script>
    <script>
        getTotal(); // 200
    </script>
</body>

// a.js
var total = 100;
function getTotal() {
    return total;
}

// b.js
var total = 200;

// 파일별로 변수나 함수를 구분해서 정의하더라도 기본적으로 모두 전역 유효 범위를 갖는 것이 자바스크립트의 특징이다.
// 전역 유효 범위는 예상치 못한 결과를 야기한다.
// 그래서 이름이 서로 충돌하지 않게 유일한 변수나 함수 이름을 고민할 필요가 있다.
// 애플리케이션이 커지고 코드가 많아질수록 문제점이 더욱 부각된다.
```

## 자바스크립트 모듈화를 위한 시도들
* `Common.js`는 브라우저뿐만 아니라 브라우저 이외의 환경인 서버, 데스크톱에서도 자바스크립트를 활용하려고 고안된 스펙이자 그룹이다.
    * 현재는 서버 런타임 환경인 `Node.js`에서 가장 활방하게 사용되고 있다.
```js
// Node.js가 설치되어 있다면 별도의 도구나 라이브러리 없이도 이와 같은 문법을 이용하여 자바스크립트의 모듈화를 구현할 수 있다.

// math.js
function sum(a, b) {
    return a + b;
}

module.exports = {
    sum
};

// app.js
var math = require('./math.js');
console.log(math.sum(10, 20)); // 30
```
* `Require.js`는 AMD(Asynchronous Module Definition)라는 비동기 모듈 정의 그룹에서 고안된 라이브러리 중 하나이다.
    * 비동기 모듈은 애플리케이션이 시작되었을 때 모든 모듈을 가져오는 것이 아니라 필요할 때 순차적으로 해당 모듈을 가져온다는 의미이다.
```html
<body>
    <!-- 라이브러리 파일 다운로드 후 다음과 같이 연결 -->
    <script src="require.js"></script>
    <script>
        require(["https://unpkg.com/vue@3/dist/vue.global.js"], function() {
            console.log(vue is loaded)
        });
    </script>
</body>
```

# 16.3 자바스크립트 모듈화 문법
## import와 export
```js
// math.js
function sum(a, b) {
    return a + b;
}

export { sum } // export로 sum 함수를 모듈화했으므로 다른 파일에서 이 함수를 불러와 사용할 수 있다.

// app.js
import { sum } from './math.js';
console.log(sum(10, 20)) // 30
```

## export default 문법
* 하나의 대상만 모듈에서 내보내고 싶을 때 사용한다.
```js
// math.js
function sum(a, b) {
    return a + b;
}

export default sum; // 해당 파일에서 하나의 대상만 내보내겠다는 의미이다.

// app.js
import sum from './math.js'; // sum 함수 하나만 가져올 수 있다.
console.log(sum(10, 20)) // 30
```

## import as 문법
* import 구문에 as 키워드를 이용하면 가져온 변수나 함수의 이름을 해당 모듈 내에서 변경하여 사용할 수 있다.
```js
// math.js
function sum(a, b) {
    return a + b;
}

export { sum }

// app.js
import { sum as add } from './math.js';
console.log(add(10, 20)) // 30
```

## import * 문법
* 특정 파일에서 내보낸 기능이 많아 import 구문으로 가져와야 할 것이 많다면 * 키워드를 사용하여 편리하게 가져올 수 있다.
```js
// math.js
function sum(a, b) {
    return a + b;
}

function substract(a, b) {
    return a - b;
}

function divide(a, b) {
    return a / b;
}

export { sum, substract, divide }

// app.js
import * as myMath from './math.js'
console.log(myMath.sum(10, 20))         // 30 
console.log(myMath.substract(30, 10))   // 20
console.log(myMath.divide(4, 2))        // 2

/*
const myMath = {
    sum: function() {},
    substract: function() {},
    divide: function() {},
}
*/
```

## export 위치
* 특정 파일에서 다른 파일이 가져다 쓸 기능을 내보낼 때 사용하는 키워드이다.
 ```js
 const pi = 3.14;
 const getHi = () => {
    return 'hi';
 }
 class Person {
    ...
 }

 export { pi, getHi, Person }

 export const pi = 3.14;
 export const getHi = () => {
    return 'hi';
 }
 export class Person {
    ...
 }
 ```

 # 16.4 타입스크립트 모듈
 * 타입스크립트의 모듈도 import와 export 구문을 동일하게 사용할 수 있다.
 * 타입스크립트 파일에 작성된 변수, 함수, 클래스 등 기능을 import, export 문법으로 내보거나 가져올 수 있다.
 ```ts
 // math.ts
function sum(a: number, b: number) {
    return a + b;
}

export { sum }

// app.ts
import { sum } from './math';
console.log(sum(10, 20)) // 30
```
* 타입을 내보내고 가져올 수 있다.
```ts
// hero.ts
interface Hulk {
    name: string;
    skill: string;
}

export { Hulk }

// app.ts
import { Hulk } from './hero';
const banner: Hulk = {
    name: '배너',
    skill: '화내기'
}
```

# 16.5 타입스크립트 모듈 유효 범위
* 자바스크립트가 변수를 선언할 때 기본적으로 전역 변수로 선언되듯이 타입스크립트 역시 전역 변수로 선언된다.
```ts
// 다른 파일에 선언된 변수들이 모두 타입스크립트의 모듈 관점에서 전역으로 등록되어 있기 때문에 같은 이름으로 함수나 타입 별칭 등 재선언이 불가능하다.

// src/util.ts
var num = 10;

// src/app.ts
var a = num; // No Error
```

* 타입스크립트에서 같은 프로젝트 내에서 이미 선언된 이름을 사용할 수 없지만 var, interface 등 재선언이나 병합 선언이 가능한 코드는 별도로 에러가 표시되지 않는다.
```ts
// util.ts
type Person = {
    name: string;
}

// app.ts
const capt: Person = {
    name: '캡틴'
};

type Person = {     // Duplicate identifier 'Person'.
    name: string;
    skill: string;
}
```
```ts
// util.ts
interface Person = {
    name: string;
}

// app.ts
const capt: Person = {
    name: '캡틴',
    skill: '방패'   // 인터페이스 정의가 병합되었으므로 변수 선언 시 skill 속성을 정의해주어야 한다.
};

// 인터페이스 정의가 병합된다. 따라서 에러가 발생하지 않는다.
interface Person = {
    name: string;
    skill: string;
}
```

# 16.6 타입스크립트 모듈화 문법
## import type 문법
* 타입을 다른 파일에서 import로 가져오는 경우 import type을 사용하여 타입 코드인지 아닌지 명시할 수 있다.
```ts
// hero.ts
interface Hulk {
    name: string;
    skill: string;
}

export { Hulk }

// app.ts
import type { Hulk } from './hero';

const banner: Hulk = {
    name: '배너',
    skill: '화내기'
}
```

## import inline type 문법
* 변수, 함수 등 실제 값으로 쓰는 코드와 타입 코드를 같이 가져올 때 사용할 수 있다.
```ts
// hero.ts
interface Hulk {
    name: string;
    skill: string;
}

function smashing() {
    return '';
}

const doctor = {
    name: '스트레인지'
}

export { Hulk, smashing, doctor };

// app.ts
import { type Hulk, smashing, doctor } from './hero';

const banner: Hulk = {
    name: '배너',
    skill: '화내기'
}
```

## import와 import type 중 어떤 문법을 써야 할까?
* 팀에서 정의된 `코딩 컨벤션`에 따르는 것이 맞다.
* 자동 완성이 주는 편리함을 따라간다면 `type`을 붙이지 않고, 코드 역할을 좀 더 명확하게 하겠다면 `type`을 붙이면 된다.

# 16.7 모듈화 전략: Barrel
* 여러 개의 파일에서 가져온 모듈을 마치 하나의 통처럼 관리하는 방식이다.
* 코드 가독성을 높일 수 있다.
```ts
// ./hero/hulk.ts
interface Banner {
    name: string;
}
export { Banner }

// ./hero/ironman.ts
interface Tony {
    name: string;
}
export { Tony }

// ./hero/captain.ts
interface Steve {
    name: string;
}
export { Steve }

// app.ts
import { Banner } from './hero/hulk';
import { Tony } from './hero/ironman';
import { Steve } from './hero/captain';

const banner: Banner = { name: '배너' };
const tony: Tony = { name: '토니' };
const steve: Steve = { name: '스티브' };
```
```ts
//./hero/index.ts
import { Banner } from './hulk';
import { Tony } from './ironman';
import { Steve } from './captain';

export { Banner, Tony, Steve }

// app.ts
import { Banner, Tony, Steve } from './hero';

const banner: Banner = { name: '배너' };
const tony: Tony = { name: '토니' };
const steve: Steve = { name: '스티브' };
```
```ts
//./hero/index.ts
export { Banner } from './hulk';
export { Tony } from './ironman';
export { Steve } from './captain';
```
# 16.8 정리
* 타입스크립트 모듈은 자바스크립트의 모듈에서 확장된 개념이다.
* type이라는 키워드를 추가로 이용할 수 있다.
* 많은 숫자의 모듈을 다룰 때 배럴이라는 모듈화 전략을 이용하여 비슷한 성격의 모듈을 모아 하나의 파일로 내보낼 수 있다.