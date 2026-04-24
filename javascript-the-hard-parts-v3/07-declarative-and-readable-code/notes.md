# JavaScript: The Hard Parts v3 - Declarative & Readable Code

## 1. 일급 객체(First-Class Object)로서의 함수

JavaScript에서 함수를 다른 함수의 입력값으로 전달할 수 있는 이유는, JavaScript에서 <b>함수가 일급 객체(first-class object)</b>이기 때문입니다.

일급 객체란, 다른 모든 객체와 동일하게 취급될 수 있다는 의미입니다. 함수는 호출(call), 실행(invoke)될 수 있는 추가 능력을 가지고 있지만, 근본적으로는 그냥 객체(object)입니다.

> JavaScript에서는 함수뿐 아니라 객체, 원시값(number, string 등) 등 <b>모든 값이 일급</b>입니다. 굳이 "함수는 일급 객체다"라고 강조하는 이유는, 다른 언어에서는 함수만 유독 이런 취급을 못 받는 경우가 많기 때문입니다.

| 일급 객체의 특성 | 설명 |
|---|---|
| 변수에 할당 | 함수를 변수(label)에 저장할 수 있습니다 |
| 프로퍼티로 할당 | 객체의 프로퍼티로 할당하면 <b>메서드(method)</b>라고 부릅니다 (예: `const user = { greet: function() { ... } }`) |
| 인수로 전달 | 다른 함수의 입력값으로 전달할 수 있습니다 |
| 반환값으로 출력 | 다른 함수의 출력값(output value)으로 반환할 수 있습니다 |

많은 다른 언어에서는 함수를 다른 함수에 전달하는 것이 함수 선언/저장의 기본 개념을 위반합니다. 하지만 JavaScript에서는 객체를 다른 함수에 전달할 수 있는 것과 마찬가지로, 함수도 자연스럽게 전달할 수 있습니다.

<details>
<summary>함수를 반환값으로 사용하면 무슨 일이 일어나나요?</summary>

함수를 다른 함수의 출력값으로 반환할 수 있습니다. 이전 예제에서는 배열을 반환했지만, 함수를 반환하는 것도 가능합니다. 이렇게 함수를 반환할 때 JavaScript에서 가장 난해한 기능인 <b>클로저(closure)</b>가 등장합니다.

</details>

---

## 2. 고차 함수(Higher-Order Function)와 콜백 함수(Callback Function)

이전 챕터에서 만들었던 코드에서, 어느 것이 고차 함수이고 어느 것이 콜백 함수인지 짚어보겠습니다.

```javascript
function copyArrayAndManipulate(array, instructions) { ... }
function multiplyBy2(input) { return input * 2; }

const result = copyArrayAndManipulate([1, 2, 3], multiplyBy2);
```

| 구분 | 함수 | 설명 |
|---|---|---|
| <b>고차 함수(Higher-Order Function)</b> | `copyArrayAndManipulate` | 다른 함수를 입력으로 받거나 출력으로 반환하는 외부 함수 |
| <b>콜백 함수(Callback Function)</b> | `multiplyBy2` | 고차 함수 안에 삽입되는 함수 |

고차 함수를 선언할 때 <b>특별한 키워드가 필요하지 않습니다</b>. `copyArrayAndManipulate`를 다른 함수를 받는다고 명시하는 특별한 방식으로 선언하지 않았습니다. 그냥 그렇게 동작할 뿐이며, 본질적으로 다른 함수와 다를 것은 없습니다.

---

## 3. "콜백(Callback)"이라는 이름에 대한 고찰

"콜백 함수"라는 용어는 사실 여기서 사용하기에 다소 어색합니다.

현재 코드에서 `multiplyBy2`는 `copyArrayAndManipulate` 내부에서 <b>즉시 실행</b>됩니다. "콜백(callback)"이라는 이름은 "나중에 다시 호출되어 돌아온다(call back)"는 뉘앙스를 주기 때문입니다.

| 대안적 명칭 | 의미 |
|---|---|
| <b>핸들러(handler)</b> | 각 요소에 무엇을 할지 처리하는 함수 |
| <b>변환 함수(transformation function)</b> | 데이터를 변환하는 함수 |
| <b>인자 함수(argument function)</b> | 인수로 전달되는 함수 |
| <b>람다 함수(lambda function)</b> | 특히 레이블 없이 전달되는 경우 다른 언어에서 사용하는 명칭 |

기술적으로는 여전히 콜백 함수라고 불리지만, 이 문맥에서는 위의 대안적 명칭이 더 적절할 수 있습니다.

<details>
<summary>그렇다면 진정한 "콜백"은 언제 등장하나요?</summary>

비동기(asynchronous) JavaScript에서 진정한 콜백을 만나게 됩니다. 다른 함수에 전달된 후 한참 뒤에 호출되어 돌아오는 함수를 보게 될 것입니다. 그때는 "콜백"이라는 이름이 정확히 맞습니다. 하지만 지금의 예제에서는 함수가 그 자리에서 즉시 실행됩니다.

