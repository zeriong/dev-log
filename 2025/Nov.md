<!-- Template !

## 📑 2025.11.11

### # React - 템플릿

```
템플릿
```

- #### 템플릿
  - 자식 컴포넌트를 정상적으로 렌더링


- #### 템플릿
  - 자식 컴포넌트를 화면에서 숨김

<br>

#### 🔍 [ [인용: 템플릿](httpswwwgooglecom) ]

<br>

---

-->

## 📑 2025.11.30

### # JavaScript - undefined와 null의 차이점

- 둘 다 **'값이 없다'**는 의미를 담고 있지만, 쓰임새와 의미에 차이가 존재

#### \* undefined

- **자바스크립트가 자동으로 할당**하는 값
- 변수를 선언했지만 값을 할당하지 않았을 때 자동 부여
- 예시: `let a;` → 자동으로 `undefined` 할당
- **"값이 아직 정의되지 않음"**을 나타냄
- 메모리 해제와 직접적인 관련 없음
  - `undefined` 상태라고 가비지 컬렉션 대상이 되는 것은 아님
  - 단순히 "값이 정의되지 않음"을 표현할 뿐

#### \* null

- **개발자가 의도적으로 할당**하는 값
- 값이 없음을 명확하게 표현하기 위해 사용
- 예시: `let b = null;` → 의도적으로 빈 값 할당
- **"의도적으로 값을 비워 둔 상태"**를 나타냄
- 메모리 관리와 관련
  - 객체를 참조하던 변수를 `null`로 설정하면 참조가 끊어짐
  - 가비지 컬렉터가 더 이상 사용되지 않는다고 판단
  - 메모리에서 제거 가능 (가비지 컬렉션 대상)
  - 대용량 데이터 객체를 `null`로 설정하여 메모리 해제 유도 가능

#### \* 비교 연산

- **느슨한 비교 (`==`)**: `null == undefined` → `true` (같게 처리)
- **엄격한 비교 (`===`)**: `null === undefined` → `false` (다르게 처리)

#### \* 정리

// undefined: 자바스크립트가 자동 할당
let a;
console.log(a); // undefined

// null: 개발자가 의도적으로 할당
let b = null;
console.log(b); // null

// 메모리 해제 유도
let largeData = { /_ 대용량 데이터 _/ };
largeData = null; // 참조 끊기 → 가비지 컬렉션 대상<br>

#### 🔍 [ [매일메일 - undefined와 null의 차이점에 대해서 설명해주세요.](https://www.maeil-mail.kr/question/63) ]

<br>

---

## 📑 2025.11.29

### # React - useEffect 호출 시점

- React의 `useEffect`는 컴포넌트의 특정 시점에 자동으로 호출되는 훅
  - **마운트**, **업데이트**, **언마운트** 시점에 호출됨

#### \* 1. 마운트 시점 (첫 렌더링 후)

- 컴포넌트가 처음 렌더링되고 나서 호출
- 데이터 초기화, 외부 API 호출, 구독 설정 등의 작업 수행
- 컴포넌트가 처음 마운트될 때 필요한 초기 작업 실행

#### \* 2. 업데이트 시점 (의존성 배열 값 변경)

- **의존성 배열에 지정된 값이 변경될 때마다** 다시 호출됨
- 실행 순서:
  1. 이전 props/state와 함께 **cleanup 함수 먼저 호출**
  2. 업데이트된 props/state와 함께 **본문 실행**
- 예시: `useEffect(() => {...}, [count])` → `count` 값 변경 시마다 호출
- **의존성 배열을 생략하면** 매 렌더링마다 호출됨

#### \* 3. 언마운트 시점 (컴포넌트 제거)

- 컴포넌트가 언마운트될 때 `return`으로 지정된 **cleanup 함수 호출**
- 이벤트 리스너 제거, 타이머 해제, 구독 취소 등의 정리 작업 수행
- useEffect를 통해 발생한 부수효과(side effect) 정리

#### \* 요약

useEffect는 다음 시점에 호출된다

1. 컴포넌트가 처음 렌더링된 후 (마운트)
2. 의존성 배열의 값이 변경될 때 (업데이트)
3. 컴포넌트가 언마운트될 때 (cleanup)

<br>

#### 🔍 [ [매일메일 - useEffect가 호출되는 시점에 대해 설명해 주세요.](https://www.maeil-mail.kr/question/64) ]

<br>

## 📑 2025.11.24

