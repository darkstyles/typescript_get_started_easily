# 5.1 인터페이스란?
객체 타입을 정의할 때 사용하는 문법이다.
인터페이스로 타입을 정의할 수 있는 부분은 아래와 같다.
* 객체의 속성과 속성 타입
* 함수의 파라미터와 반환 타입
* 함수의 스펙(파라미터 개수와 반환값 여부 등)
* 배열과 객체를 접근하는 방식
* 클래스

# 5.2 인터페이스를 이용한 객체 타입 정의
```ts
interface User {
    name: string;
    age: number;
}
const seho: User = { name: '세호', age: 36 } // No Error
const seho2: User = { name: '세호', age: '36' } // TypeError1
const seho3: User = { name: '세호', age: 36, hobby: '와인' } // TypeError2
```
## TypeError1
User 인터페이스에서 age 속성의 타입이 숫자로 정의되어 있는데 문자열 데이터 타입인 '36'이 할당되어서 에러가 발생하였다.
## TypeError2
User 인터페이스에 정의되지 않은 hobby 속성이 정의되어 에러가 발생하였다.

## 정리
인터페이스를 이용하여 객체의 속성과 데이터 타입을 정확하게 정의할 수 있다.

# 5.3 인터페이스를 이용한 함수 타입 정의
## 함수 파라미터 타입 정의
```ts
interface Person {
    name: string;
    age: number;
}

function logAge(someone: Person) {
    console.log(someone.age);
}
```
함수의 파라미터 타입은 콜론(:)이라는 타입 표기 방식을 이용해서 정의(파라미터 이름 오른쪽에 타입 정의)한다.
```ts
const captain = { name: 'Capt', age: 100 };
logAge(captain); // 100

const captain2 = { name: 'Capt' };
logAge(captain2); // Error1
```
### Error1
captain2 변수에는 name 속성만 있기 때문에 logAge()의 파라미터 타입과 일치하지 않아 에러가 발생한다.

### 정리
함수의 파라미터에 정의한 타입 조건을 만족하는 데이터만 인자로 넘길 수 있다.

## 함수 반환 타입 정의
아래와 같이 함수의 반환 타입을 정의하면 getPerson() 함수의 호출 결과를 변수에 할당 하였을 때 해당 변수가 Person 인터페이스 타입으로 추론된다.
```ts
interface Person {
    name: string;
    age: number;
}

function getPerson(someone: Person): Person {
    return someone;
}
const hulk = getPerson({ name: 'Hulk', age: 99});
```
# 5.4 인터페이스의 옵션 속성
인터페이스로 정의된 객체의 속성을 선택적으로 사용하고 싶을 때 옵션 속성을 사용한다.
```ts
interface Person {
    name?: string;
    age: number;
}

function logAge(someone: Person) {
    console.log(someone.age);
}

function logPersonInfo(you: Person) {
    console.log(you.name);
    console.log(you.age);
}
```
`logPersoninfo`에서는 name, age 두 속성이 모두 필요하고 `logAge` 에서는 age 속성이 필요할 때 `인터페이스의 옵션 속성`을 설정하여 해소할 수 있다.

# 5.5 인터페이스 상속
인터페이스의 상속으로 타입 정의를 확장할 수 있다.
## 인터페이스의 상속이란?
인터페이스를 상속받을 때 extends 예약어를 사용한다.
아래와 같이 사용하면 Developer 인터페이스가 Person 인터페이스를 상속 받았기 때문에 name, age, skill 속성을 정의한 효과가 나타난다.
```ts
interface Persion {
    name: string;
    age: number;
}

interface Developer extends Person {
    skill: string;
}
const ironman: Developer = {
    name: '아이언맨',
    age: 21,
    skill: '만들기'
}
```
이렇게 `extends` 예약어를 사용해서 인터페이스의 타입을 상속받아 활정하여 사용할 수 있다.

