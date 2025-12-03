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
