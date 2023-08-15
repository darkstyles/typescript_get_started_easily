# 3.1 변수에 타입을 정의하는 방법
* 변수 이름 뒤에 콜론(:)을 붙여서 해당 변수의 타입을 정의할 수 있다.
* 콜론(:)을 `타입 표기`라고 한다.
* `타입 표기`는 변수뿐만 아니라 함수에도 사용할 수 있다.
```ts
const name: string = 'captain';
```

# 3.2 기본 타입
주요 데이터 타입은 다음 아홉 가지 타입이다.
* string
* number
* boolean
* object
* Array
* tuple
* any
* null
* undefined

## 문자열 타입: string
* `string`은 문자열을 의미하는 타입이다.
* 특정 데이터 타입이 문자열이면 `string`타입으로 선언한다.
```ts
const name: string = 'captain';
```

## 숫자 타입: number
* 특정 변수가 숫자만 취급한다면 `number`타입으로 선언한다.
```ts
const age: number = 33;
```

## 진위 타입: boolean
* 진위 값만 취급하는 변수에는 `boolean`타입으로 선언한다.
```ts
const isLogin: boolean = false;
```

## 객체 타입: object
* 객체 유형의 데이터를 취급할 때는 `object`라는 타입을 사용한다.
* 타입스크립트의 장점을 극대화하려면 가급적 타입을 최대한 구체적으로 선언해야하므로 `object`를 구체적으로 명시하는게 좋다.
```ts
const hero: object = { name: 'captain', age: 100 };
```
## 배열 타입: Array
배열 타입은 두가지 방법으로 선언할 수 있다.
```ts
const companies: Array<string> = ['naver', 'samsung', 'google'];
const companies: string[] = ['naver', 'samsung', 'google']

const cards: Array<number> = [12, 4, 7];
const cards: number[] = [12, 4, 7];
```
## 튜플 타입: tuple
* 배열 길이가 고정되고 각 요소 타입이 정의된 배열을 튜플이라고 한다.
* 튜플 형태의 데이터를 취급할 때 `tuple`타입으로 선언한다.
```ts
const items: [string, number] = ['hi', 11]
```

## any
* `any`타입은 아무 데이터나 취급하겠다는 의미이다.
* 타입스트립트에서 자바스크립트의 유연함을 취하려고 할 때 사용하는 타입이다.
```ts
const myName: any = '캡틴';
myName = 100;
```
## null과 undefined
* 자바스크립트에서 `null`은 의도적인 빈 값을 의미한다.
* `undefined`는 변수를 선언할 때 값을 할당하지 않으면 기본적으로 할당되는 초기값이다.
* 타입스크립트 설정 파일의 `strict`옵션에 따라서 사용 여부가 결정된다.
```ts
const empty: null = null;
const notingAssigned: undefined;
```

# 3.3 함수에 타입을 정의하는 방법
## 함수란?
* 함수는 `function`이라는 예약어와 함수이름으로 함수를 선언할 수 있고, 함수 본문에 `return`을 추가해서 값을 반환하거나 함수 실행을 종료할 수 있다.
* 함수는 입력 값에 따라 출력 값이 달라진다.

## 함수의 타입 정의: 파라미터와 반환값
```ts
function sayWord(word: string): string {
    return word;
}
```

# 3.4 타입스크립트 함수의 인자 특징
* 타입스크립트에서는 파라미터와 인자의 개수가 다르면 에러가 발생한다.
* 함수를 추론하여 에러를 표시하므로 함수를 정의된 스펙에 맞게 올바르게 사용할 수 있다.

# 3.5 옵셔널 파라미터
* 함수의 파라미터를 선택적으로 사용하고 싶을 때 옵셔널 파라미터를 사용한다.
* 옵셔널 파라미터는 `?`로 표기한다.
```ts
function sayMyName(firstName: string, lastName?: string): string {
    return 'my name:' + firstName + ' ' + lastName;
}
```
