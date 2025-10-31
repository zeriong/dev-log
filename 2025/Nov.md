## 📑 2025.11.01

### # 프론트 최적화 기법 관련 유튜브 시청

1. 전송 최적화 ( 로딩의 80%는 네트워크에 있다. )

   - http/3 (핸드쉐이크가 없고 병렬 요청이 가능한 장점이 있다)
   - gzip 말고도 Brotli/Zstd 등의 향상된 압축 기법이 있다.
   - 이미지, 폰트, js, css 캐시설정 - cache-control / ETag
   - CDN 활용

    <br>

2. 이미지 최적화

   - 주로 LCP(Largest Contentful Paint)를 중요한 지표로 생각한다.
   - avif 또는 webp를 사용
   - 사이즈별 리사이즈와 lazy loading 활용용

   <br>

3. 폰트 최적화

   - woff2 적극활용
   - fout활용 + size adust로 자연스럽게 조정

   <br>

4. 더 나은 번들

   - lazy 임포트 활용
   - tree shacking 적용 되도록 설정
   - rollup-plugin-visualizer과 같은 라이브러리로 번들 사이즈를 시각화하여 확인

   <br>

5. 데이터 페칭 최적화

   - 가능한 병렬처리(Promise.all)를 활용하고 SWR, React Query 등 캐싱기능을 활용하면 좋다.

    <br>

6. 렌더링 최적화

   - 작업이 많은 경우 작업을 나누어 지연처리
     `idle callback( requestidlecallback ) 사용해서 처리할 수 있다.`
   - reflow - repaint 줄이기
     `composite 변경 활용`
   - 어쩔 수 없는 경우에 읽기-쓰기를 반복하는 것을 지양해야 함
     `반복문 처리가 필요한 경우 변수에 데이터를 담고 마지막에 적용하는 등`

   <br>

7. 프리페칭( 사전로딩 )

   - IntersectionObserver를 활용해서 특정 요소가 보일 때 head에 미리 link를 넣기도 가능
   - api를 담아두기
     `reactQuery - prefetchQuery 미리 api를 담아둘 수 있음`

   <br>

- **\*성능 모니터링 자동화**

  - LCP(가장 큰 리소스를 가져오는 시간) / INP(반응 시간) / CLS(예상치 못한 움직임)
  - web vitals - github에 있고 해당 라이브러리에서 제공하는 api를 활용하면 ga4와 연계해서 자동화 할 수도 있음

    <br>

#### 🔍 [ [프롱트 - 2025 웹 성능 최적화 기법 7가지](https://youtu.be/Ngqxa0B6DqE?si=BXXnZsI7lDbOAMUe) ]

<br>
