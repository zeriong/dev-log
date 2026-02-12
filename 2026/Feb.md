## 📑 2026.02.12

### # TypeScript - 유틸리티 타입 총정리

`TypeScript는 공통 타입 변환을 용이하게 하기 위해 유틸리티 타입(Utility Types)을 제공한다. 기존의 인터페이스, 제네릭 등의 기본 문법으로도 타입 변환이 가능하지만, 유틸리티 타입을 사용하면 훨씬 간결하게 이미 정의해 놓은 타입을 변환할 수 있다. 내부적으로 맵드 타입(Mapped Types)과 조건부 타입(Conditional Types) 문법을 조합한 것이다.`

#### * 객체(인터페이스) 관련 유틸리티 타입

- **Partial\<T\>:** T의 모든 속성을 **선택적(optional)** 으로 변경한 새로운 타입 반환
- **Required\<T\>:** T의 모든 속성을 **필수(required)** 로 변경한 새로운 타입 반환
- **Readonly\<T\>:** T의 모든 속성을 **읽기 전용(readonly)** 으로 변경한 새로운 타입 반환
- **Record\<K, T\>:** K를 속성 키로, T를 속성값의 타입으로 지정하는 새로운 타입 반환
- **Pick\<T, K\>:** T에서 K에 해당하는 속성만 **선택**하여 새로운 타입 반환
- **Omit\<T, K\>:** T에서 K에 해당하는 속성을 **제외**한 나머지로 새로운 타입 반환

```typescript
interface User {
  name: string;
  age: number;
  email: string;
}

type PartialUser = Partial<User>;    // { name?: string; age?: number; email?: string; }
type RequiredUser = Required<User>;  // 모든 속성 필수
type ReadonlyUser = Readonly<User>;  // 모든 속성 readonly

type UserRecord = Record<'name' | 'age', string>;  // { name: string; age: string; }
type PickedUser = Pick<User, 'name' | 'email'>;    // { name: string; email: string; }
type OmittedUser = Omit<User, 'email'>;             // { name: string; age: number; }
```

#### * 유니언 관련 유틸리티 타입

- **Exclude\<T, U\>:** 유니언 T에서 U에 해당하는 타입을 **제외**한 나머지 반환
- **Extract\<T, U\>:** 유니언 T에서 U와 **겹치는 타입만 추출**하여 반환
- **NonNullable\<T\>:** 유니언 T에서 **null과 undefined를 제외**한 타입 반환

```typescript
type T1 = Exclude<'a' | 'b' | 'c', 'a'>;          // 'b' | 'c'
type T2 = Extract<string | number | boolean, number | boolean>;  // number | boolean
type T3 = NonNullable<string | number | null | undefined>;       // string | number
```

#### * 함수 관련 유틸리티 타입

- **Parameters\<T\>:** 함수 T의 **매개변수 타입**을 튜플(Tuple) 타입으로 반환
- **ReturnType\<T\>:** 함수 T의 **반환(Return) 타입**을 반환
- **ConstructorParameters\<T\>:** 클래스 T의 **생성자 매개변수 타입**을 튜플 타입으로 반환
- **InstanceType\<T\>:** 클래스 T의 **인스턴스 타입**을 반환

```typescript
function greet(name: string, age: number): string {
  return `${name}은 ${age}살`;
}

type Params = Parameters<typeof greet>;   // [name: string, age: number]
type Return = ReturnType<typeof greet>;   // string

class User {
  constructor(public name: string, public age: number) {}
}

type CtorParams = ConstructorParameters<typeof User>;  // [string, number]
type Instance = InstanceType<typeof User>;              // User
```

#### * this 관련 유틸리티 타입

- **ThisParameterType\<T\>:** 함수 T의 **명시적 this 매개변수 타입** 반환 (없으면 unknown)
- **OmitThisParameter\<T\>:** 함수 T에서 **명시적 this 매개변수를 제거**한 함수 타입 반환
- **ThisType\<T\>:** 객체의 **this 컨텍스트를 T로 명시** (별도 반환 없음)

```typescript
interface ICat { name: string; }

function someFn(this: ICat, greeting: string) {
  console.log(`${greeting} ${this.name}`);
}

type ThisParam = ThisParameterType<typeof someFn>;    // ICat
type NoThis = OmitThisParameter<typeof someFn>;       // (greeting: string) => void
```

#### * 내장 문자열 조작 유틸리티 타입

- **Uppercase\<T\>:** 문자열 리터럴 타입의 모든 문자를 **대문자**로 변환
- **Lowercase\<T\>:** 문자열 리터럴 타입의 모든 문자를 **소문자**로 변환
- **Capitalize\<T\>:** 문자열 리터럴 타입의 **첫 문자만 대문자**로 변환
- **Uncapitalize\<T\>:** 문자열 리터럴 타입의 **첫 문자만 소문자**로 변환

```typescript
type T1 = Uppercase<'hello'>;      // 'HELLO'
type T2 = Lowercase<'HELLO'>;      // 'hello'
type T3 = Capitalize<'hello'>;     // 'Hello'
type T4 = Uncapitalize<'HELLO'>;   // 'hELLO'
```

#### * 실무 활용 팁

- Partial은 모든 속성을 옵셔널로 만들기 때문에 남용 시 타입 안정성이 떨어질 수 있으므로, **Pick이나 Omit을 더 활용하는 것이 권장**됨
- 유틸리티 타입은 내부적으로 맵드 타입 + 조건부 타입의 조합이므로, **원리를 이해하면 커스텀 유틸리티 타입도 직접 만들 수 있음**
- ThisParameterType, OmitThisParameter는 **--strictFunctionTypes** 옵션 활성화 시 올바르게 동작
- ThisType은 **--noImplicitThis** 플래그가 필요

#### [🔍 [ Inpa Dev - 📘 타입스크립트 유틸리티 타입 💯 총정리 ](https://inpa.tistory.com/entry/TS-%F0%9F%93%98-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9C%A0%ED%8B%B8%EB%A6%AC%ED%8B%B0-%ED%83%80%EC%9E%85-%F0%9F%92%AF-%EC%B4%9D%EC%A0%95%EB%A6%AC) ]

<br>

---
