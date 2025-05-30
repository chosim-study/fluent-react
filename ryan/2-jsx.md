# JSX

## JSX란?

- JSX는 JavaScript XML 혹은 JavaScript eXtension을 뜻함.
- JSX의 목적은 단 하나. 반복, 계산, 인라인 실행 같은 강력한 js 기능을 그대로 유지하면서 리액트 컴포넌트의 코드를 간단하게 작성하고, 읽기 쉽게 만들고, 유지보수하기 좋게 만드는 것이다.

## JSX 동작

JSX는 JSX 전처리기(ex. 바벨에 포함됨)에 의해 다음 세 가지 과정을 거쳐 바닐라 JS 코드로 트랜스파일된다.

1. 토큰화: 문자열을 의미 있는 토큰으로 분해한다.
2. 구문 분석: 토큰을 구문 트리로 변환한다.. 구문 트리는 코드의 구조를 나타내는 자료구조이다.
3. 코드 생성: 구문 트리에서 생성된 추상 구문 트리를 바닐라 JS 코드로 변환한다.

이렇게 전처리기를 통해 생성된 JS 코드는 JS 엔진에 의해 실행될 수 있다.

## JSX 프라그마

JSX 프라그마(pragma)는 JSX 코드를 JavaScript로 변환할 때 어떤 함수를 사용할지 지정하는 설정이다.

기본적으로는 `React.createElement`(예전 버전) 또는 `_jsxs`(최신 버전)가 기본값으로 설정된다.

```jsx
// jsx 프라그마의 시그니처
function pragma(tag, props, ...children)

// jsx 예시
<MyComponent prop="속성값">콘텐츠</MyComponent>

// 변환 예시
React.createElement(MyComponent, { prop: "속성값" }, "콘텐츠");
```

## 장단점

### 1. 장점

- 컴포넌트 기반 아키텍처: JSX는 컴포넌트 기반 아키텍처를 권장하는데 이는 코드를 모듈화하고 유지보수를 보다 쉽게 하는 데 도움이 된다.
- 강력한 타이핑: JSX는 강력한 타이핑을 지원하므로 오류가 발생하기 전 잡아낼 수 있다.
- 향상된 보안: 트랜스파일 시 sanitization이 수행되어 보안 상 안전

### 2. 단점

- 트랜스파일 필요: 트랜스파일을 위한 개발 도구가 추가로 필요해진다.
- 관심사 혼합: JS와 HTML의 관심사의 경계가 허물어진다. 일부는 이것이 표현과 논리(로직)를 구분하기 어렵게 한다고 주장함.
