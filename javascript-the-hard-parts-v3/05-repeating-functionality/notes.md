# JavaScript: The Hard Parts v3 - 반복되는 기능 문제 (Repeating Functionality)

## 1. 예제 함수: copyArrayAndMultiplyBy2

배열을 입력받아 각 요소를 2배로 만든 새 배열을 반환하는 함수다.

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
// result: [2, 4, 6]
```

---

## 2. 실행 흐름 (Execution Flow)

함수가 호출될 때 JavaScript 엔진이 수행하는 단계를 순서대로 정리하면 다음과 같다.

| 단계 | 위치 | 동작 |
|------|------|------|
| 1 | 전역 메모리(Global Memory) | `copyArrayAndMultiplyBy2` 함수 정의 저장 |
| 2 | 전역 메모리 | `myArray = [1, 2, 3]` 상수 저장 |
| 3 | 전역 메모리 | `result` 상수 선언 (미초기화) |
| 4 | 콜 스택(Call Stack) | `copyArrayAndMultiplyBy2` 실행 컨텍스트 생성 및 push |
| 5 | 실행 컨텍스트 내 로컬 메모리(Local Memory) | 매개변수 `array = [1, 2, 3]` 저장 |
| 6 | 실행 컨텍스트 내 로컬 메모리 | `output = []` 빈 배열 생성 |
| 7 | 실행 컨텍스트 내 로컬 메모리 | 각 요소 × 2 → output에 push (3회 반복) |
| 8 | 전역 메모리 | `output` 반환 → `result = [2, 4, 6]` 저장 |
| 9 | 콜 스택 | 실행 컨텍스트 pop |
---

## 3. 실행 컨텍스트 (Execution Context) 상세

함수가 실행되면 **새로운 실행 컨텍스트**가 생성된다. 실행 컨텍스트는 두 가지 영역으로 구성된다.

- **스레드 오브 익스큐션(Thread of Execution)**: 코드를 한 줄씩 순서대로 실행
- **로컬 메모리(Local Memory)**: 함수 내부의 변수와 매개변수를 저장

콜 스택(Call Stack)의 상태:
```
[ copyArrayAndMultiplyBy2 ]  ← 현재 실행 중
[ global ]
```

---

## 4. for 루프 동작 상세

`for (let i = 0; i < array.length; i++)` 조건에 따라 3회 반복된다.

| 반복 | i | array[i] | array[i] * 2 | output |
|------|---|----------|---------------|--------|
| 1회차 | 0 | 1 | 2 | [2] |
| 2회차 | 1 | 2 | 4 | [2, 4] |
| 3회차 | 2 | 3 | 6 | [2, 4, 6] |

<details>
<summary>for 루프의 인터페이스 한계</summary>

`for` 루프는 데이터를 다루기에 다소 구식(dated)인 인터페이스다. 배열을 순회할 때 대부분의 경우 **인덱스 위치 자체는 관심 대상이 아니고**, 그냥 다음 요소를 얻고 싶을 뿐이다. 그럼에도 인덱스(`i`)를 매번 관리해야 한다는 점이 불편하다. 숙련된 엔지니어들도 `i`의 값을 혼동하는 경우가 많다. 현대 JavaScript에는 `for...of`, `forEach`, `map` 등 더 직관적인 대안들이 존재한다.

</details>

---

## 5. 핵심 문제: DRY 원칙 위반

이 함수에서 딱 **한 글자**만 바꾼 유사한 함수가 등장한다면 어떻게 될까?

```javascript
// 각 요소를 3으로 곱하는 함수
function copyArrayAndMultiplyBy3(array) {
  const output = [];
  for (let i = 0; i < array.length; i++) {
    output.push(array[i] * 3); // ← 2 → 3, 딱 한 글자 변경
  }
  return output;
}
```

구조가 완전히 동일하고 **단 하나의 값만 다르다**. 이런 함수를 목적마다 새로 작성하는 것은 **DRY(Don't Repeat Yourself)** 원칙을 위반한다. 이것이 **일반화된 함수(Generalized Functions)** 와 **고차 함수(Higher Order Functions)** 가 필요한 이유다.
