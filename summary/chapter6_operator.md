# 6.1 유니언 타입
* `유니언 타입`은 여러 개의 타입 중 한 개만 쓰고 싶을 때 사용하는 문법이다.
* 자바스크립트 OR 연산자의 `|`을 이용하여 여러 개의 타입 중 1개를 사용하겠다고 선언할 수 있다.
```ts
function logText(text: string | number) {
    console.log(text);
}

logText('hi'); // hi
logText(100); // 100
```

# 6.2 유니언 타입의 장점
* 같은 동작을 하는 함수의 코드 중복을 줄일 수 있다.
* `string`과 `number`에서 모두 제공하는 `toString`을 자동 완성할 수 있다.
* `toString()`이라는 API 스펙을 정확히 몰랐을 때 에러를 미리 발견할 수 있다.
```ts
function logText(text: string | number) {
    console.log(text);
}

// any 타입은 타입이 없는 것과 마찬가지므로 자동 완성 및 에러를 미리 발견할 수 없다.
function logText(text: any) {
    console.log(text);
}
```

# 6.3 유니언 타입을 사용할 때 주의할 점
* 함수의 파라미터에 유니언 타입을 선언하면 함수 안에서는 두 타입의 공통 속성과 메서드만 자동 완성된다.
* 특정 타입의 속성과 메서드를 사용하고 싶다면 `typeof`나 `in`연산자를 사용하여 타입을 구분한 후 코드를 작성해야 한다. 이런 동작을 `타입 가드`라고 한다.
```ts
function introduce(someone: Person | Developer) {
    if ('age' in someone) {
        console.log(someone.age);
    }
    if ('skill' in someone) {
        console.log(someone.skill);
    }
}

function logText(test: string | number) {
    if (typeof text === 'string') {
        console.log(text.toUpperCase())
    }
    if (typeof text === 'number') {
        console.log(text.toLocaleString())
    }
}
```

# 6.4 인터섹션 타입
* `인터섹션 타입`은 타입 2개를 하나로 합쳐서 사용할 수 있는 타입이다.
* 보통 인터페이스 2개를 합치거나 타입 정의 여러 개를 하나로 합칠 때 사용합니다.
```ts
interface Avenger {
    name: string;
}

interface Hero {
    skill: string
}

function introduce(someone: Avenger & Hero) {
    console.log(someone.name);
    console.log(someone.skill);
}

introduce({ name: '캡틴', skill: '어셈블' }); // No Error
introduce({ name: '캡틴' }); // TypeError1
```
## TypeError1
`introduce()`함수의 파라미터가 `Avenger`와 `Hero`타입의 인터섹션 타입으로 정의되어 있기 때문에 두 타입의 모든 속성을 만족하는 객체를 인자로 넘겨야 하나 `skill`속성이 없는 객체를 인자로 넘겨서 에러가 발생하였다.

# 6.5 정리
* `유니언 타입`은 자바스크립트 OR 연산자의 `|`을 이용하여 여러 개의 타입 중 1개를 사용하겠다고 선언할 수 있다.
* `인터섹션 타입`은 자바스크립트 AND 연산자의 `&`을 이용하여 인터페이스 2개를 합치거나 타입 정의 여러 개를 하나로 합치겠다고 선언할 수 있다.