### # JavaScript - 싱글 스레드와 비동기 처리 메커니즘

- 자바스크립트는 **싱글 스레드(Single Thread)** 언어
  - 한 번에 하나의 작업만 처리 가능한 단일 콜 스택 구조
  - 브라우저/Node.js 환경이 제공하는 비동기 처리 메커니즘으로 동시 작업 처리 가능

#### \* 비동기 처리 메커니즘

- **Web API (브라우저) / libuv (Node.js)**
  - 비동기 작업(타이머, 네트워크 요청 등)을 위임받아 처리
  - setTimeout, fetch 등의 작업을 자바스크립트 엔진으로부터 넘겨받음
- **태스크 큐(Task Queue)**
  - Web API에서 완료된 비동기 작업이 대기하는 공간
- **이벤트 루프(Event Loop)**
  - 콜 스택이 비어있는지 확인
  - 태스크 큐의 대기 중인 작업을 콜 스택으로 가져와 실행

#### \* 동작 과정

1. 비동기 작업 발생 시 Web API에 위임
2. 자바스크립트 엔진은 다른 코드 실행 계속 진행
3. Web API에서 작업 완료 후 콜백을 태스크 큐에 추가
4. 이벤트 루프가 콜 스택이 비었는지 확인
5. 태스크 큐에서 작업을 꺼내 콜 스택으로 이동 후 실행

#### \* 장점

- UI 인터랙션이 끊기지 않음
- 대기 시간이 필요한 작업도 동시 실행되는 것처럼 동작
- 싱글 스레드임에도 효율적인 작업 관리 가능

<br>

### # 태스크 큐(Task Queue)의 종류

- #### 매크로태스크 큐(Macrotask Queue)

  - 일반적인 비동기 작업의 콜백이 저장되는 큐
  - **포함 작업**: setTimeout, setInterval, I/O 작업, 이벤트 핸들러
  - 이벤트 루프의 한 번의 반복마다 **하나의 태스크만** 처리
  - UI 업데이트나 다른 작업과 균형 있게 진행

- #### 마이크로태스크 큐(Microtask Queue)

  - **높은 우선순위**가 필요한 비동기 작업이 대기하는 큐
  - **포함 작업**: Promise.then, Promise.catch, Promise.finally, MutationObserver
  - 이벤트 루프는 **매크로태스크 실행 전** 항상 마이크로태스크 큐를 먼저 확인
  - 모든 마이크로태스크 처리 완료 후 매크로태스크로 이동
  - 매크로태스크보다 높은 우선순위로 처리

<br>

1. 콜 스택의 동기 코드 실행
2. 마이크로태스크 큐의 모든 작업 실행
3. 매크로태스크 큐에서 하나의 작업 실행
4. 다시 2번부터 반복

<br>

#### 🔍 [ [매일메일 - 자바스크립트는 싱글 스레드 언어인데, 어떻게 동시에 여러 작업들을 수행하나요?](https://www.maeil-mail.kr/question/57) ]

<br>

---

#### \* 실행 우선순위

## 📑 2025.11.22

### # 낙관적 업데이트(Optimistic Update)

- 서버 응답 이전에 UI를 미리 업데이트하는 기법
  - 성공적인 상태 업데이트가 이뤄질 거라는 가정 하에 동작
  - 사용자 요청을 서버가 성공적으로 처리할 거라고 예상하고 즉각 반영

#### \* 대표적인 사용 예시

- **좋아요 기능**
  - 사용자가 좋아요 버튼 클릭 시 서버 응답을 기다리지 않고 즉시 UI 변경
  - 서버 응답이 성공하면 그대로 유지
  - 실패 시 UI 롤백 또는 오류 메시지 표시

#### \* 장점

- 네트워크 속도와 무관한 즉각적인 피드백 제공
- 사용자 경험(UX) 개선 - 시스템을 빠르게 느끼게 함
- 네트워크 상태가 좋지 않거나 응답 시간이 길어도 사용자 경험에 영향 최소화

#### \* 단점 및 주의사항

- 서버 오류 발생 시 잠시 잘못된 정보가 표시될 수 있음
- 오류 핸들링(롤백) 로직 설계 필수
- 요청 실패율이 높은 환경에서는 잦은 롤백으로 오히려 UX 저해 가능

#### \* 적용을 지양해야 하는 경우

