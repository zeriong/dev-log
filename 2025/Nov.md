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
