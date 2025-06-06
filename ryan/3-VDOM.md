## 질문

JS 엔진외에 브라우저 내 공간에서 별도로 관리한다.

## 가상 DOM

- 가상 DOM은 DOM과 마찬가지로 HTML 문서를 자바스크립트 객체로 모델링한 것이다.
- DOM의 가벼운 복사본이 가상 DOM인데, 실제 DOM은 노드 객체로 구성되고 가상 DOM은 평범한 JS 객체로 구성된다.

## 가상 DOM의 장점

### 1. 성능

- 실제 DOM은 변경할 때마다 reflow, repaint와 같은 연산이 발생할 수 있는 반면, 가상 DOM은 그렇지 않다. (최적화하여 최소의 변경 사항에 대해서만 한꺼번에 연산 수행)
- 실제 DOM(노드 객체)보다 구조가 단순하여 가볍다.
- (불확실) 가상 DOM은 JS 엔진 내 메모리에서 관리되는 반면, 실제 DOM은 JS 엔진 외부의 브라우저 공간에서 관리되므로 접근 비용이 비싸다?
- getBoundingClientRect()는 한번에 여러 레이아웃 속성을 조회할 수 있게 해준다. 그런데 조회 도중에 레이아웃 변경이 발생하면 getBoundingClientRect()에서도 추가 리플로가 발생한다. 리액트는 레이아웃 다시 계산하는 횟수를 최적화해준다.(아마 레이아웃 계산을 전부한 후에 조회가 되도록 해두지 않았을까 싶음)

### 2. 브라우저간 호환성

- 브라우저마다 문서 모델링 방식이 달라 웹 애플리케이션의 일관성이 보장되지 않고 버그가 발생할 수 있다. 예를 들어 특정 DOM 요소와 속성을 지원하지 않는 브라우저가 있을 수 있다.
- 리액트의 Synthetic Event System은 이러한 호환성 문제를 해결한 하나의 예시이다. 기본 이벤트를 래핑하여 브라우저 별로 다르게 처리할 필요가 없게 했다. (ex. `event.target` vs `event.srcElement`, onChange 이벤트 동작의 차이)
- 루트에 이벤트 위임을 통해 이벤트 리스너를 부착하는데, 이는 다음 두 가지 이유이다.
  - 이벤트 리스너로 인한 메모리 낭비를 최소화(연산 성능도 개선)
  - 일부 요소가 이벤트 리스너 부착이 불가능한 호환성 문제를 해결
- 사실 가상 DOM과 직접적 연관이 있다기보다는 리액트의 기능으로 보는 게 맞지 않을까 싶음.

## 궁금증

- JS 런타임 구조 그려보기
- 가상 DOM으로 도출된 변경사항을 실제 DOM에 한꺼번에 일괄 업데이트 한다고 하는데, 한번에 가능하지 않은 경우도 있지 않나? 업데이트 수를 최소화한다는 의미에서 이런 표현을 한걸까 아님 정말 한번만 업데이트를 수행하는 걸까?
- js 엔진이 돌아가는 동안 브라우저 렌더링이 수행될 수 없는 이유는 뭘까?

## 중요하게 알게 된 사실

- JS엔진과 그외의 영역(HTML 파싱 등을 수행하는 브라우저 고유의 영역)의 구분
- JS엔진이 수행되는 동안 HTML 파싱 등 브라우저 동작이 멈춘다. 둘이 동시에 실행될 수는 없다. (렌더링 최적화를 위해 js 최적화가 중요한 이유임)

## 면접 연습

### 가상 DOM이란 무엇인가요?

가상 DOM은 실제 DOM의 가벼운 복사본입니다. 실제 DOM을 조작하는 것이 성능 비용이 크기 때문에 가상 DOM을 통해 성능 비용을 줄여 DOM 업데이트를 최적화합니다.

### 가상 DOM이 성능에 어떻게 도움이 되는지 자세히 설명해주세요.

첫째, 가상 DOM은 실제 DOM 객체에 비해 구조가 단순하고 가벼워 읽기/수정/생성 비용이 작습니다. 실제 DOM에는 아주 많은 메서드와 프로퍼티가 존재하지만, 가상 DOM은 그러한 세부사항들이 생략된 가벼운 객체입니다.
둘째, 가상 DOM은 조작해도 reflow, repaint와 같은 브라우저 렌더링 연산이 발생하지 않습니다. 따라서 성능 부하가 적습니다.

### 가상 DOM이 브라우저 호환성에 어떻게 도움이 되나요?

브라우저마다 DOM 스펙이 조금씩 다른 경우가 있습니다. 가상 DOM은 그러한 차이점을 핸들링하여 통일된 인터페이스를 제공하기 때문에 개발자가 브라우저 별 DOM 스펙 차이에 대해 신경을 쓰지 않아도 됩니다. 예를 들어 특정 브라우저에서는 `event.target`으로 이벤트 요소를 조회할 수 있지만, 일부 브라우저에서는 `event.srcElement`를 통해 조회합니다. 리액트에서는 브라우저 종류와 상관없이 통일시켜 `event.target`으로 조회할 수 있습니다. 또한, select의 onChange 이벤트 트리거 조건이 브라우저마다 다른 경우가 있는데(일부 브라우저에서는 같은 옵션으로 바뀌어도 onChange 트리거됨) 리액트에서는 그러한 차이를 없애줍니다.

### 리액트 Synthetic Event는 왜 존재하나요?

브라우저 호환성을 높입니다. 브라우저 별로 이벤트 스펙이 다른데, Synthetic Event는 그러한 스펙의 비일관성을 해소하고 일관된 인터페이스를 제공합니다.

### 리액트에서는 왜 루트 요소에 이벤트 리스너를 끌어올려 부착하나요?

첫쨰, 이벤트 위임을 통해 성능을 개선합니다. 여러 요소에 이벤트 리스너를 부착할 때에 비해 루트 요소에 몰아서 부착하고 이벤트 위임을 통해 처리할 때 메모리 비용이 더 적습니다.  
둘쨰, 특정 엘리먼트에서 일부 이벤트를 사용할 수 없는 호환성 문제를 방지합니다.
