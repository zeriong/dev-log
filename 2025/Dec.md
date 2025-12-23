## 📑 2025.12.23

### # Next.js 16v 주요 변경점 포스팅

#### ✍🏻 [ [zeriong - [ Next.js ] Next.js 16v 주요 변경점](https://zeriong.tistory.com/95) ]

---

### # Frontend - 모듈 번들러 (Module Bundler)

- **여러 개의 모듈(파일)을 하나 또는 여러 개의 번들 파일로 묶는 도구**
- 현대 웹 개발에서 수많은 자바스크립트 파일과 다양한 자산을 효율적으로 관리하고 최적화
- **의존성 관리, 파일 수 감소, 최적화된 배포** 가능

#### \* 모듈 번들러의 개념

**과거 방식**

```html
<!-- 예전: 여러 script 태그로 파일 로딩 -->
<script src="utils.js"></script>
<script src="data.js"></script>
<script src="main.js"></script>
```

**현재 방식**

```javascript
// 모듈 시스템 사용
import { utils } from "./utils.js";
import { data } from "./data.js";

// 번들러가 이를 하나의 bundle.js로 통합
```

파일이 여러 개로 나뉘어 있는 경우, 모듈 번들러는 이를 분석하고 **하나의 번들 파일로 만들어줌**

#### \* 모듈 번들러가 필요한 이유

**모듈 번들러가 없던 시절의 문제**

1. **HTTP 요청 증가로 성능 저하**

   - 파일 수가 많으면 각각 별도 요청 필요
   - 네트워크 오버헤드 증가

2. **파일 간 의존성 관리 어려움**

   - 로딩 순서 중요 (의존성 있는 파일은 먼저 로드해야 함)
   - 수동으로 관리하기 어려움

3. **글로벌 스코프 오염 가능성**
   - 모든 변수가 전역에 노출
   - 네이밍 충돌 위험

#### \* 모듈 번들러의 주요 역할

1. **의존성 분석**

   - 어떤 파일이 어떤 모듈을 사용하는지 파악
   - 의존성 그래프 생성

2. **파일 결합**

   - 관련 모듈들을 하나로 묶어서 로딩 속도 개선
   - HTTP 요청 수 감소

3. **최적화**

   - 코드 압축 (minification)
   - 사용하지 않는 코드 제거 (tree shaking)
   - 코드 스플리팅

4. **비JS 자산 처리**
   - CSS, 이미지, 폰트 등도 import하여 번들링 가능
   - 모든 자산을 모듈로 관리

#### \* 대표적인 모듈 번들러

**1. Webpack**

- **가장 널리 사용되는 모듈 번들러**
- 복잡한 웹 애플리케이션에 적합
- Entry, Output, Loader, Plugin 등 다양한 설정 요소
- CSS, 이미지, 폰트 등 모든 웹 자산 번들링 가능

**장점**

- 높은 유연성과 다양한 설정
- 풍부한 플러그인 생태계
- 기업 환경에서 검증됨

**단점**

- 설정이 복잡하고 학습 곡선이 높음
- 빌드 속도가 상대적으로 느림

**적합한 경우**

- 복잡한 기업형 프로젝트
- 커스터마이징이 중요한 프로젝트

```javascript
// webpack.config.js 예시
module.exports = {
  entry: "./src/index.js",
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname, "dist"),
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"],
      },
    ],
  },
};
```

**2. Vite**

- **ESM(ES Module) 기반의 최신 번들러**
- Rollup 위에 구축
- 개발 서버 속도가 매우 빠름
- HMR(Hot Module Replacement) 기능 지원

**장점**

- 개발 환경에서 즉시 시작 (No Bundle 개념)
- 설정 파일이 직관적
- React/Vue 등과 연동이 간편
- 빌드 시 Rollup 사용으로 성능 우수

**단점**

- 레거시 브라우저 지원에 추가 설정 필요
- 생태계가 상대적으로 작음

**적합한 경우**

- 빠른 개발 환경이 필요한 경우
- 모던 프레임워크 사용 시

```javascript
// vite.config.js 예시
export default {
  server: {
    port: 3000,
  },
  build: {
    outDir: "dist",
  },
};
```

**3. Rollup**

- **라이브러리 번들링에 최적화**된 모듈 번들러
- Tree shaking 기능이 강력
- 결과물이 작고 최적화됨
- ES 모듈 중심으로 구성

