## 📑 2026.01.19


### # Frontend - 가상스크롤 학습 내용 포스팅

#### ✍🏻 [ [zeriong - [ Virtual Scroll ] 데이터가 있는척, 사용자를 속여서 최적화하기](https://zeriong.tistory.com/99) ]

<br>

---

## 📑 2026.01.16

### # React2Shell 관련 내용과 사내 프론트엔드 5종 마이그레이션 과정 포스팅

#### ✍🏻 [ [zeriong - [ Next.js, React.js, Tanstack-Query, Orval, Biome.js ] 사내 프론트엔드 5종 마이그레이션 feat: React2Shell](https://zeriong.tistory.com/98) ]

<br>

---

## 📑 2026.01.15

### # Tanstack Query - gcTime & staleTime

`개요: gcTime과 staleTime에 대한 이해도가 옅어져서 복습`

#### gcTime
- 가비지컬렉션 수집 대상으로 지정되는 시간으로 **캐싱데이터를 가지고 있는 시간**이라고 이해할 수 있음

#### staleTime
- "해당 데이터가 신선하다"의 기준으로, **(재)사용기한** 같은 것

<br>

#### * 주요 특징 및 차이점

- **gcTime**
  - useQuery()를 사용한 컴포넌트가 **unmount되면 유지된다.**
  - 의미: "이 시간동안 **App을 감싼 Provider에서 캐시**를 가지고 있을게"
  - 시간이 끝나면 Provider에 저장된 쿼리 캐시데이터 삭제
- **staleTime**
  - useQuery()를 사용한 컴포넌트가 **unmount되면 초기화된다.**
  - 의미: "이 시간동안 **캐시가 남아있다면** refetch하지 않고 **캐시 사용할게**"
  - 시간이 끝나거나 unmount 시 refetch

#### * 장점
1. staleTime이 남아있다면 gcTime동안 refetch하지 않아 캐싱 데이터 기반 화면을 빠르게 제공(UX)
2. 이로 인한 통신 절차가 생략되어 비용 절감(최적화)

<br>

#### Q. staleTime은 unmount되면 stale(신선하지 않은)상태가 되어서 해당 컴포넌트에 다시 접근 시 refetch가 되는데 gcTime은 그럼 왜 필요할까?<br>
**`stale상태로 refetch를 한다고 해도 gcTime이 유효하다면 refetch가 일어나 데이터가 갱신되기 전까진 이전데이터를 보여주기 때문에 즉시 화면을 보여주게 되어 UX관점에서 더 나은 방향성이라고 볼 수 있다.( 기다리지 않아도 볼 수 있는 데이터가 이미 존재하기 때문 )`**


#### [🔍 [ ssund - TanStack Query의 gcTime과 staleTime 비교 ](https://ssund.tistory.com/181) ]

<br>

---

## 📑 2026.01.04

### # Web Server - NginX설정으로 SPA 작동 설정하기

`개요: 사내 서비스는 IaaS를 활용하기 때문에 빌드된 React파일을 배포할 때 NginX를 별도로 설정하고 다뤄야 하는데, 이 때 초기 설정해두었던 코드의 의미를 망각하여 SPA가 작동하도록 만드는 이유에 대해서 답변하지 못하는 불상사가 생겨 리마인드를 위해 작성`

```nginx
location / {
  # 경로와 index 설정
  root /www/service-app;
  index index.html index.htm;

  # fallback 처리
  try_files $uri /index.html;
}
```

위처럼 fallback 처리를 해주지 않는다면 실제 경로에 아무것도 존재하지 않기 때문에 404가 발생.<br>
`try_files $url`을 통해 fallback 처리를 해주면 추가 경로가 없는 경우 지속적으로 index.html을 바라보게 되고,<br>
JS를 실행을 통해 Client Routing이 가능하게 만들어줌


#### [🔍 [ 시소 - Nginx를 통한 SPA 앱 배포 시 주의 및 설정법 (404 핸들링, 정적 애셋 처리) ](https://velog.io/@seesaw/Nginx%EB%A5%BC-%ED%86%B5%ED%95%9C-SPA-%EC%95%B1-%EB%B0%B0%ED%8F%AC-%EC%8B%9C-%EC%A3%BC%EC%9D%98-%EB%B0%8F-%EC%84%A4%EC%A0%95%EB%B2%95-404-%ED%95%B8%EB%93%A4%EB%A7%81) ]

<br>

---

## 📑 2026.01.02

### # Javascript - Prototype vs Class

- 자바스크립트는 프로토타입 기반 객체지향 언어이며, 클래스는 이를 다루기 쉽게 만든 문법적 설탕(Syntactic Sugar)이다.
- 모든 객체는 부모 역할을 하는 프로토타입 객체와 연결(Chaining)되어 속성을 상속받는다.
- 클래스는 내부적으로 프로토타입을 사용하지만, 더 엄격한 규칙과 명확한 상속 구조를 제공한다.

#### * 주요 특징 및 차이점

- **프로토타입 (Prototype)**
  - 생성자 함수(function)와 .prototype 속성을 사용하여 상속 구현
  - 함수 호이스팅이 적용되어 선언 전에도 인스턴스 생성 가능
  - new 키워드 없이 호출 시 일반 함수로 동작하여 에러 발생 안 함 (위험요소)
- **클래스 (Class)**
  - class, extends, super 키워드를 사용하여 직관적인 상속 구조 제공
  - 호이스팅이 발생하지 않는 것처럼 동작(TDZ)하여 선언 후 사용 필수
  - 항상 strict mode로 실행되며, new 키워드 없이 호출 시 에러 발생

#### * 장점

- [Class] 코드 가독성이 높고 Java/C# 등 타 언어 개발자가 적응하기 쉬움
- [Class] 상속 구조(extends)가 명확하여 대규모 프로젝트 유지보수에 유리
- [Prototype] 런타임에 동적으로 메서드를 추가하거나 수정하는 유연함이 큼

#### * 단점 및 주의사항

- 클래스가 도입되었어도 자바스크립트의 근간인 프로토타입 체인을 이해하지 못하면 디버깅이 어려울 수 있음
- 클래스 메서드는 기본적으로 열거 불가능(non-enumerable)하게 설정됨
- 무분별한 상속 깊이는 프로토타입 체인 탐색 성능을 저하시킬 수 있음

#### * 적용 권장 사항

- **클래스 사용 권장**
  - 현대적인 프레임워크 환경이나 대규모 애플리케이션 개발 시
  - 명확한 객체 지향 설계를 통해 팀 협업 효율을 높여야 할 때
- **프로토타입 이해 필수**
  - 라이브러리 제작 등 메모리 효율 최적화가 극한으로 필요한 경우
  - 자바스크립트의 핵심 동작 원리를 파악하여 예기치 못한 에러를 방지할 때

#### * 결론

`Class는 Prototype의 기능을 안전하고 편리하게 포장한 도구이므로, 실무에서는 Class를 사용하되 그 뿌리인 Prototype의 원리를 정확히 이해하는 것이 중요하다.`
<br>

#### [🔍 [ WikiDocs - JavaScript Prototype과 Class ( 5. 클래스와 프로토타입의 차이점 ) ](https://wikidocs.net/251888) ]
