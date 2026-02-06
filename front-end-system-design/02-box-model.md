# 02-box-model.md

## 1. 박스의 구조 (Anatomy of a Box)

웹 페이지 상의 모든 HTML 요소는 본질적으로 하나의 박스(Box)입니다. 이는 웹사이트의 모든 요소에 테두리(Border)를 적용해 봄으로써 확인할 수 있으며, 대부분의 웹 UI는 서로 결합된 직사각형들의 집합체입니다. 하나의 박스는 다음과 같은 4개의 레이어로 구성됩니다.

![Anatomy of a Box (CSS Box Model)](./images/anatomy-of-a-box.png)

- **콘텐츠 박스 (Content Box):** 실제 텍스트나 이미지 등 콘텐츠가 배치되는 영역입니다.
- **패딩 박스 (Padding Box):** 콘텐츠 박스를 둘러싸는 내부 여백 영역입니다.
- **보더 박스 (Border Box):** 콘텐츠와 패딩을 감싸는 전체 테두리 영역입니다.
- **마진 박스 (Margin Box):** 요소의 테두리 바깥쪽에 위치하는 외부 공간입니다.

이 레이어들은 조합에 따라 일부를 생략하거나 다양한 방식으로 구성하는 것이 가능합니다.

---

## 2. 박스의 주요 속성

### 2.1 박스 크기 (Box Sizing)

박스의 최종 크기는 무엇을 기준으로 하느냐에 따라 크게 두 가지 방식으로 **결정됩니다.**

- **고유 크기에 의해 결정 (Intrinsic Size):**
  - **설명:** 외부에서 별도의 크기 규칙을 적용하지 않은 상태로, 박스의 높이와 너비가 오직 **내부 콘텐츠(텍스트, 이미지 등)의 양**에 의해 결정되는 방식입니다.
  - **FE 관점:** CSS로 `width`나 `height`를 명시하지 않았을 때(기본값 `auto`) 브라우저가 계산하는 방식입니다.

- **제한된 크기에 의해 결정 (Restricted/Extrinsic Size):**
  - **설명:** CSS를 통해 고정된 수치를 적용하거나, **부모 요소의 크기(외부 환경)**에 따라 박스의 크기가 제한을 받는 방식입니다.
  - **FE 관점:** 개발자가 `width: 300px`과 같이 직접 크기를 부여하거나, `flex` 레이아웃 등에서 부모의 공간에 맞춰 크기가 강제로 조정되는 경우입니다.

### 2.2 박스 타입 (Box Type)

브라우저는 렌더링 과정에서 각 HTML 요소로부터 **하나 이상의 박스를 생성(Generate)**하며,
이 박스의 타입에 따라 레이아웃 계산 방식이 달라집니다.

#### 1. 블록 레벨 박스 (Block Level Box)

- **특징:** 부모 요소 너비의 100%를 차지하며, 높이는 내부 콘텐츠 크기에 의해 결정됩니다.
- **배치:** 새로운 라인에서 시작하며 위에서 아래로(Top to Bottom) 수직으로 쌓입니다.
- **BFC(Block Formatting Context) 참여:** 블록 레벨 요소는 BFC라는 독립적인 서식 영역 내에서 배치됩니다.

```css
.block-box {
  /* 1. 박스 타입을 블록으로 명시 (div, p, h1 등은 기본값) */
  display: block;

  /* 2. 크기 조절이 아주 잘 됨 */
  width: 200px;
  height: 100px;

  /* 3. 상하좌우 마진/패딩이 레이아웃에 모두 반영됨 */
  margin: 20px;
  padding: 15px;

  background-color: #3498db;
}
```

- 혼자 한 줄을 다 차지하려고 하며, 개발자가 주는 크기 명령을 아주 잘 따릅니다. 이 박스 옆에는 아무도 올 수 없습니다. 다음 요소는 무조건 다음 줄(New Line)로 쫓겨납니다.

