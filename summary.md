## React Native 최적화 가이드 요약 (내부 참고용)

이 문서는 "React Native Optimization: The Ultimate Guide"의 주요 내용을 요약하여, 향후 React Native 애플리케이션 성능 최적화 관련 작업 시 참고 자료로 활용하기 위해 작성되었습니다.

**핵심 목표:**

*   **TTI (Time to Interactive) 개선:** 앱 초기 로딩 속도 단축.
*   **FPS (Frames Per Second) 향상:** UI 인터랙션 및 애니메이션 부드럽게 만들기 (목표: 60 FPS 이상).

**주요 최적화 영역:**

1.  **JavaScript 및 React (주로 FPS에 영향)**
2.  **Native (iOS/Android/C++) (FPS 및 TTI에 영향)**
3.  **Bundling (주로 TTI에 영향)**

---

### 1. JavaScript 및 React 최적화

*   **성능 측정 및 프로파일링:**
    *   **도구:** `React Native DevTools` (React Profiler, JS Profiler, Memory), `React Perf Monitor` (실시간 FPS), `Flashlight` (Android 상세 성능 리포트).
    *   **목표:** 불필요한 리렌더링, 긴 JS 실행 시간, JS 메모리 누수 식별.
*   **리렌더링 최적화:**
    *   **기본:** `React.memo`, `useMemo`, `useCallback`을 사용한 수동 메모이제이션.
    *   **자동:** `React Compiler` (실험적) 도입 고려 (Babel 플러그인, ESLint 규칙).
    *   **상태 관리:** `Atomic State Management` (Jotai, Zustand 등) 사용으로 전역 상태 변경 시 리렌더링 범위 최소화.
*   **UI 반응성 향상 (Concurrent React - New Architecture 필요):**
    *   `useDeferredValue`: 덜 중요한 값의 업데이트를 지연시켜 UI 반응성 유지 (예: 검색 결과).
    *   `useTransition`: 덜 중요한 상태 업데이트(렌더링 전체)를 지연시켜 UI 블로킹 방지.
    *   `Automatic Batching`: 여러 상태 업데이트를 단일 리렌더링으로 묶어 성능 개선 (React 18+).
*   **컴포넌트 최적화:**
    *   **목록:** 매우 큰 리스트에는 `ScrollView` + `map` 대신 `FlatList` 또는 `FlashList` (Shopify) 사용. (`getItemLayout`, `estimatedItemSize` 활용).
    *   **입력:** 레거시 아키텍처에서 `TextInput` 동기화 문제 발생 시 `Uncontrolled Components` 패턴 고려.
*   **애니메이션:**
    *   `React Native Reanimated` 사용 권장 (Worklet으로 UI 스레드에서 실행).
    *   `InteractionManager`: 인터랙션/애니메이션 종료 후 무거운 작업 예약.
    *   `runOnJS` / `runOnUI`: 스레드 간 함수 호출 필요 시 사용.
*   **메모리 누수:**
    *   원인: 미정리 리스너/타이머, 큰 객체를 참조하는 클로저 등.
    *   탐지: `React Native DevTools`의 Memory 탭 (Allocation instrumentation on timeline).

---

### 2. Native 최적화

*   **플랫폼 차이 이해:**
    *   iOS (Xcode, Swift/Objective-C, CocoaPods), Android (Android Studio, Kotlin/Java, Gradle) 환경 및 빌드 시스템 숙지.
*   **성능 측정 및 프로파일링:**
    *   **도구:** `Xcode Instruments` (Time Profiler, Leaks), `Android Studio Profiler` (CPU, Memory).
    *   **목표:** 네이티브 코드 병목 현상, 네이티브 메모리 누수 식별.
*   **New Architecture 이해:**
    *   **스레딩 모델:** Main/UI 스레드, JS 스레드, Native Modules 스레드 역할 이해.
    *   **Turbo Modules:** 기본적으로 지연 로딩. 동기 메소드는 JS 스레드, 비동기 메소드는 Native Modules 스레드에서 실행 (필요시 추가 스레드 생성 가능). `invalidate` 메소드 스레딩 확인.
    *   **Fabric (Renderer):** 대부분의 View 관련 작업 (생성, 업데이트)은 Main/UI 스레드에서 발생. `Yoga` 레이아웃 계산은 JS 스레드에서 수행.
