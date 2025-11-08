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