#### 2. 인라인 레벨 박스 (Inline Level Box)

- **특징:** 문자열(String)처럼 취급되어 왼쪽에서 오른쪽으로 흐르듯 배치됩니다.
- **제한:** `width`, `height`, 수직 마진 속성이 무시됩니다.
- **높이:** `line-height`에 의해 결정되며, 이 범위를 벗어나는 영역은 계산에서 제외되는 **Non-context height**가 됩니다.

```css
.inline-box {
  /* 1. 박스 타입을 인라인으로 명시 (span, a, strong 등은 기본값) */
  display: inline;

  /* 2. 중요! width와 height가 무시됩니다. 
        아무리 크게 적어도 내용물 크기만큼만 그려집니다. */
  width: 500px;
  height: 500px;

  /* 3. 좌우 마진/패딩은 적용되지만, 상하 마진은 무시됩니다.
        상하 패딩은 시각적으로는 보일 수 있으나 다른 요소의 위치를 밀어내지 못합니다. */
  margin: 10px 20px;

  background-color: #e74c3c;
}
```

- 문장 속의 단어처럼 행동합니다. 아주 겸손해서 딱 자기 내용물(Content)만큼만 자리를 차지합니다. 공간이 부족하면 다음 줄로 넘어갑니다.

#### 3. 익명 박스 (Anonymous Box)

- **특징:** 블록 레벨 요소 내부의 생 텍스트(Raw text)를 감싸기 위해 브라우저가 내부적으로 생성하는 박스입니다. 개발자가 직접 제어할 수는 없지만 블록 레벨 박스처럼 동작합니다.
- **발생 조건:** 블록 컨테이너 안에 블록 요소와 생 텍스트가 섞여 있을 때, 브라우저가 생 텍스트를 익명 블록 박스로 감싸 모든 자식을 블록 레벨로 통일합니다.

```html
<div>
  나는 텍스트입니다.
  <!-- ← 익명 블록 박스로 감싸짐 -->
  <p>나는 문단입니다.</p>
  <!-- ← 원래 블록 박스 -->
  나도 텍스트입니다.
  <!-- ← 익명 블록 박스로 감싸짐 -->
</div>
```

---

## 3. 블록 서식 맥락 (BFC, Block Formatting Context)

BFC는 블록 박스들이 특정 규칙에 따라 배치되는 독립적인 레이아웃 환경입니다.
BFC는 CSS의 "기능"이 아니라 **CSS 스펙에 정의된 렌더링 규칙**으로, 특정 CSS 속성을 적용하면 부수 효과(side effect)로 생성됩니다.

| BFC를 생성하는 CSS 속성 | 예시                                         |
| ----------------------- | -------------------------------------------- |
| `display: flow-root`    | BFC 생성을 위한 전용 값                      |
| `overflow`              | `hidden`, `auto`, `scroll`                   |
| `display`               | `inline-block`, `flex`, `grid`, `table-cell` |
| `float`                 | `left`, `right`                              |
| `position`              | `absolute`, `fixed`                          |

> 예전에는 `overflow: hidden` 같은 우회적인 방법을 썼지만, 이후 BFC 생성만을 목적으로 하는 `display: flow-root`가 CSS 스펙에 추가되었습니다.

**BFC가 필요해진 배경: float의 문제**

`float`는 원래 신문/잡지처럼 이미지 옆으로 텍스트가 감싸 흐르는 효과를 위해 설계된 CSS 속성입니다. `float`를 적용하면 요소가 Normal Flow에서 이탈하여 좌측/우측에 붙고, 주변 텍스트가 그 빈 공간을 채우며 흐릅니다. 문제는 float된 요소가 Normal Flow에서 빠져나가기 때문에 **부모가 자식의 높이를 인식하지 못해** 부모 높이가 0이 되고, 뒤따르는 요소들이 플로팅 요소와 겹치는 현상이 발생한다는 것입니다. BFC는 이 문제를 해결합니다. 부모에 BFC를 생성하면 내부의 플로팅 자식을 인식하여 높이 계산에 포함시킵니다.

