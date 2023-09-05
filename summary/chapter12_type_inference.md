# 12.1 타입 추론이란?
* 타입스크립트가 코드를 해석하여 적절한 타입을 정의하는 동작
* 변수를 초기화하거나 함수의 파라미터에 기본값을 설정하거나 반환값을 설정했을 때 지정된 값을 기반으로 적당한 타입을 제시하고 정의해 주는 것을 타입 추론이라고 합니다.

# 12.2 변수의 타입 추론 과정
* 변수 타입은 선언하는 기점에 할당된 값을 기반으로 추론된다.
```ts
let a = 'hi';   // a: string
let b = false;  // b: boolean
let c = 1;      // c: number
let d;          // d: any
d = 1           // d: any
```

# 12.3 함수의 타입 추론: 반환 타입
```ts
function sum(a: number, b: number): number {
    return a + b;
}

const result = sum(1, 2); // result: number

function sum(a: number, b: number) {    // sum(a: number, b: number): number
    return a + b;
}

const result = sum(1, 2); // result: number

function sum(a: number, b: number) {    // sum(a: number, b: number): boolean
    return a === b;
}

const result = sum(1, 2); // result: boolean
```

# 12.4 함수의 타입 추론: 파라미터 타입
```ts
function getA(a) {  // getA(a: any): any
    return a;
}

function getA(a = 10) {  // getA(a?: number): number
    return a;
}

function getA(a: number) {  // getA(a: number): string
    let c = 'hi';
    return a + c;
}
```

# 12.5 인터페이스와 제네릭의 추론 방식
```ts
interface Dropdwon<T> {
    title: string;
    value: T;
}

const shoppingItem: Dropdwon<number> = {
    title: '바지',
    value: 1,
}

interface Dropdwon {
    title: string;
    value: number;
}
```

# 12.6 복잡한 구조에서 타입 추론 방식
```ts
interface Dropdwon<T> {
    title: string;
    value: T;
}

interface DetailedDropdown<K> extends Dropdwon<K> {
    tag: string;
    description: string;
}

let shoppingItem: DetailedDropdown<number> = {
    title: '길벗 책',
    description: '쉽고 유용하다',
    tag:'타입스크립트',
    value: 1,
}

interface DetailedDropdown {
    tag: string;
    description: string;
    title: string;
    value: number;
}
```

# 12.7 정리
* 변수를 선언할 때 초깃값을 할당해 놓으면 해당 초깃값을 바탕으로 타입이 추론된다.
* 함수의 파라미터 타입이나 파라미터 기본값으로 반환 타입이 추론된다.
* 인터페이스나 제네릭 타입 역시 주어진 타입 정보로 관련 타입이 정확하게 추론된다.

