# 13.1 타입 단언이란?
* 타입스크립트의 타입 추론에 기대지 않고 개발자가 직접 타입을 명시하여 해당 타입으로 강제하는 것을 의미한다.
* `as`키워드를 붙이면 타입스크립트가 컴파일할 때 해당 코드의 타입 검사를 수행하지 않는다.
* 타입 단언을 이용하여 타입스크립트 컴파일러가 알기 어려운 타입에 대한 힌트를 제공할 수 있다.
```ts
interface Person {
    name: string;
    age: number;
}

const joo = {}     // joo: any
joo.name = '형주';  // Property 'name' does not exist on type '{}'.
joo.age = 31;      // Property 'age' does not exist on type '{}'.

const joo = {} as Person   // joo: Person: 
joo.name = '형주';          // No Error
joo.age = 31;              // No Error
```

# 13.2 타입 단언 문법
## 타입 단언의 대상
* 타입 단언은 숫자, 문자열, 객체 등 원시 값뿐만 아니라 변수나 함수의 호출 결과에도 사용할 수 있다.
```ts
function getId(id) {
    return id;
}

const myId = getId('josh') as number    // myId: number
// 위 내용은 함수의 파라미터가 any 이라 함수의 반환 타입까지 any가 되면서 as number로 강제 타입을 지정한 부분이 잘못된걸 추론하지 못한다.
// 이런 문제를 해소하기 위해서는 함수의 파라미터 타입 지정 또는 함수 파라미터의 초기값을 지정한다.
```
## 타입 단언 중첩
* 가급적 타입 단언보다는 타입 추론에 의지하는 것이 좋다.
```ts
const num = (10 as any) as number;
```

## 타입 단언을 사용할 때 주의할 점
### as 키워드를 구문 오른쪽에서만 사용한다.
* 타입 단언은 변수 이름에 사용할 수 없다.
```ts
const num = 10 as number;
const num: number = 10;
```

### 호환되지 않는 데이터 타입으로는 단언할 수 없다.
* 타입 단언을 이용하면 어떤 값이든 내가 원하는 타입으로 단어할 수 있을 것 같지만 실제로는 그렇지 않다.
* 타입이라는 것은 해당 값에 대한 부가 정보지 타입을 as로 변경한다고 해서 값 자체가 바뀌지는 않는다.
```ts
const num = 10 as string;   // Type Error
const num = 10 as any;      // No Error
```

### 타입 단언 남용하지 않기
* 타입 단언은 코드를 실행하는 시점에서 아무런 역할도 하지 않기 때문에 에러에 취약한 측면이 있다.
* 타입 에러를 해결하기 위해 타입 단언을 사용하여 타입 에러를 해결했지만 실행 에러는 미리 방지하지 못한다.

# 13.3 null 아님 보장 연산자: !
* null 타입을 체크할 때 유용하게 쓰는 연산자이다.
* null 아님 보장 연산자(!)를 사용하면 null 체크 로직을 일일이 추가해야 하는 수고를 덜 수 있다.
```ts
function shuffleBooks(books: Books | null) {
    const result = books.shuffle();     // 'books' is possibly 'null'.
    const result = books!.shuffle();    // No Error
    return result;
}
```

# 13.4 정리
* 특정 값의 타입을 타입스크립트 컴파일러의 해석에 따르지 않고 개발자가 직접 정의하는 것이 타입 단언이다.
* 타입 단언을 쓰면 타입 에러는 해결할 수 있지만 실제 실행 시점의 에러를 해결하지는 못한다
* 타입 단언보다는 타입 추론에 의지하길 권한다.