**장점**

- 깨끗하고 작은 번들 생성
- 강력한 tree shaking
- 코드 스플리팅 지원
- 플러그인 생태계 우수

**단점**

- 일반 웹앱보다는 라이브러리에 특화
- 복잡한 애플리케이션에는 설정이 까다로움

**적합한 경우**

- JS 라이브러리나 NPM 패키지 제작
- 작은 번들 크기가 중요한 경우

```javascript
// rollup.config.js 예시
export default {
  input: "src/index.js",
  output: {
    file: "dist/bundle.js",
    format: "esm",
  },
};
```

**4. Parcel**

- **설정이 거의 필요 없는 (zero config) 번들러**
- 파일 타입을 자동 인식
- 필요한 로더와 플러그인을 내부적으로 설정

**장점**

- 빠르게 시작 가능
- 설정 파일 불필요
- 빌드 성능 우수
- 캐싱 및 병렬 처리로 성능 향상

**단점**

- 고도화된 설정에는 한계
- 대규모 프로젝트에서는 제약

**적합한 경우**

- 소규모 프로젝트
- 입문자나 빠른 프로토타이핑

```bash
# 설정 없이 바로 실행
parcel index.html
```

**5. Turbopack**

- **Vercel에서 개발한 Rust 기반 차세대 번들러**
- Webpack의 창시자가 개발에 참여한 Webpack 후속작
- Next.js 13부터 기본 개발 서버로 채택

**장점**

- 증분 빌드로 빠른 개발 환경
- 강력한 캐싱 시스템
- 병렬 처리로 성능 극대화
- Next.js와 완벽한 통합

**단점**

- 현재 베타 상태
- Next.js 외 생태계는 미흡
- 프로덕션 사용 시 신중히 고려 필요

**적합한 경우**

- Next.js 13+ 기반 대규모 프로젝트
- 최신 성능 최적화가 필요한 경우

```javascript
// next.config.js
module.exports = {
  experimental: {
    turbo: {
      // Turbopack 설정
    },
  },
};
```

#### \* 번들러 비교표

| 번들러        | 속도      | 설정 난이도 | 주요 용도       | 특징                       |
| ------------- | --------- | ----------- | --------------- | -------------------------- |
| **Webpack**   | 보통      | 높음        | 복잡한 웹앱     | 높은 유연성, 풍부한 생태계 |
| **Vite**      | 매우 빠름 | 낮음        | 모던 웹앱       | 즉시 시작, ESM 기반        |
| **Rollup**    | 빠름      | 보통        | 라이브러리      | 강력한 tree shaking        |
| **Parcel**    | 빠름      | 매우 낮음   | 소규모 프로젝트 | Zero config                |
| **Turbopack** | 매우 빠름 | 낮음        | Next.js         | Rust 기반, 증분 빌드       |

#### \* 번들러 선택 가이드

```
프로젝트 성격에 따른 추천:

- 복잡한 기업형 프로젝트
   → Webpack
   (높은 유연성, 복잡한 요구사항 대응)

- 빠른 개발 환경 + 모던 프레임워크
   → Vite
   (React, Vue, Svelte와 함께 빠른 개발)

- JS 라이브러리나 NPM 패키지 제작
   → Rollup
   (작은 번들, 강력한 tree shaking)

- 간단한 사이드 프로젝트 또는 입문
   → Parcel
   (설정 없이 바로 시작)

- Next.js 기반 대규모 프로젝트
   → Turbopack
   (빠른 HMR, 증분 빌드 - 베타 주의)
```

#### \* 주요 개념

**Tree Shaking**

```javascript
// utils.js
export function used() {
  /* ... */
}
export function unused() {
  /* ... */
}

// main.js
import { used } from "./utils";

// 번들 결과: unused는 제거됨 (tree shaking)
```

**Code Splitting**

```javascript
// 동적 import로 코드 분할
const module = await import("./heavy-module.js");

// 결과: heavy-module.js는 별도 청크로 분리
// 필요할 때만 로딩 (Lazy Loading)
```

**Hot Module Replacement (HMR)**

```javascript
// 개발 중 파일 수정 시
// 전체 페이지 새로고침 없이
// 변경된 모듈만 교체
// → 개발 속도 향상
```