*   **View 최적화:**
    *   `View Flattening`: React Native가 자동으로 레이아웃 전용 뷰를 제거하여 계층 구조 단순화. 네이티브 컴포넌트 자식 구조에 영향 줄 수 있으므로 `collapsable={false}`로 제어 가능.
    *   **디버깅:** Xcode의 `Debug View Hierarchy`, Android Studio의 `Layout Inspector` 사용.
*   **네이티브 모듈 개발:**
    *   `React Native Builder Bob`으로 라이브러리 보일러플레이트 생성.
    *   Modern 언어 사용: Kotlin (Android), Swift (iOS, Objective-C++ 브릿징 필요).
    *   백그라운드 스레드 활용: `DispatchQueue` (iOS), `Coroutines` (Android)로 무거운 작업 오프로드.
    *   C++ 활용: 플랫폼 독립적 로직이나 고성능 필요시 사용 (JNI, Objective-C++ 오버헤드 고려).
*   **메모리 관리:**
    *   **개념:** 수동 관리 (C++), 자동 관리 - 참조 카운팅 (Swift/Objective-C), 가비지 컬렉션 (Kotlin/Java/JS).
    *   **C++:** 스마트 포인터 (`unique_ptr`, `shared_ptr`, `weak_ptr`) 사용 권장.
    *   **Swift/ObjC:** ARC 작동 방식 이해, `weak` 참조로 순환 참조 방지.
    *   **Kotlin/Java:** GC 작동 방식 이해, `WeakReference`, `WeakHashMap` 활용, 리소스 (리스너 등) 해제 철저.
    *   **누수 탐지:** Xcode `Leaks` Instrument, Android Studio `Memory Profiler` 활용.

---

### 3. Bundling 최적화

*   **JS 번들:**
    *   **Hermes:** 기본 엔진. Bytecode (HBC) 사용. 파싱/로딩 시간을 빌드 시점으로 옮겨 TTI 개선. 기존 웹 방식 코드 스플리팅 효과 감소.
    *   **분석 도구:** `source-map-explorer`, `Expo Atlas`, `Re.Pack` + (`webpack-bundle-analyzer`, `bundle-stats`, `Statoscope`, `Rsdoctor`).
    *   **목표:** 불필요한 라이브러리/코드 식별 및 제거, 초기 실행 코드 최소화.
*   **앱 번들 (APK/AAB/IPA):**
    *   **분석 도구:** `Ruler` (Gradle 플러그인), `App Store Connect`, `Xcode App Size Report`, `Emerge Tools`.
    *   **목표:** 다운로드 및 설치 크기 최소화.
*   **코드 제거 및 축소:**
    *   `Tree Shaking`: 미사용 코드 제거. `Re.Pack` (Webpack/Rspack)에서 효과적. Expo SDK 52+ 실험적 지원. `metro-serializer-esbuild`로 Metro에서 유사 효과 가능.
    *   `R8` (Android): `minifyEnabled true`로 코드 축소, 최적화, 난독화 활성화. `proguard-rules.pro`로 규칙 관리. `shrinkResources true`로 리소스 추가 축소.
    *   **Barrel Exports 피하기:** 필요한 모듈만 직접 임포트하거나, Tree Shaking이 가능한 환경 구성.
*   **에셋 최적화:**
    *   `Native Assets Folder`: 이미지 등에 `@2x`, `@3x` 접미사 사용. Android는 자동 처리. iOS는 `RNAssets.xcassets` 설정 필요 (Build Phases 수정). 플랫폼의 App Thinning/Bundle 기능으로 기기별 최적 에셋 전달.
*   **기타:**
    *   **JS 번들 압축 비활성화 (Android):** `app/build.gradle`에 `noCompress += ["bundle"]` 추가. Hermes의 mmap 성능 극대화 (TTI 개선), 설치 크기는 약간 증가할 수 있음.
    *   **라이브러리 선택:** 웹 라이브러리보다 React Native 전용/네이티브 구현 라이브러리 선호 (예: `react-native-screens`, `react-native-quick-crypto`).

---

이 요약본을 기반으로 React Native 앱의 성능을 진단하고 개선하는 데 필요한 정보와 기법을 적용할 것입니다.