# HTML link 요소의 rel 속성 값 preconnect, preload, prefetch에 대해 설명해주세요.

## HTML `<link>` 요소 및 `rel` 속성

### `<link>` 요소의 역할

HTML에서 `<link>` 요소는 현재 문서와 외부 리소스 사이의 연결 관계를 지정하는데 사용된다. 주로 문서의 `<head>` 내에 위치하며, 외부 스타일시트를 연결하거나 파비콘을 지정하는 용도로 사용된다.

### `rel` 속성

`rel` 속성은 `<link>`가 가리키는 외부 리소스와 현재 HTML 문서 사이에 어떤 관계가 있는지 정의한다. 이 속성값이 브라우저에게 해당 리소스를 어떻게 처리해야 하는지에 대한 명령이나 힌트를 제공한다.

## `Resource Hint`

`리소스 힌트(Resource Hint)`는 브라우저에게 미래에 필요할 리소스에 대해 미리 알려주어, 연결 및 다운로드 과정을 브라우저의 유휴 시간(Idle Time)에 미리 시작하도록 유도하는 `<link>`의 `rel` 속성값들의 집합이다. 이는 브라우저의 기본 리소스 로드 결정에 영향을 미치는 권고 사항이며, 브라우저는 네트워크 상황이나 자체적인 판단에 따라 힌트를 무시할 수도 있다.

주요 리소스로는 `Preconnect`, `preload`, `prefetch`, `dns-prefetch` 등이 있다.

### `preconnect`

`preconnect`는 브라우저가 특정 `origin`에 대한 네트워크 연결을 미리 설정하도록 지시하는 속성이다. DNS 조회, TCP 연결, TLS 핸드셰이크 과정을 미리 완료하여 리소스 로드 지연을 줄일 수 있다. 리소스 자체를 다운로드 하는 것이 아닌, 리소스를 가져와야하는 외부 도메인이 있을 때 연결을 미리 준비하는 것이다.

```html
<link
  rel="preconnect"
  href="https://external-resource.com"
  crossorigin="anonymous"
/>
```

> `dns-prefetch`
>
> 다른 도메인 리소스에 접근하기 위해 필수적인 DNS 조회(Lookup) 시간을 미리 수행하여 대기 시간을 줄이는 속성이다. 네트워크 통신 과정 중 가장 초기에 발생하는 도메인 이름을 IP 주소롤 변환하는 작업만 브라우저 유휴 시간에 미치 처리한다. 페이지에서 많은 수의 외부 도메인을 사용해야 하거나, `preconnect`를 지원하지 않는 구형 브라우저 환경에서 연결 최적화가 필요할 때 사용된다.
>
> - `preconnect`와의 차이점  
>   `preconnect`는 DNS 조회는 물론 TCP 연결 및 TLS 협상까지 완료하는 반면, `dns-prefetch`는 오직 DNS 조회만 수행하기 때문에 네트워크 오버헤드가 더 적다.

### `preload`

`preload`는 현재 페이지에서 반드시 필요하며 곧 사용될 특정 리소스를 다른 리소스보다 높은 우선순위로 미리 가져오도록 브라우저에 지시하는 속성이다. 리소스가 실제로 사용되기 전에 미리 다운로드를 완료하여 렌더링 블로킹을 방지하고 로드 시간을 단축한다. 웹 폰트, 히어로 이미지, 현재 페이지에 필수적인 CSS/JS 파일 등 현재 화면에 즉시 필요한 리소스에 사용된다.

> `히어로 이미지(Hero Image)`
>
> 웹사이트 디자인에서 가장 중요하고 눈에 띄는 대형 이미지 또는 비디오

> `FOUT` 현상을 `preload`로 방지할 수 있다.
> `FOUT(Flash of Unstyled Text)`
>
> 웹페이지가 처음 로딩될 때 웹 폰트가 아직 다운로드되지 않은 상태에서 기본 폰트로 먼저 텍스트가 표시되었다가, 웹 폰트가 로딩되면 스타일이 바뀌면서 글자가 '번쩍' 바뀌는 현상을 말한다.

> `preload`를 사용할 경우 `as` 속성을 통해 리소스 종류를 명시해야 한다.