#### \* 정리

- 모듈 번들러는 **웹 애플리케이션의 성능과 유지 보수성을 높이는 핵심 도구**
- 각 번들러는 **목적과 사용성에 따라 특화된 강점** 보유
- **프로젝트의 성격에 맞는 도구를 선택**하는 것이 중요
- 현대 웹 개발에서는 필수적인 도구

<br>

#### 🔍 [ [Dachae' DevLog - [모듈 번들러] 프론트엔드 필수 도구의 개념과 대표 번들러 비교](https://dachaes-devlogs.tistory.com/56) ]

<br>

---


## 📑 2025.12.18

### # React - Error Boundary를 효과적으로 사용하는 방법

- Error Boundary는 단순히 에러를 잡는 것을 넘어 **전략적으로 배치하고 활용**하는 것이 중요
- 적절한 에러 처리 전략으로 사용자 경험과 애플리케이션 안정성 향상

#### \* Error Boundary 배치 전략

**1. 계층적 Error Boundary 구조**

```javascript
// 전역 레벨 - 최상위 폴백
<GlobalErrorBoundary fallback={<ErrorPage />}>
  <App>
    {/* 페이지 레벨 - 페이지별 에러 처리 */}
    <PageErrorBoundary fallback={<PageError />}>
      <Dashboard>
        {/* 컴포넌트 레벨 - 세부 기능별 에러 처리 */}
        <WidgetErrorBoundary fallback={<WidgetError />}>
          <ChartWidget />
        </WidgetErrorBoundary>
      </Dashboard>
    </PageErrorBoundary>
  </App>
</GlobalErrorBoundary>
```

**계층별 역할**:

- **전역 레벨**: 예상치 못한 치명적 에러 처리, 전체 앱 보호
- **페이지 레벨**: 페이지 단위 에러 격리, 다른 페이지는 정상 동작
- **컴포넌트 레벨**: 세부 기능 에러 격리, 페이지의 나머지 부분은 정상 동작

**2. 기능별 Error Boundary 분리**

```javascript
// API 호출 관련 에러
<APIErrorBoundary
  fallback={<RetryableError onRetry={refetch} />}
>
  <UserList />
</APIErrorBoundary>

// 렌더링 에러
<RenderErrorBoundary
  fallback={<ComponentError />}
>
  <ComplexChart data={data} />
</RenderErrorBoundary>
```

#### \* 재사용 가능한 Error Boundary 구현

**1. Props로 커스터마이징 가능한 구조**

```javascript
class CustomErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      hasError: false,
      error: null,
      errorInfo: null,
    };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    this.setState({ error, errorInfo });

    // 에러 로깅 서비스 (Sentry, LogRocket 등)
    if (this.props.onError) {
      this.props.onError(error, errorInfo);
    }

    // 에러 리포팅
    if (this.props.logError) {
      console.error("Error Boundary caught:", error, errorInfo);
    }
  }

  handleReset = () => {
    this.setState({
      hasError: false,
      error: null,
      errorInfo: null,
    });

    if (this.props.onReset) {
      this.props.onReset();
    }
  };

  render() {
    if (this.state.hasError) {
      // 커스텀 Fallback 컴포넌트
      if (this.props.fallback) {
        return typeof this.props.fallback === "function"
          ? this.props.fallback({
              error: this.state.error,
              errorInfo: this.state.errorInfo,
              resetError: this.handleReset,
            })
          : this.props.fallback;
      }

      // 기본 Fallback UI
      return (
        <div>
          <h2>문제가 발생했습니다</h2>
          <button onClick={this.handleReset}>다시 시도</button>
        </div>
      );
    }

    return this.props.children;
  }
}
```

**2. 실무 활용 예시**

```javascript
// 재시도 가능한 에러 UI
<CustomErrorBoundary
  fallback={({ error, resetError }) => (
    <div className="error-container">
      <h3>데이터를 불러오지 못했습니다</h3>
      <p>{error.message}</p>
      <button onClick={resetError}>다시 시도</button>
    </div>
  )}
  onError={(error, errorInfo) => {
    // Sentry 같은 에러 트래킹 서비스로 전송
    Sentry.captureException(error, { extra: errorInfo });
  }}
  logError={process.env.NODE_ENV === "development"}
>
  <DataTable />
</CustomErrorBoundary>
```

