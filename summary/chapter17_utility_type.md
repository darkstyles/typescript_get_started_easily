# 17.1 유틸리티 타입이란?
* 이미 정의되어 있는 타입 구졸르 변경하여 재사용하고 싶을 때 사용하는 타입이다.
* 타입스크립트에서 미리 정의해 놓은 내장 타입이다.

# 17.2 Pick 유틸리티 타입
* 특정 타입의 속성을 뽑아서 새로운 타입을 만들어 낼 때 사용한다.
* 이미 존재하는 타입의 특정 속성만 추출해서 새로운 타입으로 정의할 수 있다.
```ts
Pick<대상 타입, '대상 타입의 속성 이름'>
Pick<대상 타입, '대상 타입의 속성 1 이름' | '대상 타입의 속성 2 이름'>
```
```ts
interface UserProfile {
    id: string;
    name: string;
    address: string;
}

type HulkProfile = Pick<UserProfile, 'id' | 'name'>;

const helk: HulkProfile = {
    id: '1',
    name: '헐크'
}
```
https://github.com/ghaiklor/type-challenges-solutions/blob/main/ko/easy-pick.md

# 17.3 Omit 유틸리티 타입
* 특정 타입에서 속성 몇개를 제외한 나머지 속성으로 새로운 타입을 생성할 때 사용한다.
```ts
Omit<대상 타입, '대상 타입의 속성 이름'>;
Omit<대상 타입, '대상 타입의 속성 1 이름' | '대상 타입의 속성 2 이름'>;
```
```ts
interface UserProfile {
    id: string;
    name: string;
    address: string;
}

type User = Omit<UserProfile, 'address'>;
```

https://github.com/ghaiklor/type-challenges-solutions/blob/main/ko/medium-omit.md

# 17.4 Partial 유틸리티 타입
* 특정 타입의 모든 속성을 모두 옵션 속성으로 변환한 타입을 생성해 준다.
```ts
Partial<대상 타입>
```
```ts
interface Todo {
    id: string;
    title: string;
}

type OptionalTodo = Partial<Todo>;
```

# 17.5 Exclude 유틸리티 타입
* 유니언 타입을 구성하는 특정 타입을 제외할 때 사용한다.
* Exclude 타입은 유니언 타입을 변형한다.
```ts
Exclude<대상 유니언 타입, '제거할 타입 이름'>;
Exclude<대상 유니언 타입, '제거할 타입 이름 1' | '제거할 타입 이름 2'>;
```
```ts
type Languages = 'C' | 'Java' | 'TypeScript' | 'React';
type TrueLanguages = Exclude<Languages, 'React'>;
type WebLanguages = Exclude<Languages, 'C' | 'Java' | 'React'>;
```
https://www.totaltypescript.com/uses-for-exclude-type-helper

# 17.6 Record 유틸리티 타입
* 타입 1개를 속성의 키로 받고 다른 타입 1개를 속성 값으로 받아 객체 타입으로 변환해 준다.
```ts
Record<객체 속성의 키로 사용할 타입, 객체 속성의 값으로 사용할 타입>
```
```ts
type HeroProfile = {
    skill: string;
    age: number;
}

type HeroNames = 'thor' | 'hulk' | 'capt';
type Heroes = Record<HeroNames, HeroProfile>;
```
https://www.totaltypescript.com/uses-for-extract-type-helper
https://www.totaltypescript.com/the-empty-object-type-in-typescript