```html
<link
  rel="preload"
  href="/fonts/my-font.woff2"
  as="font"
  crossorigin="anonymous"
/>
```

### `prefetch`

`prefetch`는 향후에 필요할 가능성이 있는 리소스를 브라우저의 유휴 시간에 낮은 우선순위로 미리 가져오도록 지시하는 속성이다. 현재 화면에 즉시 필요하지는 않지만, 다음 탐색에서 사용될 가능성이 높은 리소스를 미리 도르하여 캐시에 저장한다. 사용자가 다음 화면으로 이동할 가능성이 높을 경우, 다음 화면에 필요한 css 리소스를 미리 준비하는 방식으로 사용할 수 있다. (e.g. 장바구니 페이지에서 결제 페이지의 CSS/JS를 미리 로드)

```html
<link rel="prefetch" href="/next-page.css" as="style" />
```

## 사용 예시

### HTML 예시

```html
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <title>리소스 힌트 예시</title>

    <!-- 렌더링을 차단하는 CSS를 가능한 빨리 확보하기 위해 HTML 파싱 초기 단계에서 다운로드를 시작하도록 하는 preload -->
    <link rel="preload" href="/styles/main.css" as="style" />

    <!-- 웹폰트를 미리 다운로드하여 FOUT/FOIT 같은 폰트 로딩 지연 문제를 줄이고CRP 초기 단계에서 폰트를 확보하여 텍스트 렌더링을 빠르게 하기 위해 preload 사용 -->
    <link
      rel="preload"
      href="/fonts/custom-webfont.woff2"
      as="font"
      crossorigin
    />

    <!-- 주요 스크립트를 빠르게 다운로드하여 브라우저 파싱 중에 리소스 준비 시간을 단축하기 위해 preload 사용 -->
    <link rel="preload" href="/scripts/main.js" as="script" />

    <!-- 실제 스타일을 적용하는 CSS. preload한 리소스를 즉시 사용할 수 있도록 연결 -->
    <link rel="stylesheet" href="/styles/main.css" />

    <!-- 다음 페이지에서 필요할 가능성이 높은 이미지 리소스를 현재 페이지에 영향을 주지 않는 백그라운드에서 미리 다운로드 -->
    <link rel="prefetch" href="/images/next-page-hero.jpg" as="image" />

    <!-- 다음 페이지나 지연 로드될 가능성이 있는 분석 스크립트를 낮은 우선순위로 미리 준비해두기 위한 prefetch -->
    <link rel="prefetch" href="/scripts/analytics.js" as="script" />

    <!-- 외부 API 서버에 요청하기 위해 필요한 DNS 조회를 미리 수행하여 실제 네트워크 요청 시 지연(DNS lookup time)을 줄이기 위한 dns-prefetch -->
    <link rel="dns-prefetch" href="https://api.example.com" />

    <!-- CDN 같은 외부 도메인의 DNS 조회 시간을 줄이기 위해 미리 DNS lookup을 수행 -->
    <link rel="dns-prefetch" href="https://external-cdn.com" />

    <!-- API 서버와 통신하기 위해 DNS + TCP + TLS 핸드셰이크까지 미리 수행하여 실제 요청을 더 빠르게 진행할 수 있도록 하기 위한 preconnect -->
    <link rel="preconnect" href="https://api.example.com" />

    <!-- 구글 외부 리소스(지도 API 등)와의 연결을 
          DNS, TCP, TLS 레벨에서 미리 열어 네트워크 지연을 최소화하기 위한 preconnect
          cross-origin 요청이므로 인증 정보(TLS session 재사용)를 위해 crossorigin 필요 -->
    <link rel="preconnect" href="https://maps.google.com" crossorigin />
  </head>
  <body>
    <h1>리소스 힌트 활용 예시 페이지</h1>
    <p>
      이 페이지는 다양한 리소스 힌트를 사용하여 로딩 성능을 최적화하고 있습니다.
    </p>

    <!-- 메인 스크립트는 렌더링 차단을 피하기 위해 defer 사용 -->
    <script src="/scripts/main.js" defer></script>
  </body>
</html>
```

### React 예시

`react-helmet`을 사용하여 컴포넌트 내부에서 동적으로 `<head>` 태그에 `<link>` 요소를 추가할 수 있다.