#### \* Error Boundary와 함께 사용하는 패턴

**1. Suspense와 결합**

```javascript
<ErrorBoundary fallback={<ErrorUI />}>
  <Suspense fallback={<LoadingUI />}>
    <LazyComponent />
  </Suspense>
</ErrorBoundary>
```

**2. React Query와 결합**

```javascript
// React Query의 에러는 컴포넌트 내에서 처리
// 렌더링 에러는 Error Boundary가 처리
<ErrorBoundary fallback={<PageError />}>
  <QueryClientProvider client={queryClient}>
    <UserProfile />
  </QueryClientProvider>
</ErrorBoundary>
```

**3. 에러 복구 로직**

```javascript
function DataComponent() {
  const [key, setKey] = useState(0);

  return (
    <ErrorBoundary
      key={key} // key 변경으로 Error Boundary 리셋
      fallback={
        <div>
          <p>에러 발생</p>
          <button onClick={() => setKey((k) => k + 1)}>컴포넌트 재생성</button>
        </div>
      }
    >
      <DataDisplay />
    </ErrorBoundary>
  );
}
```

#### \* Error Boundary가 잡지 못하는 에러 처리

**1. 이벤트 핸들러 에러**

```javascript
function Button() {
  const handleClick = () => {
    try {
      // 에러 발생 가능한 로직
      riskyOperation();
    } catch (error) {
      // 이벤트 핸들러는 try-catch로 직접 처리
      console.error("Button error:", error);
      showErrorToast(error.message);
    }
  };

  return <button onClick={handleClick}>클릭</button>;
}
```

**2. 비동기 코드 에러**

```javascript
function AsyncComponent() {
  const [error, setError] = useState(null);

  useEffect(() => {
    fetchData().catch((err) => {
      // 비동기 에러는 상태로 관리
      setError(err);
    });
  }, []);

  if (error) {
    return <div>에러: {error.message}</div>;
  }

  return <div>데이터 표시</div>;
}
```

#### \* 모니터링 및 로깅 통합

```javascript
class MonitoredErrorBoundary extends React.Component {
  componentDidCatch(error, errorInfo) {
    // 에러 트래킹 서비스로 전송
    const errorReport = {
      error: error.toString(),
      errorInfo: errorInfo.componentStack,
      timestamp: new Date().toISOString(),
      userAgent: navigator.userAgent,
      url: window.location.href,
      userId: getCurrentUserId(), // 사용자 정보
    };

    // Sentry
    Sentry.captureException(error, {
      extra: errorReport,
    });

    // 자체 로깅 시스템
    logErrorToServer(errorReport);
  }

  render() {
    // ... 렌더링 로직
  }
}
```

#### \* 실무 Best Practices

1. **세분화된 Error Boundary 배치**

   - 전체 앱을 하나의 Error Boundary로 감싸지 말 것
   - 기능별, 페이지별로 적절히 분리

2. **의미 있는 에러 메시지**

   - 사용자에게는 친화적인 메시지
   - 개발자에게는 상세한 에러 정보

3. **복구 메커니즘 제공**

   - 재시도 버튼
   - 이전 페이지로 돌아가기
   - 홈으로 이동

4. **에러 모니터링**

   - 프로덕션 환경에서 발생하는 모든 에러 추적
   - 패턴 분석으로 근본 원인 파악

5. **개발 vs 프로덕션 환경 구분**
   - 개발: 상세한 에러 스택 표시
   - 프로덕션: 사용자 친화적 메시지

#### \* 정리

- Error Boundary는 **전략적으로 배치**하여 에러를 적절히 격리
- **재사용 가능한 구조**로 구현하여 유지보수성 향상
- **에러 모니터링**과 통합하여 프로덕션 이슈 추적
- Error Boundary가 **잡지 못하는 에러**는 별도 처리 필요
- **사용자 경험**을 최우선으로 고려한 에러 처리

<br>

#### 🔍 [ [카카오 엔터테인먼트 - React의 Error Boundary를 이용하여 효과적으로 에러 처리하기](https://tech.kakaoent.com/front-end/2022/221110-error-boundary/) ]

<br>

---

## 📑 2025.12.11

### # 테오콘 2025 - 12/7 Track A 참여 후기