```javascript
// 비동기 콜백 — 2초 뒤에 함수가 "다시 호출(call back)"됨
setTimeout(function() {
  console.log("2초 후 실행!");
}, 2000);
```

</details>

---

## 4. 선언적(Declarative) 코드 vs 명령형(Imperative) 코드

콜백 함수와 고차 함수의 가장 큰 장점 중 하나는 코드를 단순화하고 <b>DRY(Don't Repeat Yourself, 중복 배제)</b> 원칙을 지킬 수 있게 해준다는 것입니다. 이를 통해 <b>선언적(declarative)</b>이고 가독성 높은 코드를 작성할 수 있습니다.

### 명령형(Imperative) 코드 — "어떻게(How)" 기술

`copyArrayAndManipulate` 함수의 <b>내부</b>를 보면, 각 요소를 하나씩 꺼내서 변경하는 단계별 절차가 기술되어 있습니다.

```javascript
function copyArrayAndManipulate(array, instructions) {
  const output = [];
  for (let i = 0; i < array.length; i++) {
    output.push(instructions(array[i]));
  }
  return output;
}
```

### 선언적(Declarative) 코드 — "무엇을(What)" 기술

사용하는 쪽의 코드는 훨씬 가독성이 높습니다:

```javascript
const result = copyArrayAndManipulate([1, 2, 3], multiplyBy2);
// "이 데이터를 가져다가 각 요소에 2를 곱하여 조작하라"
```

이 한 줄은 원하는 결과를 선언적으로 표현합니다: "이 데이터를 복사하고, 이 변환을 적용해서 조작하라."

### 선언적 코드의 본질

어떤 선언적 코드도 그 내부(under the hood)에는 반드시 <b>명령형 코드</b>가 존재합니다. 실제로 어떻게 수행할지 기술해야 하기 때문입니다. 선언적 코드는 호출하는 쪽(call site)에서 "어떻게"가 아닌 <b>"무엇을" 원하는지만 선언</b>하기 때문에 코드의 가독성이 훨씬 높아집니다.

| 스타일 | 초점 | 예시 |
|---|---|---|
| <b>명령형(Imperative)</b> | 어떻게(How) 수행하는지 | for 루프로 각 요소를 하나씩 처리 |
| <b>선언적(Declarative)</b> | 무엇을(What) 원하는지 | `copyArrayAndManipulate(data, transform)` |

특히 `filter`나 `reduce` 같은 내장 메서드를 사용할 때는, 데이터를 직접 손으로 처리하는 코드보다 훨씬 가독성이 높습니다.

---

## 5. 콜백의 중요성: 비동기 JavaScript의 근간

이 개념들은 <b>기술 면접</b>에서 매우 자주 등장합니다. 그리고 <b>비동기(asynchronous) JavaScript의 근간</b>이기도 합니다.

비동기 JavaScript야말로 JavaScript를 강력하고 인기 있게 만드는 핵심이며, <b>웹의 언어</b>로서 그렇게 중요한 이유입니다. 동시에 많은 작업을 처리하고, 특히 여러 다른 서버와 통신하는 데 있어 콜백은 그 패턴의 핵심적인 부분입니다.

콜백이 기반이 되는 비동기 패턴들:

- <b>프로미스(Promise)</b>
- <b>async/await</b>
- 그 외 비동기 관련 개념들

---

## Quiz

**Q1.** JavaScript에서 함수가 "일급 객체"라고 불리는 이유로 <b>틀린</b> 것은?

a) 변수에 할당할 수 있다\
b) 다른 함수의 인수로 전달할 수 있다\
c) 함수 선언 시 특별한 키워드가 필요하다\
d) 다른 함수의 반환값으로 출력할 수 있다

<details>
<summary>정답 보기</summary>

**c) 함수 선언 시 특별한 키워드가 필요하다** — 고차 함수를 선언할 때 특별한 키워드는 필요하지 않습니다. 일반 함수와 동일한 방식으로 선언합니다.

</details>

---

**Q2.** 다음 코드에서 고차 함수와 콜백 함수를 각각 고르시오.

```javascript
function copyArrayAndManipulate(array, instructions) { ... }
function multiplyBy2(input) { return input * 2; }

const result = copyArrayAndManipulate([1, 2, 3], multiplyBy2);
```

<details>
<summary>정답 보기</summary>

고차 함수: **`copyArrayAndManipulate`** — 다른 함수를 인수로 받는 외부 함수입니다.\
콜백 함수: **`multiplyBy2`** — 고차 함수에 인수로 전달되는 함수입니다.

</details>

---

**Q3.** `multiplyBy2`를 "콜백 함수"라고 부르는 것이 다소 어색한 이유는?

a) `multiplyBy2`는 함수가 아니라 메서드이기 때문\
b) `multiplyBy2`는 전달된 후 즉시 실행되지, 나중에 다시 호출되는 것이 아니기 때문\
c) `multiplyBy2`는 고차 함수이지 콜백이 아니기 때문\
d) `multiplyBy2`는 비동기적으로 실행되기 때문

<details>
<summary>정답 보기</summary>

