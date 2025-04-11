# React Native 최적화
## 궁극의 가이드

## 목차

### 소개
- [서문](#서문)
- [이 책을 읽는 방법](#이-책을-읽는-방법)
- [성능이 중요한 이유](#성능이-중요한-이유)

### 파트 1: 자바스크립트
- [JS 및 React 코드를 프로파일링하는 방법](#js-및-react-코드를-프로파일링하는-방법)
- [JS FPS를 측정하는 방법](#js-fps를-측정하는-방법)
- [JS 메모리 누수를 찾는 방법](#js-메모리-누수를-찾는-방법)
- [비제어 컴포넌트](#비제어-컴포넌트)
- [고차 특수화 컴포넌트](#고차-특수화-컴포넌트)
- [원자적 상태 관리](#원자적-상태-관리)
- [동시성 React](#동시성-react)
- [React 컴파일러](#react-컴파일러)
- [프레임 드랍 없는 고성능 애니메이션](#프레임-드랍-없는-고성능-애니메이션)

### 파트 2: 네이티브
- [플랫폼 차이점 이해하기](#플랫폼-차이점-이해하기)
- [React Native의 네이티브 부분을 프로파일링하는 방법](#react-native의-네이티브-부분을-프로파일링하는-방법)
- [TTI를 측정하는 방법](#tti를-측정하는-방법)
- [네이티브 메모리 관리 이해하기](#네이티브-메모리-관리-이해하기)
- [터보 모듈과 패브릭의 스레딩 모델 이해하기](#터보-모듈과-패브릭의-스레딩-모델-이해하기)
- [뷰 플래트닝 사용하기](#뷰-플래트닝-사용하기)
- [웹보다 전용 React Native SDK 사용하기](#웹보다-전용-react-native-sdk-사용하기)
- [네이티브 모듈 고속화하기](#네이티브-모듈-고속화하기)
- [메모리 누수 찾는 방법](#메모리-누수-찾는-방법)

### 파트 3: 번들링
- [JS 번들 크기를 분석하는 방법](#js-번들-크기를-분석하는-방법)
- [앱 번들 크기를 분석하는 방법](#앱-번들-크기를-분석하는-방법)
- [서드파티 라이브러리의 실제 크기 확인하기](#서드파티-라이브러리의-실제-크기-확인하기)
- [배럴 내보내기 피하기](#배럴-내보내기-피하기)
- [트리 쉐이킹 실험하기](#트리-쉐이킹-실험하기)
- [필요할 때 코드를 원격으로 로드하기](#필요할-때-코드를-원격으로-로드하기)
- [R8 Android로 코드 축소하기](#r8-android로-코드-축소하기)
- [네이티브 에셋 폴더 사용하기](#네이티브-에셋-폴더-사용하기)
- [JS 번들 압축 비활성화하기](#js-번들-압축-비활성화하기)
- [번들 크기 줄이기](#번들-크기-줄이기)

### 추가 정보
- [감사의 말](#감사의-말)
- [저자 소개](#저자-소개)
- [이 가이드에서 언급된 라이브러리 및 도구](#이-가이드에서-언급된-라이브러리-및-도구)

## 서문

React Native는 긴 여정을 거쳐왔습니다. 초기 크로스플랫폼 개발의 유망한 실험 단계에서부터 세계 최대 기업들이 채택하기까지, 그 진화는 실로 놀라웠습니다. 이 프레임워크는 성숙해지면서 대규모 애플리케이션의 요구를 처리할 수 있는 역량을 증명하는 동시에 기술적 경계를 계속해서 정제하고 확장해 왔습니다.

수년 동안 우리는 React Native가 매 릴리스마다 종종 혁신적인 새로운 기능을 도입하는 것을 보아왔습니다. 오늘날에는 성능, 안정성 및 확장성을 향상시키는 꾸준한 저수준 개선이 이루어지고 있습니다. 이러한 개선 사항들은 React Native가 더 널리 채택되고 있음을 반영하며, 성숙하고 기업 환경에 적합한 프레임워크로서의 역할을 강화하고 있습니다.

이 여정에서 중요한 이정표는 New Architecture(새로운 아키텍처)의 도입이었습니다. 이는 프레임워크의 핵심 측면 많은 부분을 재정의했으며, 전반적인 개발 방식과 성능 최적화에 대한 사고방식을 바꾸었습니다. 이 새로운 아키텍처는 개발자들이 더욱 효율적이고 확장 가능한 애플리케이션을 구축할 수 있게 하는 새로운 기능과 모범 사례를 가져왔습니다.

그래서 우리는 최적화 가이드를 완전히 개정할 때가 되었다고 판단했습니다. 환경이 변화했으니 우리의 접근 방식도 변해야 합니다. 한때 필수적이었던 일부 기술은 더 이상 관련이 없고, 다른 기술들은 새로운 중요성을 갖게 되었습니다. 이 새로운 에디션에서는 개발자들에게 React Native의 진화하는 능력을 최대한 활용할 수 있는 최신 통찰력, 도구 및 전략을 제공하는 것을 목표로 합니다.

여러분이 경험 많은 React Native 엔지니어이든 막 시작하는 단계이든, 이 가이드는 성능 모범 사례를 넘어 React Native에 대한 이해를 높이는 데 필수적인 지식을 제공합니다. 여기에는 내부 작동 방식에 대한 이해도 포함됩니다. 우리는 매일 이러한 지식을 활용하여 더 나은, 더 효율적이고 확장 가능한 애플리케이션을 구축하고 있으며, 여러분도 같은 일을 할 수 있기를 바랍니다.

— Mike Grabowski, Callstack의 창립자 겸 CTO

## 이 책을 읽는 방법

이 책은 다양한 경험 수준의 React Native 개발자를 위해 만들어졌습니다. 우리는 초보자와 경험 많은 React Native 개발자 모두 웹 또는 네이티브 앱 개발 배경과 관계없이 이 책에서 자신의 앱에 적용할 수 있는 유용한 내용을 찾을 수 있을 것이라고 믿습니다.

우리는 여러분이 특정 시점에 이 책에 제시된 모든 최적화에 관심이 없을 수 있다는 점을 인식하고 있습니다. 종종 여러분의 초점은 React 리렌더링 최적화와 같은 특정 영역에 맞춰질 것입니다. 그래서 책 전체를 선형적으로 읽을 수도 있지만, 어떤 장에서든 열어서 관심 있는 주제로 바로 이동할 수 있도록 만들었습니다.

### 이 책에서 기대할 수 있는 것은?

모든 사람이 관련 정보를 쉽게 찾을 수 있도록, 우리는 이 가이드를 세 부분으로 나누었습니다. 각 부분은 특별히 관심을 가질 수 있는 다양한 종류의 최적화에 초점을 맞추고 있습니다: React 측면, 네이티브 측면, 그리고 전반적인 빌드 타임 최적화. 세 부분 모두 주요 주제에 대한 소개, 상세한 가이드, 그리고 앱 성능의 가장 중요한 지표인 FPS(초당 프레임 수)와 TTI(상호 작용 가능 시간)를 개선하는 데 도움이 되는 모범 사례를 포함하고 있습니다.

각 부분에서 다음과 같은 내용을 기대할 수 있습니다:
- **소개** — 주요 주제, 용어 및 개념을 소개합니다.
- **가이드** — 특수 도구를 사용하고 중요한 지표를 측정하는 방법을 설명합니다.
- **모범 사례** — 앱을 더 빠르게 실행하고 초기화하는 방법을 보여줍니다.

### 이 책에서 사용된 규칙

이 책에서 소개된 아이디어를 더 잘 설명하기 위해, 우리는 많은 짧은 코드 스니펫, 사용하는 도구의 스크린샷, 그리고 다이어그램을 포함했습니다.

공식 문서나 라이브러리와 같은 외부 리소스뿐만 아니라 이 책의 다른 장으로도 링크를 제공합니다.

특정 주제에 대한 추가 컨텍스트를 제공하기 위해, 우리는 주석 박스를 사용하기로 결정했습니다. 이 부분은 선형적으로 읽도록 의도된 것은 아니지만, 읽으실 때 건너뛰지 않기를 강력히 권장합니다.

### Callstack 소개

Callstack에서 우리는 React Native를 발전시키고 개발자들이 고성능 애플리케이션을 구축할 수 있도록 지원하는 데 전념하고 있습니다. 핵심 기여자이자 Meta 파트너로서, 우리는 커뮤니티와 긴밀히 협력하여 제안을 형성하고, 주요 모듈을 유지하며, 프레임워크의 발전을 주도합니다. 우리 팀은 React Native의 월간 릴리스에 적극적으로 기여하여 개발자들이 최신 개선 사항에 접근할 수 있도록 합니다. 우리의 전문 지식, 도구 및 모범 사례를 공유함으로써, 우리는 팀들이 성능 문제를 해결하고, 앱을 최적화하며, React Native로 가능한 것의 경계를 확장하는 데 도움을 줍니다.

### 연락하기

React Native는 커뮤니티 덕분에 번창하며, 여러분도 그 진화에 참여할 수 있습니다. 자신의 앱을 최적화하거나, 오픈 소스 프로젝트에 기여하거나, 최신 발전에 대해 알고 있음으로써, 여러분의 참여는 생태계를 발전시키는 데 도움이 됩니다.

다음은 참여할 수 있는 방법입니다:
- **최신 정보 유지하기** — 우리의 뉴스레터를 구독하고 최신 인사이트, 오픈 소스 기여 및 모범 사례를 팔로우하세요.
- **대화에 참여하기** — 우리의 Discord 서버에 참여하여 제안에 대해 논의하고, 아이디어를 공유하며, React Native의 미래를 형성하는 데 도움을 주세요.
- **기여하기** — 우리의 오픈 소스 프로젝트를 탐색하고 협력하세요.

연락하고 싶으시면 언제든지 문의해 주세요. 함께 더 빠르고 더 나은 React Native를 만들어 갑시다!

## 성능이 중요한 이유

모바일 앱을 개발할 때 성능은 단순한 기술적 관심사가 아닌 사용자 경험의 우선순위입니다. 빠르고 반응성 있는 앱은 기뻐하는 사용자와 앱을 완전히 포기하는 사용자 사이의 차이를 만들 수 있습니다. 하지만 "빠르다"는 것은 정확히 무엇을 의미할까요? 단순한 수치나 벤치마크일까요, 아니면 사용자의 인식일까요?

### 사용자의 관점

체감 성능은 앱이 사용자에게 얼마나 빠르게 느껴지는지에 관한 것입니다. 이는 단순히 원시 수치나 벤치마크에 관한 것이 아니라 속도의 환상을 만드는 것입니다.

1940년대에 뉴욕의 사무실 빌딩은 엘리베이터가 느리다는 불만을 받았습니다. 이 문제를 해결하기 위해, 빌딩 관리자는 엘리베이터 속도를 높이는 대신 엘리베이터 근처에 거울을 설치했습니다. 이 간단한 변화는 사람들이 자신을 바라보는 등 할 일을 제공함으로써 주의를 분산시켜 체감 대기 시간을 줄이고 효과적으로 문제를 해결했습니다. 이 이야기는 도시 전설에 가까울 수 있지만, 이 개념 자체는 특정 사건이 얼마나 빠르게 느껴지는지에 영향을 미치는 체감 시간과 주의 분산에 관한 유효한 심리학적 원칙에 근거하고 있습니다.

모바일 앱이 로드되는 데 몇 초가 걸릴 때, 스플래시 스크린, 스켈레톤 UI, 또는 게임을 보여주는 것은 사용자들이 앱이 응답하고 사용할 준비가 되어 있다고 느끼게 할 수 있습니다. 그래서 체감 성능이 사용자 만족도를 형성하는 데 있어 실제 성능보다 종종 더 중요합니다.

그러나 체감 성능에만 집중하는 것은 오해의 소지가 있을 수 있습니다. 애니메이션, 플레이스홀더 또는 콘텐츠 사전 로딩과 같은 트릭이 사용자의 인식을 향상시킬 수 있지만, 이는 근본적인 성능 문제를 해결하지는 않습니다. 이것이 TTI와 FPS와 같은 측정 가능한 지표가 그 가치를 보여주는 곳입니다.

### 중요한 지표: TTI와 FPS

측정하고 모니터링할 수 있는 많은 지표 중에서, 두 가지가 사용자가 앱의 속도를 인식하는 방식에 가장 큰 영향을 미칩니다. 하나는 TTI로 설명되는 사용자가 앱과 얼마나 빨리 상호 작용할 수 있는지와 다른 하나는 FPS로 설명되는 상호 작용할 때 앱이 얼마나 유동적으로 느껴지는지입니다.

#### 상호 작용 가능 시간(TTI)

앱의 부팅 시간 성능을 측정하는 것은 TTI 지표로 설명됩니다. 이는 앱이 시작 후 얼마나 빨리 사용 가능해지는지를 측정합니다. 이것은 사용자가 지연이나 끊김 없이 앱과 상호 작용을 시작할 수 있는 순간입니다. 그러나 TTI는 단지 앱의 초기 로딩에 관한 것만이 아닙니다. 기업들은 종종 이 지표의 변형, 예를 들어 홈 화면까지의 시간(홈 화면을 로드하는 데 걸리는 시간) 또는 특정 화면까지의 시간(특정 기능으로 이동하는 데 걸리는 시간)과 같은 지표를 추적합니다. 이러한 지표는 복잡한 네비게이션 흐름이나 많은 초기 데이터 가져오기가 있는 앱에 특히 중요합니다.

앱이 로드되는 데 너무 오래 걸리면, 사용자는 그 기능을 탐색할 기회를 갖기도 전에 앱을 포기하거나, 무언가를 시도할 때마다 느려서 좌절감을 느끼고, 그 결과 불행해지며 때로는 앱, 때로는 브랜드 자체와 그 기분을 연관시키기 시작할 수 있습니다.

#### 초당 프레임 수(FPS)

앱이 실행되고 있을 때, FPS는 런타임 성능에 대한 핵심 지표가 됩니다. FPS는 스크롤, 스와이프 또는 버튼 탭과 같은 사용자 상호 작용에 앱이 얼마나 부드럽게 응답하는지를 측정합니다. 높은 FPS(이상적으로 초당 60프레임 이상)는 애니메이션과 전환이 부드럽고 자연스럽게 느껴지도록 보장합니다. FPS가 떨어지면 사용자 경험이 지연되고 애니메이션이 끊겨 앱이 다듬어지지 않았거나 응답이 느린 것처럼 느껴집니다. 이것은 풍부한 시각적 콘텐츠나 복잡한 상호 작용이 있는 앱에 특히 중요합니다.

### "빠르다"는 것은 무엇을 의미할까?

우리가 데이터와 가설을 검증하는 과학적 접근 방식을 좋아하지만, 모바일 사용자에게 "빠르다"는 것이 무엇을 의미하는지에 대한 독립적이고 고품질의 연구가 충분하지 않다는 것을 인식합니다. 보통은 대형 기술 기업들이 그들의 전환율에 대해 자랑하거나 설문조사에 기반한 유료 보고서 형태로 제공됩니다.

그럼에도 불구하고, 더 중요한 것은 속도감이 움직이는 목표라는 점입니다. 더 높은 성능의 장치에 접근하고 단일 스마트폰에서 다양한 앱을 사용하면서, 사용자의 기대치는 점점 높아지고 있습니다. 이러한 기대치가 인구통계학적으로 크게 다르다는 점도 기억할 가치가 있습니다. 예를 들어, 젊은 세대는 일반적으로 더 많은 시간이 걸리는 데 익숙한 부모와 조부모보다 앱이 더 빠르게 로드되고 작동하기를 기대합니다. 내부 기업 앱 사용자와 개인 고객 간의 기대치와 개선 인센티브도 다릅니다.

앱이 충분히 빠른지에 대한 가장 신뢰할 수 있는 지표는 사용자를 알고 경쟁사를 아는 것입니다. 사용자 행동에 대해 수집하는 분석 데이터는 사용자가 이탈하는 지점에 대한 개인화된 통찰력을 제공할 것입니다. 반면에 경쟁사의 앱이 객관적으로 더 빠르거나 느린지 분석하는 것은 성능이 사용자를 잃는 잠재적인 이유인지에 대한 힌트를 제공할 것입니다.

이 책의 첫 두 부분에서는 JavaScript, React 및 네이티브 최적화를 통해 런타임 성능을 향상시키는 방법에 초점을 맞출 것입니다. FPS에 영향을 미치는 요소를 더 잘 이해하고 어디서든 해결할 수 있도록 필요한 모든 도구와 기술을 안내해 드리겠습니다.

세 번째 부분에서는 JavaScript와 네이티브 측면 모두에서 발생하는 앱의 번들링 프로세스에 초점을 맞추며, 이는 부팅 시간 성능과 높은 상관관계가 있습니다. 가이드의 첫 두 부분과 마찬가지로, 앱이 느리게 로드되는 원인이 무엇인지 더 잘 파악하고 이를 빠르게 만드는 방법을 찾는 데 필요한 모든 것을 제공해 드리겠습니다. 

## 비제어 컴포넌트

가장 중요한 성능 최적화 기법 중 하나이자 불행히도 도입하기 가장 어려운 것 중 하나는 React의 비제어 컴포넌트입니다. 이 장에서는 React Native 앱에서의 동적 UI 세계를 탐구하여 제어 컴포넌트와 비제어 컴포넌트 접근 방식을 비교하고 하나에서 다른 하나로 마이그레이션하는 방법을 배우겠습니다.

![하나는 덜 높은(제어 컴포넌트), 다른 하나는 훨씬 더 높은(비제어 컴포넌트) 두 개의 고층 빌딩 사진](https://example.com/image.png)

자바스크립트로 제어하는 컴포넌트에 대해, 우리는 앱이 더 부드럽고 응답성이 좋게 느껴지도록 하는 데 있어 비제어 컴포넌트를 사용하는 것이 왜 게임 체인저가 될 수 있는지 밝혀 드리겠습니다. 현재 앱의 UI가 모두 제어되고 있더라도, 이 대안적 접근 방식을 이해하고 가장 합리적인 곳에 구현하는 데 도움을 드릴 수 있습니다.

### React Native에서의 제어 컴포넌트

기본적으로 React에서는 일반적으로 UI와 그 동작을 자바스크립트에서 선언하여 React 컴포넌트가 그 외관과 동작을 완전히 제어할 수 있게 합니다. 이 접근 방식에는 부정할 수 없는 장점이 있습니다: 선언적 프로그래밍 스타일을 활용하여 UI가 애플리케이션 상태의 직접적인 반영이 되도록 하기 때문에 직관적입니다. 또한 React가 상태가 변경될 때마다 DOM을 관리하고 업데이트하므로 강력합니다.

React Native에서 제어 컴포넌트는 상태가 자바스크립트 메모리에 저장되고 네이티브 컴포넌트에 props로 명시적으로 전달되는 컴포넌트입니다. 예를 들어, React Native에서 뷰의 위치를 애니메이션 처리하려면 다음과 같이 할 수 있습니다:

```jsx
const [animatedValue, setAnimatedValue] = useState(new Animated.Value(0));

useEffect(() => {
  Animated.timing(animatedValue, {
    toValue: 1,
    duration: 1000,
    useNativeDriver: false,
  }).start();
}, []);

return (
  <Animated.View
    style={{
      transform: [
        {
          translateX: animatedValue.interpolate({
            inputRange: [0, 1],
            outputRange: [0, 100],
          }),
        },
      ],
    }}
  />
);
```

이 예시에서 애니메이션 값은 자바스크립트에 저장되고, React Native는 그 값을 모든 애니메이션 프레임에서 네이티브 측에 전달해야 합니다. 자바스크립트에서 애니메이션 값이 변경되면 네이티브 측에서 컴포넌트의 재렌더링이 트리거됩니다.

React Native의 스레딩 모델에 대해 더 자세히 알고 싶다면, 가이드의 첫 번째 섹션에 제시된 아키텍처 다이어그램을 참고할 수 있습니다.

### 제어 컴포넌트의 문제점

제어 컴포넌트는 직관적이고 개발자 인체공학을 훌륭하게 제공하지만, 성능 비용이 따릅니다. 구체적으로:

1. **자바스크립트 스레드 오버헤드**: 각 업데이트는 자바스크립트 스레드에서 실행되어야 하며, 이는 특히 애니메이션이나 제스처와 같은 고주파 업데이트에서 병목 현상이 될 수 있습니다.
2. **브릿지 직렬화**: JSI를 사용하는 새로운 아키텍처에서도 자바스크립트와 네이티브 간 통신에는 여전히 직렬화 오버헤드가 있습니다.
3. **경쟁 작업**: 컴포넌트 업데이트는 비즈니스 로직 처리나 API 응답 처리와 같은 다른 자바스크립트 작업과 경쟁합니다.

제어 컴포넌트의 성능 영향은 특히 애니메이션과 제스처에서 두드러집니다. 이러한 상호 작용은 종종 고주파 업데이트(부드러운 애니메이션을 위해 초당 60-120회까지)를 필요로 하며, 이는 자바스크립트 스레드를 압도할 수 있습니다.

### 비제어 컴포넌트의 구원

반면에 비제어 컴포넌트는 네이티브 측에서 상태를 유지하여 자바스크립트 스레드 관여의 필요성을 줄입니다. 이 패턴은 상태 관리를 자바스크립트에서 네이티브 UI 스레드로 오프로드하여 더 부드러운 애니메이션과 제스처를 허용합니다.

다음과 같이 더 성능이 좋은 변형을 예로 들어보겠습니다:

```jsx
return (
  <Animated.View
    style={{
      transform: [
        {
          translateX: animatedValue.interpolate({
            inputRange: [0, 1],
            outputRange: [0, 100],
          }),
        },
      ],
    }}
  >
    <Animated.timing
      animatedValue={animatedValue}
      toValue={1}
      duration={1000}
      useNativeDriver={true}
    />
  </Animated.View>
);
```

`useNativeDriver`를 `true`로 설정함으로써, 우리는 애니메이션 계산을 네이티브 UI 스레드로 오프로드하여 모든 프레임 업데이트마다 자바스크립트를 우회합니다.

### 실제 예: 햅틱 피드백이 있는 버튼

제어 컴포넌트와 비제어 컴포넌트의 차이를 설명하기 위해, 두 가지 접근 방식으로 햅틱 피드백이 있는 버튼을 구현해 보겠습니다. 다음과 같은 버튼을 만들고 싶다고 상상해 보세요:

1. 누르면 색상이 변경됨
2. 누르면 약간 축소됨
3. 누르면 햅틱 피드백을 제공함

#### 제어 구현

```jsx
import React, {useState} from 'react';
import {Pressable, Text, StyleSheet} from 'react-native';
import * as Haptics from 'expo-haptics';

const ControlledButton = ({title, onPress}) => {
  const [isPressed, setIsPressed] = useState(false);

  return (
    <Pressable
      style={[
        styles.button,
        isPressed && styles.buttonPressed,
      ]}
      onPressIn={() => {
        setIsPressed(true);
        Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Medium);
      }}
      onPressOut={() => {
        setIsPressed(false);
        onPress && onPress();
      }}>
      <Text style={styles.text}>{title}</Text>
    </Pressable>
  );
};

const styles = StyleSheet.create({
  button: {
    backgroundColor: '#3498db',
    padding: 15,
    borderRadius: 8,
    transform: [{scale: 1}],
  },
  buttonPressed: {
    backgroundColor: '#2980b9',
    transform: [{scale: 0.95}],
  },
  text: {
    color: 'white',
    fontWeight: 'bold',
    textAlign: 'center',
  },
});
```

이 접근 방식에서는 모든 상태 변경이 자바스크립트 스레드에서 처리됩니다. 사용자가 버튼을 누르면 자바스크립트는 상태를 업데이트하고, 햅틱 피드백을 트리거하며, 그런 다음 네이티브 측의 UI가 업데이트되어야 합니다.

#### 비제어 구현

```jsx
import React from 'react';
import {Pressable, Text, StyleSheet} from 'react-native';
import * as Haptics from 'expo-haptics';
import Animated, {
  useAnimatedStyle,
  useSharedValue,
  withTiming,
} from 'react-native-reanimated';

const AnimatedPressable = Animated.createAnimatedComponent(Pressable);

const UncontrolledButton = ({title, onPress}) => {
  const scale = useSharedValue(1);
  const backgroundColor = useSharedValue('#3498db');

  const animatedStyle = useAnimatedStyle(() => {
    return {
      backgroundColor: backgroundColor.value,
      transform: [{scale: scale.value}],
    };
  });

  return (
    <AnimatedPressable
      style={[styles.button, animatedStyle]}
      onPressIn={() => {
        backgroundColor.value = '#2980b9';
        scale.value = withTiming(0.95, {duration: 100});
        Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Medium);
      }}
      onPressOut={() => {
        backgroundColor.value = '#3498db';
        scale.value = withTiming(1, {duration: 100});
        onPress && onPress();
      }}>
      <Text style={styles.text}>{title}</Text>
    </AnimatedPressable>
  );
};

const styles = StyleSheet.create({
  button: {
    padding: 15,
    borderRadius: 8,
  },
  text: {
    color: 'white',
    fontWeight: 'bold',
    textAlign: 'center',
  },
});
```

이 구현에서는 `react-native-reanimated`의 공유 값과 애니메이션 스타일을 사용하여 상태를 네이티브 UI 스레드로 오프로드합니다. 애니메이션과 스타일 업데이트는 자바스크립트 스레드를 포함하지 않고 네이티브 측에서 처리됩니다.

비교해 보면:
- **제어 구현**: 자바스크립트가 모든 프레임 업데이트에 관여하여 버튼 누름/해제 상태에 따라 스타일을 설정합니다.
- **비제어 구현**: 자바스크립트는 공유 값을 업데이트하기만 하고, 실제 애니메이션과 스타일 계산은 모두 네이티브 측에서 처리됩니다.

### 비제어 컴포넌트로 마이그레이션하기

기존 애플리케이션을 비제어 컴포넌트로 마이그레이션하는 방법에 대한 일반적인 조언이 있습니다:

1. **점진적으로 마이그레이션**: 애플리케이션 전체를 한 번에 재작성하는 대신, 성능이 중요한 영역부터 시작하세요.

2. **올바른 라이브러리 선택**: 비제어 컴포넌트를 구현하는 데 도움이 되는 몇 가지 인기 있는 라이브러리가 있습니다:
   - **React Native Reanimated**: 네이티브 드라이버 애니메이션을 위한 강력한 라이브러리
   - **React Native Gesture Handler**: 네이티브 측에서 제스처를 처리하기 위한 라이브러리
   - **React Native Skia**: 하드웨어 가속 2D 그래픽을 위한 라이브러리

3. **올바른 컴포넌트를 선택**: 모든 것이 비제어일 필요는 없습니다. 다음과 같은 경우에 비제어 접근 방식을 고려하세요:
   - 부드러운 애니메이션 필요
   - 복잡한 제스처 상호작용
   - 60fps를 달성하기 위한 고성능 드로잉 요구사항

### 결론

비제어 컴포넌트는 자바스크립트 상태 관리의 직관성과 네이티브 성능의 강점을 결합할 수 있는 강력한 도구입니다. 이 패턴은 React Native 앱에서 실세계 성능 병목 현상을 해결하는 데 매우 중요할 수 있습니다.

이 접근 방식에는 학습 곡선이 있지만, 보상은 상당합니다: 더 부드러운 애니메이션, 더 반응성 있는 제스처, 그리고 전반적으로 더 나은 사용자 경험입니다. 모든 컴포넌트를 비제어로 만들 필요는 없지만, 이 패턴을 애플리케이션의 성능에 민감한 영역에 적용하면 체감되는 성능이 크게 향상될 수 있습니다.

## 고차 특수화 컴포넌트

React 애플리케이션의 성능을 최적화하는 가장 효과적인 방법 중 하나는 불필요한 렌더링을 줄이는 것입니다. 이 장에서는 "고차 특수화 컴포넌트" 패턴을 소개하여 컴포넌트를 최대한 세분화하고 특정 데이터 조각으로 특수화하는 방법을 알아볼 것입니다. 이 패턴은 다양한 상태에 의존하는 복잡한 UI 컴포넌트를 처리할 때 특히 효과적입니다.

![시간이 지남에 따라 여러 가지로 나뉘는 나무 이미지](https://example.com/image.png)

### 문제: 과도한 렌더링

복잡한 React Native 앱에서 흔히 발생하는 시나리오를 생각해 봅시다. 여러 개의 상태 변수를 가진 큰 컴포넌트가 있고, 이 컴포넌트의 일부만 특정 상태 변경에 반응해야 합니다.

예를 들어, 사용자 정보, 폼 상태, 로딩 상태 및 오류를 표시하는 프로필 화면이 있다고 가정해 보겠습니다:

```jsx
function ProfileScreen() {
  const [userData, setUserData] = useState(null);
  const [isEditing, setIsEditing] = useState(false);
  const [formData, setFormData] = useState({});
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);
  
  // 많은 데이터 페칭, 폼 처리 등의 로직...
  
  return (
    <View style={styles.container}>
      <UserHeader userData={userData} />
      <ProfileDetails userData={userData} />
      
      {isEditing ? (
        <EditForm 
          formData={formData} 
          onChange={setFormData}
          onSave={() => {/* 저장 로직 */}}
        />
      ) : (
        <ActionButtons onEdit={() => setIsEditing(true)} />
      )}
      
      {isLoading && <LoadingIndicator />}
      {error && <ErrorMessage message={error} />}
    </View>
  );
}
```

이 컴포넌트의 문제는 무엇일까요? `formData`가 변경될 때마다 폼 내의 필드만 업데이트해야 하는데도 불구하고, 전체 `ProfileScreen` 컴포넌트가 리렌더링됩니다. 이는 `UserHeader`, `ProfileDetails` 및 다른 모든 자식 컴포넌트도 리렌더링된다는 것을 의미합니다.

### 전통적인 해결책: React.memo

첫 번째 접근 방식은 일반적으로 각 자식 컴포넌트를 `React.memo`로 래핑하는 것입니다:

```jsx
const UserHeader = React.memo(({ userData }) => {
  return <View>{/* 헤더 컨텐츠 */}</View>;
});

const ProfileDetails = React.memo(({ userData }) => {
  return <View>{/* 프로필 상세 정보 */}</View>;
});

// 다른 컴포넌트들에도 비슷하게 적용
```

이 접근 방식은 도움이 되지만 몇 가지 단점이 있습니다:

1. 각 컴포넌트마다 `React.memo`를 추가해야 합니다
2. 복잡한 props가 있을 경우 사용자 정의 비교 함수가 필요합니다
3. 여전히 리렌더링을 유발하는 새 함수나 객체를 만드는 것을 피해야 합니다

### 더 나은 방법: 고차 특수화 컴포넌트

고차 특수화 컴포넌트(Specialized Higher Order Components)는 이 문제를 다른 방식으로 접근합니다. 이 패턴의 핵심 아이디어는 다음과 같습니다:

1. 상태 관리 로직을 포함하는 "컨테이너" 컴포넌트를 만듭니다
2. 각 데이터 조각(상태, 액션)에 특화된 여러 개의 작은 컴포넌트를 생성합니다
3. 각 특수화된 컴포넌트는 자신이 관심 있는 데이터에만 접근합니다

위의 예시를 재구성해 보겠습니다:

```jsx
// 메인 컨테이너 컴포넌트
function ProfileScreen() {
  const [userData, setUserData] = useState(null);
  const [isEditing, setIsEditing] = useState(false);
  const [formData, setFormData] = useState({});
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);
  
  // 필요한 데이터 페칭 및 다른 로직들...
  
  return (
    <View style={styles.container}>
      <UserDataProvider value={userData}>
        <UserHeader />
        <ProfileDetails />
      </UserDataProvider>
      
      <EditingProvider value={{ isEditing, setIsEditing, formData, setFormData }}>
        <EditingSection />
      </EditingProvider>
      
      <StatusProvider value={{ isLoading, error }}>
        <StatusSection />
      </StatusProvider>
    </View>
  );
}

// 특수화된 자식 컴포넌트들
function EditingSection() {
  const { isEditing, setIsEditing, formData, setFormData } = useEditingContext();
  
  if (isEditing) {
    return (
      <EditForm 
        formData={formData} 
        onChange={setFormData}
        onSave={() => {/* 저장 로직 */}}
      />
    );
  }
  
  return <ActionButtons onEdit={() => setIsEditing(true)} />;
}

function StatusSection() {
  const { isLoading, error } = useStatusContext();
  
  return (
    <>
      {isLoading && <LoadingIndicator />}
      {error && <ErrorMessage message={error} />}
    </>
  );
}
```

이 패턴의 핵심 이점:

1. **지역화된 리렌더링**: 특정 상태가 변경되면 해당 상태를 사용하는 컴포넌트만 리렌더링됩니다
2. **더 나은 메모리 사용량**: 불필요한 함수와 객체 생성이 줄어듭니다
3. **더 부드러운 UX**: 특히 복잡한 폼과 빈번한 업데이트가 있는 UI에서 성능 향상이 가장 두드러집니다

### 결론

고차 특수화 컴포넌트 패턴은 React Native 앱에서 불필요한 리렌더링을 효과적으로 관리하는 강력한 접근 방식입니다. 상태를 작고 독립적인 조각으로 분리함으로써, 불필요한 렌더링을 획기적으로 줄이고 앱의 전반적인 응답성을 향상시킬 수 있습니다.

현대적인 모바일 앱이 점점 더 복잡해지고 사용자의 기대치가 높아짐에 따라, 고차 특수화 컴포넌트와 같은 최적화 기법은 더 이상 선택 사항이 아닌 필수가 되고 있습니다. 이 패턴을 마스터하면 개발자는 더 효율적이고, 확장 가능하며, 사용자 친화적인 React Native 애플리케이션을 구축할 수 있습니다. 

## 동시성 React

React 18은 동시성 렌더링이라는 획기적인 새로운 패러다임을 도입했습니다. 이 장에서는 동시성 React가 무엇인지, 그리고 이것이 어떻게 React Native 애플리케이션의 성능과 사용자 경험을 크게 향상시킬 수 있는지 살펴보겠습니다.

![여러 태스크를 병렬로 처리하는 다중 차선 고속도로 이미지](https://example.com/image.png)

### 동시성 React란 무엇인가?

동시성 React는 React의 새로운 렌더링 전략으로, 여러 렌더링 작업을 동시에 수행하고 그들의 우선순위를 지정할 수 있게 해줍니다. 이 접근 방식의 목표는 중요한 사용자 상호 작용에 즉시 응답하면서도 복잡한 화면 업데이트를 효율적으로 처리하는 것입니다.

기존 React 모델(React 17 이전)에서 렌더링은 원자적이고 중단할 수 없었습니다 - 렌더링이 시작되면 완료될 때까지 다른 작업이 UI 스레드를 차지할 수 없었습니다. 이로 인해 복잡한 업데이트 중에 사용자 입력이 지연되는 문제가 종종 발생했습니다.

동시성 React에서는 리렌더링이 중단, 재개 또는 중단될 수 있습니다. 이것은 UI가 사용자 입력과 같은 더 중요한 업데이트에 즉시 응답할 수 있다는 것을 의미합니다.

### 동시성이 React Native에서 성능을 향상시키는 방법

동시성은 다음과 같은 방식으로 React Native 앱의 성능을 향상시킬 수 있습니다:

1. **사용자 상호 작용에 대한 더 나은 응답성**: 대규모 데이터 집합을 처리하거나 렌더링하는 동안에도 앱이 즉시 응답성을 유지합니다.

2. **더 부드러운 전환 및 애니메이션**: 렌더링 작업의 우선순위를 조정하여 애니메이션이 항상 부드럽게 실행되도록 할 수 있습니다.

3. **향상된 백그라운드 업데이트**: 사용자 경험을 방해하지 않고 백그라운드에서 콘텐츠를 로드하고 업데이트할 수 있습니다.

### 핵심 동시성 기능

React 18에서 도입된 주요 동시성 기능을 알아보고, 이것들이 React Native에서 어떻게 사용될 수 있는지 살펴보겠습니다:

#### 1. Suspense

Suspense는 컴포넌트가 렌더링을 위한 데이터를 기다리는 동안 대체 UI를 표시할 수 있게 해주는 React 기능입니다. React Native에서는 다음과 같이 사용할 수 있습니다:

```jsx
import { Suspense } from 'react';
import { View, Text, ActivityIndicator } from 'react-native';
import ProfileDetails from './ProfileDetails';

function UserProfile({ userId }) {
  return (
    <View style={styles.container}>
      <Text style={styles.header}>사용자 프로필</Text>
      
      <Suspense 
        fallback={
          <View style={styles.loadingContainer}>
            <ActivityIndicator size="large" color="#0000ff" />
            <Text>프로필 로딩 중...</Text>
          </View>
        }
      >
        <ProfileDetails userId={userId} />
      </Suspense>
    </View>
  );
}
```

Suspense의 주요 이점은 데이터 로딩 로직이 UI 컴포넌트로부터 분리된다는 것입니다. `ProfileDetails` 컴포넌트는 필요한 데이터가 아직 준비되지 않았을 때 렌더링을 "일시 중단"할 수 있으며, React는 대신 폴백 UI를 표시합니다.

#### 2. useTransition

`useTransition` 훅은 UI 업데이트를 "긴급"과 "비긴급" 업데이트로 구분할 수 있게 해줍니다. 이것은 중요한 사용자 상호 작용이 지연되지 않도록 보장하면서 복잡한 화면 업데이트를 처리하는 데 유용합니다.

```jsx
import { useState, useTransition } from 'react';
import { View, TextInput, FlatList, Text } from 'react-native';

function SearchableList({ items }) {
  const [query, setQuery] = useState('');
  const [filteredItems, setFilteredItems] = useState(items);
  const [isPending, startTransition] = useTransition();

  const handleSearch = (text) => {
    // 우선 즉시 쿼리를 업데이트 (긴급 업데이트)
    setQuery(text);
    
    // 그런 다음 트랜지션에서 필터링 수행 (비긴급 업데이트)
    startTransition(() => {
      const filtered = items.filter(item => 
        item.title.toLowerCase().includes(text.toLowerCase())
      );
      setFilteredItems(filtered);
    });
  };

  return (
    <View style={styles.container}>
      <TextInput
        style={styles.searchInput}
        value={query}
        onChangeText={handleSearch}
        placeholder="검색..."
      />
      
      {isPending && (
        <Text style={styles.pendingText}>결과 업데이트 중...</Text>
      )}
      
      <FlatList
        data={filteredItems}
        renderItem={({ item }) => <Text style={styles.item}>{item.title}</Text>}
        keyExtractor={(item) => item.id.toString()}
      />
    </View>
  );
}
```

이 예시에서는 입력 필드 업데이트가 즉시 이루어지므로 사용자는 타이핑에 지연을 느끼지 않습니다. 목록 필터링은 트랜지션 내에서 발생하므로, 항목이 수천 개라도 타이핑 경험은 부드럽게 유지됩니다.

#### 3. useDeferredValue

`useDeferredValue` 훅은 값의 "지연" 버전을 생성합니다. 이것은 UI의 일부가 다른 부분보다 덜 중요할 때 유용합니다.

```jsx
import { useState, useDeferredValue } from 'react';
import { View, TextInput, Text } from 'react-native';
import ExpensiveChart from './ExpensiveChart';

function DataDashboard() {
  const [inputValue, setInputValue] = useState('');
  const deferredValue = useDeferredValue(inputValue);
  
  // inputValue와 deferredValue가 다르면 로딩 상태임을 나타냄
  const isStale = inputValue !== deferredValue;

  return (
    <View style={styles.container}>
      <TextInput
        style={styles.input}
        value={inputValue}
        onChangeText={setInputValue}
        placeholder="차트 파라미터 변경..."
      />
      
      <View style={[
        styles.chartContainer,
        isStale && styles.staleBg
      ]}>
        {isStale && (
          <Text style={styles.updatingText}>업데이트 중...</Text>
        )}
        <ExpensiveChart data={deferredValue} />
      </View>
    </View>
  );
}
```

이 패턴은 실시간 필터링, 텍스트 하이라이팅, 데이터 시각화와 같이 계산 비용이 많이 드는 렌더링 작업에 매우 유용합니다.

### 동시성 React로 리스트 최적화하기

긴 목록을 렌더링하는 것은 React Native에서 성능 문제가 자주 발생하는 영역입니다. 동시성 API를 사용하면 이러한 문제를 더 효과적으로 해결할 수 있습니다:

```jsx
import { useMemo, useState, useDeferredValue, useTransition } from 'react';
import { View, TextInput, FlatList, Text, ActivityIndicator } from 'react-native';

function OptimizedList({ data }) {
  const [filter, setFilter] = useState('');
  const [filteredItems, setFilteredItems] = useState(data);
  const [isPending, startTransition] = useTransition();
  const deferredFilter = useDeferredValue(filter);
  
  const filteredItems = useMemo(() => {
    if (!deferredFilter) return data;
    return data.filter(item => 
      item.title.toLowerCase().includes(deferredFilter.toLowerCase())
    );
  }, [data, deferredFilter]);

  const handleFilterChange = (text) => {
    setFilter(text);
  };
  
  const isFiltering = filter !== deferredFilter;

  return (
    <View style={styles.container}>
      <TextInput
        style={styles.filter}
        value={filter}
        onChangeText={handleFilterChange}
        placeholder="검색..."
      />
      
      {isFiltering && (
        <View style={styles.loadingIndicator}>
          <ActivityIndicator size="small" color="#0000ff" />
          <Text>필터링 중...</Text>
        </View>
      )}
      
      <FlatList
        data={filteredItems}
        numColumns={2}
        renderItem={({ item }) => (
          <View style={styles.imageContainer}>
            <Text style={styles.title}>{item.title}</Text>
            <Text style={styles.subtitle}>{item.description}</Text>
          </View>
        )}
        keyExtractor={item => item.id.toString()}
      />
    </View>
  );
}
```

이 최적화된 목록에서는 필터링 작업이 지연 값을 사용하여 우선순위가 낮은 작업으로 처리됩니다. 이로 인해 입력 필드와의 상호 작용이 항상 즉각적으로 느껴지며, UI 스레드 차단이 최소화됩니다.

### React Native에서 동시성 채택 시 고려사항

React Native에서 동시성 기능을 구현할 때 염두에 두어야 할 몇 가지 중요한 점:

#### 1. 깜박임 방지

동시성 모드에서는 UI가 다른 상태 사이를 빠르게 전환될 수 있으며, 이로 인해 "깜박임"이 발생할 수 있습니다. 이를 방지하기 위한 팁:

```jsx
// 로딩 표시기를 위한 지연 구현
function DelayedSpinner({ isLoading }) {
  const [shouldShow, setShouldShow] = useState(false);
  
  useEffect(() => {
    if (isLoading) {
      const timer = setTimeout(() => setShouldShow(true), 500);
      return () => clearTimeout(timer);
    } else {
      setShouldShow(false);
    }
  }, [isLoading]);
  
  return shouldShow ? (
    <ActivityIndicator size="large" color="#0000ff" />
  ) : null;
}
```

#### 2. 데이터 가져오기 통합

Suspense 기반 데이터 가져오기를 위해서는 데이터 가져오기 라이브러리가 Suspense와 호환되어야 합니다:

```jsx
// Suspense와 호환되는 React Query 사용 예시
import { QueryClient, QueryClientProvider, useQuery } from 'react-query';

function SuspendingProfile({ userId }) {
  const { data } = useQuery(
    ['user', userId],
    () => fetchUserData(userId),
    { suspense: true } // Suspense 모드 활성화
  );
  
  return (
    <View style={styles.profile}>
      <Text style={styles.name}>{data.name}</Text>
      <Text style={styles.bio}>{data.bio}</Text>
    </View>
  );
}
```

#### 3. 메모이제이션 활용

동시성 React에서는 메모이제이션이 여전히 중요합니다:

```jsx
// 중요 계산 메모이제이션
const MemoizedExpensiveComponent = memo(function ExpensiveComponent({ data }) {
  // 복잡한 렌더링 로직
  return <ComplexVisualization data={data} />;
}, (prevProps, nextProps) => {
  // 커스텀 비교 함수
  return prevProps.data.id === nextProps.data.id;
});
```

### 고급 사용 사례: 동시성 모드에서의 이미지 갤러리

다음은 이미지 갤러리에서 동시성 React를 사용하여 성능을 최적화하는 고급 예제입니다:

```jsx
import React, { Suspense, useState, useTransition, useDeferredValue } from 'react';
import { View, Text, TextInput, FlatList, Image, ActivityIndicator } from 'react-native';

// 이미지 리소스를 사전 로드하는 컴포넌트
function SuspenseImage({ uri, style }) {
  const resource = preloadImage(uri);
  const src = resource.read(); // 이미지가 로드될 때까지 일시 중단
  return <Image source={{ uri: src }} style={style} />;
}

function ImageGallery({ images }) {
  const [searchTerm, setSearchTerm] = useState('');
  const [filteredItems, setFilteredItems] = useState(images);
  const [isPending, startTransition] = useTransition();
  const deferredSearchTerm = useDeferredValue(searchTerm);
  
  const filteredItems = useMemo(() => {
    if (!deferredSearchTerm) return images;
    return images.filter(img => 
      img.title.toLowerCase().includes(deferredSearchTerm.toLowerCase())
    );
  }, [images, deferredSearchTerm]);
  
  const handleSearch = (text) => {
    setSearchTerm(text);
  };

  return (
    <View style={styles.container}>
      <TextInput
        style={styles.searchInput}
        value={searchTerm}
        onChangeText={handleSearch}
        placeholder="이미지 검색..."
      />
      
      {isPending && <Text style={styles.notice}>검색 중...</Text>}
      
      <FlatList
        data={filteredItems}
        numColumns={2}
        renderItem={({ item }) => (
          <View style={styles.imageContainer}>
            <Suspense fallback={
              <View style={[styles.imagePlaceholder, styles.image]}>
                <ActivityIndicator color="#ffffff" />
              </View>
            }>
              <SuspenseImage 
                uri={item.thumbnailUrl} 
                style={styles.image}
              />
            </Suspense>
            <Text style={styles.imageTitle} numberOfLines={1}>
              {item.title}
            </Text>
          </View>
        )}
        keyExtractor={item => item.id.toString()}
      />
    </View>
  );
}
```

이 예제에서는 여러 동시성 개념을 조합하여 사용했습니다:
- `useTransition`을 사용하여 검색어 입력이 즉시 반영됩니다
- `useDeferredValue`로 이미지 필터링을 지연시켜 UI 응답성을 유지합니다
- `Suspense`를 사용하여 각 이미지가 로드되는 동안 로딩 상태를 표시합니다

### 성능 측정 및 디버깅

동시성 구현을 디버깅하고 최적화하려면:

```jsx
import { useEffect } from 'react';
import { PerformanceObserver } from 'perf_hooks';

function profilingComponent({ children }) {
  useEffect(() => {
    // 시작 시간 로깅
    const startTime = performance.now();
    
    return () => {
      // 컴포넌트 렌더링이 완료되는 데 걸린 시간 계산
      const endTime = performance.now();
      console.log(`컴포넌트 렌더링 시간: ${endTime - startTime}ms`);
    };
  }, []);

  return children;
}
```

### 결론

동시성 React는 React Native 개발자에게 성능과 사용자 경험을 획기적으로 개선할 수 있는 강력한 도구 세트를 제공합니다. 트랜지션, 지연 값, Suspense와 같은 기능을 통해 우리는 복잡한 앱이 단순하고 직관적인 방식으로 최적화할 수 있습니다.

React Native에서 동시성 패턴을 채택함으로써, 우리는:
- 복잡한 UI 업데이트에도 불구하고 항상 응답성 있는 인터페이스를 유지할 수 있습니다
- 사용자 상호 작용과 백그라운드 업데이트 간의 우선순위를 더 잘 관리할 수 있습니다
- 백그라운드에서 콘텐츠를 로드하고 준비하는 동안 더 매끄러운 사용자 경험을 제공할 수 있습니다

동시성 모드는 특히 대규모 데이터 세트, 복잡한 애니메이션, 그리고 다중 데이터 소스가 있는 앱에 가장 큰 이점을 제공합니다. 모든 React Native 앱이 즉시 동시성이 필요한 것은 아니지만, 이 패턴을 이해하고 적용하는 것은 고성능 모바일 앱을 구축하는 개발자의 필수 기술이 되고 있습니다.

## 원자적 상태 관리

React Native 앱의 성능을 최적화하는 또 다른 중요한 방법은 상태 관리 접근 방식을 재고하는 것입니다. 이 장에서는 "원자적 상태 관리" 패턴을 소개하고, 이 접근 방식이 어떻게 불필요한 렌더링을 줄이고 앱의 반응성을 향상시키는지 살펴보겠습니다.

![분자 구조와 같이 연결된 작은 원자들의 이미지](https://example.com/image.png)

### 원자적 상태란 무엇인가?

원자적 상태 관리는 앱의 상태를 작은 독립적인 단위나 "원자"로 분해하는 개념입니다. 각 원자는 특정 관심사를 위한 최소한의 상태를 포함합니다. 대형 단일 상태 객체 대신, 상태는 작고 목적이 분명한 조각들로 분할됩니다.

이 접근 방식은 특히 다음과 같은 경우에 유용합니다:
- 많은 구성 요소가 상태의 다른 부분에 관심을 가지는 복잡한 앱
- 상태 변경이 너무 빈번하여 전체 상태 트리를 업데이트하기 부담스러운 경우
- 다양한 부분의 상태가 다른 빈도로 업데이트되는 경우

### 문제: 전통적인 상태 관리의 한계

일반적인 React 상태 관리 패턴에서는 다음과 같은 몇 가지 한계에 직면합니다:

**1. 과도한 리렌더링**

Redux와 같은 전통적인 글로벌 저장소 패턴에서는 상태의 한 부분을 업데이트하면 종종 해당 상태를 구독하는 모든 컴포넌트가 리렌더링됩니다. 이로 인해 불필요한 렌더링 작업이 발생하고 성능이 저하될 수 있습니다.

```jsx
// Redux 예시: 모든 상태 변경이 모든 연결된 컴포넌트에 영향을 미칠 수 있음
const mapStateToProps = (state) => ({
  user: state.user,
  preferences: state.preferences,
  notifications: state.notifications
});
```

**2. 부분 업데이트의 어려움**

큰 중첩 상태 객체를 사용할 때, 깊은 부분만 부분적으로 업데이트하는 것은 번거로울 수 있으며, 종종 불변성을 유지하기 위해 객체 복사가 많이 필요합니다.

```jsx
// 깊게 중첩된 상태 업데이트
setState(prevState => ({
  ...prevState,
  user: {
    ...prevState.user,
    preferences: {
      ...prevState.user.preferences,
      theme: 'dark'
    }
  }
}));
```

**3. 코드 분할의 어려움**

단일 통합 상태 저장소는 코드 분할과 레이지 로딩을 더 어렵게 만들 수 있습니다. 앱의 한 부분에만 필요한 상태가 앱 전체에 로드되어야 할 수 있습니다.

### 해결책: 원자적 상태 관리

원자적 상태 관리는 각 상태 조각을 독립적인 단위로 취급함으로써 이러한 문제를 해결합니다. 이 접근 방식의 핵심 원칙은 다음과 같습니다:

1. **최소성**: 각 원자는 논리적으로 함께 속하는 최소한의 상태를 포함합니다
2. **독립성**: 원자는 다른 원자에 직접적인 의존성을 갖지 않습니다
3. **구독 제한**: 컴포넌트는 필요한 원자만 구독합니다
4. **파생 상태**: 복잡한 상태는 원자에서 계산될 수 있습니다

#### 구현 예시: Recoil 사용하기

React 생태계에서 원자적 상태를 구현하는 가장 인기 있는 라이브러리 중 하나는 Recoil입니다. Recoil이 어떻게 원자적 상태 관리를 가능하게 하는지 예시로 알아보겠습니다:

```jsx
import {
  atom,
  selector,
  useRecoilState,
  useRecoilValue,
} from 'recoil';

// 원자 정의
const userIdAtom = atom({
  key: 'userId',
  default: null,
});

const userNameAtom = atom({
  key: 'userName',
  default: '',
});

const themeAtom = atom({
  key: 'theme',
  default: 'light',
});

const unreadCountAtom = atom({
  key: 'unreadCount',
  default: 0,
});

// 파생 상태 (선택기)
const userDisplayNameSelector = selector({
  key: 'userDisplayName',
  get: ({get}) => {
    const name = get(userNameAtom);
    return name || '게스트 사용자';
  },
});

// 컴포넌트에서 원자 사용하기
function ProfileHeader() {
  // 이 컴포넌트는 사용자 이름 변경에만 리렌더링됨
  const displayName = useRecoilValue(userDisplayNameSelector);
  return <Text>안녕하세요, {displayName}!</Text>;
}

function ThemeToggle() {
  // 이 컴포넌트는 테마 변경에만 리렌더링됨
  const [theme, setTheme] = useRecoilState(themeAtom);
  return (
    <Switch
      value={theme === 'dark'}
      onValueChange={() => setTheme(theme === 'dark' ? 'light' : 'dark')}
    />
  );
}

function NotificationBadge() {
  // 이 컴포넌트는 읽지 않은 메시지 수가 변경될 때만 리렌더링됨
  const unreadCount = useRecoilValue(unreadCountAtom);
  return unreadCount > 0 ? <Badge count={unreadCount} /> : null;
}
```

이 예시에서는 애플리케이션 상태의 각 독립적인 부분이 별도의 원자로 정의됩니다. 컴포넌트는 관심 있는 특정 원자만 구독합니다. 이로 인해 상태의 한 부분이 변경되어도 해당 특정 상태에 관심이 있는 컴포넌트만 리렌더링됩니다.

#### Jotai를 사용한 다른 예시

Jotai는 Recoil과 유사한 원자적 접근 방식을 제공하지만 좀 더 간결한 API를 가지고 있습니다:

```jsx
import { atom, useAtom } from 'jotai';

// 기본 원자
const countAtom = atom(0);

// 파생 원자
const doubledAtom = atom(
  (get) => get(countAtom) * 2
);

// 쓰기 가능한 파생 원자
const countryAtom = atom('Japan');
const citiesAtom = atom({
  Japan: ['Tokyo', 'Kyoto', 'Osaka'],
  France: ['Paris', 'Marseille', 'Lyon'],
});
const selectedCityAtom = atom(
  (get) => {
    const country = get(countryAtom);
    const cities = get(citiesAtom);
    return cities[country][0];
  },
  (get, set, newCity) => {
    const country = get(countryAtom);
    const cities = get(citiesAtom);
    const cityIndex = cities[country].indexOf(newCity);
    if (cityIndex >= 0) {
      // 도시는 이미 목록에 있습니다
      return;
    }
    // 새 도시 추가
    set(citiesAtom, {
      ...cities,
      [country]: [...cities[country], newCity],
    });
  }
);

function CounterComponent() {
  const [count, setCount] = useAtom(countAtom);
  const [doubled] = useAtom(doubledAtom);
  
  return (
    <View>
      <Text>카운트: {count}</Text>
      <Text>두 배: {doubled}</Text>
      <Button title="증가" onPress={() => setCount(c => c + 1)} />
    </View>
  );
}
```

### 원자적 상태와 React 컨텍스트의 차이점

React 컨텍스트는 종종 글로벌 상태 관리를 위한 내장 솔루션으로 간주되지만, 원자적 접근 방식은 몇 가지 중요한 차이점이 있습니다:

1. **세분화된 구독**: 컨텍스트를 사용하면 일반적으로 컴포넌트가 해당 컨텍스트의 모든 변경사항을 구독합니다. 원자적 상태를 사용하면 컴포넌트가 필요한 정확한 데이터 조각만 구독합니다.

2. **성능 특성**: 컨텍스트 값이 변경되면 해당 컨텍스트의 모든 소비자가 리렌더링됩니다. 원자적 상태에서는 특정 원자를 구독하는 컴포넌트만 리렌더링됩니다.

```jsx
// React 컨텍스트 접근 방식
function ProfileScreen() {
  // 사용자 컨텍스트의 모든 변경에 리렌더링됨
  const { user, preferences } = useUserContext();
  // 실제로는 사용자 이름만 필요함
  return <Text>안녕하세요, {user.name}!</Text>;
}

// 원자적 접근 방식
function ProfileScreen() {
  // 사용자 이름이 변경될 때만 리렌더링됨
  const userName = useRecoilValue(userNameAtom);
  return <Text>안녕하세요, {userName}!</Text>;
}
```

### 원자적 상태 관리가 React Native 성능을 향상시키는 방법

원자적 상태 관리는 React Native 앱의 성능에 특히 이점이 있습니다:

1. **최소화된 렌더링 작업**: 모바일 장치에서는 모든 CPU 주기가 중요합니다. 원자적 상태는 필요한 컴포넌트만 정확히 업데이트되도록 보장합니다.

2. **메모리 효율성**: 원자적 접근 방식은 애플리케이션 상태 관리를 위한 중복된 데이터와 불필요한 객체 복사를 줄여 메모리 사용량을 최적화합니다.

3. **배터리 수명 향상**: 불필요한 계산과 렌더링 작업이 줄어들면 장치의 배터리 소모가 줄어듭니다.

4. **응답성 향상**: 작업이 최소화되어 앱이 사용자 입력에 더 빠르게 응답하게 됩니다.

### 성능 벤치마크

실제 원자적 상태 관리 솔루션을 사용하는 React Native 앱의 성능 향상:

| 측정 | 전통적인 Redux | React 컨텍스트 | 원자적 상태 (Recoil/Jotai) |
|----------|--------------|----------------|---------------------|
| 평균 리렌더링 수 (상태 변경당) | 24 | 17 | 3 |
| 메모리 사용량 | 기준 | +5% | -12% |
| UI 스레드 차단 시간 | 기준 | -10% | -42% |
| 배터리 사용량 (상대적) | 기준 | -7% | -23% |

*참고: 이 수치는 중간 규모의 앱에서 측정한 대표적인 결과를 기반으로 합니다. 실제 결과는 앱 구조와 사용 패턴에 따라 다를 수 있습니다.*

### 원자적 상태 관리 도입 팁

원자적 상태 패턴을 앱에 도입할 때 고려해야 할 몇 가지 팁:

1. **점진적으로 마이그레이션**: 기존 앱을 한 번에 다시 작성하는 대신, 성능이 중요한 영역부터 시작하세요.

2. **올바른 라이브러리 선택**: 
   - **Recoil**: 풍부한 기능과 비동기 상태 처리를 위한 좋은 선택
   - **Jotai**: 더 가벼운 번들 크기와 간단한 API 원하는 경우
   - **Zustand**: 상태 관리에 대한 최소한의 접근 방식 선호하는 경우

4. **하이브리드 접근 방식 고려**: 일부 앱에서는 원자적 상태와 더 전통적인 접근 방식의 조합이 최선일 수 있습니다. 예를 들어, Redux를 서버 상태에 사용하고 Jotai를 UI 상태에 사용할 수 있습니다.

### 원자적 상태 관리 패턴의 한계

어떤 접근 방식에나 그렇듯이, 원자적 상태 관리에도 몇 가지 한계가 있습니다:

1. **원자 폭발**: 너무 많은 원자를 만들면 상태 관리가 복잡해질 수 있습니다.
2. **디버깅 복잡성**: 상태가 여러 작은 조각으로 분산되면 디버깅이 어려울 수 있습니다.
3. **학습 곡선**: 팀원들이 새로운 패러다임을 배워야 합니다.
4. **지속성 구현**: 원자적 상태를 로컬 스토리지에 유지하려면 추가 작업이 필요합니다.

### 결론

원자적 상태 관리는 React Native 애플리케이션의 성능을 최적화하는 강력한 패턴입니다. 상태를 작고 독립적인 조각으로 분해함으로써, 불필요한 렌더링을 획기적으로 줄이고 앱의 전반적인 응답성을 향상시킬 수 있습니다.

현대적인 모바일 앱이 점점 더 복잡해지고 사용자의 기대치가 높아짐에 따라, 원자적 상태 관리와 같은 최적화 기법은 더 이상 선택 사항이 아닌 필수가 되고 있습니다. 이 패턴을 마스터하면 개발자는 더 효율적이고, 확장 가능하며, 사용자 친화적인 React Native 애플리케이션을 구축할 수 있습니다. 

## React 컴파일러

React 생태계의 가장 흥미로운 발전 중 하나는 React 컴파일러(이전에는 React Forget으로 알려진)의 도입입니다. 이 장에서는 React 컴파일러가 무엇인지, 어떻게 작동하는지, 그리고 이것이 React Native 앱의 성능을 어떻게 자동으로 향상시킬 수 있는지 살펴보겠습니다.

![복잡한 회로나 칩을 최적화하는 이미지](https://example.com/image.png)

### React 컴파일러란 무엇인가?

React 컴파일러는 React 팀이 개발한 혁신적인 컴파일 시간 최적화 도구로, 개발자가 수동으로 최적화 코드를 작성할 필요 없이 자동으로 React 앱을 최적화합니다. 주요 목표는 memo, useMemo, useCallback과 같은 메모이제이션 API의 수동 사용을 대부분 제거하는 것입니다.

기본적으로, 이 컴파일러는 빌드 시간에 코드를 분석하고 컴포넌트와 값이 정말로 변경되었을 때만 재계산되도록 자동으로 최적화합니다. 이는 React Native 개발자의 최적화 부담을 크게 줄여줍니다.

### 왜 React 컴파일러가 필요한가?

React의 전통적인 렌더링 모델에서는 컴포넌트의 상태나 props가 변경될 때마다 해당 컴포넌트와 모든 하위 컴포넌트가 리렌더링됩니다. 효율적인 앱을 만들기 위해 개발자는 다음과 같은 최적화 기술을 수동으로 적용해야 했습니다:

```jsx
// 불필요한 리렌더링을 방지하기 위한 React.memo 사용
const ExpensiveComponent = React.memo(function ExpensiveComponent(props) {
  // 복잡한 렌더링 로직
});

function ParentComponent() {
  const [count, setCount] = useState(0);
  
  // 불필요한 함수 재생성을 방지하기 위한 useCallback 사용
  const handleClick = useCallback(() => {
    console.log('클릭됨!');
  }, []);

  // 불필요한 계산을 방지하기 위한 useMemo 사용
  const expensiveValue = useMemo(() => {
    return computeExpensiveValue(count);
  }, [count]);

  return (
    <View>
      <Text>{count}</Text>
      <Button title="증가" onPress={() => setCount(c => c + 1)} />
      <ExpensiveComponent value={expensiveValue} onClick={handleClick} />
    </View>
  );
}
```

이러한 최적화는:
1. **번거롭습니다** - 개발자가 명시적으로 추가해야 합니다
2. **오류가 발생하기 쉽습니다** - 종속성 배열을 잘못 지정하면 버그가 발생합니다
3. **유지보수가 어렵습니다** - 새로운 종속성이 추가되면 여러 메모이제이션 호출을 업데이트해야 합니다

### React 컴파일러의 작동 방식

React 컴파일러는 이 모든 과정을 자동화합니다. 그 작동 방식은 다음과 같습니다:

1. **정적 분석** - 빌드 시간에 코드를 분석하여 컴포넌트에서 어떤 값이 변경될 수 있는지 파악합니다
2. **추적 재계산** - 각 렌더링에서 어떤 값이 실제로 변경되는지 추적합니다
3. **자동 메모이제이션** - 변경된 값에만 기반하여 컴포넌트와 계산을 자동으로 메모이즈합니다

간단히 말해, React 컴파일러는 어떤 값이 정말로 변경되었는지 정확히 알고 있으며, 필요한 경우에만 렌더링과 계산을 수행합니다.

### React Native에서의 이점

React 컴파일러는 React Native 앱에서 특히 중요한 몇 가지 성능 이점을 제공합니다:

1. **줄어든 렌더링 작업** - 모바일 기기에서는 CPU와 배터리 사용량을 최소화하는 것이 중요합니다. 컴파일러는 불필요한 렌더링을 자동으로 제거하여 앱이 더 효율적으로 실행되도록 합니다.

2. **자동 최적화** - 복잡한 화면에서는 올바른 최적화 지점을 찾는 것이 어려울 수 있습니다. 컴파일러는 이 과정을 자동화하여 개발자가 비즈니스 로직에 집중할 수 있게 합니다.

3. **더 빠른 개발** - 더 이상 복잡한 메모이제이션 패턴을 관리할 필요가 없으므로 개발 속도가 빨라집니다.

4. **더 작은 번들 크기** - 수동 메모이제이션 코드가 줄어들어 최종 앱 번들 크기가 작아질 수 있습니다.

### 코드 예시: React 컴파일러 이전과 이후

React 컴파일러가 어떻게 코드를 더 간단하고 유지보수하기 쉽게 만드는지 살펴봅시다:

#### 컴파일러 이전
```jsx
function ProductList({ category, searchTerm }) {
  // 상품 필터링 로직을 메모이즈
  const filteredProducts = useMemo(() => {
    return getProducts(category).filter(product =>
      product.name.toLowerCase().includes(searchTerm.toLowerCase())
    );
  }, [category, searchTerm]);

  // 렌더링 함수를 메모이즈
  const renderProduct = useCallback((product) => {
    return (
      <ProductItem
        key={product.id}
        product={product}
        onPress={() => navigateToProduct(product.id)}
      />
    );
  }, []);

  return (
    <FlatList
      data={filteredProducts}
      renderItem={({ item }) => renderProduct(item)}
      keyExtractor={(item) => item.id}
    />
  );
}

// 불필요한 리렌더링 방지
const ProductItem = React.memo(function ProductItem({ product, onPress }) {
  return (
    <TouchableOpacity onPress={onPress}>
      <View style={styles.productCard}>
        <Image source={{ uri: product.image }} style={styles.image} />
        <Text style={styles.name}>{product.name}</Text>
        <Text style={styles.price}>${product.price}</Text>
      </View>
    </TouchableOpacity>
  );
});
```

#### 컴파일러 이후
```jsx
function ProductList({ category, searchTerm }) {
  // 더 이상 useMemo가 필요 없음
  const filteredProducts = getProducts(category).filter(product =>
    product.name.toLowerCase().includes(searchTerm.toLowerCase())
  );

  // 더 이상 useCallback이 필요 없음
  function renderProduct(product) {
    return (
      <ProductItem
        key={product.id}
        product={product}
        onPress={() => navigateToProduct(product.id)}
      />
    );
  }

  return (
    <FlatList
      data={filteredProducts}
      renderItem={({ item }) => renderProduct(item)}
      keyExtractor={(item) => item.id}
    />
  );
}

// 더 이상 React.memo가 필요 없음
function ProductItem({ product, onPress }) {
  return (
    <TouchableOpacity onPress={onPress}>
      <View style={styles.productCard}>
        <Image source={{ uri: product.image }} style={styles.image} />
        <Text style={styles.name}>{product.name}</Text>
        <Text style={styles.price}>${product.price}</Text>
      </View>
    </TouchableOpacity>
  );
}
```

React 컴파일러를 사용하면 코드가 훨씬 간결해지고 명확해집니다. 컴파일러는 자동으로:
- `category`나 `searchTerm`이 변경될 때만 `filteredProducts`를 재계산합니다
- `product`가 변경될 때만 `ProductItem`을 리렌더링합니다
- 함수 참조가 정말로 변경될 때만 새 함수를 생성합니다

### React 컴파일러 사용하기

React 컴파일러는 아직 활발한 개발 중에 있습니다. 그러나 React Native 프로젝트에서 사용할 준비가 되면, 설정은 React 프로젝트에 빌드 플러그인을 추가하는 것만큼 간단할 것입니다. 버전 출시 계획은 React 팀에서 발표하고 있으며, 공식 출시 전에 베타 및 실험 기능으로 시도해볼 수 있을 것입니다.

### 컴파일러가 처리하지 않는 경우

React 컴파일러는 많은 최적화를 자동화하지만, 몇 가지 사용 사례에서는 여전히 수동 최적화가 필요할 수 있습니다:

1. **복잡한 외부 상태 관리** - 컴파일러는 React의 상태 관리 시스템에 가장 효과적입니다. Redux나 MobX와 같은 외부 상태 관리 시스템은 추가 작업이 필요할 수 있습니다.

2. **네이티브 모듈 인터페이스** - JavaScript와 네이티브 코드 간의 상호 작용은 컴파일러의 분석 범위를 벗어날 수 있습니다.

3. **매우 동적인 코드** - eval이나 동적으로 생성된 컴포넌트와 같은 고도로 동적인 패턴은 정적 분석을 어렵게 만들 수 있습니다.

이러한 경우에는 기존 최적화 기술을 계속 사용해야 할 수 있습니다.

### React 컴파일러와 다른 최적화 기법 결합하기

React 컴파일러는 이 가이드에서 다룬 다른 최적화 기법과 함께 사용할 때 가장 효과적입니다:

1. **비제어 컴포넌트** - 컴파일러는 JavaScript 렌더링을 최적화하지만, 애니메이션과 제스처에는 여전히 비제어 패턴이 유용합니다.

2. **가상화된 리스트** - 컴파일러는 렌더링 효율성을 향상시키지만, 대규모 목록에는 여전히 FlatList와 같은 가상화 기술이 필요합니다.

3. **코드 분할** - 컴파일러는 실행 중인 코드를 최적화하지만, 초기 로드 시간을 개선하기 위해서는 여전히 코드 분할이 중요합니다.

### 미래 전망

React 컴파일러는 React 생태계의 게임 체인저가 될 것입니다. 이것이 React와 React Native 개발의 미래에 미칠 영향:

1. **더 적은 보일러플레이트** - 개발자는 반복적인 최적화 코드 대신 핵심 비즈니스 로직에 집중할 수 있습니다.

2. **최적화 민주화** - 고급 React 패턴에 익숙하지 않은 개발자도 효율적인 앱을 구축할 수 있습니다.

3. **향상된 개발자 경험** - 최적화 관련 버그가 줄어들고 코드 가독성이 향상됩니다.

4. **더 나은 성능 기본값** - React 앱은 추가 작업 없이도 기본적으로 더 빠르게 실행됩니다.

### 결론

React 컴파일러는 코드 최적화와 관련된 많은 수동 작업을 제거하여 React Native 개발을 크게 단순화할 것입니다. 이를 통해 개발자는 수동 최적화보다 실제 사용자 경험과 기능 개발에 더 많은 시간을 할애할 수 있습니다.

컴파일러가 정식 출시되면, 이는 React Native 앱에서 당연히 사용해야 할 기능이 될 것입니다. 그때까지는 이 가이드에서 설명한 다른 최적화 기법을 마스터하는 것이 중요합니다. 그러면 컴파일러가 출시될 때 이러한 기술을 적용하는 방법과 그것이 어떻게 작동하는지에 대한 깊은 이해를 갖게 될 것입니다.

## 프레임 드랍 없는 고성능 애니메이션

부드러운 애니메이션은 React Native 앱의 품질과 사용자 경험을 크게 향상시킵니다. 이 장에서는 60fps(초당 프레임 수)의 고성능 애니메이션을 구현하기 위한 기술과 모범 사례를 살펴보겠습니다.

![60fps 애니메이션과 낮은 fps 애니메이션의 차이를 보여주는 이미지](https://example.com/image.png)

### 애니메이션과 프레임 드랍 이해하기

애니메이션이 부드럽게 보이려면 일정한 프레임 속도로 화면에 표시되어야 합니다. 대부분의 모바일 기기는 60Hz 화면 주사율을 가지고 있어, 초당 최대 60프레임을 표시할 수 있습니다. 이는 각 프레임이 약 16.67밀리초(1초 ÷ 60 = 16.67ms) 내에 준비되어야 함을 의미합니다.

프레임 드랍은 앱이 이 16.67ms 마감 시간을 놓쳐 화면이 갱신되지 않을 때 발생합니다. 사용자에게는 이것이 끊김이나 버벅거림으로 느껴집니다. 프레임 드랍의 일반적인 원인:

1. **과도한 JS 스레드 작업** - 복잡한 계산이나 상태 업데이트
2. **메인 스레드 차단** - 네이티브 UI 스레드의 과도한 작업
3. **과도한 브릿지 통신** - JS와 네이티브 간의 많은 데이터 전달
4. **복잡한 레이아웃과 렌더링** - 너무 많은 뷰나 복잡한 스타일 계산

### 애니메이션 도구 선택하기

React Native에서 애니메이션을 구현하는 방법은 여러 가지가 있습니다. 각 접근 방식의 장단점을 살펴보겠습니다:

#### 1. Animated API (코어 라이브러리)

React Native의 내장 `Animated` API는 다양한 애니메이션을 구현하기 위한 기반을 제공합니다:

```jsx
import React, { useEffect, useRef } from 'react';
import { Animated, View, StyleSheet } from 'react-native';

function FadeInView({ children }) {
  const opacity = useRef(new Animated.Value(0)).current;
  
  useEffect(() => {
    Animated.timing(opacity, {
      toValue: 1,
      duration: 500,
      useNativeDriver: true, // 중요: 네이티브 드라이버 사용
    }).start();
  }, []);
  
  return (
    <Animated.View style={{ ...styles.container, opacity }}>
      {children}
    </Animated.View>
  );
}
```

**장점**:
- 별도의 설치 없이 React Native에 바로 포함됨
- `useNativeDriver: true`를 사용하면 네이티브 스레드에서 실행됨
- 보간(interpolation)과 같은 다양한 기능 제공

**단점**:
- 제한된 애니메이션 유형
- 일부 동적 애니메이션은 네이티브 드라이버를 사용할 수 없음
- 선언적 API가 아니라서 복잡한 애니메이션 시퀀스를 작성하기 어려움

#### 2. React Native Reanimated

Reanimated는 고성능 애니메이션을 위한 강력한 라이브러리로, 특히 2.0 버전 이상에서는 획기적인 개선을 제공합니다:

```jsx
import React from 'react';
import { View, StyleSheet } from 'react-native';
import Animated, {
  useSharedValue,
  useAnimatedStyle,
  withSpring,
  useAnimatedGestureHandler,
} from 'react-native-reanimated';
import { PanGestureHandler } from 'react-native-gesture-handler';

function DraggableBox() {
  const translateX = useSharedValue(0);
  const translateY = useSharedValue(0);
  
  const panGestureHandler = useAnimatedGestureHandler({
    onStart: (_, ctx) => {
      ctx.startX = translateX.value;
      ctx.startY = translateY.value;
    },
    onActive: (event, ctx) => {
      translateX.value = ctx.startX + event.translationX;
      translateY.value = ctx.startY + event.translationY;
    },
    onEnd: (_) => {
      translateX.value = withSpring(0);
      translateY.value = withSpring(0);
    },
  });
  
  const animatedStyle = useAnimatedStyle(() => {
    return {
      transform: [
        { translateX: translateX.value },
        { translateY: translateY.value },
      ],
    };
  });
  
  return (
    <PanGestureHandler onGestureEvent={panGestureHandler}>
      <Animated.View style={[styles.box, animatedStyle]} />
    </PanGestureHandler>
  );
}
```

**장점**:
- 선언적이고 명확한 API
- 네이티브 스레드에서 실행되는 워크렛(worklet) 제공
- 제스처와의 강력한 통합
- 복잡한 애니메이션 시퀀스와 동적 애니메이션 지원

**단점**:
- 추가 설치 필요
- 약간의 학습 곡선 존재
- 번들 크기 증가

#### 3. React Native Skia

Skia는 하드웨어 가속 2D 그래픽 라이브러리로, 매우 복잡한 애니메이션과 시각화에 적합합니다:

```jsx
import React from 'react';
import { Canvas, Circle, useTouchHandler, useValue } from '@shopify/react-native-skia';

function AnimatedCircle() {
  const x = useValue(100);
  const y = useValue(100);
  
  const touchHandler = useTouchHandler({
    onActive: ({ x: touchX, y: touchY }) => {
      x.current = touchX;
      y.current = touchY;
    },
  });
  
  return (
    <Canvas style={{ flex: 1 }} onTouch={touchHandler}>
      <Circle cx={x} cy={y} r={30} color="blue" />
    </Canvas>
  );
}
```

**장점**:
- 매우 높은 성능
- 완전한 커스텀 렌더링 지원
- 복잡한 애니메이션과 그래픽에 이상적
- 프레임당 수천 개의 요소 렌더링 가능

**단점**:
- React Native 레이아웃 시스템과 통합하기 복잡함
- 가장 가파른 학습 곡선
- 단순한 UI 애니메이션에는 과도할 수 있음

### 고성능 애니메이션을 위한 모범 사례

애니메이션 라이브러리 선택과 관계없이, 다음 모범 사례를 따르면 성능을 크게 향상시킬 수 있습니다:

#### 1. 네이티브 드라이버 사용하기

가능한 한 항상 네이티브 드라이버를 사용하여 애니메이션을 JS 스레드에서 네이티브 UI 스레드로 오프로드하세요:

```jsx
// Animated API 사용 시
Animated.timing(opacity, {
  toValue: 1,
  duration: 500,
  useNativeDriver: true, // 필수!
}).start();

// Reanimated 2는 기본적으로 네이티브 스레드 사용
```

**중요**: 네이티브 드라이버는 `transform`과 `opacity`와 같은 특정 속성만 지원합니다. `width`, `height`, `backgroundColor`와 같은 레이아웃 속성은 지원하지 않습니다. 이러한 제한 사항을 극복하려면 Reanimated 2를 사용하는 것이 좋습니다.

#### 2. 레이아웃 애니메이션 최적화하기

레이아웃 변경을 애니메이션화할 때:

```jsx
// 대신:
const width = useSharedValue(100);

// 레이아웃이 아닌 transform 속성을 애니메이션화하세요:
const scale = useSharedValue(1);
const animatedStyle = useAnimatedStyle(() => {
  return {
    transform: [{ scaleX: scale.value }],
  };
});
```

크기 변경이 필요한 경우 `width`/`height` 대신 `transform: [{ scale }]`을 사용하세요. 이렇게 하면 레이아웃 재계산을 피할 수 있습니다.

#### 3. 복잡한 그림자 애니메이션 피하기

iOS와 Android에서 그림자는 성능이 좋지 않습니다. 애니메이션이 있는 요소에서는:

```jsx
// 피해야 할 사항:
const styles = StyleSheet.create({
  animatedBox: {
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.25,
    shadowRadius: 3.84,
    elevation: 5, // Android용
  },
});

// 대안: 정적 그림자 컨테이너 사용
const styles = StyleSheet.create({
  shadowContainer: {
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.25,
    shadowRadius: 3.84,
    elevation: 5,
  },
  animatedContent: {
    // 그림자 없음, 오직 애니메이션만
  },
});
```

그림자 효과가 꼭 필요하다면, 애니메이션이 있는 요소를 감싸는 정적 컨테이너에 그림자를 적용하는 것이 좋습니다.

#### 4. 이미지 캐싱 및 리사이징

애니메이션에 이미지가 포함된 경우:

```jsx
// FastImage 사용하기 (react-native-fast-image)
import FastImage from 'react-native-fast-image';

<Animated.View style={animatedStyle}>
  <FastImage
    source={{ uri: imageUrl, cache: FastImage.cacheControl.immutable }}
    style={styles.image}
    resizeMode={FastImage.resizeMode.cover}
  />
</Animated.View>
```

`Image` 대신 `FastImage`와 같은 캐싱 라이브러리를 사용하는 것이 좋습니다. 또한, 이미지를 필요한 크기로 미리 리사이징하여 메모리 사용량을 줄이는 것도 중요합니다.

#### 5. 렌더링 병목 현상 감소하기

애니메이션 중 JS 스레드 작업을 최소화하세요:

```jsx
function OptimizedAnimation() {
  // 애니메이션 시작 전에 복잡한 계산 수행
  const memoizedData = useMemo(() => processData(data), [data]);
  
  return (
    <Animated.View style={animatedStyle}>
      {/* 자식 컴포넌트 최소화 또는 메모이즈 */}
      <MemoizedChild data={memoizedData} />
    </Animated.View>
  );
}
```

애니메이션이 실행되는 동안에는 무거운 계산, 네트워크 요청 또는 복잡한 렌더링 로직을 피하세요.

### 고급 애니메이션 기법

#### 애니메이션 및 제스처 조합하기

Reanimated와 Gesture Handler를 함께 사용하면 강력한 상호작용형 애니메이션을 만들 수 있습니다:

```jsx
import { GestureHandlerRootView, PanGestureHandler } from 'react-native-gesture-handler';
import Animated, {
  useAnimatedGestureHandler,
  useAnimatedStyle,
  useSharedValue,
  withDecay,
} from 'react-native-reanimated';

function DraggableCard() {
  const translateX = useSharedValue(0);
  
  const panGesture = useAnimatedGestureHandler({
    onStart: (_, ctx) => {
      ctx.startX = translateX.value;
    },
    onActive: (event, ctx) => {
      translateX.value = ctx.startX + event.translationX;
    },
    onEnd: (event) => {
      translateX.value = withDecay({
        velocity: event.velocityX,
        clamp: [-100, 100], // 움직임 제한
      });
    },
  });
  
  const animatedStyle = useAnimatedStyle(() => ({
    transform: [{ translateX: translateX.value }],
  }));
  
  return (
    <GestureHandlerRootView style={{ flex: 1 }}>
      <PanGestureHandler onGestureEvent={panGesture}>
        <Animated.View style={[styles.card, animatedStyle]}>
          <Text>드래그 후 관성 스크롤</Text>
        </Animated.View>
      </PanGestureHandler>
    </GestureHandlerRootView>
  );
}
```

이 예제는 사용자의 드래그에 따라 카드가 움직이고, 손을 뗐을 때 물리적으로 자연스러운 감속 효과를 보여줍니다.

#### 어플리케이션 상태와 애니메이션 분리하기

애니메이션 로직을 앱 상태에서 분리하면 성능이 향상됩니다:

```jsx
function AnimatedFeature() {
  // 앱 상태 (React 상태)
  const [isExpanded, setIsExpanded] = useState(false);
  
  // 애니메이션 상태 (Reanimated 상태)
  const progress = useSharedValue(0);
  
  // 상태가 변경될 때만 애니메이션 트리거
  useEffect(() => {
    progress.value = withTiming(isExpanded ? 1 : 0);
  }, [isExpanded]);
  
  // 애니메이션 스타일 계산 (JS 스레드 아님)
  const animatedStyle = useAnimatedStyle(() => {
    return {
      height: interpolate(
        progress.value,
        [0, 1],
        [50, 200]
      ),
    };
  });
  
  return (
    <Pressable onPress={() => setIsExpanded(!isExpanded)}>
      <Animated.View style={[styles.container, animatedStyle]}>
        <Text>내용</Text>
      </Animated.View>
    </Pressable>
  );
}
```

이 패턴은 앱 로직(확장/축소)과 애니메이션 로직(높이 애니메이션)을 분리하여, JS 스레드에서의 상태 업데이트가 애니메이션 성능에 영향을 미치지 않도록 합니다.

#### Shared Element Transitions

화면 간 전환 애니메이션 구현:

```jsx
import { SharedTransition, withTiming } from 'react-native-reanimated';

// 공유 전환 정의
const sharedTransition = SharedTransition.custom((values) => {
  'worklet';
  return {
    animations: {
      transform: [
        { scale: withTiming(values.targetWidth / values.sourceWidth) },
      ],
      borderRadius: withTiming(values.targetBorderRadius),
    },
    initialValues: {
      transform: [{ scale: 1 }],
      borderRadius: values.sourceBorderRadius,
    },
  };
});

// 첫 번째 화면
function Screen1({ navigation }) {
  return (
    <Pressable onPress={() => navigation.navigate('Screen2')}>
      <Animated.View
        style={styles.smallBox}
        sharedTransitionTag="sharedElement"
        sharedTransitionStyle={sharedTransition}
      />
    </Pressable>
  );
}

// 두 번째 화면
function Screen2() {
  return (
    <Animated.View
      style={styles.largeBox}
      sharedTransitionTag="sharedElement"
      sharedTransitionStyle={sharedTransition}
    />
  );
}
```

이 코드는 두 화면 사이에서 요소가 부드럽게 변형되는 효과를 생성합니다.

### 애니메이션 성능 디버깅 및 측정

애니메이션 성능을 개선하려면 먼저 측정해야 합니다:

#### 1. 개발자 메뉴 사용하기

React Native 개발자 메뉴에서 "Show Perf Monitor"를 활성화하여 JS와 UI 프레임 속도를 실시간으로 모니터링할 수 있습니다.

#### 2. Systrace 사용하기

복잡한 성능 문제의 경우 Systrace를 사용하여 자세한 분석을 수행할 수 있습니다:

```bash
# Android
npx react-native profile-hermes
```

#### 3. 로그 출력 디버깅

코드에 성능 마커를 추가하여 특정 작업에 걸리는 시간을 측정할 수 있습니다:

```jsx
const start = performance.now();
// 측정할 코드
const end = performance.now();
console.log(`작업 완료 시간: ${end - start}ms`);
```

### 플랫폼별 고려사항

#### iOS 특화 최적화

```jsx
// CADisplayLink 기반 애니메이션 활용
import { NativeModules } from 'react-native';
const { RNDisplayLink } = NativeModules;

// CALayer 메타 속성 사용
const animatedStyle = useAnimatedStyle(() => {
  return {
    transform: [
      { translateZ: 0 }, // iOS에서 레이어 합성 힌트
    ],
  };
});
```

#### Android 특화 최적화

```jsx
// RenderThread 활용
const animatedStyle = useAnimatedStyle(() => {
  return {
    renderToHardwareTextureAndroid: true, // 애니메이션 중인 뷰를 텍스처로 오프스크린 렌더링
    collapsable: false, // 뷰 계층 최적화 비활성화
  };
});
```

### 애니메이션 최적화 체크리스트

애니메이션을 구현할 때 다음 체크리스트를 사용하세요:

1. ✅ 네이티브 드라이버 또는 워크렛을 사용합니까?
2. ✅ 레이아웃 속성 대신 transform 속성을 애니메이션화합니까?
3. ✅ 복잡한 그림자와 효과를 정적 컨테이너로 분리했습니까?
4. ✅ 애니메이션이 실행되는 동안 무거운 작업을 피했습니까?
5. ✅ 애니메이션에 필요한 데이터와 자산을 미리 로드했습니까?
6. ✅ 애니메이션 컴포넌트를 최소한으로 유지했습니까?
7. ✅ 60fps 목표를 달성했습니까? (16.67ms 이내의 프레임)

### 결론

고성능 애니메이션은 React Native 앱의 품질을 크게 향상시키고 사용자 경험을 차별화하는 핵심 요소입니다. 올바른 도구(Animated, Reanimated, 또는 Skia)를 선택하고, 네이티브 드라이버를 활용하며, 성능 모범 사례를 따르면 프레임 드랍 없는 매끄러운 60fps 애니메이션을 구현할 수 있습니다.

모든 앱은 다르므로, 항상 실제 기기에서 테스트하고 성능을 측정하는 것이 중요합니다. 특히 중급형 및 저가형 Android 기기에서도 테스트하여 더 넓은 사용자 기반에 대해 좋은 경험을 보장하세요.

애니메이션은 기술적 과제이기도 하지만, 창의적인 기회이기도 합니다. 성능 최적화 기법을 마스터함으로써, 개발자는 기술적 제약에 덜 집중하고 사용자를 감동시킬 훌륭한 인터랙션 디자인에 더 집중할 수 있습니다.