### 3.1 BFC의 역할과 특징

- **격리성:** BFC 내부의 요소 배치는 외부 요소에 영향을 주지 않으며, 외부의 플로팅 요소가 BFC 내부로 침범하지 못합니다.
  <details>
  <summary>예시: 신문 페이지의 광고와 기사</summary>
  - **외부 요소** = 기사 본문 영역 바깥에 있는 플로팅 광고 배너
  - **내부 요소** = 기사 본문 안의 텍스트와 이미지들

  ```html
  <!-- 외부: 페이지 좌측에 떠 있는 광고 -->
  <div style="float: left; width: 150px;">광고 배너</div>

  <!-- 내부: 기사 본문 영역 -->
  <article style="display: flow-root;">
    <h1>기사 제목</h1>
    <p>기사 본문 텍스트...</p>
  </article>
  ```

  `<article>`에 BFC가 없으면 광고 배너가 기사 본문을 침범하여 텍스트가 광고를 감싸며 흐르지만, BFC를 생성하면 기사 본문이 독립적인 레이아웃 환경이 되어 외부 광고와 겹치지 않고 자기 영역을 온전히 지킵니다.

  </details>

- **마진 병합:** 동일한 BFC에 속한 인접 박스 간에는 수직 마진 병합(Margin Collapsing)이 발생합니다.
  <details>
  <summary>예시: 초기 BFC에서의 마진 병합과 방지</summary>

  모든 웹 페이지는 `<html>`이 만드는 **초기 BFC(Initial BFC)**를 가지고 있어, 별도 설정 없이도 블록 요소들은 같은 BFC에 속합니다.

  ```html
  <!-- 같은 BFC(초기 BFC) → 마진 병합 발생: 간격 30px -->
  <div style="margin-bottom: 30px;">박스 A</div>
  <div style="margin-top: 20px;">박스 B</div>
  ```

  30 + 20 = 50px가 될 것 같지만, 같은 BFC 안이므로 큰 값인 **30px만** 적용됩니다.

  ```html
  <!-- 박스 B를 새로운 BFC로 감싸기 → 마진 병합 방지: 간격 50px -->
  <div style="margin-bottom: 30px;">박스 A</div>
  <div style="display: flow-root;">
    <div style="margin-top: 20px;">박스 B</div>
  </div>
  ```

  박스 B가 다른 BFC에 속하게 되어 마진 병합이 발생하지 않고 50px가 됩니다. 참고로 `flex`/`grid` 컨테이너의 자식에도 마진 병합이 적용되지 않습니다.

  </details>

- **플로팅 인식:** BFC를 생성한 부모 요소는 내부의 플로팅 자식 요소를 인식하여 높이 계산에 포함시킵니다.

### 3.2 코드 예시: BFC 유무에 따른 차이

**1) 플로팅 자식 높이 인식 (BFC 생성으로 해결)**

```html
<div style="border: 2px solid red;">
  <div style="float: left; width: 50px; height: 50px; background: cyan;"></div>
</div>

<div style="display: flow-root; border: 2px solid blue;">
  <div style="float: left; width: 50px; height: 50px; background: cyan;"></div>
</div>
```

**2) 외부 플로팅 요소와의 겹침 방지**

```html
<div style="float: left; width: 100px; height: 100px; background: orange;">
  Float 요소
</div>

<div style="background: lightgrey;">일반 블록 요소</div>

<div style="overflow: hidden; background: lightgreen;">BFC 블록 요소</div>
```

---

## 4. 박스 모델의 수학적 계산 (Box Mathematics)

`box-sizing` 속성에 따라 박스의 실제 크기를 계산하는 수식이 달라집니다.