- **결제, 거래 내역 등 중요한 데이터를 다루는 경우**
  - 민감도 높은 정보가 순간적으로 잘못 표시되면 사용자 경험 크게 저해
  - 요청 실패 시 신뢰도 문제 발생 가능
- **네트워크 환경이 불안정한 경우**
  - 높은 실패율로 인한 잦은 롤백 발생
  - 서버 응답을 기다리는 것이 더 나은 판단일 수 있음

#### \* 결론

`요청 성공 가능성이 높고, 사용자 경험을 즉시 개선하는 데 큰 장점이 있을 때 적용하는 것이 적합하다.`

<br>

#### 🔍 [ [매일메일 - 낙관적 업데이트에 관하여 설명해주세요.](https://www.maeil-mail.kr/question/55) ]

<br>

## 📑 2025.11.21

### # React + Vite & NginX gzip 압축 최적화 과정 포스팅

- #### 번들사이즈: [ 1,025 KB ➔ 330 KB ] - 약 3.1배 감소 ( 67.8% )
- #### 다운로드 속도: [ 794ms ➔ 169ms ] - 약 4.7배 단축 ( 78.7% )
- #### LCP: [ 1.24s ➔ 0.58s ] - 약 2.1배 단축 ( 53.2% )

#### ✍🏻 [ [zeriong - [ React + Vite & NginX: Gzip Compression ] 번들 사이즈 최적화로 UX향상 시키기](https://zeriong.tistory.com/92) ]

<br>

---

## 📑 2025.11.20

### # React 커스텀 A4 규격 PDF 컴포넌트 구현 & 최적화 과정 포스팅

#### \* [ 18784.40ms ➔ 633.80ms ] - 약 30배 향상( 약 96.6% 시간 단축 ) 경험

#### ✍🏻 [ [zeriong - [ React: PDF Generator ] 맞춤형 PDF 렌더링 성능 최적화](https://zeriong.tistory.com/91) ]

<br>

---

## 📑 2025.11.19

### # Vanilla JS - React와 유사한 작동 방식 구현 유튜브 시청

- #### 프레임워크/라이브러리 사용 이유

  - DOM 변경점을 쉽게 감지하고 반영할 수 있는 구조 제공

- #### React의 특징

  - `innerHTML` 없이 `setState`나 리터럴 문법으로 UI 즉시 반영 가능

- #### 변경 감지 방식의 진화

  - 과거: `setInterval`로 주기적 변경 감시
  - 현재: `Proxy` / `Reflect`로 객체 변화를 감지

- #### Proxy의 장점
  - 유연한 트랩 핸들러(`get`, `set`, `has`, `apply` 등)로 객체 접근·변경을 중간에 가로채어 state 감시 가능

<br>

#### 🔍 [ [코딩애플 - 상남자 특) 리액트 직접 만들어서 씀](https://youtu.be/wakCXia3CEA?si=UcPRnsFdWLXYuh6o) ]

<br>

---

## 📑 2025.11.16

### # 링크를 통한 개인정보 유출 위험과 보안 관련 유튜브 시청

- #### URL 단축 및 QR코드의 위험성

  - 링크를 안 누르면 안전? → **아니다**
  - URL 단축 서비스를 통해 실제 주소를 숨길 수 있음
  - QR코드는 URL 전체를 보여주지 않아 더욱 위험할 수 있음
  - 실제 도착지를 확인하기 전까지 안전 여부 판단 불가

- #### 유명 사이트의 Redirect 기능 악용

  - Naver, Google, Reddit 등 신뢰할 수 있는 사이트들도 위험할 수 있음
  - 쿼리 스트링을 통한 redirect URL 기능이 존재하는 경우가 있음
  - 일부 페이지는 대응이 안 되어 있어 악용 가능성 존재
  - 신뢰할 수 있는 도메인이라도 URL 파라미터를 주의 깊게 확인 필요

- #### CSRF(Cross-Site Request Forgery) 공격

  - **공격 방식**
    - 피싱 사이트에서 정상 사이트(Reddit 등)의 댓글 작성 엔드포인트를 form으로 요청
    - 사용자가 이미 로그인되어 있어 쿠키에 토큰이 존재하는 경우
    - 사용자 계정으로 의도하지 않은 댓글이 작성될 수 있음
    - 계정 정보를 직접 알지 못해도 공격 가능
  - **대응 방법**
    - 쿠키에 `same-site` 속성 설정
    - CSRF 토큰 발급 및 검증 로직 추가
    - 중요한 작업 시 해당 토큰 제출을 필수 조건으로 설정
  - **실제 사례**: 야후(Yahoo)가 CSRF 취약점으로 문제가 된 적이 있음
  - 대부분 사이트가 대응되어 있지만 완전히 안심할 수는 없음

