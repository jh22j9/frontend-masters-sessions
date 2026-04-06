# JavaScript: The Hard Parts v3 - Pass a Function as an Argument

## 1. 개요: 일반화된 함수의 필요성

이전 챕터에서 `copyArrayAndMultiplyBy2`, `copyArrayAndDivideBy2`, `copyArrayAndAdd3` 같은 함수들을 만들었습니다. 이들은 매우 반복적이고 재사용성이 떨어지는 함수들이었습니다.

이번에는 <b>배열의 각 요소에 적용할 연산을 미리 정하지 않고</b>, 실행 시점에 함수로 전달받는 일반화된 함수 `copyArrayAndManipulate`를 만들어 봅니다.

이 과정을 통해 우리가 매일 사용하는 `map`, `reduce`, `filter`의 <b>내부 동작 원리</b>를 이해할 수 있습니다. 또한 ES2023에서 추가된 `toReversed` 같은 새로운 배열 메서드도 이 패턴을 따르도록 설계되었다는 것을 알게 됩니다.

---

## 2. `copyArrayAndManipulate` 코드 구조

```javascript
function copyArrayAndManipulate(array, instructions) {
  const output = [];
  for (let i = 0; i < array.length; i++) {
    output.push(instructions(array[i]));
  }
  return output;
}

function multiplyBy2(input) {
  return input * 2;
}

const result = copyArrayAndManipulate([1, 2, 3], multiplyBy2);
```

| 매개변수 | 역할 |
|---|---|
| <b>`array`</b> | 처리할 입력 배열 |
| <b>`instructions`</b> | 각 요소에 적용할 함수 (TBD — 실행 시점에 결정) |

핵심 아이디어: 연산 부분을 <b>매개변수(parameter)</b>로 남겨두고, 실행 시점에 구체적인 함수를 <b>인수(argument)</b>로 전달합니다.

---

## 3. 단계별 실행 추적

### 3-1. 전역 메모리에 저장

1. `copyArrayAndManipulate` → 함수 코드 전체 저장
2. `multiplyBy2` → 함수 코드 전체 저장
3. `result` → <b>미초기화(uninitialized)</b> 상태

<details>
<summary>const와 미초기화(uninitialized) 상태</summary>

`const`로 선언된 변수는 이전의 `var`와 달리 `undefined`가 아닌 <b>미초기화(uninitialized)</b> 상태입니다. `const`는 재할당(reassignment)을 금지하므로, 만약 `undefined`로 초기화하면 실제 값 할당이 "재할당"이 되어버립니다. 이를 방지하기 위해 값이 할당되기 전까지는 접근할 수 없는 미초기화 상태(TDZ: Temporal Dead Zone)로 남습니다.

</details>

### 3-2. `copyArrayAndManipulate` 호출

`copyArrayAndManipulate([1, 2, 3], multiplyBy2)`를 호출하면 <b>새로운 실행 컨텍스트(execution context)</b>가 생성되고, 콜 스택(call stack)에 추가됩니다.

```
[copyArrayAndManipulate]  ← 새로 추가
[global]                  ← 항상 아래에 위치
```

### 3-3. 로컬 메모리(Local Memory) 설정

| 레이블 | 값 | 설명 |
|---|---|---|
| `array` | `[1, 2, 3]` | 첫 번째 인수 |
| `instructions` | `multiplyBy2`의 함수 코드 | 두 번째 인수 |
| `output` | `[]` | 빈 배열로 초기화 |

<b>중요</b>: 전달되는 것은 `multiplyBy2`라는 레이블이 아니라, 그 레이블이 가리키는 <b>함수 코드 자체</b>입니다. 이 함수 코드는 `copyArrayAndManipulate` 내부에서 `instructions`라는 새로운 이름으로 참조됩니다. 원래의 `multiplyBy2`라는 이름은 더 이상 사용되지 않습니다.

이렇게 레이블이 바뀌기 때문에 `multiplyBy2`뿐 아니라 `add3`, `divideBy2` 등 <b>어떤 함수든</b> 전달할 수 있습니다.