**예시 상황:** 가로 300px 카드에 패딩 20px, 테두리 10px을 적용해야 한다고 가정합니다.

1.  **content-box (기본값):**
    - 수식: $최종 너비 = width + padding + border$
    - `width`를 **콘텐츠 영역만**으로 해석하므로, padding과 border가 바깥으로 추가됩니다.

    ```css
    .card {
      box-sizing: content-box;
      width: 300px;
      padding: 20px;
      border: 10px solid black;

      /* 최종 너비 = 300 + 20*2 + 10*2 = 360px */
      /* 의도한 300px보다 커져서 레이아웃이 깨질 수 있음 */
    }
    ```

2.  **border-box:**
    - 수식: $최종 너비 = width$ (단, 해당 width 안에 padding과 border가 포함됨)
    - `width`를 **테두리를 포함한 전체 너비**로 해석하므로, 의도한 크기가 그대로 유지됩니다.

    ```css
    .card {
      box-sizing: border-box;
      width: 300px;
      padding: 20px;
      border: 10px solid black;

      /* 최종 너비 = 300px (그대로) */
      /* 콘텐츠 영역 = 300 - 20*2 - 10*2 = 240px로 자동 축소 */
    }
    ```

    - **실무 패턴:** 모든 요소에 `border-box`를 전역 적용하는 것이 일반적입니다.

    ```css
    *,
    *::before,
    *::after {
      box-sizing: border-box;
    }
    ```

---

## Q&A

<details>
<summary><strong>Q: inline-block은 블록 레벨 박스인가, 인라인 레벨 박스인가?</strong></summary>

**A:** `inline-block`은 **인라인 레벨 박스**입니다. 정확히는 **외부적으로는 인라인**, **내부적으로는 블록**으로 동작하는 하이브리드 박스입니다.

- **외부 동작 (Outer Display Type):** 인라인 요소처럼 텍스트 흐름 안에서 왼쪽→오른쪽으로 나란히 배치됩니다. 새로운 줄을 만들지 않습니다.
- **내부 동작 (Inner Display Type):** 블록 박스처럼 `width`, `height`, 수직 `margin`, `padding`을 모두 정상적으로 적용할 수 있습니다.

즉, 순수한 인라인 박스에서는 무시되었던 `width`, `height`, 수직 `margin` 등의 속성을 사용할 수 있으면서도, 블록처럼 줄 전체를 차지하지 않고 인라인 흐름에 참여하는 것이 핵심입니다.

> CSS Display Module Level 3 스펙에서는 `display: inline-block`을 `display: inline flow-root`의 축약형(shorthand)으로 정의합니다. 즉, 외부 디스플레이 타입은 `inline`이고, 내부 디스플레이 타입은 `flow-root`(새로운 BFC를 생성하는 블록 컨테이너)입니다.

</details>

<details>
<summary><strong>Q: 인라인 박스에서 수직 마진(margin-top/bottom)이 무시되는 이유는?</strong></summary>

**A:** 인라인 박스는 **텍스트 흐름(Line Box)의 일부**로 취급되기 때문입니다. 라인 박스의 높이는 오직 `line-height`에 의해 결정되는데, 만약 개별 인라인 요소가 수직 마진으로 라인 높이를 제각각 밀어낼 수 있다면:

1. **줄 간격이 요소마다 불규칙해져** 텍스트 가독성이 파괴됩니다
2. 같은 라인 안의 **다른 인라인 요소들과의 수직 정렬이 깨집니다**
3. 라인 박스 높이 계산이 극도로 복잡해집니다

따라서 CSS 스펙은 인라인 박스의 수직 공간 계산을 `line-height` 하나로 통일하고, `margin-top`/`margin-bottom`은 레이아웃 계산에서 무시하도록 설계했습니다.

