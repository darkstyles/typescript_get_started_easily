# 18.1 맵드 타입 첫 번째 예시: in
* 이미 정의된 타입으로 새로운 타입을 생성할 수 있다.
```ts
type HeroNames = 'capt' | 'hulk' | 'thor';
type HeroAttendance = {
    [Name in HeroNames]: boolean;
}
```

# 18.2 map() API로 이해하는 맵드 타입
* 특정 배열의 각 요소를 변환하여 새로운 배열로 만들어 준다.
* 기존 배열 값을 변경하지 않고 새로운 배열을 생성한다.

# 18.3 맵드 타입 두 번째 예시: keyof
* 문자열 유니언 타입을 이용하여 객체 형태의 타입으로 변환할 수 있다.
* 객체 형태의 타입에서 일부 타입 정의만 변경한 새로운 객체 타입을 정의할 수 있다.
```ts
interface Hero {
    name: string;
    skill: string;
}

type HeroPropCheck = {
    [H in keyof Hero]: boolean
};

type HeroPropCheck = {
    [H in 'name' | 'skill']: boolean
}
```

# 18.4 맵드 타입을 사용할 때 주의할 점
* 인덱스 시그니처 문법 안에서 사용하는 in앞의 타입 이름은 개발자 마음대로 지을 수 있다.
* 문자열 유니언 타입, 인터페이스, 타입 별칭으로 정의된 타입도 맵드 타입으로 변환할 수 있다.
```ts
interface Hero {
    name: string;
    skill: string;
}

type HeroPropCheck = {
    [H in keyof Hero]: boolean
};

type Hero = {
    name: string;
    skill: string;
}

type HeroPropCheck = {
    [H in keyof Hero]: boolean
};
```
```ts
// No Error
type UserName = string;
type AddressBook = {
    [U in UserName]: number;
}

// Error
type Login = boolean;
type LoginAuth = {
    [L in Login]: string;
}
```

# 18.5 매핑 수정자
* 맵드 타입으로 타입을 변환할 때 속성 성질을 변환할 수 있도록 도와주는 문법이다.
```ts
type Hero = {
    name: string;
    skill: string;
}

// 필수값 -> 옵션값으로 변환
type HeroOptional = {
    [H in keyof Hero]?: string;
}

// 옵션값 -> 필수값으로 변환
type HeroRequired<T> = {
    [Property in keyof T]-?: T[Property];
}

const capt: HeroRequired<HeroOptional> = {
    name: '캡틴',
    skill: '방패 던지기'
}
```
