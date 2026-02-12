# JavaScript: The Hard Parts - Call Stack

## 1. 개요

함수를 호출하면 실행 흐름이 함수 내부로 이동했다가 다시 돌아옵니다. 하지만 자바스크립트는 이 "돌아갈 위치"를 자동으로 알지 못합니다. 이를 추적하기 위한 데이터 구조가 바로 **콜 스택(Call Stack)**입니다.

---

## 2. 자바스크립트 엔진의 3대 핵심 구성 요소

앞서 Thread of Execution과 Memory 두 가지를 살펴보았는데, 여기에 **Call Stack**이 더해져 자바스크립트 런타임의 3대 핵심 구성 요소가 완성됩니다.

1.  **Thread of Execution (실행 흐름):** 코드를 한 줄씩 읽고 실행하는 단일 스레드입니다.
2.  **Memory (메모리):** 변수, 함수 정의, 데이터가 저장되는 공간입니다. (Variable Environment)
3.  **Call Stack (콜 스택):** 현재 실행 중인 위치를 추적하고, 함수 종료 시 돌아갈 곳을 관리하는 데이터 구조입니다.

> **"Do code, save stuff, keep track of what code's being run."** — 이 세 가지가 자바스크립트 런타임의 핵심이며, 나머지 모든 것은 이 위에 얹어지는 기능(feature)입니다.

---

## 3. 실행 프로세스 시각화

앞 장에서 코드가 실행될 때 Thread of Execution과 Memory가 함께 작동하는 것을 보았습니다. 이 둘을 하나로 묶은 환경을 **실행 컨텍스트(Execution Context)**라고 부릅니다. 코드가 실행되면 글로벌 실행 컨텍스트가, 함수가 호출되면 로컬 실행 컨텍스트가 각각 생성됩니다.

```javascript
const num = 3;

function multiplyBy2 (inputNumber) {
  const result = inputNumber * 2;
  return result;
}

const output = multiplyBy2(num);
const newOutput = multiplyBy2(10);
```

### 1단계: 글로벌 실행 컨텍스트 (Global Execution Context)
코드가 실행되자마자 가장 먼저 생성됩니다. 다른 언어(Java 등)에서는 `main()` 함수를 명시적으로 호출해야 하지만, 자바스크립트는 파일을 실행하는 순간 **자동으로** 글로벌 실행 컨텍스트가 생성됩니다.

* **글로벌 메모리 (Variable Environment):**
    * `num`: 3
    * `multiplyBy2`: `function...` (함수 정의 전체 저장)
    * `output`: `uninitialized` (아직 실행 전)
* **콜 스택(Call Stack):**
    * `[ Global Execution Context ]` (스택의 가장 밑바닥에 위치)

**콜 스택 시각화:**
```
┌─────────────────────────────┐
│ Global Execution Context    │ ← 현재 실행 중
└─────────────────────────────┘
```

### 2단계: 첫 번째 함수 호출 (`multiplyBy2(num)`)
`output`을 결정하기 위해 함수가 실행됩니다.

1.  **Local Execution Context 생성 및 Push:** 함수만을 위한 새로운 실행 컨텍스트가 생성되고 콜 스택에 푸시됩니다.
    * **로컬 메모리 (Variable Environment):** `inputNumber: 3`, `result: 6`
2.  **Return:** `return` 키워드를 만나면 실행 흐름이 결과값을 들고 함수를 빠져나옵니다.
3.  **Pop:** 콜 스택에서 `multiplyBy2` 실행 컨텍스트가 제거(Pop)됩니다.

**콜 스택 시각화 (함수 실행 중):**
```
┌─────────────────────────────┐
│ multiplyBy2 실행 컨텍스트    │ ← 현재 실행 중
├─────────────────────────────┤
│ Global Execution Context    │ ← 대기 중
└─────────────────────────────┘
```

**상세 구조 (실행 컨텍스트와 Variable Environment):**
```
콜 스택 (Call Stack):
┌─────────────────────────────────┐
│ Local Execution Context         │ ← 함수 호출 시 생성
│  ┌─────────────────────────────┐│
│  │ Variable Environment        ││ ← 독립적인 메모리 공간
│  │ (로컬 메모리)                ││    (실행 컨텍스트의 일부)
│  │ - inputNumber: 3            ││
│  │ - result: 6                 ││
│  └─────────────────────────────┘│
├─────────────────────────────────┤
│ Global Execution Context        │
│  ┌─────────────────────────────┐│
│  │ Variable Environment        ││
│  │ (글로벌 메모리)              ││
│  │ - num: 3                    ││
│  │ - multiplyBy2: [힙 참조]    ││
│  └─────────────────────────────┘│
└─────────────────────────────────┘
```