## 인터페이스를 상속할 때 참고 사항
* 부모 인터페이스에 정의된 타입을 자식 인터페이스에서 모두 보장해 주어야 한다.
```ts
interface Persion {
    name: string;
    age: number;
}

interface Developer extends Person {
    name: number;
} // Error
```
* 상속을 여러번 할 수 있다.
```ts
interface Hero {
    power: boolean;
}

interface Persion extends Hero {
    name: string;
    age: number;
}

interface Developer extends Person {
    skill: string;
}

const ironman: Developer = {
    name: '아이언맨',
    age: 21,
    skill: '만들기',
    power: true
}
```

# 5.6 인터페이스를 이용한 인덱싱 타입 정의
인텍싱이란 객체의 특정 속성을 접근하거나 배열의 인덱스로 특정 요소에 접근하는 동작을 의미한다.
## 배열 인덱싱 타입 정의
배열을 인덱싱할 때 인터페이스로 인덱스와 요소의 타입을 정의할 수 있다.
```ts
interface StringArray {
    [index: number]: string;
}

const companies: StringArray = ['삼성', '네이버', '구글'];
companies[0]; // 삼성
companies[1]; // 네이버
```
아래와 같이 인덱스 타입을 선언하게 되면 에러가 발생한다.
이유는 배열의 인덱스는 숫자여야 하는데 문자열로 인덱스 타입을 강제하여 배열 정의에 위배되어 에러가 발생한다.
```ts
interface StringArray {
    [index: string]: string;
}

const companies: StringArray = ['삼성', '네이버', '구글'];
```

## 객체 인덱싱 타입 정의
아래와 같이 사용하면 money는 number로 타입이 추론된다.
```ts
interface SalaryMap {
    [level: string]: number;
}

const salary: SalaryMap {
    junior: 100
}
const money = salary['junior'];
```
아래와 같이 사용하면 money는 string으로 타입이 추론된다.
```ts
interface SalaryMap {
    [level: string]: string;
}

const salary: SalaryMap {
    junior: '100원'
}
const money = salary['junior'];
```
> 객체 속성 접근은 `object['key']` 또는 `object.key` 모두 가능하지만 속성 이름에 숫자나 `-`등 특수 기호가 들어가면 체이닝 방식으로 접근할 수 없다.

## 인덱스 시그니처란?
정확히 속성 이름을 명시하지 않고 속성 이름의 타입과 속성 값의 타입을 정의하는 문법을 인덱스 시그니처라고 한다.
명시적인 속성 이름으로 객체 인터페이스를 작성하게 되면 속성이 추가될 때마다 인터페이스에 속성을 정의해줘야 하는데 `인덱스 시그니처`를 사용하게 되면
속성 이름과 속성 값의 타입이 같다면 1개든 100개든 n개든 모두 추가할 수 있다.

## 인덱스 시그니처는 언제 쓸까?
* `인덱스 시그니처`가 적용되어 인터페이스가 작성되어 있다면 코드 작성 시 해당 인터페이스를 타입으로 사용하는 곳에서 코드 자동 완성이 되지 않는다.
```ts
interface User {
    [property: string]: string;
}
```
* `id, name` 속성이 무조건 들어간다면 아래와 같이 `인덱스 시그니처`와 함께 선언하여 인터페이스를 타입으로 사용하는 곳에서 `id, name`에 대해서 코드 자동 완성이 된다.
```ts
interface User {
    [property: string]: string;
    id: string;
    name: string;
}
```
객체의 속성 이름과 속성 값이 정해져 있는 경우에는 명시적으로 정의하고, 속성 이름은 모르지만 타입을 아는 경우에는 인덱스 시그니처를 활용한다.

# 5.7 정리
* 객체의 속성 이름과 속성 값의 타입을 정의하여 변수와 함수의 파라미터, 반환 타입에 사용 가능하다.
* 옵션 속성인 `?`를 이용하여 인터페이스의 속성을 선택적으로 사용 가능하다.
* `extends`로 인터페이스를 상속 가능하다.
* 인터페이스에서 인덱싱 타입을 정의하고 인덱스 시그니처로 사용 가능하다.