- #### 소셜 해킹(Social Engineering) 사례
  - **Linus Tech Tips 채널 해킹 사건**
    - 광고 제안서로 위장한 PDF 파일을 이메일로 수신
    - PDF 실행 시 겉으로는 아무 반응이 없었음
    - 백그라운드에서 YouTube 채널 해킹 스크립트가 실행됨
    - YouTube 계정 정보를 복사하여 보이지 않는 브라우저로 조작
    - 채널이 코인 광고 채널로 강제 편집됨
  - **문제점**
    - YouTube 스튜디오에서 채널 권한을 클릭 몇 번으로 쉽게 변경 가능
    - 보안보다 사용성이 우선되어 설계된 UI/UX의 취약점
  - **교훈**: 신뢰할 수 있는 출처라도 파일 실행 시 주의 필요

<br>

#### 🔍 [ [코딩애플 - 링크 하나로 진짜 개인정보를 '털' 수 있을까](https://youtu.be/FG1h4ik53eg) ]

<br>

---

## 📑 2025.11.15

### # Python LLM 챗봇 서버 리팩터링 및 통신 취소 기능 구현 과정 포스팅

#### ✍🏻 [ [zeriong - [ Python: Async HTTP ] 사내 LLM 챗봇 서버 비동기 전환을 통한 요청 취소 및 논블로킹 구조 개선기](https://zeriong.tistory.com/90) ]

<br>

---

## 📑 2025.11.14

### # Three.js Camera handling 트러블슈팅 과정 포스팅

#### ✍🏻 [ [zeriong - [ Three.js Camera handling ] Map화면 이탈 현상 막기](https://zeriong.tistory.com/89) ]

<br>

---

https://zeriong.tistory.com/89

## 📑 2025.11.13

### # openapi-generator-cli를 활용한 사내 api 자동 생성 구현 과정 포스팅

#### ✍🏻 [ [zeriong - [ openapi-generator-cli ] 사내 API 생성 자동화 도입기](https://zeriong.tistory.com/88) ]

<br>

---

## 📑 2025.11.12

### # OpenAPI + Orval로 swgger-ui spec 기반 api 자동생성

- **OpenAPI 명세(Swagger Spec)** 를 기반으로  
  **타입 안전한 API 클라이언트 코드를 자동 생성**해주는 도구.
- **TypeScript**, **React Query**, **Axios**, **SWR** 등 다양한 클라이언트 형태를 지원.
- API 스펙 변경 시, `npx orval` 명령어 한 번으로  
  최신 타입 정의 및 요청 함수 재생성 가능.
- 명세에 따라 **함수, 타입, 훅(useXXX)** 이 자동으로 만들어지므로  
  수동 작성 오류나 중복 코드를 최소화할 수 있음.
- `mutator` 옵션을 통해 custom fetch/axios 인스턴스를 주입 가능  
  → 인증 토큰, 에러 핸들링, 로깅 등 프로젝트 정책에 맞게 통합 가능.
- `mock` 옵션을 통해 **MSW(Mock Service Worker)** 와 연동한 테스트 환경도 지원.
- 결과적으로, **API 계약 일관성 유지 + 개발 생산성 향상 + 타입 안정성 확보**가 가능.

<br>

#### \* 간단한 예시

| 역할              | 설명                                      |
| ----------------- | ----------------------------------------- |
| `openapi.json`    | 백엔드에서 정의한 API 스펙 (Swagger 문서) |
| `orval.config.ts` | 생성 설정 파일 (입력/출력, 옵션 등)       |
| `npx orval`       | 명령 실행 시 자동 코드 생성               |
| `useGetUsers()`   | 예시: 자동 생성된 React Query 훅          |

<br>
 
- **OpenAPI (Swagger Spec)** 으로 API 명세 작성  
  → 프론트엔드/백엔드 간 명확한 **계약(Contract)** 확보  
- **Orval** 로 명세 기반 **TypeScript 타입 + API 호출 함수 자동 생성**
- `orval.config.ts` 에서 자동 생성 경로 및 옵션 설정  
  - `input.target`: OpenAPI 명세 경로 또는 URL  
  - `output.target`: 생성될 코드 위치  
  - `client`: 사용할 클라이언트 타입 (`react-query`, `axios`, etc.)
  - `override`: custom mutator, query 옵션 등 설정
