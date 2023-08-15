# 7.1 타입 별칭이란?
* `타입 별칭`은 특정 타입이나 인터페이스 등을 참조할 수 있는 타입 변수를 의미한다.
* 타입에 의미를 부여해서 별도의 이름으로 부르는 것이다.
* 타입 별칭을 사용하면 타입에 의미를 담아 여러 곳에 재사용할 수 있다.
* 타입을 선언하고 다시 다른 타입을 할당할 수 없다.
```ts
type MyName = string;
const capt: MyName = '캡틴';

type MyMessage = string | number;
const message: MyMessage = '안녕하세요';
function logText(text: MyMessage) { 
    //...
}
type MyMessage = string; // Error
```

# 7.2 타입 별칭과 인터페이스의 차이점
## 코드 에디터에서 표기 방식 차이
* 타입 별칭을 정의하고 이 타입 별칭에 마우스 커서를 올리면 타입 별칭에 정의된 정보가 미리보기로 표시된다.
* 인터페이스를 정의하고 이 인터페이스에 마우스 커서를 올리면 인터페이스명만 표시된다.

## 사용할 수 있는 타입의 차이
* 인터페이스는 주로 객체의 타입을 정의하는데 사용된다.
* 타입 별칭은 일반 타입에 이름을 짓거나 유니언 타입, 인터섹션 타입 등에도 사용된다.
* 인터페이스로는 제네릭이나 유틸리티 타입을 정의할 수 없다.
* 인터페이스와 타입 별칭의 정의를 함께 사용할 수 있다.
```ts
type ID = string;
type Product = TShirt | Shoes;
type Teacher = Person & Adult;

type Gilbut<T> = {
    book: T;
}
type MyBeer = Pick<Beer, 'brand'>

interface Person {
    name: string;
    age: number;
}
type Adult = {
    old: boolean;
}
type Teacher = Person & Adult;
```

## 타입 확장 관점에서 차이
`타입 확장`이란 이미 정의되어 있는 타입들을 조합해서 더 큰 의미의 타입을 생성하는 것을 의미한다.
### 인터페이스의 타입 확장
* 인터페이스는 타입을 확장할 때 상속이라는 개념을 이용한다.
* `extends` 키워드를 사용하여 부모 인터페이스의 타입을 자식 인터페이스에 상속하여 사용한다.
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
* 인터페이스는 `선언 합병`이라는 성질을 이용하여 동일한 이름으로 인터페이스를 여러 번 선언했을 때 해당 인터페이스의 타입 내용을 합친다.
```ts
interface Persion {
    name: string;
    age: number;
}
interface Persion {
    address: string;
}

const seho: Person = {
    name: '세호',
    age: 30,
    address: '광교'
}
```
### 타입 별칭의 타입 확장
* 타입 별칭은 인터섹션 타입으로 객체 타입을 2개 합쳐서 사용한다.
```ts
type Persion = {
    name: string;
    age: number;
}

type Developer = {
    skill: string;
}
const ironman: Person & Developer = {
    name: '아이언맨',
    age: 21,
    skill: '만들기'
}

type Joo = Person & Developer;
const joo: Joo = {
    name: '행주',
    age: 21,
    skill: '웹개발'
}
```

# 7.3 타입 별칭은 언제 쓰는 것이 좋을까?
* 2021년 이전의 타입스크립트 공식 문서에서는 `좋은 소프트웨어는 확장이 용이해야 한다.`는 관점에서 `타입 별칭`보다 `인터페이스`의 사용을 권장했다.
* 2023년 현재의 공식 문서에는 위 내용이 없고 일단 `인터페이스`를 주로 사용해보고 `타입 별칭`이 필요할 때 `타입 별칭`을 쓰라고 안내한다.

## 타입 별칭으로만 정의할 수 있는 타입들
* 타입 별칭으로만 정의할 수 있는 타입은 주요 데이터 타입이나 인터섹션, 유니언 타입이다.
* 타입 별칭은 제네릭, 유틸리티 타입, 맵드 타입과도 연동하여 사용할 수 있다.
  * 제네릭은 인터페이스와 타입 별칭에 모두 사용할 수 있지만 유틸리티 타입, 맵드 타입은 타입 별칭으로만 정의할 수 있다.
```ts
type MyString = string;
type StringOrNumber = string | number;
type Admin = Person & Developer;
type Dropdown<T> = {
    id: string;
    title: T;
}
type Admin = { name: string; age: number; role: string; }
type OnlyName = Pick<Admin, 'name'>
```

## 백엔드와의 인터페이스 정의
* 서비스 요구 사항의 변경으로 사용자 객체의 속성이 추가되거나 다른 객체 정보와 결합하여 표시되어야 한다면 기존 타입의 확장이라는 측면에서 인터페이스로 정의하는 것이 더 수월하다.
* 유연하게 타입을 확장하는 관점에서는 타입 별칭보다 인터페이스가 더 유리하다.
```ts
interface Admin {
    role: string;
    department: string;
}

interface User extends Admin {
    id: string;
    name: string;
}

interface User {
    skill: string;
}
```
