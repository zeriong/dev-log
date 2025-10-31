 ---

## 📑 2025.10.31 `( 🎃Happy Halloween )`

### # AWS 연쇄 폭발 사건 관련 유튜브 시청

#### `개요: 모각코 단톡방이 있었는데 모두들 AWS가 터져서 난리였다고 한다... 하지만 재직중인 회사(큐비트온)는 작은 회사라서 나스서버를 별도로 사무실에서 돌리고 있어 문제가 없었다! ㅎㅎ`

- EPIC GAME, EA서버 등 AWS를 주로 사용하는 많은 서비스가 AWS서버에 접근할 수 없는 문제가 생겼다...

### * DynamoDB 폭발의 이유 (DNS 이슈)
- **\*EC2 (서버형 컴퓨터 대여) -** **DynamoDB**를 이용해 대여 가능한 컴퓨터 목록을 관리했기 때문에 **하나의 트랜젝션이 과하게 지연되는 문제**로 지연된 데이터가 삭제되어 처리 불가 상태가 되면서 **새로운 컴퓨터에 접근 자체가 불가능**해져버렸다.

- **\*컴퓨터 렌탈 담당 서비스 -** DynamoDB 복구 후, **그동안 밀렸던 컴퓨터 렌탈 요청이 한꺼번에 쏟아져** 들어와서 렌탈 담당 서비스가 과부하로 폭발
<br> ***(3시간에 걸쳐 복구)***

- **\*EC2 네트워크 설정 -** 컴퓨터들의 네트워크 설정 정보가 **지연되어 처리가 늦어지면서 렌탈 컴퓨터들의 인터넷이 안되는 문제 발생**
<br> ***(3시간에 걸쳐 복구)***

- **\*NLB (트래픽 분산 서비스) -** **EC2가 불안정**해지자, EC2의 상태를 주기적으로 확인(헬스 체크)하는 **NLB 노드들도 불안정해져서 지연 시간이 발생**
<br> ***(3시간에 걸쳐 복구)***

- **\*Lambda (코드 실행 서비스) -** 코드 정보들을 ***DynamoDB에 저장***해 두었기 때문에 **똑같이 폭발**
<br> ***(3시간에 걸쳐 복구)***

- **\*SQS (메시지 대기열) -** Lambda 관련 **백로그가 과하게 많이 쌓여 처리에 시간이 오래 걸리는 문제 발생**
<br> ***(3시간에 걸쳐 복구)***

- **\*ECS/EKS (도커 컨테이너 서비스) -** **EC2를 기반**으로 하기 때문에 **컨테이너들이 시작이 안 되어서 웹 서버들이 마비되는 문제 발생**
<br> ***(3시간에 걸쳐 복구)***

- **\*AWS 로그/고객센터 -** 해당 기능 구현에도 **DynamoDB를 사용했기 때문에 접속이 안 되는 사태가 발생**

<br>

`이렇게 연쇄적인 의존성에 의해 처참한 사태가 발생하고야 말았다... 총 복구시간은 약 15시간`

<br>

#### 🔍 [ [애플코딩 - 어이없는 AWS 연쇄 폭발 사건](https://youtu.be/ZDhEQcL5bYg?si=Q6uQsJXEcXiGannA) ]

<br>
 
 ---

## 📑 2025.10.30

### # 리액트의 render phase와 commit phase

#### `개요: 매일메일이라는 서비스를 구독했는데 얼마전에 공부했던 react의 렌더링, fiber architecture의 연장선으로 보여 학습`

- render phase는 변경된 상태나 props에 따라 어떤 UI가 변경되어야 할지 결정하는 단계
  - DOM을 실제로 업데이트하지 않고 Virture DOM에서 변경사항을 순수하게 계산하는 과정
  - 성능에 영향을 주지 않도록 중단하거나 다시 실행이 가능한 상태<br>
    ➔ `React18부터 제공하는 Concurrent Mode로 비동기적 처리 가능`
- commit phase는 render phase에서 변경이 결정된 UI를 DOM에 반영하고 브라우저에 렌더링하는 단계
  - DOM업데이트 이후엔 useEffect같은 사이드이팩트를 발생시키는 훅을 실행
- 요악:<br>
  `render phase - Virture DOM을 통해 변경사항 계산`<br>
  `commit phase - 결정된 변경사항을 DOM에 업데이트 후 사이드 이팩트 훅을 실행`

### # render phase와 commit phase가 동기화될 때의 특징

- 단계적 진행
  - render phase가 끝난 후 즉시 commit phase가 실행되는 것이 아닌, 우선순위가 높은 작업을 먼저 처리 가능<br>
    `동기화가 필요한 작업을 효율적으로 관리하여 사용자 경험을 개선`
- 병목관리
  - render phase에서 모든 변경 사항이 fiber Tree에 준비된 상태로 commit phase로 넘어감<br>
    `render ➔ commit 단계적 일관성 유지 가능`<br>
    `UI의 변경이 정확하게 동기화 되고 불필요한 리렌더링 방지 가능`

#### # fiber tree
- React의 렌더링 엔진의 핵심 구조로 컴포넌트를 효율적으로 관리하기 위한 내부 자료 구조
- 어떤 컴포넌트를, 어떻게 업데이트지를 계산하기 위한 실행 단위의 트리 구조
- Fiber 노드 하나는 하나의 React 컴포넌트를 표현함
  - state, props, effect, 자식/부모요소 관계, 우선순위 등을 정보로 담고 있음<br>

```
Fiber Tree는 Current Tree와 Work-in-Progress-tree(WIP Tree)가 존재한다.

[ Current Tree ]: 현재 화면을 표현하고 있는 Fiber Tree
[ WIP Tree ]: 새로운 렌더링 결과를 계산하고 있는 Fiber Tree

1. render phase
  - state/props 변경을 감지
  - 새로운 Fiber Tree를 생성하며 diff를 계산

2. commit phase
  - 변경이 결정되면 WIP Tree를 Current Tree로 교체하 DOM 업데이트

3. 화면 갱신 완료
```

### * React는 fiber tree를 통해 작업을 쪼개고, 우선순위 기반으로 스케줄링이 가능해졌다.
`( Fiber Architecture의 존재 의의 )`

<br>

#### 🔍 [ [리액트의 render phase와 commit phase에 대해서 설명해주세요.](https://www.maeil-mail.kr/question/30) ]

<br>

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