- 자동 생성된 훅 예시: `useGetUsers()` → 바로 React 컴포넌트에서 호출 가능

<br>

#### \* 장점

- 타입 안정성(Type-Safe)
- 명세 변경 시 자동 반영
- 중복 코드 제거 및 일관성 유지
- React Query 등과 자연스럽게 통합 가능

#### \* 단점

- Swagger 명세 최신화 필수
- 생성 코드 버전 관리 필요
- 공통 에러 핸들링 및 인터셉터 고려

<br>

#### 🔍 [ [인용: ph8nt0m - 타입 안전 API 클라이언트: NextJS에서 OpenAPI와 Orval 활용하기](https://velog.io/@ph8nt0m/%ED%83%80%EC%9E%85-%EC%95%88%EC%A0%84-API-%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8-NextJS%EC%97%90%EC%84%9C-OpenAPI%EC%99%80-Orval-%ED%99%9C%EC%9A%A9%ED%95%98%EA%B8%B0) ]

<br>

---

## 📑 2025.11.11

### # JS - Symbol

- Symbol은 유일한(Unique) 식별자를 생성하는 원시 타입(Primitive Type)
- 객체의 숨김 속성 키(Hidden Property Key) 를 만들 때 주로 사용
- Symbol()로 생성할 때마다 항상 새로운 값이 만들어짐
- 절대 중복되지 않기 때문에 server-side가 존재하는 환경에서 전역 공유 방지가 가능<br>
  `나는 Next.js에서 전역 공유 방지용으로 활용한 경험이 있다.`
- 직접 접근하지 않는 한 외부에서 식별 불가능<br>
  `보안상 유리 (XSS 완화)`

<br>

#### \* 내가 활용한 예제

`공용 dialog symbols`

```javascript
/**
 * 목적: Dialog resolve 함수를 저장하기 위한 Symbol 키 정의
 * 장점: 전역 오염 방지, 충돌 방지, 타입 안정성 향상
 */

// Confirm Dialog용 Symbol
export const CONFIRM_DIALOG_RESOLVE_SYMBOL = Symbol.for(
  "app.dialog.confirm.resolve"
);

// Alert Dialog용 Symbol
export const ALERT_DIALOG_RESOLVE_SYMBOL = Symbol.for(
  "app.dialog.alert.resolve"
);

// Window 타입 확장 (TypeScript 지원)
declare global {
  interface Window {
    [CONFIRM_DIALOG_RESOLVE_SYMBOL]?: (value: boolean) => void;
    [ALERT_DIALOG_RESOLVE_SYMBOL]?: (value: boolean) => void;
  }
}

/**
 * Symbol을 사용하여 resolve 함수 저장
 * @param symbol - CONFIRM_DIALOG_RESOLVE_SYMBOL 또는 ALERT_DIALOG_RESOLVE_SYMBOL
 * @param resolver - Promise resolve 함수
 */
export const setDialogResolver = (
  symbol: symbol,
  resolver: (value: boolean) => void
): void => {
  if (typeof window !== "undefined") {
    (window as any)[symbol] = resolver;
  }
};

/**
 * Symbol을 사용하여 resolve 함수 가져오기
 * @param symbol - CONFIRM_DIALOG_RESOLVE_SYMBOL 또는 ALERT_DIALOG_RESOLVE_SYMBOL
 * @returns resolve 함수 또는 undefined
 */
export const getDialogResolver = (
  symbol: symbol
): ((value: boolean) => void) | undefined => {
  if (typeof window !== "undefined") {
    return (window as any)[symbol];
  }
  return undefined;
};

/**
 * Symbol을 사용하여 resolve 함수 제거
 * @param symbol - CONFIRM_DIALOG_RESOLVE_SYMBOL 또는 ALERT_DIALOG_RESOLVE_SYMBOL
 */
export const clearDialogResolver = (symbol: symbol): void => {
  if (typeof window !== "undefined") {
    delete (window as any)[symbol];
  }
};

```

<br>

#### 🔍 [ [인용: Inpa Dev - [JS] 📚 자바스크립트 자료형 Symbol 🚩 정리 - Inpa Dev 👨‍💻](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EC%9E%90%EB%A3%8C%ED%98%95-Symbol-%F0%9F%9A%A9-%EC%A0%95%EB%A6%AC) ]