---

## 4. for 루프 실행 과정

### 반복 1: `i = 0`

1. `array[0]` → 숫자 `1`로 평가(evaluate)됩니다
2. `instructions` → `multiplyBy2` 함수 코드로 평가됩니다
3. `instructions(array[0])` = `multiplyBy2(1)` 실행
4. <b>새로운 실행 컨텍스트 생성</b> → 콜 스택에 `instructions` 추가

```
[instructions (multiplyBy2)]  ← 새로 추가
[copyArrayAndManipulate]
[global]
```

5. 로컬 메모리: `input = 1`
6. `return input * 2` → `2` 반환
7. 실행 컨텍스트 종료 → 콜 스택에서 팝(pop)
8. `output.push(2)` → `output = [2]`

### 반복 2: `i = 1`

1. `array[1]` → 숫자 `2`
2. `instructions(2)` = `multiplyBy2(2)` 실행
3. 새로운 실행 컨텍스트 생성 → 콜 스택에 추가
4. `input = 2`, `return 2 * 2` → `4` 반환
5. 콜 스택에서 팝
6. `output.push(4)` → `output = [2, 4]`

### 반복 3: `i = 2`

1. `array[2]` → 숫자 `3`
2. `instructions(3)` = `multiplyBy2(3)` 실행
3. 새로운 실행 컨텍스트 생성 → 콜 스택에 추가
4. `input = 3`, `return 3 * 2` → `6` 반환
5. 콜 스택에서 팝
6. `output.push(6)` → `output = [2, 4, 6]`

### 전체 과정 요약

| i | `array[i]` | `instructions(array[i])` | 반환값 | output 상태 |
|---|---|---|---|---|
| 0 | 1 | `multiplyBy2(1)` | 2 | `[2]` |
| 1 | 2 | `multiplyBy2(2)` | 4 | `[2, 4]` |
| 2 | 3 | `multiplyBy2(3)` | 6 | `[2, 4, 6]` |

---

## 5. 반환과 결과 할당

`return output` → `copyArrayAndManipulate`의 호출 전체가 `[2, 4, 6]`으로 대체됩니다.

```javascript
const result = copyArrayAndManipulate([1, 2, 3], multiplyBy2);
// result → [2, 4, 6]
```

`copyArrayAndManipulate`의 실행 컨텍스트는 <b>콜 스택에서 제거</b>되고, 로컬 메모리도 함께 사라집니다. 전역 상수 `result`에 `[2, 4, 6]`이 할당됩니다.

---

## 6. 레이블 평가(Evaluation)의 원리

JavaScript에서 코드가 실행될 때, <b>모든 레이블(label)은 메모리에 저장된 실제 값으로 대체</b>됩니다.

```javascript
output.push(instructions(array[i]));
```

이 한 줄에서 일어나는 평가 과정:

| 레이블 | 평가 결과 |
|---|---|
| `array` | `[1, 2, 3]` |
| `array[i]` (i=0일 때) | 숫자 `1` |
| `instructions` | `multiplyBy2`의 함수 코드 |
| `instructions(array[i])` | `multiplyBy2(1)` 실행 → `2` |

---

## 7. 괄호(Parentheses)의 중요성

함수를 <b>참조</b>하는 것과 <b>실행</b>하는 것은 괄호의 유무로 결정됩니다.

| 표현 | 의미 |
|---|---|
| `instructions` | 함수 코드 자체를 참조 (복사) |
| `instructions(1)` | 함수를 실행하여 결과값을 반환 |

괄호가 없으면 함수 코드를 복사하는 것에 불과하고, 괄호가 있어야 "이 입력값으로 실행하라"는 의미가 됩니다. `output.push(instructions(array[i]))` 에서 `instructions`에 괄호가 붙어 있기 때문에 함수가 실행되고, 그 반환값이 `push`의 인수로 전달됩니다.

---

## 8. 기능을 감싸는 방법: 함수(Function)

JavaScript에서 기능(functionality)을 전달하려면 반드시 <b>함수(function)</b>로 감싸야 합니다.