#### ✍🏻 [ [zeriong - [ TEOConf 2025 ] 테오콘 2025 - 12/7 Track A 참여 후기](https://zeriong.tistory.com/94) ]

---

## 📑 2025.12.08

### # Web - CORS (Cross-Origin Resource Sharing)

- **서로 다른 출처(origin)에서 제공되는 리소스에 접근할 수 있도록 허용하는 정책**
- 동일 출처 정책(Same-Origin Policy)의 제한을 안전하게 우회하는 메커니즘

#### * 동일 출처 정책 (Same-Origin Policy)

- 브라우저에 보안상의 이유로 기본 적용되는 정책
- 같은 출처에서 제공되지 않는 리소스는 브라우저가 차단
- 다른 출처의 서버에 요청 시 응답에 접근 불가
- 보안을 강화하지만 **합법적인 요청까지 차단될 수 있음**

#### * 출처(Origin)란?

`https://example.com:443/path?query=1`
- 프로토콜(Protocol): https
- 도메인(Domain): example.com
- 포트(Port): 443

위 3가지가 모두 동일해야 **"같은 출처"**


#### * CORS가 필요한 이유

- SOP로 인해 정상적인 API 통신도 차단됨
- 프론트엔드(예: localhost:3000)와 백엔드(예: api.example.com)가 다른 도메인일 경우
- 외부 API를 사용하는 경우
- 마이크로서비스 아키텍처에서 서비스 간 통신

#### * CORS 적용 방법 (서버 측)

**1. Access-Control-Allow-Origin 헤더 설정**

```javascript
// Express.js 예시
app.use((req, res, next) => {
  // 모든 출처 허용 (보안상 권장하지 않음)
  res.header('Access-Control-Allow-Origin', '*');
  
  // 또는 특정 출처만 허용
  res.header('Access-Control-Allow-Origin', 'https://example.com');
  
  next();
});
```

**2. Access-Control-Allow-Methods 헤더**

```javascript
// 허용할 HTTP 메서드 지정
res.header('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE, OPTIONS');**3. Access-Control-Allow-Headers 헤더**

// 허용할 헤더 지정
res.header('Access-Control-Allow-Headers', 'Content-Type, Authorization');
```

#### * 실무 적용 예시

**서버 측 설정 (Node.js/Express)**

```javascript
const cors = require('cors');

// 옵션 1: 모든 출처 허용 (개발 환경)
app.use(cors());

// 옵션 2: 특정 출처만 허용 (프로덕션 환경)
app.use(cors({
  origin: ['https://example.com', 'https://www.example.com'],
  methods: ['GET', 'POST', 'PUT', 'DELETE'],
  credentials: true, // 쿠키 포함 허용
  allowedHeaders: ['Content-Type', 'Authorization']
}));
```

#### * CORS와 보안: CSRF 공격

**동일 출처 정책이 막고자 하는 공격**

- 주로 **CSRF(Cross-Site Request Forgery)** 공격의 위력을 낮춤
- CSRF 공격의 원리:
  1. 피해자가 악성 웹사이트 방문
  2. 악성 사이트가 피해자의 브라우저를 통해 다른 사이트에 요청
  3. 피해자의 인증 정보(쿠키 등)가 자동으로 포함됨
  4. 의도치 않은 요청이 실행됨 (송금, 정보 변경 등)

**SOP의 역할**
- 악성 사이트에서 다른 출처의 서버로 요청을 보내거나 응답에 접근하는 것을 차단
- CSRF 공격의 효과를 줄여줌

#### * CORS 요청 흐름

**1. 단순 요청 (Simple Request)** <br>
2. Access-Control-Allow-Methods 헤더

```javascript
// 허용할 HTTP 메서드 지정
res.header('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE, OPTIONS');
```

3. Access-Control-Allow-Headers 헤더

```javascript
// 허용할 헤더 지정
res.header('Access-Control-Allow-Headers', 'Content-Type, Authorization');ol-Request-Method: POST
Access-Control-Request-Headers: Content-Type

// 서버 응답
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://example.com
Access-Control-Allow-Methods: POST, GET
Access-Control-Allow-Headers: Content-Type
```

#### * 프론트엔드 개발자의 역할
- CORS는 **서버에서 설정**하는 것
- 프론트엔드 개발자는:
  1. 백엔드 개발자에게 클라이언트 도메인 허용 요청
  2. 개발 환경에서는 프록시 설정으로 우회 가능

