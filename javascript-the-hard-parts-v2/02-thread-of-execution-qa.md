## Lesson Review

---

<details>
<summary><strong>1. 자바스크립트에서 식별자(Identifier)란 무엇을 의미하나요?</strong></summary>
<br />
컴퓨터 메모리에 저장되는 모든 것(변수, 상수, 함수 등)에 붙여진 <strong>라벨(이름)</strong>을 의미합니다.
<hr />
<em>The label for anything being stored in the computer's memory, such as variables, constants, or functions.</em>
</details>
<br />

<details>
<summary><strong>2. 자바스크립트에서 함수 호출(Function Call)임을 나타내는 표시는 무엇인가요?</strong></summary>
<br />
함수 이름 뒤에 붙는 <strong>괄호 <code>()</code></strong>의 존재입니다.
<hr />
<em>The presence of parentheses <code>()</code> after the function name.</em>
</details>
<br />

<details>
<summary><strong>3. 자바스크립트에서 상수(Constant)와 변수(Variable)의 차이점은 무엇인가요?</strong></summary>
<br />
<strong>상수(const)</strong>는 한 번 값이 할당되면 저장된 값을 변경할 수 없지만, <strong>변수(variable)</strong>는 저장된 값을 바꿀 수(varied) 있습니다.
<hr />
<em>A constant cannot have its stored value changed once assigned, while a variable can have its value varied.</em>
</details>
<br />

<details>
<summary><strong>4. 자바스크립트는 함수가 호출되기 전, 초기화되지 않은 상수(uninitialized constant)를 어떻게 처리하나요?</strong></summary>
<br />
상수를 <code>undefined</code>로 설정하지 않고, 함수 호출이 완료되어 값이 반환될 때까지 <strong>값이 없는 상태(uninitialized)</strong>로 둡니다.
<hr />
<em>It does not set the constant to <code>undefined</code>, but rather leaves it without a value until the function call is completed.</em>
</details>

---


## 보충 학습 
### 🔍 Q. 강의에 나온 '메모리'는 정확히 어디에 위치하나요?

**"자바스크립트 엔진이 컴퓨터의 RAM으로부터 빌려온 논리적인 저장 구역입니다."**

1. **물리적 위치:** 여러분 컴퓨터의 **RAM** (전기적 신호로 데이터를 저장하는 하드웨어).
2. **논리적 위치:** 자바스크립트 엔진 내부의 **힙(Heap)**과 **콜 스택(Call Stack)** 영역.

### 🔍 Q. 엔진이 RAM을 '빌려온다'는 게 구체적으로 무슨 뜻인가요?

**자바스크립트 엔진이 여러분 컴퓨터의 실제 RAM 자원을 직접 점유해서 사용한다**는 뜻입니다.

1. **실행 프로세스:**
   * 브라우저나 Node.js를 실행하는 순간, OS(Windows, macOS 등)는 "이 프로그램은 이만큼의 메모리가 필요하구나"라고 판단하여 **실제 RAM의 빈 공간(물리 주소)**을 떼어줍니다.
   * 자바스크립트 엔진은 그 떼어준 RAM 공간 위에 자신만의 **Global Memory** 지도를 그립니다.

2. **물리적 실체:**
   * 강의에서 본 `num = 3`이라는 데이터는, 실제 RAM 속에 있는 수많은 트랜지스터와 커패시터에 **전기적 신호(0과 1)** 형태로 저장됩니다.

3. **'빌려온다'고 표현하는 이유:**
   * 프로그램이 종료(브라우저 탭을 닫거나 Node 프로세스를 끌 때)되면, 엔진이 사용하던 그 RAM 공간을 OS가 다시 회수해서 다른 프로그램이 쓸 수 있게 돌려주기 때문입니다.

**[요약]**
자바스크립트 엔진의 메모리는 추상적인 개념이 아닙니다. 엔진은 **여러분의 실제 컴퓨터 RAM을 하드웨어적으로 사용**하고 있으며, 우리가 변수를 선언할 때마다 RAM의 물리적인 공간 일부가 채워지고 있는 것입니다.