**b) `multiplyBy2`는 전달된 후 즉시 실행되지, 나중에 다시 호출되는 것이 아니기 때문** — "콜백(callback)"이라는 이름은 "나중에 다시 호출되어 돌아온다(call back)"는 뉘앙스를 갖고 있어, 즉시 실행되는 경우에는 어색합니다.

</details>

---

**Q4.** 선언적(declarative) 코드와 명령형(imperative) 코드의 관계에 대한 설명으로 올바른 것은?

a) 선언적 코드는 명령형 코드 없이도 동작할 수 있다\
b) 명령형 코드는 선언적 코드보다 항상 성능이 좋다\
c) 어떤 선언적 코드도 내부에는 반드시 명령형 코드가 존재한다\
d) 명령형 코드와 선언적 코드는 완전히 별개의 개념이다

<details>
<summary>정답 보기</summary>

**c) 어떤 선언적 코드도 내부에는 반드시 명령형 코드가 존재한다** — 호출하는 쪽(call site)은 "무엇을" 원하는지만 선언하지만, 실제로 "어떻게" 수행할지는 함수 내부에 명령형으로 기술되어 있어야 합니다.

</details>

---

**Q5.** 함수를 객체의 프로퍼티로 할당하면 무엇이라고 부르는가?

<details>
<summary>정답 보기</summary>

**메서드(method)** — 예: `const user = { greet: function() { ... } }`에서 `greet`은 `user` 객체의 메서드입니다.

</details>

---

## Q&A

<details>
<summary>리액트에서 "더 선언적으로 작성하라"는 코드 리뷰 피드백을 받는 대표적인 케이스는?</summary>

본질적으로 <b>"상태(state)로 표현할 수 있는 것을 명령형 코드로 처리하고 있다"</b>는 의미입니다. 실무에서 자주 지적되는 패턴은 다음과 같습니다.

### 1. `useRef` + `useEffect`로 DOM 직접 조작

```jsx
// 🚫 명령형
useEffect(() => {
  if (isOpen) ref.current.style.display = 'block';
  else ref.current.style.display = 'none';
}, [isOpen]);

// ✅ 선언적
<div className={isOpen ? 'modal visible' : 'modal'}>
```

### 2. 파생 상태(derived state)를 `useState` + `useEffect`로 관리

```jsx
// 🚫 명령형 - state 동기화를 위한 useEffect
const [total, setTotal] = useState(0);
useEffect(() => {
  setTotal(items.reduce((sum, i) => sum + i.price, 0));
}, [items]);

// ✅ 선언적 - 렌더링 중 계산
const total = items.reduce((sum, i) => sum + i.price, 0);
```

React 공식 문서 <b>"You Might Not Need an Effect"</b>의 대표 안티패턴입니다.

### 3. flag state 여러 개로 상태 표현

```jsx
// 🚫 불가능한 상태 조합이 가능해짐
const [isLoading, setIsLoading] = useState(false);
const [isError, setIsError] = useState(false);
const [isSuccess, setIsSuccess] = useState(false);

// ✅ 하나의 union 타입 state로 모델링
const [status, setStatus] = useState('idle');
// 'idle' | 'loading' | 'success' | 'error'
```

<b>불가능한 상태를 표현 불가능하게</b> 만드는 것이 핵심입니다.

### 4. 폼 리셋을 `useEffect`로 처리

```jsx
// 🚫 prop 변화 시 state 초기화
useEffect(() => {
  setName('');
  setEmail('');
}, [userId]);

// ✅ key prop으로 컴포넌트 재생성
<ProfileForm key={userId} userId={userId} />
```

"리셋해라"를 명령하지 않고, "이건 다른 폼이다"라고 선언합니다.

### 5. `for` 루프로 리스트 조작

```jsx
// 🚫 명령형
const filtered = [];
for (let i = 0; i < users.length; i++) {
  if (users[i].active) filtered.push(users[i]);
}

// ✅ 선언적 - 고차 함수
const filtered = users.filter(u => u.active);
```

### 6. state chaining (state가 state를 유발)

```jsx
// 🚫 명령형
useEffect(() => {
  setFullName(`${firstName} ${lastName}`);
}, [firstName, lastName]);

// ✅ 선언적
const fullName = `${firstName} ${lastName}`;
```

### 핵심 원칙

| 신호 | 의심할 점 |
|---|---|
| `useRef` + `style`/`classList` 조작 | 상태로 className 결정하면 되지 않나? |
| `useEffect`로 state 동기화 | 렌더링 중 계산으로 충분하지 않나? |
| `useEffect`로 state 초기화 | `key` prop으로 해결되지 않나? |
| flag state 여러 개 | 하나의 union 타입 state로 묶을 수 있지 않나? |
| `for` / `while` 루프 | `map`/`filter`/`reduce`로 쓸 수 있지 않나? |

한 문장으로 요약하면, <b>"UI = f(state)라는 등식을 최대한 지키고, React가 대신 할 수 있는 일을 직접 하지 말라"</b>는 이야기입니다.

</details>