**개발 환경 프록시 설정 예시**

```javascript
// package.json (Create React App)
{
  "proxy": "http://localhost:4000"
}

// vite.config.js (Vite)
export default {
  server: {
    proxy: {
      '/api': 'http://localhost:4000'
    }
  }
}
```

#### * 정리

- **CORS**: 다른 출처 간 리소스 공유를 안전하게 허용하는 메커니즘
- **SOP**: 보안을 위한 브라우저의 기본 정책
- **서버 측 설정 필요**: Access-Control-* 헤더로 허용 범위 지정
- **보안**: CSRF 등의 공격을 방어하면서도 필요한 통신은 허용

<br>

#### 🔍 [ [매일메일 - CORS(Cross-Origin Resource Sharing)는 무엇이며 왜 필요한가요?](https://www.maeil-mail.kr/question/78) ]

<br>

---

## 📑 2025.12.07

### # React - useEffect 호출 시점

- React의 `useEffect`는 컴포넌트의 특정 시점에 자동으로 호출되는 훅
- **마운트**, **업데이트**, **언마운트** 시점에 호출됨

#### * 1. 마운트 시점 (첫 렌더링 후)

- **컴포넌트가 처음 렌더링되고 나서 호출**
- 수행 가능한 작업:
  - 데이터 초기화
  - 외부 API 호출
  - 구독(subscription) 설정
  - 이벤트 리스너 등록
- 컴포넌트가 처음 마운트될 때 필요한 초기 작업 실행

```javascript
useEffect(() => {
  // 컴포넌트 마운트 시 한 번만 실행
  console.log('컴포넌트가 마운트되었습니다');
  
  // API 호출 예시
  fetchData();
}, []); // 빈 의존성 배열 = 마운트 시에만 실행#### * 2. 업데이트 시점 (의존성 배열 값 변경)
```

- **의존성 배열에 지정된 값이 변경될 때마다** 다시 호출
- 실행 순서:
  1. 이전 props/state로 **cleanup 함수 먼저 호출**
  2. 업데이트된 props/state로 **본문 실행**

```javascript
useEffect(() => {
  console.log(`count가 ${count}로 변경되었습니다`);
  
  return () => {
    // cleanup 함수 (다음 effect 실행 전 또는 언마운트 시)
    console.log('cleanup 실행');
  };
}, [count]); // count 값이 변경될 때마다 실행#### * 3. 매 렌더링마다 호출 (의존성 배열 생략)
```

- **의존성 배열을 넘기지 않으면** 매 렌더링마다 호출됨
- 성능 이슈 발생 가능성 있어 주의 필요

```javascript
useEffect(() => {
  // 모든 렌더링마다 실행
  console.log('컴포넌트가 렌더링되었습니다');
}); // 의존성 배열 없음
```

#### * 4. 언마운트 시점 (컴포넌트 제거)

- **컴포넌트가 언마운트될 때 cleanup 함수 호출**
- cleanup 함수는 `useEffect`의 return 값으로 지정
- 수행 가능한 작업:
  - 이벤트 리스너 제거
  - 타이머 해제
  - 구독 취소
  - WebSocket 연결 종료
- useEffect를 통해 발생한 부수효과(side effect) 정리

```javascript
useEffect(() => {
  // 이벤트 리스너 등록
  const handleScroll = () => {
    console.log('스크롤 이벤트');
  };
  
  window.addEventListener('scroll', handleScroll);
  
  return () => {
    // cleanup: 이벤트 리스너 제거
    window.removeEventListener('scroll', handleScroll);
  };
}, []);
```

#### * 의존성 배열에 따른 동작

| 의존성 배열 | 실행 시점 | 사용 사례 |
|------------|----------|----------|
| `[]` (빈 배열) | 마운트 시 1회만 | 초기 데이터 로딩, 구독 설정 |
| `[value]` | 마운트 + value 변경 시 | 특정 값에 반응하는 동작 |
| 생략 | 매 렌더링마다 | 특수한 경우에만 사용 (성능 주의) |

<br>

#### 🔍 [ [매일메일 - useEffect가 호출되는 시점에 대해 설명해 주세요.](https://www.maeil-mail.kr/question/64) ]

<br>

---

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