```jsx
import { Helmet } from "react-helmet-async";

function MyComponent() {
  return (
    <Helmet>
      {/* 폰트 preload 예시 */}
      <link
        rel="preload"
        href="/fonts/MyFont.woff2"
        as="font"
        crossorigin="anonymous"
      />

      {/* 외부 API preconnect 예시 */}
      <link rel="preconnect" href="https://api.external-service.com" />
    </Helmet>
  );
}
```

### Next.js 예시

Next.js 환경에서는 프레임워크가 제공하는 `Link`, `Image` 컴포넌트를 사용하여 개발자가 직접 신경쓰지 않아도 자동으로 가장 효율적인 `prefetch`나 `preload`를 적용할 수 있다.

- React와 유사하게, Next.js의 내장된 `next/head` 컴포넌트를 사용하여 `<head>`에 리소스 힌트를 직접 추가할 수 있다.

  ```jsx
  import Head from "next/head";

  function MyPage() {
    return (
      <>
        <Head>
          {/* 중요 이미지 preload 예시 */}
          <link rel="preload" href="/images/hero-lg.webp" as="image" />

          {/* 외부 CDN preconnect 예시 */}
          <link rel="preconnect" href="https://cdn.thirdparty.com" />
        </Head>
        {/* ... 나머지 페이지 콘텐츠 ... */}
      </>
    );
  }
  ```

````

- `next/link`를 사용하여 페이지간 이동 링크를 만들면, Next.js는 해당 링크가 화면에 봉리 때(뷰포트에 들어왔을 때) 해당 페이지의 CSS/JS 청크를 자동으로 prefetch한다. 이는 사용자가 링크를 클릭하기 전에 다음 페이지 로드를 시작하여, 페이지 전환 속도를 매우 빠르게 만든다.


  ```jsx
  import Link from 'next/link';

  function Navigation() {
    // 사용자가 이 링크를 스크롤해서 보게 되면,
    // Next.js는 /products 페이지에 필요한 JS 파일을 자동으로 prefetch합니다.
    return (
      <Link href="/products">
        제품 페이지로 이동
      </Link>
    );
  }
````

> Next.js 13 버전 이후에는 `Link` 컴포넌트의 `prefetch` 속성이 기본값으로 `true`이며, `false`로 설정하여 자동 `prefetch`를 비활성화할 수 있다.

- Next.js의 `Image` 컴포넌트는 이미지 최적화와 함께, priority 속성을 제공한다. 히어로 이미지에 `priority` 속성을 추가하면, Next.js는 해당 이미지 파일에 대해 자동으로 `preload` 힌트를 생성하여 가장 먼저 로드되도록 한다.

  ```jsx
  import Image from "next/image";
  import heroImage from "../public/images/hero.jpg";

  function HeroSection() {
    return (
      <Image
        src={heroImage}
        alt="주요 히어로 이미지"
        priority // 이 이미지에 대해 Next.js가 자동으로 preload 힌트 삽입
      />
    );
  }
  ```

## 공식문서 참고

> Stacking context is a **three-dimensional conceptualization of HTML elements** along an imaginary z-axis relative to the user, who is assumed to be facing the viewport or the webpage. The stacking context determines how elements are layered on top of one another along the z-axis (think of it as the "depth" dimension on your screen). Stacking context determines the visual order of how overlapping content is rendered.

`Stacking context`는 사용자가 웹페이지를 바라보고 있다고 가정했을 때, HTML 요소들을 가상의 z축(깊이) 상에서 3차원적으로 개념화한 것입니다. `Stacking context`는 요소들이 화면에서 겹칠 때 어떤 순서로 쌓여 보이는지를 결정합니다.

### 참고자료

- [Preload, prefetch and other resource priorities](https://web.dev/preload-prefetch/)
- [Preloading critical assets](https://developer.mozilla.org/en-US/docs/Web/HTML/Link_types/preload)
- [Resource Hints](https://w3c.github.io/resource-hints/)
  > Priority Hints (우선순위 힌트)
- [Priority Hints](https://web.dev/priority-hints/)
  > Critical Rendering Path (CRP)
- [Critical Rendering Path](https://web.dev/critical-rendering-path/)
