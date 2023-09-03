# 14.1 타입 가드란?
* 여러 개의 타입으로 지정된 값을 특정 위치에서 원하는 타입으로 구분하는 것을 의미한다.
* 여러 타입이 있을 때 내가 원하는 타입을 뽑기 위해 다른 타입들을 막아 낸다는 의미이다.
```ts
function updateinput(textInput: number | string | boolean) {
    if (typeof textInput === 'number') {
        textInput   // textInput: number
    }
}
```

# 14.2 왜 타입 가드가 필요할까?
```ts
function updateinput(textInput: number | string | boolean) {
    // 숫자를 받아 소수점 두자리까지만 만들어서 반환하고 싶다면 toFixed 내장 API를 사용하여야 하므로 타입 가드가 필요하다.
}
```

## 타입 단언으로 타입 에러 해결하기
* 아래와 같이 하면 타입 에러는 해결되지만 두 가지 문제가 발생한다.
  * 실행 시점의 에러를 막을 수 없다.
  * 타입 단언을 계속해서 사용해야 한다.
```ts
function updateinput(textInput: number | string | boolean) {
    (textInput as number).toFixed(2);
}
```

## 타입 가드로 문제점 해결하기
* 아래와 같이 함수 안에서 타입별로 나누어 로직을 작성한다.
```ts
function updateinput(textInput: number | string | boolean) {
    if (typeof textInput === 'number') {
        textInput.toFixed(2);
        return;
    }
    if (typeof textInput === 'string') {
        console.log(textInput.length);
        return;
    }
}
```

# 14.3 타입 가드 문법
* 타입 가드에 사용하는 주요 연산자는 다음과 같다.
  * typeof
  * instanceof
  * in

## typeof 연산자
* 특정 코드의 타입을 문자열 값으로 반환해준다.
```ts
typeof 10;              // 'number'
typeof 'hello';         // 'string'
typeof function() {};   // 'function'
```
* typeof 연산자를 사용하여 특정 위치에서 원하는 타입으로 구분할 수 있다.
```ts
function printText(text: string | number) {
    if (typeof text === 'string') {
        console.log(text.trim());
    }
    if (typeof text === 'number') {
        console.log(text.toFixed(2))
    }
}
```

## instanceof 연산자
* 변수가 대상 객체의 프로토타입 체인에 포함되는지 확인하여 true/false를 반환해 준다.
```ts
function Person(name, age) {
    this.name = name;
    this.age = age;
}

const captain = new Person('캡틴', 100);
captain instanceof Person;  // true

const hulk = { name: '헐크', age: 79 };
hulk instanceof Person;     // false
```

* 타입 가드 측면에서 활용 방법
```ts
function fetchInfoByProfile(profile: Person | string) {
    if (profile instanceof Person) {
        console.log(profile.name);
        console.log(profile.age);
    } else {
        console.warn('문자열 형식입니다.')
    }
}
```

## in 연산자
* 객체에 속성이 있는지 확인해준다.
```ts
const book = {
    name: '길벗',
    rank: 1
}

console.log('name' in book) // true
console.log('address' in book) // false
```

* 타입 가드 측면에서 활용 방법
```ts
interface Book {
    name: string;
    rank: number;
}

interface OnlineLecture {
    name: string;
    url: string;
}

function learnCourse(material: Book | OnlineLecture) {
    if ('url' in material) {
        // OnlineLecture
    }
    if ('rank' in material) {
        // Book
    }
}
```

# 14.4 타입 가드 함수
* 타입 가드 함수란 타입 가드 역할을 하는 함수를 의미한다.
* 주로 객체 유니언 타입 중 하나를 구분하는 데 사용하며, in연산자와 역할은 같지만 좀 더 복잡한 경우에도 사용한다.
```ts
// 일반적인 타입 가드 적용 예시
interface Person {
    name: string;
    age: number;
}

interface Developer {
    name: string;
    skill: string;
}

function isPerson(someone: Person | Developer): someone is Person {
    return (someone as Person).age !== undefined;
}

function greet(someone: Person | Developer) {
    if (isPerson(someone)) {
        console.log(someone.age);
    } else {
        console.log(someone.skill);
    }
}

// 더 복잡한 타입 가드 예시
interface Hero {
    name: string;
    nickname: string;
}

interface Person {
    name: string;
    age: number;
}

interface Developer {
    name: string;
    age: string;
    skill: string;
}

function isPerson(someone: Hero | Person | Developer): someone is Person {
    if ('age' in someone) {
        // Person.age와 Developer.age를 구분할 수 없다.
        someone.age
    }

    return typeof (someone as Person).age === 'number'
}

function greet(someone: Hero | Person | Developer) {
    if (isPerson(someone)) {
        console.log(someone.age);
    }
}
```

# 14.5 구별된 유니언 타입
* 유니언 타입을 구성하는 여러 개의 타입을 특정 속성의 유무가 아니라 특정 속성 값으로 구분하는 타입 가드 문법을 의미한다.
```ts
interface Person {
    name: string;
    age: number;
    industry: 'common';
}

interface Developer {
    name: string;
    age: string;
    industry: 'tech';
}

function greet(someone: Person | Developer) {
    if (someone.industry === 'common') {
        console.log(someone.age);
    } else {
        console.log(someone.age);
    }
}
```