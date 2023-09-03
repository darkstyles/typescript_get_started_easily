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
