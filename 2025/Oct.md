---

## 📑 2025.10.29

### # React Compiler

#### `개요: 재직중인 큐비트온(주) 회사 in-house 서비스 포탈페이지를 next14 -> 16, react18 -> 19, webpack -> turbopack, npm -> pnpm으로 마이그레이션/컨버팅 하면서 react19의 최적화 기술인 react-compiler에 대해 알아보게 되었다.`

- 오토 메모이제이션을 통해 리액트 앱을 빌드 시점에서 최적화하는 도구이다.
- 리액트측에서는 "10년에 걸친 대규모 엔지니어링의 노력의 결실고, 새로운 10년의 시작이다" 라고 당차게 표현함<br>
  ( DX의 혁신적인 개선을 말하고 싶었던 것 같음 )
- 리액트 측에서 점진적인 react-compiler를 적용시킬 수 있는 가이드를 제안해주고 있다.
- react dev tools에서 확인해봤을 때 Memo라는 스티커가 붙어있으면 자동으로 최적화중이라는 의미
- 그럼 useMemo, useCallback, React.memo는 어떻게 되나?<br>
  - 기본적으로 react-compiler가 더 정확하다고 소개하고 있고 정밀제어가 필요한 경우 활용해도 좋다.
  - 기존에 적용되어있던 메모이제이션 훅들은 충분히 테스트 후 제거하거나 그대로 두라고 명시하고 있다.
- 앞으로 새로운 리액트 앱은 반드시 react-compiler를 사용해야 한다고 권고하고 있다.
- 리액트 규칙에 위반된 경우 react-compiler가 정상적용이 되지 않을 수 있기 때문에 ESLint의 규칙을 준수하는 것 또한 핵심 요소 중 하나라고 언급함.


 #### 🔍 [ [수코딩 - React Compiler 1.0 완벽 정리｜드디어 베타 끝!](https://www.youtube.com/watch?v=4WyLSzwRMGg) ]

 <br>

 ---

## 📑 2025.10.28

### # React - Fiber Architecture

#### `개요: 면접질문으로 등장했는데 답변하지 못하였고 너무 궁금해서 공부하게 되었다.`

- React Fiber는 React core 알고리즘의 구현체이다.
- Reconciliation이란 React에서 어떤 부분들이 변해야할지 서로 다른 두 개의 트리를 비교하는데 사용하는 알고리즘이며 Virture DOM 뒤에서 실행된다.
- 리스트는 key를 이용하여 비교되며 key는 stable하고 예측 가능하며, 동일한 리스트 내에서 unique해야 한다. (children)
- fiber는 스케줄링에 강점을 갖도록 하는 것으로 아래 조건을 만족해야 한다.
     - 작업을 멈추고 나중에 다시 돌아오기
     - 다른 유형의 일에 우선권 부여하기
     - 이전에 완성한 작업을 재사용하기
     - 더 이상 필요하지 않은 작업을 중단하기
- React가 변경을 감지하는 것은key, tag의 type, props가 있다.
- 실제 DOM 변경은 commit 단계에서만 일어나며, Fiber는 변경 작업(effect list)을 모아 커밋 시 적용
- fiber는 각 노드를 **작은 작업 단위(unit of work, fiber)** 로 만들어 중단/재개, 우선순위 스케줄링, 시간 분할이 가능하게 함.
- **실제 DOM 변경은 commit 단계에서만 이루어지고, render(reconciliation) 단계는 비교와 작업 생성만 수행.**
- setState는 update 트리거다.

 #### 🔍 [ [acdlite - React Fiber Architecture](https://github.com/acdlite/react-fiber-architecture) ]

 <br>