<br>

---

## 📑 2025.11.10

### # React - useEffectEvent

`useEffectEvent`는 기존 useEffect에서 자주 발생하던 " **_의존성 관리와 최신 상태 참조_** " 의 충돌을 해결하기 위한 새로운 패턴이다.<br>
이를 통해 **불필요한 재연결**이나 **이중 effect 실행 없이, 항상 최신 상태를 참조하는 안정적인 로직**을 작성할 수 있다.

<br>

```javascript
// 기존 예제 코드
function ChatRoom({ roomId, theme }) {
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.on("connected", () => {
      showNotification("Connected!", theme);
    });
    connection.connect();
    return () => connection.disconnect();
  }, [roomId, theme]); // theme 변경 시에도 재연결하게 됨
}
```

```javascript
// useEffectEvent를 활용하여 우아한 분리가 가능해짐
function ChatRoom({ roomId, theme }) {
  // 내부 state는 실행 시점에서 항상 최신 상태를 참조하게 됨
  const onConnected = useEffectEvent(() => {
    showNotification("Connected!", theme);
  });

  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.on("connected", () => {
      onConnected();
    });
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]); // theme은 의존성 배열에서 제외하여 분리
}
```

<br>

#### 🔍 [ [인용: ZeroChoTV - React 필수 기능 - useEffectEvent (React 19.2 신기능)](https://youtube.com/shorts/RlEppB9pan8?si=gQgRtKWfLBagXxYS) ]

<br>

---

## 📑 2025.11.09

### # React - Activity

```
<Activity> 컴포넌트는 기존의 조건부 렌더링({condition && <Component />})의 대안으로
컴포넌트를 완전히 언마운트하지 않고 숨길 수 있는 방법을 제공한다.

마운트/언마운트 시 cleanup 등은 제대로 실행되면서 mode가 hidden일 때 사용자에게 보이지 않지만
state가 휘발되지 않고 유지되는 기능적 장점이 있다.
( 이전에는 필요하다면 해당 기능을 직접 구현해야 했지만 이젠 Activity 컴포넌트를 통해 활용 가능 )
```

- #### "visible" mode

  - 자식 컴포넌트를 정상적으로 렌더링
  - 이펙트(useEffect 등)를 마운트
  - 업데이트를 정상 우선순위로 처리
  - 사용자에게 보임

- #### "hidden" mode
  - 자식 컴포넌트를 화면에서 숨김
  - 이펙트를 언마운트 (cleanup 함수 실행)
  - 모든 업데이트를 낮은 우선순위로 지연 처리
  - 중요: 컴포넌트 상태는 그대로 유지됨

<br>

#### 🔍 [ [인용: ZeroChoTV - React가 일을 하기 시작했다 - Activity (React 19.2 신기능)](https://youtube.com/shorts/RlEppB9pan8?si=gQgRtKWfLBagXxYS) ]

<br>

---

## 📑 2025.11.08

### # 검색창에 google.com을 치면 일어나는 일

- #### 캐싱 DNS 레코드를 우선 탐색하며 레코드가 없다면 단계별로 탐색을 실행한다.

  1. **브라우저** 캐시 확인<br>

  2. **OS** 캐시 확인<br>
     `systemcall로 접근 가능한 OS 캐시`<br>

  3. **라우터** 캐시 확인<br>
     `집에서 사용하는 공유기 같은 개념 ( ISP와 연결을 담당하는 시스템 )`<br>

  4. **ISP** 캐시 확인<br>
     `ISP는 Internet Service Provider의 약자로 SKT, KT 등 인터넷 통신사를 의미`<br><br>

  [ 브라우저 기준으로 가까운 순으로 캐싱 DNS 레코드를 우선 탐색하여 연결을 시도한다. ]<br><br>

- #### 캐싱된 DNS 레코드가 없다면 ISP의 DNS서버가 DNS-query를 통해 주소를 탐색한다.

  - DNS - Recursor가 도메인 level 간 Recursive Search를 진행한다.<br>
    `Recursive Search: DNS-query를 재귀적으로 날리며 주소를 찾거나 더 이상 찾을 수 없을 때까지 탐색하는 것`<br>

<img width="1241" height="679" alt="image" src="https://github.com/user-attachments/assets/6214ebeb-4a92-4394-8881-4781b01e512d" />

