# CSS 전처리기(CSS Preprocessor)란 무엇인가요?

## CSS 전처리기(CSS Preprocessor)
CSS 전처리기(CSS Preprocessor)란 CSS가 기본적으로 제공하지 않는 기능(변수, 중첩, 로직, 모듈화 등)을 추가하여 더 구조적이고 재사용 가능한 스타일 코드를 작성할 수 있도록 도와주는 도구이다. 전처리기로 작성된 코드는 브라우저가 직접 해석하지 않기 때문에, 빌드 도구에 의해 일반 CSS로 컴파일된 후 브라우저에 전달된다. 대표적인 전처리긷에는 Sass / SCSS, Less, Stylus 가 있다.

### 필요성
일반 CSS의의 경우 한계점이 존재한다.
- 반복되는 스타일 재사용 불가 : 변수, 함수가 없기 때문에 같은 패딩, 색상 등을 여러 번 반복 작성해야한다.
- 스타일 구조화의 어려움 : CSS는 전역 스코프 기반이기 때문에 네임스페이스 관리가 어렵다.
- 조건 처리 및 계산의 어려움 : CSS에서는 기본적인 연산을 처리하는 데에 어려움이 있다.

이러한 문제를 해결하기 위해 프로그래밍 언어적 요소를 도입한다.

### 특징
- 변수(Variables) 지원
  중앙 관리가 가능하여 디자인 시스템 구축에 유리하다.
  ```scss
  $primary: #007bff;

  button {
    background-color: $primary;
  }
  ```

- 중첩(Nesting)
  CSS에서는 불가능한 선택자 중첩이 가능하여 구조적 작성이 가능하다.

  ```scss
  .card {
    padding: 20px;

    .title {
      font-size: 18px;
    }
  }
  ```

- 믹스인(Mixins)과 함수(Functions)
  공통 코드를 함수처럼 재사용한다.
  
  ```scss
  @mixin flex-center {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  ```

- 파일 분리 및 임포트(Modules)
  스타일을 여러 파일로 나눠 유지보수가 용이하다.

- 실행 전 컴파일(BULID 단계 처리)
  전처리기는 JS 번들러(Vite, Webpack)와 토압되어 빌드 타임에 CSS로 변환된 후에 배포된다.

## 추가로 학습하면 좋을 개념
- Zero Runtime CSS
- CSS-in-JS와의 비교