### 🔍 Q. 자바스크립트 엔진은 어디에 존재하는가?

자바스크립트 엔진은 독립적으로 실행되는 프로그램이 아니라, 이를 실행해 주는 **'호스트 환경(Runtime)'** 내부에 포함되어 존재합니다.

1. **소프트웨어적 위치 (Runtime):** 
   - **웹 브라우저:** 크롬(V8), 사파리(JavaScriptCore) 등 브라우저 프로그램 내부에 탑재되어 있습니다.
   - **Node.js:** 브라우저 없이 서버나 일반 OS에서 자바스크립트를 실행할 수 있도록 V8 엔진을 내장한 환경입니다.

2. **물리적 위치 (Hardware):**
   - 엔진이 실행되는 동안, 컴퓨터의 **RAM(메모리)** 공간 일부를 할당받아 그 안에 데이터(Memory)를 저장하고 실행 순서(Thread)를 관리합니다.

**[한 줄 요약]**
자바스크립트 엔진은 **브라우저나 Node.js 같은 실행 환경 내부에 탑재된 모듈**이며, 실제 구동 시에는 **컴퓨터의 RAM** 위에서 존재하며 동작합니다.

### Q: Thread of Execution은 Feature일까, Action일까?

Will Sentance는 Thread of Execution을 자바스크립트의 **첫 번째 핵심 Feature(기능)**라고 정의합니다.

* **기능적 관점 (Feature):** 자바스크립트 엔진이 태생적으로 코드를 위에서 아래로, 한 번에 한 줄씩 훑으며 실행하도록 설계된 '특성' 그 자체를 의미합니다.
* **실행적 관점 (Action):** 이 Feature 덕분에 우리가 작성한 코드(Action)들이 순서대로 '실행(Execute)'될 수 있습니다.

**[비유]**
- **Thread of Execution (Feature):** 영사기가 필름을 한 프레임씩 빛으로 쏘아 보내는 **기능**.
- **Individual Lines of Code (Action):** 필름에 담긴 **장면 하나하나**.

### 🔍 Q. 우리가 비유한 '커서'는 실제 컴퓨터 공학(CS)에서 무엇인가요?

우리가 "실행 흐름을 따라 움직이는 커서"라고 부른 것의 실제 명칭은 **프로그램 카운터(Program Counter, PC)** 또는 **인스트럭션 포인터(Instruction Pointer)**입니다.

#### 1. 프로그램 카운터(PC)의 역할
* **정의:** CPU 내부에 있는 아주 작은 저장 공간(Register)으로, **"다음에 실행할 명령어의 메모리 주소"**를 가리킵니다.
* **작동:** CPU가 한 줄을 실행하면 PC는 자동으로 다음 줄의 주소로 업데이트됩니다. 이것이 커서가 아래로 내려가는 현상의 실체입니다.



#### 2. "커서가 점프한다"는 것의 의미
* **함수 호출:** 괄호 `()`를 만나면 CPU는 PC의 값을 함수 코드가 저장된 메모리 주소로 강제 변경합니다.
* **제어 흐름:** 이렇게 PC 값을 조절하여 코드의 실행 순서를 바꾸는 것을 **제어 흐름(Control Flow)**이라고 합니다.

#### 3. 스레드(Thread)와의 관계
* **싱글 스레드:** 자바스크립트 엔진 전체에 **PC(커서)가 딱 하나**만 존재한다는 뜻입니다. 
* **결과:** 커서가 하나뿐이므로, 한 번에 두 군데의 코드를 읽을 수 없고, 반드시 현재 가리키는 지점을 해결해야만 다음으로 넘어갈 수 있습니다.

---

### 🎹 최종 비유 요약
* **메모리:** 악보 (데이터 저장소)
* **CPU:** 피아노 (연산 장치)
* **커서(PC):** 연주자의 **시선이 머무는 음표** (다음 실행 위치)
* **스레드:** 시선을 따라 건반을 누르며 나아가는 **연주 흐름 그 자체**