**핵심**: 실행 컨텍스트는 Variable Environment를 **포함**하며, 전체가 콜 스택에 위치합니다.

### 3단계: 두 번째 함수 호출 (`multiplyBy2(10)`)
`newOutput`을 결정하기 위해 다시 호출됩니다.

1.  **Local Execution Context 생성 및 Push:** 이전 호출과는 **완전히 독립된** 새로운 실행 컨텍스트가 생성되고 콜 스택에 푸시됩니다.
    * **로컬 메모리 (Variable Environment):** `inputNumber: 10`
2.  **Pop:** 실행 완료 후 스택에서 제거되고 다시 `Global Execution Context`로 돌아옵니다.

**콜 스택 시각화 (함수 실행 중):**
```
┌─────────────────────────────┐
│ multiplyBy2 실행 컨텍스트    │ ← 현재 실행 중
├─────────────────────────────┤
│ Global Execution Context    │ ← 대기 중
└─────────────────────────────┘
```

---

## 4. 주요 요약 (Key Takeaways)

* **함수 저장 방식:** 함수는 메모리에 한 번 정의되어 저장되지만, 호출될 때마다 새로운 실행 컨텍스트를 생성하여 실행됩니다. 참고로 함수는 메모리에 직접 저장되는 것이 아니라 **참조(reference/pointer)**로 저장되며, 이로 인한 동작 차이는 이후 type coercion 섹션에서 다룹니다.
* **콜 스택의 대원칙:** 자바스크립트는 오직 **콜 스택의 가장 최상단(Top)**에 있는 작업만 수행합니다. (LIFO: Last In, First Out)
* **Return의 역할:** 현재 실행 중인 컨텍스트를 종료하고 콜 스택에서 제거하며, 바로 아래에 대기 중인 컨텍스트(Global 등)로 제어권을 넘깁니다.
* **Global의 제거 시점:** Global 실행 컨텍스트는 모든 코드 실행이 끝나고, **나중에 돌아와서 실행될 것이 아무것도 없을 때** 비로소 콜 스택에서 제거됩니다. (이 조건은 비동기 섹션에서 다시 다룹니다.)

---

> **스터디 토론 과제**
> 만약 `multiplyBy2` 함수 내부에서 또 다른 함수 `logResult()`를 호출한다면, 콜 스택의 모양은 순간적으로 어떻게 변할까요? 직접 그려보며 이야기해봅시다.

---

## Q&A

<details>
<summary><strong>Q: "코드가 실행됐다"는 것은 정확히 어떤 시점인가요?</strong></summary>

**A:** "코드가 실행됐다"는 자바스크립트 엔진이 파일을 받아서 **처리를 시작한 순간**을 의미합니다. 이 시점에 Global Execution Context가 생성되고, 콜 스택에 `Global`이 push되며, 실행 커서가 첫 번째 줄 직전에 위치합니다.

트리거 시점은 호스트 환경에 따라 다릅니다.

| 환경 | 트리거 시점 |
| :--- | :--- |
| **Node.js** | 터미널에서 `node 파일명` 명령을 실행한 순간 |
| **브라우저** | HTML 파서가 `<script>` 태그를 만나는 순간 |

브라우저의 경우 더 구체적인 흐름은 다음과 같습니다.

1. 사용자가 URL에 접근
2. 서버가 HTML 파일을 응답으로 보냄
3. 브라우저가 HTML을 위에서 아래로 파싱
4. `<script>` 태그를 만남 → 파싱을 멈추고 JS 엔진에 코드를 넘김
5. JS 엔진이 Global Execution Context를 생성하고 실행 시작

</details>

<details>
<summary><strong>Q: 그러면 브라우저에서 HTML 파싱은 누가 담당하나요? </strong></summary>

**A:** **HTML 파싱은 렌더링 엔진(또는 HTML 파서)이 담당**합니다.

브라우저는 크게 두 가지 주요 컴포넌트로 구성됩니다:

| 컴포넌트 | 역할 | 예시 |
| :--- | :--- | :--- |
| **렌더링 엔진** | HTML을 파싱하여 DOM 트리 생성<br>CSS를 파싱하여 CSSOM 트리 생성<br>레이아웃 계산 및 페이지 렌더링 | Chrome의 Blink<br>Safari의 WebKit<br>Firefox의 Gecko |
| **자바스크립트 엔진** | JavaScript 코드만 실행 | Chrome의 V8<br>Firefox의 SpiderMonkey<br>Safari의 JavaScriptCore |

