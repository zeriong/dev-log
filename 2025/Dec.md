## 📑 2025.12.06

### # React - Error Boundary

- **컴포넌트에서 발생하는 오류를 잡아내고, 전체 애플리케이션이 다운되는 것을 방지하기 위한 특수한 컴포넌트**
- 오류 발생 시 대체 UI를 제공하여 애플리케이션의 신뢰성과 사용자 경험 향상
- **클래스형 컴포넌트에서만 사용 가능**

#### * Error Boundary의 필요성

- React는 기본적으로 비동기 작업의 오류를 자동으로 처리하지 않음
- 오류 발생 시 문제점:
  - 페이지 전체가 하얗게 변함
  - 사용자 입장에서 알 수 없는 화면 표시
  - 사용자 경험 크게 저해
  - 대규모 애플리케이션의 신뢰성 문제

#### * Error Boundary의 역할

- **에러가 발생한 영역에서 대체 UI를 표시**
- **애플리케이션의 나머지 부분은 정상 동작**
- 오류가 발생한 컴포넌트만 대체 UI로 전환
- 애플리케이션의 안정성 유지
- 사용자에게 오류 메시지나 대체 화면 제공

#### * 구현 방법

클래스형 컴포넌트의 두 가지 라이프사이클 메서드 사용:

1. **getDerivedStateFromError**
   - 오류 발생 시 상태 업데이트
   - 대체 UI를 렌더링하도록 상태 변경

2. **componentDidCatch**
   - 오류 정보를 로깅
   - 에러 리포팅 서비스에 전송 가능

```javascript
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // 다음 렌더링에서 대체 UI를 표시하도록 상태 업데이트
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    // 오류 로깅
    console.error('Error caught by boundary:', error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      // 대체 UI 렌더링
      return <h1>문제가 발생했습니다. 잠시 후 다시 시도해주세요.</h1>;
    }

    return this.props.children;
  }
}
```

#### * 사용 예시
```javascript
function App() {
  return (
    <ErrorBoundary>
      <MyComponent />
    </ErrorBoundary>
  );
}
```

#### * 선언형 처리의 의미

- **"무엇을 해야 하는지"를 정의하는 방식**
- "어떻게 할지"에 대한 세부 절차를 직접 작성하지 않아도 됨
- 예: "이 컴포넌트가 오류를 감지하면 특정 대체 UI를 보여준다"
- 실제 오류 처리 절차는 컴포넌트가 알아서 처리

#### * 유지 보수성 향상 이유

1. **높은 가독성**
   - 선언형 코드가 명령형 코드보다 직관적이고 간결
   - Error Boundary로 감싼 영역의 에러 처리 방식을 한눈에 파악 가능

2. **명확한 관심사 분리**
   - 비즈니스 로직과 에러 처리 로직이 명확하게 분리
   - 코드의 복잡성 감소

3. **재사용성**
   - Error Boundary를 여러 컴포넌트에 재사용 가능
   - 일관된 에러 처리 패턴 유지

#### * 장점 정리

- 애플리케이션 전체 다운 방지
- 부분적 오류 격리 (오류 발생 영역만 영향)
- 사용자 친화적 에러 메시지 제공
- 선언형 에러 처리로 코드 가독성 향상
- 유지 보수성 개선

#### * 주의사항

Error Boundary가 **캐치하지 못하는** 에러:
- 이벤트 핸들러 내부의 에러
- 비동기 코드 (setTimeout, Promise 등)
- 서버 사이드 렌더링
- Error Boundary 자체에서 발생한 에러

<br>

#### 🔍 [ [매일메일 - Error Boundary가 무엇이며, 이를 사용하는 이유는 무엇인가요?](https://www.maeil-mail.kr/question/75) ]

<br>

---

## 📑 2025.12.04

### # JavaScript - 함수 선언식과 함수 표현식의 차이점

- 자바스크립트에서 함수를 정의하는 두 가지 방법
- **주요 차이점: 호이스팅(Hoisting)**

#### * 함수 선언식 (Function Declaration)

- 이름이 있는 함수
- **호이스팅이 발생**
  - 자바스크립트 엔진이 코드 실행 전에 메모리에 로드
  - 코드 내 어디서든 호출 가능
  - 선언 위치보다 앞에서 호출해도 정상 작동

```javascript
// 선언 전 호출 가능 (호이스팅)
console.log(add(2, 3)); // 5

function add(a, b) {
    return a + b;
}
```

#### * 함수 표현식 (Function Expression)

- **변수에 익명 함수를 할당**하는 방식
- 할당된 변수명으로 호출
- **호이스팅이 되지 않음**
  - 변수에 할당된 이후에만 호출 가능
  - 코드 흐름상 변수가 선언된 후에만 사용 가능
  - 선언 전 호출 시 에러 발생
 
```javascript
// 선언 전 호출 불가능 (호이스팅 안됨)
console.log(multiply(2, 3)); // ReferenceError: Cannot access 'multiply' before initialization

const multiply = function (a, b) {
    return a * b;
};

// 선언 후 호출 가능
console.log(multiply(2, 3)); // 6#### * 비교 정리
```

