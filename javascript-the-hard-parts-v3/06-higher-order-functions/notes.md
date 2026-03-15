# JavaScript: The Hard Parts v3 - 고차 함수 (Higher Order Functions)

## 1. 문제 제기: 또 처음부터?

이전 챕터에서 `copyArrayAndMultiplyBy2`를 작성했다. 이번엔 각 요소를 **2로 나누는** 함수가 필요하다.

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
// result: [0.5, 1, 1.5]
```

함수 이름과 연산자 하나만 다를 뿐, 구조가 완전히 동일하다. 그럼에도 **처음부터 다시 작성해야 한다**. 저장된 함수는 수정할 수 없기 때문이다.

---

## 2. 실행 흐름 (Execution Flow)

`copyArrayAndDivideBy2([1, 2, 3])` 호출 시 엔진이 수행하는 단계:

| 단계 | 위치 | 동작 |
|------|------|------|
| 1 | 전역 메모리(Global Memory) | `copyArrayAndDivideBy2` 함수 정의 저장 |
| 2 | 전역 메모리 | `myArray = [1, 2, 3]` 저장 |
| 3 | 콜 스택(Call Stack) | `copyArrayAndDivideBy2` 실행 컨텍스트 push |
| 4 | 로컬 메모리(Local Memory) | 파라미터(parameter) `array = [1, 2, 3]` 저장 |
| 5 | 로컬 메모리 | `output = []` 빈 배열 생성 |
| 6 | 로컬 메모리 | 각 요소 ÷ 2 → output에 push (3회 반복) |
| 7 | 전역 메모리 | `output`에 할당된 값 반환 → `result = [0.5, 1, 1.5]` |
| 8 | 콜 스택 | 실행 컨텍스트 pop |

<details>
<summary>파라미터(parameter)와 인수(argument) 구분</summary>

- **파라미터(parameter)**: 함수 정의 시 선언하는 변수 이름 (예: `array`)
- **인수(argument)**: 함수 호출 시 실제로 전달하는 값 (예: `[1, 2, 3]`)

파라미터에 인수가 할당(assign)된다.

</details>

---

## 3. for 루프 실행 상세

| 반복 | i | array[i] | array[i] / 2 | output |
|------|---|----------|--------------|--------|
| 1회차 | 0 | 1 | 0.5 | [0.5] |
| 2회차 | 1 | 2 | 1 | [0.5, 1] |
| 3회차 | 2 | 3 | 1.5 | [0.5, 1, 1.5] |

반환 시 `output`이라는 식별자(identifier)가 아닌, **그 식별자에 할당된 값** `[0.5, 1, 1.5]`이 반환된다.

---

## 4. 핵심 문제: DRY 원칙 위반

새로운 연산(+3, ×3 등)이 필요할 때마다 동일한 구조의 함수를 반복 작성하고 있다.

```javascript
function copyArrayAndDivideBy2(array) { ... }  // ÷ 2
function copyArrayAndMultiplyBy2(array) { ... } // × 2
function copyArrayAndAdd3(array) { ... }         // + 3
// ← 구조는 완전히 동일, 연산만 다름
```

이는 **DRY(Don't Repeat Yourself)** 원칙을 위반한다. 반복되는 패턴을 한 번만 작성하고 재사용해야 한다.

| 공통 패턴 | 매번 달라지는 부분 |
|-----------|------------------|
| 배열 입력 받기 | 각 요소에 적용할 **연산** |
| 빈 output 배열 생성 | |
| 입력 배열 순회 | |
| 결과를 output에 push | |
| output의 값 반환 | |

데이터 재사용 문제는 **파라미터**로 해결했다. 기능(functionality) 재사용 문제도 같은 방식으로 해결할 수 있다.

---

## 5. 해결책: 기능을 파라미터로 전달

**직관적 아이디어**: 변하는 연산 부분만 빈칸으로 남겨두고, 함수 실행 시 채워 넣는다.

> JavaScript에서 코드 조각을 문자열(string)로 전달해 실행하는 것은 불가능하다.
> 기능을 전달하려면 **함수(function)로 감싸야** 한다.

```javascript
// 일반화된 함수
function copyArrayAndManipulate(array, instructions) {
  const output = [];
  for (let i = 0; i < array.length; i++) {
    output.push(instructions(array[i]));
  }
  return output;
}

// 전달할 기능을 함수로 정의
function multiplyBy2(input) { return input * 2; }

// 실행: 데이터와 기능을 함께 전달
const result = copyArrayAndManipulate([1, 2, 3], multiplyBy2);
// result: [2, 4, 6]
```

`instructions` 파라미터가 실행 시 `multiplyBy2` 함수로 채워지고, `instructions(array[i])`는 `multiplyBy2(array[i])`와 동일하게 동작한다.

![copyArrayAndManipulate 실행 흐름](./images/06-copy-array-and-manipulate.png)

---

## 6. 고차 함수 (Higher Order Functions)

**고차 함수(Higher Order Function)**: 다른 함수를 인수로 받거나, 함수를 반환하는 함수.

| 역할 | 이름 | 예시 |
|------|------|------|
| 다른 함수를 인수로 받는 함수 | **고차 함수(Higher Order Function)** | `copyArrayAndManipulate` |
| 인수로 전달되는 함수 | **콜백 함수(Callback Function)** | `multiplyBy2` |

```javascript
// 같은 고차 함수로 다양한 연산을 재사용
const doubled  = copyArrayAndManipulate([1, 2, 3], (x) => x * 2);  // [2, 4, 6]
const halved   = copyArrayAndManipulate([1, 2, 3], (x) => x / 2);  // [0.5, 1, 1.5]
const plusThree = copyArrayAndManipulate([1, 2, 3], (x) => x + 3); // [4, 5, 6]
```

더 이상 연산마다 새 함수를 작성할 필요가 없다. **한 번 작성하고, 기능을 주입해 재사용**한다.

<details>
<summary>왜 문자열로 코드를 전달할 수 없나?</summary>

일부 언어는 문자열로 된 코드를 파싱해 실행할 수 있다. JavaScript에서도 `eval()`을 통해 가능하지만, 보안과 성능 문제로 사용이 권장되지 않는다. 함수는 코드를 안전하게 번들링(bundle)하고 전달하는 JavaScript의 공식적인 메커니즘이다.

</details>