- 연산자 `* 2`를 직접 전달할 수는 없습니다
- 기능을 함수 안에 감싸면 매개변수로 전달할 수 있습니다
- 이것이 <b>고차 함수(Higher-Order Function)</b>의 핵심 원리입니다

```javascript
// 기능을 함수로 감싸서 전달
copyArrayAndManipulate([1, 2, 3], multiplyBy2);  // ×2
copyArrayAndManipulate([1, 2, 3], add3);          // +3
copyArrayAndManipulate([1, 2, 3], divideBy2);     // ÷2
```

`copyArrayAndManipulate`는 연산을 매개변수 `instructions`로 받기 때문에 <b>어떤 기능이든</b> 받아들일 수 있는 범용 함수가 됩니다.

---

## 9. `map`, `reduce`, `filter`와의 연결

`copyArrayAndManipulate`는 사실상 `Array.prototype.map`의 내부 동작 원리와 동일합니다.

```javascript
// 우리가 만든 함수
const result = copyArrayAndManipulate([1, 2, 3], multiplyBy2);

// 내장 메서드
const result = [1, 2, 3].map(multiplyBy2);
```

| 내장 메서드 | 역할 |
|---|---|
| `map` | 각 요소에 함수를 적용하여 새 배열 반환 |
| `reduce` | 배열을 하나의 값으로 축약 |
| `filter` | 조건에 맞는 요소만 걸러 새 배열 반환 |

역사적으로 `reverse` 같은 일부 배열 메서드는 이 패턴을 따르지 않았습니다 (원본 배열을 변경). ES2023에서 추가된 `toReversed` 같은 메서드는 원본을 변경하지 않고 새 배열을 반환하는 패턴을 따르도록 만들어졌습니다.

---

## 10. 핵심 개념 정리

| 개념 | 설명 |
|---|---|
| <b>고차 함수(Higher-Order Function)</b> | 함수를 인수로 받거나 함수를 반환하는 함수 |
| <b>콜백 함수(Callback Function)</b> | 다른 함수에 인수로 전달되는 함수 (예: `multiplyBy2`) |
| <b>매개변수(Parameter)</b> | 함수 정의 시 선언하는 자리표시자 — TBD로 남겨둠 |
| <b>인수(Argument)</b> | 함수 호출 시 실제로 전달하는 값 |
| <b>실행 컨텍스트(Execution Context)</b> | 함수가 호출될 때마다 생성되는 실행 환경 |
| <b>콜 스택(Call Stack)</b> | 현재 실행 중인 함수를 추적하는 LIFO 스택 |
| <b>레이블 평가(Label Evaluation)</b> | 실행 시 모든 레이블이 메모리의 실제 값으로 대체되는 과정 |
| <b>괄호 호출(Parentheses Invocation)</b> | `()` — 함수를 참조가 아닌 실행으로 전환 |

---

## Q&A

<details>
<summary>reverse() 같은 배열 메서드가 원본을 변경하는 것은 의도적인 설계였습니까?</summary>

의도적인 설계였습니다. JavaScript가 1995년에 처음 만들어졌을 때는 메모리가 제한적이었기 때문에, 새 배열을 생성하는 대신 원본 배열을 직접 변경(mutate)하는 것이 효율적이라고 판단한 것입니다.

```javascript
// 원본을 변경하는 메서드 (mutating)
const arr = [1, 2, 3];
arr.reverse();     // arr 자체가 [3, 2, 1]로 변경됨
arr.sort();        // arr 자체가 정렬됨
arr.splice(0, 1);  // arr에서 직접 요소 제거
```

하지만 시간이 지나면서 이런 방식이 사이드 이펙트(side effect)를 유발하고 버그를 만들기 쉽다는 것이 밝혀졌습니다. 그래서 ES2023에서 원본을 변경하지 않는 복사 버전이 추가되었습니다.

| 기존 (mutating) | ES2023 (non-mutating) |
|---|---|
| `reverse()` | `toReversed()` |
| `sort()` | `toSorted()` |
| `splice()` | `toSpliced()` |

</details>