| 구분 | 함수 선언식 | 함수 표현식 |
|------|------------|------------|
| 형태 | `function name() {}` | `const name = function() {}` |
| 호이스팅 | 발생 (어디서든 호출 가능) | 안됨 (선언 후에만 호출) |
| 사용 시점 | 코드 어디서든 | 변수 할당 이후만 |
| 에러 | 선언 전 호출 시 정상 작동 | 선언 전 호출 시 ReferenceError |

#### * 실무 활용

```javascript
// 함수 선언식 - 호이스팅 활용
init(); // 정상 작동

function init() {
  console.log('초기화 완료');
}

// 함수 표현식 - 명확한 코드 흐름
const config = {
  apiUrl: 'https://api.example.com'
};

const fetchData = function() {
  return fetch(config.apiUrl);
};

fetchData(); // config 이후에 호출하여 명확한 의존성#### * 정리
```

- **함수 선언식**: 호이스팅이 되어 코드 어디서든 호출 가능
- **함수 표현식**: 변수에 할당된 후에만 사용 가능
- 선택 기준: 코드 구조와 의도에 따라 적절히 선택

<br>

#### 🔍 [ [매일메일 - 함수 선언식과 함수 표현식의 차이점에 대해서 설명해주세요.](https://www.maeil-mail.kr/question/68) ]

<br>

---


## 📑 2025.12.03

### # JavaScript - Promise

- **비동기 작업을 관리하고, 해당 작업의 성공 또는 실패 결과를 나중에 사용할 수 있도록 하는 객체**
- 비동기 작업의 완료 여부를 약속해주는 개념
- 콜백 지옥 문제를 해결하고 비동기 처리의 가독성을 높임

#### * 등장 배경

- 자바스크립트는 비동기 처리를 위해 콜백 함수를 많이 사용
- 코드가 복잡해짐에 따라 콜백이 중첩되는 **"콜백 지옥"** 문제 발생
- Promise는 비동기 처리의 가독성을 높이고 코드 흐름을 명확하게 관리

#### * Promise의 3가지 상태

1. **Pending (대기)**
   - 비동기 작업이 아직 완료되지 않은 초기 상태
   
2. **Fulfilled (이행)**
   - 비동기 작업이 성공적으로 완료되어 값을 반환한 상태
   
3. **Rejected (거부)**
   - 비동기 작업이 실패하여 오류를 반환한 상태

#### * 상태 전환

- `Pending` → `Fulfilled` 또는 `Rejected`로 전환
- 한번 상태가 전환되면 다른 상태로 변경되지 않음
- 결과 값을 통해 작업의 성공 여부 확인 가능

#### * 핵심 메서드

- **resolve()**
  - 비동기 작업이 성공했을 때 값을 전달
  - Promise를 `fulfilled` 상태로 전환
  
- **reject()**
  - 비동기 작업이 실패했을 때 오류를 전달
  - Promise를 `rejected` 상태로 전환

#### * 장점

- 코드의 가독성 향상
- 비동기 작업의 흐름 제어에 유용
- 여러 Promise를 순차적으로 연결 가능 (Promise Chaining)
- `Promise.all()`, `allSettled()` 등으로 병렬 비동기 작업 처리 가능

#### * 단점

1. **복잡한 에러 처리**
   - 단일 체인에서는 간단하지만 여러 Promise가 중첩되면 복잡도 증가
   - `then()` 체인 내 중간 단계에서 발생하는 에러를 세밀하게 다루기 어려움
   - 다양한 에러를 모두 처리하려면 코드가 복잡해질 수 있음

2. **콜백 지옥을 완전히 해결하지는 못함**
   - 비동기 작업이 복잡하게 중첩되면 여전히 여러 `then()` 메서드가 연속 사용
   - 순차적 실행 시 `then()` 체인이 길어지면 들여쓰기 구조가 복잡해짐
   - `async/await`을 통해 개선 가능

#### * 예시
```javascript
// Promise 생성
const promise = new Promise((resolve, reject) => {
  // 비동기 작업
  setTimeout(() => {
    const success = true;
    
    if (success) {
      resolve('작업 성공!'); // fulfilled 상태로 전환
    } else {
      reject('작업 실패!'); // rejected 상태로 전환
    }
  }, 1000);
});

// Promise 사용
promise
  .then(result => console.log(result)) // 성공 시
  .catch(error => console.error(error)) // 실패 시
  .finally(() => console.log('작업 완료')); // 항상 실행<br>
```

<br>

#### 🔍 [ [매일메일 - 자바스크립트 Promise에 대해서 아는 대로 설명해주세요.](https://www.maeil-mail.kr/question/65) ]

<br>

---

## 📑 2025.12.01

### # React & Kakao map SDK를 활용한 위치기반 서비스 구현 & 최적화 과정 포스팅

#### \* FPS [ 15.3 ➔ 55.7 ] - 약 3.6배 향상( 효율 약 72.5% ) 경험
#### \* 반응 속도 효율 [ 65.53ms ➔ 17.96ms ] - 약 3.6배 향상( 효율 약 72.6% ) 경험
#### \* 생성 프레임 [ 474 ➔ 1671 ] - 약 3.5배 더 많은 프레임 생성( 효율 약 71.6% ) 경험

<br>

#### ✍🏻 [ [zeriong - [ React: Map Marker Optimization ] GPU 가속과 Batch 처리를 활용한 지도 성능 개선기](https://zeriong.tistory.com/93) ]