**관계**: HTML 파싱 중 `<script>` 태그를 만나면, 렌더링 엔진이 파싱을 멈추고 JavaScript 엔진에게 코드 실행을 요청합니다. JavaScript 실행이 완료되면 다시 HTML 파싱을 재개합니다.

</details>

<details>
<summary><strong>Q: 파싱(Parsing)이 정확히 무엇인가요?</strong></summary>

**A:** **파싱은 텍스트 형태의 데이터를 컴퓨터가 이해하고 처리할 수 있는 구조화된 형태로 변환하는 과정**입니다.

### HTML 파싱 예시:

**입력 (텍스트)**:
```html
<div>
  <h1>Hello</h1>
  <p>World</p>
</div>
```

**파싱 과정**: 문자열을 토큰으로 분해하고 구조 분석

**출력 (DOM 트리)**:
```
div (Element)
├─ h1 (Element)
│  └─ "Hello" (Text)
└─ p (Element)
   └─ "World" (Text)
```

### 파싱의 목적:

1. **구조화**: 단순 텍스트 → 트리 구조로 변환
2. **검증**: 문법 오류 체크
3. **접근성**: JavaScript에서 `document.querySelector()` 등으로 DOM 조작 가능

간단히 말하면: **"읽어서 이해 가능한 구조로 만드는 과정"**입니다.

</details>

<details>
<summary><strong>Q: DOM 트리는 어디에 생성되나요?</strong></summary>

**A:** **브라우저의 메모리(Heap Memory)**에 생성됩니다.

### 생성 과정:

1. **렌더링 엔진**이 HTML을 파싱
2. 파싱 결과를 **브라우저 메모리**에 객체 구조로 저장
3. 각 HTML 요소가 **Node 객체**로 변환되어 트리 구조 형성

### 흐름 시각화:

```
HTML 파일 (디스크)
    ↓ (파싱)
렌더링 엔진
    ↓ (생성)
메모리 상의 DOM 트리 (객체들의 계층 구조)
    ↓ (접근)
JavaScript (document 객체를 통해 조작)
```

### 예시:

HTML:
```html
<div id="app">
  <h1>Title</h1>
</div>
```

메모리 상의 객체 구조 (단순화):
```javascript
{
  tagName: "DIV",
  id: "app",
  children: [
    {
      tagName: "H1",
      textContent: "Title",
      children: []
    }
  ]
}
```

JavaScript에서 `document.getElementById('app')`를 호출하면 이 **메모리 상의 객체를 참조**하게 됩니다.

**핵심**: DOM은 파일이 아니라 메모리에 존재하는 살아있는(live) 객체 구조입니다.

</details>

<details>
<summary><strong>Q: 함수는 정확히 어디에 저장되나요?</strong></summary>

**A:** 함수는 **힙 메모리(Heap)**에 저장됩니다.

JavaScript 엔진의 메모리는 크게 두 영역으로 나뉩니다:

| 메모리 영역 | 저장되는 것 | 특징 |
|:---|:---|:---|
| **콜 스택 (Call Stack)** | 실행 컨텍스트<br>원시값(숫자, 문자열 등) | LIFO 구조<br>함수 호출 시 푸시/팝 |
| **힙 메모리 (Heap)** | 객체, 배열, 함수 | 참조 타입의 실제 데이터 저장 |

### 함수의 저장 구조:

```
힙 메모리 (Heap):
┌─────────────────────────────┐
│ function multiplyBy2 {      │ ← 실제 함수 코드
│   const result = ...        │
│   return result;            │
│ }                           │
└─────────────────────────────┘
         ↑
         │ (참조/포인터)
         │
콜 스택 (Call Stack):
┌─────────────────────────────┐
│ Global Execution Context    │
│   Variable Environment:     │
│   - multiplyBy2: [참조] ────┘
│   - num: 3                  │
└─────────────────────────────┘
```

### 핵심:

- **함수 정의 (코드)**: 힙 메모리에 한 번만 저장
- **함수 참조 (변수)**: 실행 컨텍스트의 Variable Environment에 저장 (콜 스택과 연결)
- **실행 컨텍스트**: 함수 호출 시마다 콜 스택에 새로 생성

이것이 문서 91번째 줄에서 말하는 **"참조(reference/pointer)로 저장"**의 의미입니다.

</details>