```
이미지와 같이 Root Domains부터 시작하여 탐색한다.

1. Root Domains - "www.google.com" ip를 얻고싶다고 요청

2. TLD( Top-Level-Domains )
  => [ com. ]

3. SLD( Second-Level-Domains )
  => [ google. + com. ]

4. TLD( Third-Level-Domains )
  => [ www. + google. + com. ]

결과:
[ www.google.com. ]
가장 마지막에 "."이 포하되어있지만 이는 루트 도메인을 명시한 완전한 도메인 이름인
FQDN( Fully Qualified Domain Name )이고 실제로는 .을 제외해야 접근이 가능하도록
하는 리버스 프록시를 적용해두는 것이 보편적이다.
```

<br>

- #### 브라우저가 TCP/IP 프로토콜을 사용해 서버와 연결하고 웹사이트를 렌더링한다.

  - 브라우저는 올바른 IP를 수신받으면 IP 주소와 일치하는 서버와 연결하여 정보 전송
  - 인터넷 프로토콜( IP / Internet Protocol )을 통해 연결을 구축
    `보편적으로 TCP 전송제어 프로토콜을 사용`
  - TCP/IP three-way handshake 프로세스를 통해서 클라이언트와 서버의 connection이 이루어짐
  - https를 사용하는 경우 데이터를 안전하게 연결하기 위해 TLS(Transport Layer Security) 핸드쉐이크 과정이 추가됨<br>
    `TLS를 통해 전송 데이터가 암호화되어 보안성이 향상된다.`

<br><br>

#### 정리:

`www.google.com을 검색하면?`

1. 캐싱된 DNS레코드를 `브라우저` -> `OS` -> `라우터(공유기)` -> `ISP(통신사)` 순으로 우선 탐색
2. 캐싱데이터가 없다면 DNS-query를 날려 Recursive search를 진행
3. `Root Domains` -> `TLD` -> `SLD` -> `TLD` 순서로 DNS-query를 지속적으로 날리며 실패하거나 마지막까지 성공할때까지 탐색
4. 올바른 IP를 수신 받은 경우 TCP/IP 프로토콜을 통해 연결( https를 사용하는 경우 TLS를 통해 암호화 연결 )
5. 웹사이트 제공( 렌더링 )

<br>

#### 🔍 [ [인용: Duu.log - 브라우저에 www.google.com 을 치면 일어나는일 ](https://velog.io/@o1011/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%97%90-www.google.com-%EC%9D%84-%EC%B9%98%EB%A9%B4-%EC%9D%BC%EC%96%B4%EB%82%98%EB%8A%94%EC%9D%BC) ]

<br>

---

## 📑 2025.11.07

### # CommonJS와 ES Module의 차이점

- CommonJS
  - `require`키워드를 사용해 모듈을 가져오고 동기적으로 작동하며 주로 Node.js 환경에서 서버를 구성할 때 많이 사용해왔다.<br>
    `동기적으로 작동하기 때문에 모듈이 로드될 때까지 다음 코드가 실행되지 않고 기다린다.`
- ESM( ES Module )
  - ES6부터 도입된 기술로 브라우저와 Node.js 모두 활용 가능하며 모듈을 비동기적으로 로드한다.<br>
    `모듈을 가져올 때는 "import"를 사용하고 내보낼 땐 export를 사용한다.`<br>
    `ESM은 정적 분석이 가능하여 트리쉐이킹 최적화 작업에 유리하다.`

#### 🔍 [ [인용: https://www.maeil-mail.kr/question/38](https://www.maeil-mail.kr/question/38) ]

<br>

---

## 📑 2025.11.04

### # Concurrent Rendering

```
동시성 렌더링: 렌더링을 긴급/전환 업데이트로 분류하여 긴급한 렌더링 작업을 우선 업데이트 함
이는 속도 연산 속도 개선이 아닌, 우선순위를 만들어 긴급한 업데이트를 우선 처리하여 UI 차단을 최소화 시키는 것이다.

startTransition:
startTransition로 감싸진 set 함수는 전환 업데이트로 처리됨

useTransition:
startTransition함수, isPending을 반환하며 지연 중일 경우 isPending는 true가 됨

useDeferredValue:
startTransition을 사용하지 못하고나 props를 지연하고 싶을 때 사용하며 반환 값으로 화면 업데이트 시 지연처리 함
```

- React19에서는 Concurrent Rendering을 기본모드로 적용된다.<br>

