# JavaScript: The Hard Parts - Thread of Execution

## 1. 개요
자바스크립트 엔진이 코드를 실행할 때 발생하는 두 가지 핵심 프로세스를 이해하는 단계입니다. 자바스크립트는 내부적으로 크게 두 가지 기능을 수행합니다.

1. **Thread of Execution (실행 스레드):** 코드를 한 줄씩(Line by line) 읽고 실행하는 과정
2. **Memory (메모리):** 실행 중에 마주치는 데이터(변수, 상수)와 코드(함수)를 나중에 사용하기 위해 저장하는 공간

> **"이것은 마법이나 추상화가 아닙니다. 자바스크립트가 실제로 '보이지 않는 곳(Under the hood)'에서 수행하는 실제 물리적 동작입니다."**

---
## 2. 코드 실행 흐름 분석 (Walkthrough)

아래 코드가 실행될 때, '실행 커서'가 어떻게 움직이며 메모리에 무엇을 기록하는지 단계별로 분석합니다.

```javascript
1: const num = 3;
2:
3: function multiplyBy2(inputNumber) {
4:   const result = inputNumber * 2;
5:   return result;
6: }
7:
8: const output = multiplyBy2(num);
```

### (1) 데이터의 저장 (Line 1)
* **Execution Flow:** 커서가 첫 번째 줄에 위치합니다.
* **Memory:** CPU 연산을 통해 전역 메모리에 식별자 `num`을 생성하고 값 `3`을 할당합니다. 이제 메모리 지도에는 `num: 3`이 기록됩니다.

### (2) 함수의 정의 및 건너뛰기 (Line 3 ~ 6)
* **Execution Flow:** 커서가 Line 3에 도달합니다. 엔진이 `function` 키워드를 인식하면, 실행의 초점은 함수 내부(Line 4~5)를 밟지 않고 **Line 6 다음으로 즉시 이동(Jump)**합니다.
* **Memory:** 식별자 `multiplyBy2`라는 Key에 함수의 **코드 전체 내용**이 데이터 뭉치(Bundle) 형태로 저장됩니다. (강의에서는 이를 `f`가 적힌 박스로 시각화합니다.)
* **특징:** 함수 내부 코드는 실행된 것이 아니라, 나중에 호출될 때를 대비해 메모리에 **'보관'**만 된 상태입니다.

### (3) 함수의 호출과 실행 (Line 8)
* **Execution Flow:** 커서가 Line 8에 도달합니다.
* **Memory:** 우선 `output`이라는 식별자를 메모리에 등록합니다. 아직 값이 반환되지 않았으므로 **미초기화(Uninitialized)** 상태로 둡니다.
* **Jump 명령:** 괄호 **`()`**를 확인하는 순간, 커서는 현재 위치(Line 8)를 기억해둔 채 **함수가 저장된 메모리 위치(Line 3)로 실행의 초점을 옮깁니다.**
* **Evaluation:** 인자로 넘겨준 `num`은 메모리 조회를 통해 실제 값 `3`으로 치환(평가)되어 함수 내부로 전달됩니다.

---

## 3. 핵심 용어 정리

| 용어 | 정의 및 특징 |
| :--- | :--- |
| **Identifier** | 메모리에 저장된 데이터나 코드에 붙인 라벨 (식별자) |
| **Memory** | 데이터를 저장하는 공간. 프로그래밍에서 데이터를 보관하는 위치에 대한 세련된 표현 |
| **Thread of Execution** | 코드를 한 줄씩 엮어 내려가며 실행하는 자바스크립트의 특성 |
| **Parenthesis `()`** | 자바스크립트에게 해당 함수를 즉시 실행하라고 알리는 트리거(명령) |

---

## 4. 요약 및 결론
자바스크립트는 **"정해진 순서대로 실행하고(Thread)"**, **"필요한 것을 기억하는(Memory)"** 기본 원칙에 충실합니다. 우리가 작성하는 모든 복잡한 로직은 결국 이 두 가지 기능이 결합되어 만들어지는 물리적인 결과물입니다.

## 5. Lesson Review
---

<details>
<summary><strong>1. 자바스크립트에서 식별자(Identifier)란 무엇을 의미하나요?</strong></summary>
컴퓨터 메모리에 저장되는 모든 것(변수, 상수, 함수 등)에 붙여진 <strong>라벨(이름)</strong>을 의미합니다.
</details>
<br />
<details>
<summary><strong>2. 자바스크립트에서 함수 호출(Function Call)임을 나타내는 표시는 무엇인가요?</strong></summary>
함수 이름 뒤에 붙는 <strong>괄호 <code>()</code></strong>의 존재입니다.
</details>
<br />
<details>
<summary><strong>3. 자바스크립트에서 상수(Constant)와 변수(Variable)의 차이점은 무엇인가요?</strong></summary>
<strong>상수(const)</strong>는 한 번 값이 할당되면 저장된 값을 변경할 수 없지만, <strong>변수(let/var)</strong>는 저장된 값을 바꿀 수(varied) 있습니다.
</details>
<br />
<details>
<summary><strong>4. 자바스크립트는 함수가 호출되기 전, 초기화되지 않은 상수(uninitialized constant)를 어떻게 처리하나요?</strong></summary>
상수를 <code>undefined</code>로 설정하지 않고, 함수 호출이 완료되어 값이 반환될 때까지 <strong>값이 없는 상태(uninitialized)</strong>로 둡니다.
</details>
