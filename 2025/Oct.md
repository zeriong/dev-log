## 📑 2025.10.28

- React - Fiber Architecture

<br>

개요: 면접질문으로 등장했는데 답변하지 못하였고 너무 궁금해서 공부하게 되었다.
1. React Fiber는 React core 알고리즘의 구현체이다.
2. Reconciliation이란 React에서 어떤 부분들이 변해야할지 서로 다른 두 개의 트리를 비교하는데 사용하는 알고리즘이며 Virture DOM 뒤에서 실행된다.
3. 리스트는 key를 이용하여 비교되며 key는 stable하고 예측 가능하며, 동일한 리스트 내에서 unique해야 한다. (children)
4. fiber는 스케줄링에 강점을 갖도록 하는 것으로 아래 조건을 만족해야 한다.
     - 작업을 멈추고 나중에 다시 돌아오기
     - 다른 유형의 일에 우선권 부여하기
     - 이전에 완성한 작업을 재사용하기
     - 더 이상 필요하지 않은 작업을 중단하기
5. React가 변경을 감지하는 것은key, tag의 type, props가 있다.
6. 실제 DOM 변경은 commit 단계에서만 일어나며, Fiber는 변경 작업(effect list)을 모아 커밋 시 적용
7. fiber는 각 노드를 **작은 작업 단위(unit of work, fiber)** 로 만들어 중단/재개, 우선순위 스케줄링, 시간 분할이 가능하게 함.
8. **실제 DOM 변경은 commit 단계에서만 이루어지고, render(reconciliation) 단계는 비교와 작업 생성만 수행.**
9. setState는 update 트리거다.

 #### 🔍 [ [acdlite - React Fiber Architecture](https://github.com/acdlite/react-fiber-architecture) ]

<br>

---