> 참고로 수직 **패딩**(`padding-top`/`padding-bottom`)은 시각적으로는 렌더링되지만, 라인 박스 높이를 늘리거나 주변 요소를 밀어내지는 못합니다. 이것이 본문에 언급된 **Non-content height** 영역입니다.

</details>

<details>
<summary><strong>Q: 텍스트가 없는 빈 span에도 line-height가 적용되는가?</strong></summary>

**A:** `display` 타입에 따라 다릅니다.

**`display: inline` (빈 span)**

- 텍스트가 없으면 **라인 박스(Line Box) 자체가 생성되지 않습니다.** `line-height`가 적용될 대상이 없으므로 사실상 높이가 0입니다.
- `padding`이나 `border`를 주면 시각적으로는 보이지만, 라인 박스가 없으므로 주변 요소를 밀어내지 못합니다 (Non-content height).

**`display: inline-block` (빈 span)**

- 텍스트가 없어도 내부에 **블록 컨테이너가 생성**됩니다.
- CSS 스펙에서 정의하는 **"strut"**(보이지 않는 너비 0의 가상 문자)이 라인 박스를 하나 만들어내기 때문에, `line-height`에 의해 높이가 결정됩니다.

```css
/* 둘 다 텍스트 없는 빈 span */
.empty-inline {
  display: inline;
  line-height: 50px;
  background: red;
  /* → 아무것도 보이지 않음 (라인 박스 없음) */
}

.empty-inline-block {
  display: inline-block;
  line-height: 50px;
  background: blue;
  /* → 높이 50px의 파란 박스가 보임 (strut에 의한 라인 박스 생성) */
}
```

정리하면, `line-height`가 작동하려면 **라인 박스가 존재해야** 하고, 빈 `inline` 요소는 라인 박스를 만들지 않기 때문에 `line-height`가 무의미합니다.

</details>

<details>
<summary><strong>Q: display: flex도 BFC를 생성하는데, flex 내부에서도 마진 병합이 일어나는가?</strong></summary>

**A:** 아닙니다. `flex` 컨테이너 내부에서는 마진 병합이 **일어나지 않습니다.** 핵심은 BFC를 "생성하는 것"과 "참여하는 것"을 구분하는 것입니다.

- **외부:** `display: flex`는 블록 레벨 박스로서 초기 BFC에 **참여**
- **내부:** BFC가 아닌 **FFC(Flex Formatting Context)**를 **생성**

마진 병합은 BFC 내부의 블록 박스들 사이에서만 발생하는 규칙입니다. flex 컨테이너 내부는 FFC이므로 마진 병합 규칙 자체가 적용되지 않습니다. `grid`도 마찬가지로 내부에 GFC(Grid Formatting Context)를 생성합니다.

</details>

<details>
<summary><strong>Q: 그러면 블록 레벨 박스가 "BFC에 참여한다"는 것은 어떤 의미인가?</strong></summary>

**A:** 해당 **BFC의 레이아웃 규칙에 따라 배치된다**는 의미입니다. "참여한다" = "그 맥락의 규칙을 따른다"로 이해하면 됩니다. BFC에 참여하는 박스는 다음 규칙을 따릅니다:

1. **수직으로 쌓인다** — 위에서 아래로 하나씩 배치
2. **인접 마진이 병합된다** — 같은 BFC 안의 이웃 박스와 수직 마진 병합 발생
3. **왼쪽 가장자리가 컨테이너에 정렬된다** — 각 박스의 왼쪽 끝이 부모의 왼쪽 끝에 맞춰짐

이것을 다른 서식 맥락과 비교하면 명확합니다:

|             | BFC에 참여     | FFC에 참여            |
| ----------- | -------------- | --------------------- |
| 쌓이는 방향 | 수직 (위→아래) | 기본 수평 (좌→우)     |
| 마진 병합   | 발생           | 발생하지 않음         |
| 크기 결정   | 부모 너비 100% | flex 규칙에 따라 유연 |

</details>