- React17 이전 버전에서는 렌더링이 동기적으로 작동했다.<br>
  `컴포넌트 트리 전체를 렌더링할 때 도중에 멈출 수 없어 한번 실행되면 끝까지 렌더링을 완료해야 했음`<br>
  `- 대형컴포넌트 트리에서는 렌더링이 잠시 freeze 되는 문제가 있음`<br>
  `- 사용자가 스크롤, 입력 등 다른 작업을 하더라도 렌더링이 끝날 때까지 응답이 늦어짐`

<br>

**\*활용법** <br>

```
지연시키고 싶은 dispatcher를 Concurrent rendering hook(startTransition/useDeferredValue)
으로 감싸면 해당 dispatcher는 전환(후순위) 업데이트가 되어 내부 Concurrent rendering hook으로
감싸지 않은 dispatcher와 로직을 실행한 후 마지막으로 dispatch된다.
```

<br>

#### 🔍 [ [학습에 참고한 출처: https://beomy.github.io/tech/react/concurrent-rendering/ ](https://beomy.github.io/tech/react/concurrent-rendering/) ]

<br>

---

## 📑 2025.11.02

### # Edge case & Corner case 용어 정리

- Edge case( 경계 조건 )
  - 주로 예상하기 어려운 특이케이스를 엣지케이스라고 한다.<br>
    `글자 오버플로우, 빠른 입력 이벤트, 디바운스/스로틀링 타이밍 오차 등 `
- Corner case( 복합 경계 조건 )
  - 2가지 이상의 Edge case가 동시에 발생하는 케이스를 코너케이스라고 한다.<br>
    `특정 validate가 필요한 인풋에 텍스트를 붙여넣기를 했을 때 지정 length를 넘기며 validate가 무시되는 경우 등`

#### 🔍 [ [학습에 참고한 블로그 출처: https://daryeou.tistory.com/203#google_vignette](https://daryeou.tistory.com/203#google_vignette) ]

<br>

---

## 📑 2025.11.01

### # 프론트 최적화 기법 관련 유튜브 시청

1. 전송 최적화 ( 로딩의 80%는 네트워크에 있다. )

   - http/3 (핸드쉐이크가 없고 병렬 요청이 가능한 장점이 있다)
   - gzip 말고도 Brotli/Zstd 등의 향상된 압축 기법이 있다.
   - 이미지, 폰트, js, css 캐시설정 - cache-control / ETag
   - CDN 활용

2. 이미지 최적화

   - 주로 LCP(Largest Contentful Paint)를 중요한 지표로 생각한다.
   - avif 또는 webp를 사용
   - 사이즈별 리사이즈와 lazy loading 활용용

3. 폰트 최적화

   - woff2 적극활용
   - fout활용 + size adust로 자연스럽게 조정

4. 더 나은 번들

   - lazy 임포트 활용
   - tree shacking 적용 되도록 설정
   - rollup-plugin-visualizer과 같은 라이브러리로 번들 사이즈를 시각화하여 확인

5. 데이터 페칭 최적화

   - 가능한 병렬처리(Promise.all)를 활용하고 SWR, React Query 등 캐싱기능을 활용하면 좋다.

6. 렌더링 최적화

   - 작업이 많은 경우 작업을 나누어 지연처리
     `idle callback( requestidlecallback ) 사용해서 처리할 수 있다.`
   - reflow - repaint 줄이기
     `composite 변경 활용`
   - 어쩔 수 없는 경우에 읽기-쓰기를 반복하는 것을 지양해야 함
     `반복문 처리가 필요한 경우 변수에 데이터를 담고 마지막에 적용하는 등`

7. 프리페칭( 사전로딩 )

   - IntersectionObserver를 활용해서 특정 요소가 보일 때 head에 미리 link를 넣기도 가능
   - api를 담아두기
     `reactQuery - prefetchQuery 미리 api를 담아둘 수 있음`

- **\*성능 모니터링 자동화**

  - LCP(가장 큰 리소스를 가져오는 시간) / INP(반응 시간) / CLS(예상치 못한 움직임)
  - web vitals - github에 있고 해당 라이브러리에서 제공하는 api를 활용하면 ga4와 연계해서 자동화 할 수도 있음

    <br>

#### 🔍 [ [프롱트 - 2025 웹 성능 최적화 기법 7가지](https://youtu.be/Ngqxa0B6DqE?si=BXXnZsI7lDbOAMUe) ]

<br>