# 아이템 19 추론 가능한 타입을 사용해 장황한 코드 방지하기
* 타입스크립트가 타입을 추론할 수 있다면 타입 구문을 작성하지 않는 게 좋다.
* 이상적인 타입스크립트 코드는 함수/메서드 시그니처에 타입 구문을 포함하지만, 함수 내에서 생성된 지역 변수에는 타입 구문을 넣지 않는다. 타입 구문을 생략하여 방해되는 것들을 최소화하고 코드를 읽는 사람이 구현 로직에 집중할 수 있게 하는 것이 좋다.
* 추론될 수 있는 경우라도 객체 리터럴과 함수 반환에는 타입 명시를 고려해야 한다. 이는 내부 구현의 오류가 사용자 코드 위치에 나타나는 것을 방지해준다.
## 타입이 추론될 수 있음에도 여전히 타입을 명시하고 싶은 상황: 객체 리터럴
* 타입 추론이 가능할지라도 구현상의 오류가 함수를 호출한 곳까지 영향을 미치지 않도록 하기 위해 타입 구문을 명시하는 게 좋다.
```ts
interface Product {
    name: string;
    id: string;
    price: number;
}

function logProduct(product: Product) {
    const {id, name, price} = product;
    console.log(id, name, price);
}

const product: Product = {
    name: 'ticket',
    id: '048188',
    price: 28.99
}

logProduct(product)

const product2 = {
    name: 'ball',
    id: 14235,
    price: 35
}

const product3: Product = {
    name: 'ball',
    id: 14235, // Type 'number' is not assignable to type 'string'.
    price: 35
}

logProduct(product2) // Argument of type '{ name: string; id: number; price: number; }' is not assignable to parameter of type 'Product'.
```
## 타입이 추론될 수 있음에도 여전히 타입을 명시하고 싶은 상황: 함수 반환
* 반환 타입을 명시하면, 구현상의 오류가 사용자 코드의 오류로 표시되지 않는다.
```ts
const cache: {[ticker: string]: number} = {};
function getQuote(ticker: string) {
    if (ticker in cache) {
        return cache[ticker]
    }
    return fetch('https://quotes.example.com/?q=${ticker}')
        .then(response => response.json())
        .then(quote => {
            cache[ticker] = quote;
            return quote
        })
}

getQuote('MSFT').then(considerBuying) // Property 'then' does not exist on type 'number | Promise<any>'.

const cache: {[ticker: string]: number} = {};
function getQuote(ticker: string): Promise<number> {
    if (ticker in cache) {
        return cache[ticker] // Type 'number' is not assignable to type 'Promise<number>'.
    }
    return fetch('https://quotes.example.com/?q=${ticker}')
        .then(response => response.json())
        .then(quote => {
            cache[ticker] = quote;
            return quote
        })
}

getQuote('MSFT').then(considerBuying)
```
# 아이템 21 타입 넓히기
* 타입 스크립트가 작성된 코드를 정적 분석할 때 변수는 `가능한` 값들의 집합인 타입을 가진다.
* 예를 들어 상수를 사용해서 변수를 초기화할 때 타입을 명시하지 않으면 타입 체커는 타입을 결정해야 하는데 지정된 단일 값을 가지고 할당 가능한 값들의 집합을 유추하는 과정을 타입스크립트에서는 `넓히기`라고 한다.
```ts
interface Vector3 {
    x: number;
    y: number;
    z: number;
}
function getComponent(vector: Vector3, axis: 'x' | 'y' | 'z') {
    return vector[axis];
}

let x = 'x'; // x: string으로 추론됨
let vec = {x:10, y:20, z:30};
getComponent(vec, x); // Argument of type 'string' is not assignable to parameter of type '"x" | "y" | "z"'

const x = 'x'; // const로 변수를 선언하게 되면 재할당을 할 수 없으므로 x는 더 좁은 타입인 'x'로 추론된다.
let vec = {x:10, y:20, z:30};
getComponent(vec, x); // No Error
```
* 객체의 경우 타입스크립트의 넓히기 알고리즘은 각 요소를 let으로 할당된 것처럼 다룬다. 그래서 요소의 값을 같은 타입으로 재할당할 수 있다.
```ts
const v = {
    x: 1
};
v.x = 2;
v.x = '2'
```
## 타입스크립트의 기본 동작을 제정의하는 세가지 방법
* 명시적 타입 구문 제공
```ts
const v: { x: 1|3|5 } = {
    x:1
}
```
* 타입 체커에 추가적인 문맥을 제공(예를 들어 함수의 매개변수로 값을 전달)
* const 단언문을 사용
```ts
const v1 = {
    x: 1,
    y: 2,
}; // {x: number, y: number}
const v2 = {
    x: 1 as const,
    y: 2
}; // {x: 1, y: number}
const v3 = {
    x: 1,
    y: 2
} as const; // {readonly x: 1, readonly y: 2}
```
# 아이템 23 한꺼번에 객체 생성하기
```ts
interface Point { x: number, y: number };
const pt: Point = {}
pt.x = 3; // Error
pt.y = 4;

const pt = {} as Point;
pt.x = 3; // No Error
pt.y = 4;

const pt: Point = {
    x: 3,
    y: 4
}

const p0 = {}; // 객체 전개를 사용하면 속성을 추가하지 않는다.
const p1 = {...p0, x: 3};
const pt: Point = {...p1, y: 4};
```
# 아이템 26 타입 추론에 문맥이 어떻게 사용되는지 이해하기
* 튜플 사용시 주의점
```ts
function panTo(where: [number, number]) {}
panTo([10, 20]);

const loc = [10, 20];
panTo(loc) // loc: number[] 으로 추론

// 타입 선언 제공
const loc2: [number, number] = [10, 20];
panTo(loc2);

// as const를 이용한 타입 좁히기
// const는 단지 값을 가리키는 참조가 변하지 않는 얕은 상수인 반면, as const는 그 값이 내부까지 상수라는 사실을 타입스크립트에게 알려준다.
function panTo2(where: readonly [number, number]) {}
const loc3 = [10, 20] as const;
panTo2(loc3);

const loc4 = [10, 20, 30] as const;
panTo2(loc4); // 호출하는 곳에서 에러가 발생하며 중첩된 객체라면 오류가 발생한 원인 파악이 어렵다
```
* 객체 사용 시 주의점
```ts
type Language = 'JavaScript' | 'TypeScript' | 'Python';
interface GovernedLanguage {
    language: Language;
    organization: string;
}

function complain(language: GovernedLanguage){}
complain({ language: 'TypeScript', organization: 'Microsoft' });

const ts = {
    language: 'TypeScript',
    organization: 'Microsoft'
};
complain(ts); // Error
```
* 콜백 사용 시 주의점
```ts
function callWithRandomNumbers(fn: (n1: number, n2: number) => void) {
    fn(Math.random(), Math.random());
}

callWithRandomNumbers((a, b) => { console.log(a + b) });

const fn = (a, b) => { // Error
    console.log(a + b);
}
callWithRandomNumbers(fn)

```