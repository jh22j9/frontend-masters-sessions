# JavaScript: The Hard Parts v3 - Non-Mutating Array Methods

## 1. 보너스 확장: 최신 JavaScript 배열 메서드

이번 내용은 <b>보너스 확장</b>입니다. 앞선 챕터들처럼 메모리 다이어그램을 세세하게 그려가며 다루지는 않지만, 최신 JavaScript 기능들을 살펴보는 것은 흥미롭고 유익합니다.

Hard Parts는 결코 <b>기능 목록(laundry list of features)</b>을 나열하는 강좌가 아닙니다. Hard Parts는 <b>정신 모델(mental model)</b>에 관한 것입니다. 그러니 이 챕터는 순전히 "JavaScript가 계속 진화하고 있다"는 사실을 확인하는 작은 확장으로 받아들이면 됩니다.

이 모든 새 기능들에 대한 올바른 태도는 다음과 같습니다.

> ❌ "세상에, 이 언어가 점점 따라가기 어려워지는군."
> ✅ "좋아, 필요할 때 문서에서 찾아 읽어볼 수 있는 내장 함수가 더 생겼구나."

이런 함수들에 대해서는 <b>MDN 같은 공식 문서</b>나 ChatGPT를 통해 알아볼 수 있습니다.

---

## 2. 왜 원본 배열을 변경하면 안 되는가

앞서 만든 `copyArrayAndManipulate`는 각 요소에 변경이 적용된 <b>완전히 새로운 배열</b>을 만들어냈고, <b>원본 배열은 변경하지 않았습니다</b>. 방금 본 내장 `map`도 동일합니다.

```javascript
const myArray = [1, 2, 3];
const sameResult = myArray.map(input => input * 2);

// sameResult → [2, 4, 6]
// myArray    → [1, 2, 3]  ← 그대로 유지!
```

이것이 <b>매우 중요한</b> 이유는, 함수가 <b>명시적인 출력(explicit output) 바깥에서 어떤 결과도 일으키지 않기를</b> 원하기 때문입니다. 입력 데이터를 변경한다면, 그것은 코드의 정의 바깥 어딘가에서 벌어지는 <b>부수 효과(side effect)</b>이며, 그 결과는 코드 정의 안에 명시적으로 드러나 있지 않습니다.

### UI 환경에서 특히 중요한 이유

트윗 목록이나 Spotify 노래 목록을 <b>역순으로 화면에 표시</b>하고 싶다고 가정해봅시다. 이때 `reverse`를 실행해서 <b>기반 데이터(underlying data) 자체</b>가 뒤집혀 버린다면 곤란합니다.

거의 대부분의 경우, 데이터를 뒤집는 목적은 <b>표시하기 위해서</b>이거나 <b>다른 평가로 전달하기 위해서</b>이지, 데이터의 본질 자체를 바꾸기 위한 것이 아닙니다.

특히 UI가 많은 환경에서는 <b>데이터 자체를 조작하려는 것이 아니라, 데이터를 가져와 표시를 위해 조작</b>하는 경우가 대부분입니다. 사용자가 보는 방식은 끊임없이 바꾸게 되지만, 그 기반 데이터는 건드리지 말아야 합니다.

---

## 3. 변경(Mutating) 메서드: reverse, sort, splice

JavaScript 초창기부터 존재해온 주요 배열 메서드 중 일부는 <b>메서드가 실행되는 대상 배열 자체를 직접 변경</b>합니다.

> 2015년 ES6에서 대대적인 변화의 물결이 일었을 때에도 "그건 우리의 변경(mutating) 배열 메서드고, 필요한 만큼 그대로 두겠다"는 식으로 남겨졌습니다. 강사의 표현을 빌리면 이것은 <b>범죄(crime)</b>에 가깝지만, 당황할 필요는 없습니다.

<b>대표적인 변경 메서드 3종:</b> `reverse`, `sort`, `splice`

![mutating array methods memory diagram](./images/09-mutating-array-methods.png)

```javascript
const array1 = [1, 2, 3];

array1.reverse();       // array1 → [3, 2, 1]
array1.splice(1, 1, 6); // array1 → [3, 6, 1]
array1.sort();          // array1 → [1, 3, 6]

// 원본 [1, 2, 3]은 완전히 사라졌습니다.
```

### 단계별 추적

| 호출 | 동작 | `array1` 결과 |
|---|---|---|
| 초기 | — | `[1, 2, 3]` |
| `.reverse()` | 입력 배열을 뒤집어 <b>덮어씀</b> | `[3, 2, 1]` |
| `.splice(1, 1, 6)` | 인덱스 1에서 1개 요소를 제거하고 `6`을 삽입 | `[3, 6, 1]` |
| `.sort()` | 숫자 순서 체계에 따라 자동 정렬 | `[1, 3, 6]` |

`array1.reverse()`를 호출하면 <b>뒤집힌 새 배열을 반환값으로 받아 새 라벨에 할당하는 것이 아니라</b>, 입력 배열 자체가 변경됩니다.

<details>
<summary>splice 인자 읽는 법</summary>

`splice`는 <b>변경하고자 하는 위치</b>와 <b>변경할 요소의 개수</b>를 받고, 그 자리에 넣을 값을 받습니다.

`array1.splice(1, 1, 6)` → "인덱스 1에서, 1개 요소를, 숫자 6으로 교체하라"

강사도 언급했듯이 splice 메서드의 설계 자체를 깊이 파고드는 것이 이 챕터의 목적은 아닙니다.

</details>

---

## 4. 비변경(Non-Mutating) 대응 메서드: toReversed, toSpliced, toSorted

다행히 JavaScript는 <b>30년도 채 되지 않아</b>(정말 빠른 업데이트죠) 입력 배열을 변경하지 않는 새로운 배열 메서드들을 추가했습니다.

`toReversed`, `toSpliced`, `toSorted` <b>내부에서 무슨 일이 일어나는지</b>는 우리가 만든 `copyArrayAndManipulate`와 정확히 동일합니다. 즉, <b>내부에서 완전히 새로운 배열을 만들고, 그 안에 변경된 요소들을 채워 넣습니다</b>.

![non-mutating array methods memory diagram](./images/09-non-mutating-array-methods.png)

```javascript
const array2 = [1, 2, 3];

const reversed = array2.toReversed();       // [3, 2, 1]
const spliced  = array2.toSpliced(1, 1, 6); // [1, 6, 3]
const sorted   = array2.toSorted();         // [1, 2, 3]

// array2 → [1, 2, 3]  ← 그대로 고정!
```

| 라벨 | 호출 | 결과 | 원본 `array2` |
|---|---|---|---|
| `reversed` | `array2.toReversed()` | `[3, 2, 1]` | `[1, 2, 3]` |
| `spliced` | `array2.toSpliced(1, 1, 6)` | `[1, 6, 3]` | `[1, 2, 3]` |
| `sorted` | `array2.toSorted()` | `[1, 2, 3]` (이미 오름차순) | `[1, 2, 3]` |

### 변경 vs 비변경 비교

| 구분 | 변경(Mutating) | 비변경(Non-Mutating) |
|---|---|---|
| 뒤집기 | `reverse()` | `toReversed()` |
| 정렬 | `sort()` | `toSorted()` |
| 삽입/삭제 | `splice()` | `toSpliced()` |
| 원본 배열 | <b>변경됨</b> | <b>유지됨</b> |
| 반환값 | 변경된 원본(또는 제거된 요소) | <b>완전히 새로운 배열</b> |

> <b>결과 비교:</b> `array1`은 이 시점에 완전히 변경되어 `[1, 3, 6]`이 되었고 원본 `[1, 2, 3]`은 사라진 지 오래입니다. 반면 `array2`는 여전히 `[1, 2, 3]`이며, 뒤집기·splice·정렬은 각각 <b>새로운 배열</b>에서 수행되었습니다.

---

## 5. 메서드 체이닝: 라벨을 따로 저장할 필요가 없다

더 좋은 점은, 중간 결과를 <b>모두 별도의 라벨에 저장할 필요가 없다</b>는 것입니다. `toReversed`의 출력이 `[3, 2, 1]`이라는 것을 알기 때문에, 그 결과에 곧바로 `toSpliced`를, 다시 그 결과에 `toSorted`를 호출할 수 있습니다.

```javascript
const array2 = [1, 2, 3];

const result = array2
  .toReversed()        // [3, 2, 1]  ← 새 배열
  .toSpliced(1, 1, 6)  // [3, 6, 1]  ← 또 새 배열
  .toSorted();         // [1, 3, 6]  ← 또 새 배열

// array2 → [1, 2, 3]  ← 여전히 고정!
```

매번 "변경된 배열"에 대해 실행하지만, 그것은 <b>매번 완전히 새로운 배열</b>이므로 원본 `array2`는 그대로 고정되어 있습니다.

> 강사는 "사실 처음부터 이렇게 보여줬어야 했다"고 언급합니다. 모든 변경을 연쇄적으로 수행하면서도 `array2`는 그대로 유지된다는 점이 핵심이기 때문입니다.

---

> 📌 <b>여기서부터 논지가 바뀝니다.</b> 섹션 1~5는 <b>"변경 vs 비변경"</b>이 주제였습니다. 섹션 6부터는 <b>"JavaScript의 배열 메서드는 계속 발전하고 있다"</b>와 <b>"고차 함수가 도처에 있다"</b>로 초점이 옮겨갑니다. 강사의 표현대로 "지속적인 혁신이 있다는 것을 보여주려는 것"이며, 그래서 챕터가 <b>"고차 함수는 어디에나 있다"</b>로 끝납니다.
>
> | 섹션 | 메서드 | 비변경 사례 | 고차 함수 사례 |
> |---|---|---|---|
> | 6 | `flat` | ✅ | ❌ (인자가 숫자) |
> | 7 | `findLastIndex` | (해당 없음) | ✅ |
> | 8 | `Object.groupBy` | ✅ | ✅ |
>
> `flat`은 두 논지를 잇는 다리이고, `findLastIndex`는 <b>mutation과 무관한 순수 조회 메서드</b>입니다 (아래 섹션 7 참고).

## 6. flat: 중첩 배열 평탄화 (ES2019)

`flat` 역시 확실히 <b>비변경(non-mutating)</b> 메서드입니다.

> 강사의 말: "장담하건대 이것이 옛날에 만들어졌다면 100% 원본 배열을 변경했을 것입니다."

배열 내부에 다른 배열이 들어 있을 때 `flat`을 호출하면, 그 <b>하위 배열(subarray)을 꺼내어 메인 배열의 개별 요소들로 펼쳐줍니다</b>.

```javascript
const deepArray = [1, 2, [1, 2, 2]];

const flattened = deepArray.flat();
// → [1, 2, 1, 2, 2]
```

### depth 인자

| 호출 | 의미 |
|---|---|
| `.flat()` | 기본값 <b>1단계</b>만 평탄화 |
| `.flat(2)` | 2단계 깊이까지 평탄화 |
| `.flat(Infinity)` | 몇 단계든 <b>끝까지</b> 평탄화 |

> `Infinity`라는 이름에 대해 강사는 "정말 형편없는 이름"이라고 평합니다. 하지만 여러 겹의 하위 배열을 다룰 때 원하는 만큼 깊이 들어가도록 값을 전달할 수 있습니다.

<details>
<summary>Q: depth는 몇 개의 층이 밖으로 "나오는지"를 말하나요, 몇 개를 "풀어내고 싶은지"를 말하나요?</summary>

<b>바로 그것입니다.</b> depth만큼의 층이 밖으로 나오게 되는 것이며, 그것이 "한 층을 꺼낸다"는 의미의 평탄화입니다.

</details>

---

## 7. findLastIndex: 뒤에서부터 찾기 (ES2023)

> ⚠️ <b>이 메서드는 mutation 논쟁의 대상이 아닙니다.</b> `findLastIndex`는 배열을 훑어 인덱스 하나를 돌려주는 <b>순수 조회(read-only) 메서드</b>이므로, 바꿀 대상이 애초에 없습니다. 여기 등장하는 이유는 ① ES2023 신규 기능이라는 연표상의 데이터 포인트, ② <b>콜백을 인자로 받는 고차 함수</b> 예시 두 가지입니다.

배열에서 어떤 값이 <b>마지막으로 위치한 인덱스</b>를 찾을 수 있게 해주는 메서드입니다. `findIndex`가 앞에서부터 찾는 것과 달리, `findLastIndex`는 <b>마지막 요소부터 거꾸로</b> 거슬러 올라갑니다.

```javascript
const flattened = [1, 2, 1, 2, 2];

const lastIndex = flattened.findLastIndex(element => element === 2);
// → 4
```

<b>동작 과정:</b>

1. <b>익명 함수(anonymous function)</b>를 인자로 전달합니다.
2. 배열의 <b>마지막 요소부터</b> 거꾸로 각 요소를 받아 함수를 실행합니다.
3. 요소가 `2`와 같으면 `true`를, 아니면 `false`를 반환합니다.
4. 첫 번째로 `true`가 나오는 순간(= 뒤에서 처음 만나는 `2`), 그 <b>인덱스를 반환</b>합니다.

| 인덱스 | 0 | 1 | 2 | 3 | <b>4</b> |
|---|---|---|---|---|---|
| 값 | 1 | 2 | 1 | 2 | <b>2</b> |
| 탐색 순서 | ← | ← | ← | ← | <b>시작</b> |

5개 요소 배열 `[1, 2, 1, 2, 2]`에서 뒤에서부터 처음 만나는 `2`는 인덱스 `4`이므로, `4`가 반환됩니다.

---

## 8. Object.groupBy: 조건별 그룹화 (ES2024)

가장 최근에 출시된 것으로, 사실 <b>SQL에서 매우 인기 있는 함수</b>입니다. 이 역시 다른 함수를 인자로 전달받는 <b>고차 함수(higher-order function)</b>이며, 입력 배열을 변경하지 않습니다.

```javascript
const flattened = [1, 2, 1, 2, 2];

const grouped = Object.groupBy(flattened, num =>
  num % 2 === 0 ? 'even' : 'odd'
);

// → { odd: [1, 1], even: [2, 2, 2] }
```

<b>동작 원리:</b>

- 값들의 목록을 받아, 전달한 함수로 <b>각 요소를 평가</b>합니다.
- 함수가 <b>반환하는 라벨의 개수만큼</b> 결과 객체의 키가 만들어집니다.
- `'even'`으로 평가된 값은 `even` 배열로, `'odd'`로 평가된 값은 `odd` 배열로 들어갑니다.

> `groupBy`는 내부적으로 꽤 <b>정교한 함수</b>입니다. 여기서는 라벨이 `even`, `odd` 두 개였지만, "이 조건이면 이 라벨, 이 조건이면 이 라벨, 그 외에는 이 라벨"처럼 <b>3개 이상의 라벨</b>을 반환하면 결과 객체에도 3개의 키가 생성됩니다.

---

## 9. 요약: 고차 함수는 어디에나 있다

이 챕터의 목적은 각 메서드를 하나하나 짚어보는 것이 아니라, <b>고차 함수(higher-order function)가 JavaScript 배열 메서드의 주요한 부분을 차지한다</b>는 점을 확인하는 것입니다.

고차 함수는 <b>읽기 좋은 코드(readable code)</b>를 만들어줍니다.

```javascript
Object.groupBy(array, condition)
// "이 배열을, 이 조건으로 그룹화하라" 라고 그대로 읽힙니다.
```

### 이번 챕터에서 다룬 메서드 정리

| 메서드 | 도입 시기 | 역할 | 원본 변경 |
|---|---|---|---|
| `reverse` / `sort` / `splice` | 초기부터 | 뒤집기 / 정렬 / 삽입·삭제 | ⚠️ <b>변경함</b> |
| `toReversed` / `toSorted` / `toSpliced` | ES2023 | 위 3종의 비변경 버전 | ✅ 유지 |
| `flat` | ES2019 | 중첩 배열 평탄화 | ✅ 유지 |
| `findLastIndex` | ES2023 | 뒤에서부터 인덱스 탐색 | — (조회 전용) |
| `Object.groupBy` | ES2024 | 조건별 그룹화 | ✅ 유지 |

<details>
<summary>"원본 변경" 축이 적용되는 메서드와 아닌 메서드</summary>

배열 메서드는 세 부류로 나뉘며, <b>mutation 논쟁이 성립하는 것은 첫 번째 부류뿐</b>입니다.

<b>1. 변형해서 결과를 만드는 메서드</b> — 같은 일을 하는데 원본을 건드릴지 말지 <b>선택지가 존재</b>합니다.

```javascript
reverse / sort / splice   // 변형 O, 원본 파괴 O  ← 문제
map / filter / flat       // 변형 O, 원본 파괴 X  ← 정답
```

<b>2. 읽기만 하는 메서드</b> — 인덱스나 불리언 하나를 돌려줄 뿐이라 바꿀 대상이 없습니다. "비변경"이 칭찬이 되지 못하고, 자동으로 참인 값입니다.

```javascript
indexOf / includes / find / findLastIndex / at / some / every
```

<b>3. 변경이 목적인 메서드</b> — 이름부터 변경을 선언하고 있고, 보통 방금 내가 만든 배열에 쓰므로 문제가 없습니다.

```javascript
push / pop / shift / unshift
```

</details>

### 표준 모델

`copyArrayAndManipulate`가 그러했듯, 방대한 양의 JavaScript 배열 메서드는 다음과 같은 <b>표준 모델</b>을 따릅니다.

1. <b>완전히 새로운 배열</b>을 만든다.
2. 각 요소에 적용될 <b>지시사항(함수)</b>을 인자로 받는다.
3. 변경된 모든 요소를 담은 <b>새 배열을 반환</b>한다.
4. <b>입력 배열은 절대 건드리지 않는다.</b>

---

## Q&A

<details>
<summary>UI를 위해 데이터를 변경해도 DB는 수정되지 않으니, API 응답에는 이전 데이터가 유지되는 것 아닌가요? 여기서 말하는 "기반 데이터"는 풀스택 관점의 DB를 뜻하나요?</summary>

DB가 아니라 <b>클라이언트 메모리 안의 원본 배열</b>을 뜻합니다. 순수 FE 관점의 이야기입니다.

`array.reverse()`는 DB에 아무 영향이 없고, 리페치하면 서버는 원래 순서대로 다시 응답합니다. 문제는 <b>리페치 전까지, 같은 배열 참조를 공유하는 모든 코드</b>입니다.

```javascript
const { data: tweets } = useQuery(['tweets'], fetchTweets);
// tweets === 캐시가 들고 있는 바로 그 배열 (복사본이 아님)

const reversed = tweets.reverse(); // 캐시 안의 배열을 직접 뒤집어버림
```

`reverse()`는 새 배열을 만들지 않고 그 자리에서 뒤집으므로 <b>캐시가 오염</b>됩니다. DB는 멀쩡한데 클라이언트가 들고 있는 "서버 상태의 사본"이 망가진 것입니다.

<b>FE에서 실제로 터지는 지점들</b>

1. <b>다른 컴포넌트가 같은 데이터를 볼 때</b> — 같은 쿼리 키를 구독하는 두 컴포넌트가 하나의 배열을 공유하므로, 한쪽의 mutation이 다른 쪽 렌더 결과를 바꿉니다.

2. <b>리렌더가 아예 일어나지 않음</b>

```javascript
const [items, setItems] = useState([3, 1, 2]);

items.sort();        // 내용은 바뀌었지만
setItems(items);     // 참조가 동일 → React가 "안 바뀜"으로 판단, 리렌더 스킵
```

React는 `Object.is`로 이전 상태와 비교하므로, 같은 참조를 다시 넣으면 무시합니다.

3. <b>메모이제이션이 깨짐</b> — `useMemo(() => items.sort(), [items])`처럼 참조가 그대로면 재계산은 안 되는데 내용은 이미 변형되어 있고, `React.memo`로 감싼 자식도 props 참조가 같아 업데이트를 못 받습니다.

4. <b>재실행 시 결과가 달라짐</b> — StrictMode의 이중 렌더처럼 같은 코드가 두 번 돌면 `reverse()`는 원래대로 되돌아옵니다. 순수 함수라면 몇 번을 실행해도 같은 결과여야 합니다.

<b>정리</b>

강의의 요지는 "DB를 지키자"가 아니라, <b>클라이언트 메모리 안의 진실의 원천(source of truth)을 표시 로직이 건드리지 못하게 하자</b>는 원칙입니다. 서버 상태는 리페치로 복구되지만, 그 사이에 발생한 잘못된 렌더링·틀린 계산 결과·사용자가 이미 누른 액션은 되돌릴 수 없습니다. Redux Toolkit이 Immer를 쓰고 React가 상태 불변성을 요구하는 것도 같은 이유입니다.

</details>

<details>
<summary>toReversed가 없고 reverse만 존재하던 시절, FE 개발에서는 어떻게 대응했나요?</summary>

`toReversed`는 ES2023에 추가되었으므로, 그 전까지는 <b>"먼저 복사하고, 복사본을 변경한다"</b>는 관용구를 사용했습니다.

<b>1. 복사 후 변경</b>

ES5 시절에는 `slice()`가 사실상 "배열 복사" 관용구였습니다.

```javascript
var reversed = array.slice().reverse();
var sorted   = array.slice().sort();
// array.concat().reverse() 형태를 쓰기도 했습니다.
```

ES6 이후에는 의도가 더 명확한 스프레드 문법이 표준이 되었습니다.

```javascript
const reversed = [...array].reverse();
const sorted   = [...array].sort((a, b) => a - b);
// Array.from(array).reverse() 도 동일한 역할입니다.
```

<b>2. toReversed와의 차이</b>

| 코드 | 동작 |
|---|---|
| `[...array].reverse()` | 새 배열 생성 → 그 새 배열을 mutate → 반환 |
| `array.toReversed()` | 내부에서 새 배열을 만들어 반환 |

<b>결과는 동일합니다.</b> 다만 `toReversed`는 "복사"라는 중간 단계가 메서드 이름 안에 감춰져 더 읽기 좋고, 복사를 깜빡할 여지가 없습니다. 반면 `[...array].reverse()`는 <b>스프레드를 하나 빠뜨리는 순간 원본이 변경되는데, 코드는 여전히 동작하는 것처럼 보입니다</b>. 이것이 실무에서 흔한 버그 원인이었습니다.

```javascript
const reversed = array.reverse();   // ← 스프레드 누락. 동작은 하지만 array도 뒤집힘
```

<b>3. 도구로 실수를 방지</b>

- <b>ESLint</b>: `no-param-reassign`(`{ props: true }`), `eslint-plugin-fp`, `eslint-plugin-immutable` 등으로 mutation 자체를 금지
- <b>Object.freeze</b>: 개발 모드에서 데이터를 얼려 mutation 시도 시 strict mode에서 `TypeError` 발생
- <b>Immutable.js (2014, Facebook)</b>: 불변 자료구조를 별도 타입으로 제공. React 초기에 널리 쓰였으나 `list.get(0)`처럼 일반 배열과 API가 달라 학습·상호운용 비용이 커서 지금은 거의 쓰이지 않습니다.
- <b>Immer (2018)</b>: Proxy로 "변경하는 것처럼 보이는 코드"를 받아 실제로는 새 객체를 생성. Redux Toolkit이 내장한 이유입니다.

```javascript
const next = produce(state, draft => {
  draft.items.reverse();  // 변경 문법을 그대로 쓰지만
});                       // state는 유지되고 next만 새로 생성됩니다.
```

<b>4. 현재</b>

`toReversed` / `toSorted` / `toSpliced`는 브라우저 지원이 충분해졌지만(2023년 이후 버전), 구형 환경을 지원해야 한다면 core-js 폴리필이 필요합니다. 실무에서는 여전히 `[...array].sort()` 쪽을 더 자주 보게 되는데, 두 방식 모두 <b>원본을 건드리지 않는다</b>는 점에서는 동일하므로 어느 쪽이든 맞습니다.

</details>

<details>
<summary>초창기 JavaScript는 왜 배열을 직접 변경하도록 설계되었나요? 직접 변경이 바람직한 상황도 있나요?</summary>

<b>왜 그렇게 설계되었나</b>

<b>1. 1995년의 메모리 제약</b> — JavaScript는 1995년 Brendan Eich가 10일 만에 만들었고, 당시 브라우저가 쓸 수 있는 메모리는 수 MB 수준이었습니다. 배열을 뒤집기 위해 같은 크기의 배열을 하나 더 만드는 것은 실제 비용이었고, 제자리(in-place) 알고리즘은 추가 메모리 없이 동작하므로 자연스러운 선택이었습니다.

<b>2. 당시 언어들의 관행</b> — C++의 `std::reverse`, Java의 `Collections.reverse` 모두 제자리 변경 방식이었습니다. `splice`라는 이름 자체가 원본을 변경하는 Perl의 `splice`에서 온 것입니다. 즉 이상한 결정이 아니라 <b>당시의 관행</b>이었습니다.

<b>3. 함수형 패러다임 이전이었다</b> — JS 배열 메서드는 서로 다른 두 시대의 화석이 한 언어에 공존합니다.

| 시기 | 메서드 | 성격 |
|---|---|---|
| 1995 (ES1) | `reverse`, `sort`, `splice`, `push`, `pop` | 명령형 · 제자리 변경 |
| 2009 (ES5) | `map`, `filter`, `reduce`, `forEach` | 함수형 · 비변경 |
| 2023 (ES2023) | `toReversed`, `toSorted`, `toSpliced` | 함수형 · 비변경 |

`map`/`filter`가 표준에 들어온 2009년에는 이미 함수형 스타일이 널리 받아들여진 뒤였습니다. 강의에서 "2015년 ES6 때 왜 안 고쳤냐"고 한탄하는 지점이 이 시간 격차입니다.

<b>4. 고칠 수 없었다 — "Don't break the web"</b> — 이미 수억 개 페이지가 `reverse()`의 변경 동작에 의존하므로 동작을 바꾸면 전부 깨집니다. 그래서 고치는 대신 <b>새 이름을 붙였습니다</b>. 이름 짓기의 위험을 보여주는 사건이 `flat`인데, 원래 이름은 `Array.prototype.flatten`이었으나 MooTools가 같은 이름을 프로토타입에 덧씌워 놓은 탓에 표준 배포 시 사이트들이 깨졌고, 결국 표준 쪽이 `flat`으로 이름을 양보했습니다 (2018년, "SmooshGate").

<b>직접 변경이 바람직한 상황</b>

판단 기준은 하나입니다 — <b>그 배열의 참조를 나만 갖고 있는가?</b>

<b>1. 함수 안에서 만들어 함수 안에서 끝나는 배열</b>

```javascript
function collectEven(nums) {
  const result = [];
  for (const n of nums) {
    if (n % 2 === 0) result.push(n);  // ← mutation이지만 전혀 문제없음
  }
  return result;
}
```

`result`는 함수 밖의 누구도 모르는 배열이므로, `push`는 mutation이어도 <b>외부에서 관찰할 수 없어 부수 효과가 아닙니다</b>. 이를 매번 `[...result, n]`으로 바꾸면 O(n²)가 되어 오히려 나쁩니다.

<b>2. 성능이 실제로 중요한 지점</b> — 10만 개 요소 정렬처럼 복사본 생성이 곧 할당·GC 압력이 되는 경우, 또는 게임 루프·캔버스 렌더링처럼 프레임마다 배열을 새로 만들 수 없는 경우입니다.

<b>3. TypedArray와 버퍼</b>

```javascript
const pixels = imageData.data;  // Uint8ClampedArray
pixels[i] = 255;                // 설계 자체가 제자리 변경 전제
```

오디오 처리, 이미지 픽셀 조작, WebGL 버퍼는 "고정된 메모리 블록을 직접 쓴다"는 것이 존재 이유이므로 불변성을 고집하면 목적에 어긋납니다.

<b>4. Immer의 draft</b> — `produce(state, draft => { draft.items.reverse(); })`에서 `draft`는 Immer가 만든 Proxy이고 변경 기록이 새 객체 생성으로 번역됩니다. 소유권이 명확히 나에게 있는 임시 객체입니다.

<b>정리</b>

원칙은 "불변성이 항상 옳다"가 아니라 <b>"공유된 값을 변경하지 말라"</b>입니다. 강의가 문제 삼는 것도 mutation 자체가 아니라 <b>함수가 명시적 반환값 바깥에서 결과를 일으키는 것</b>입니다.

`reverse`/`sort`/`splice`가 위험한 이유는 mutation을 해서가 아니라, <b>보통 남의 데이터(props, 캐시, 상태)에 대고 호출하게 되는 위치</b>에 있기 때문입니다. 반면 `push`는 대개 방금 내가 만든 배열에 씁니다. 같은 mutation인데 평판이 다른 이유가 여기에 있습니다.

</details>

---

## Quiz

**Q1.** 다음 코드를 모두 실행한 뒤 `array1`의 값은?

```javascript
const array1 = [1, 2, 3];

array1.reverse();
array1.splice(1, 1, 6);
array1.sort();
```

a) `[1, 2, 3]`\
b) `[3, 2, 1]`\
c) `[3, 6, 1]`\
d) `[1, 3, 6]`

<details>
<summary>정답 보기</summary>

<b>d) `[1, 3, 6]`</b> — 세 메서드 모두 <b>원본을 직접 변경</b>합니다. `reverse()`로 `[3, 2, 1]`, `splice(1, 1, 6)`으로 인덱스 1의 요소를 `6`으로 교체해 `[3, 6, 1]`, `sort()`로 `[1, 3, 6]`이 됩니다. 원본 `[1, 2, 3]`은 이미 사라진 지 오래입니다.

</details>

---

**Q2.** (단답형) `reverse` / `sort` / `splice`에 각각 대응하는 <b>비변경 메서드</b>의 이름과, 이들이 도입된 ECMAScript 버전은?

<details>
<summary>정답 보기</summary>

<b>`toReversed` / `toSorted` / `toSpliced`, ES2023</b> — "Change Array by copy" 제안으로 함께 표준에 들어왔습니다. 같은 제안에 인덱스 대입의 비변경 버전인 `with`도 포함되어 있습니다. 기존 메서드의 동작을 바꾸면 웹이 깨지므로(Don't break the web), 고치는 대신 <b>새 이름을 붙인</b> 결과입니다.

</details>

---

**Q3.** 다음 React 코드에서 화면이 갱신되지 않는 이유는?

```javascript
const [items, setItems] = useState([3, 1, 2]);

items.sort();
setItems(items);
```

a) `sort()`는 비동기라서 `setItems` 호출 시점에 정렬이 끝나지 않았기 때문\
b) `sort()`가 원본을 제자리에서 변경해 참조가 그대로이고, React가 이전 상태와 같다고 판단하기 때문\
c) `sort()`가 새 배열을 반환하는데 그 반환값을 쓰지 않았기 때문\
d) `useState`의 초기값이 배열일 때는 `setItems`로 갱신할 수 없기 때문

<details>
<summary>정답 보기</summary>

<b>b)</b> — `sort()`는 새 배열을 만들지 않고 <b>같은 배열을 그 자리에서</b> 정렬합니다. React는 `Object.is`로 이전 상태와 비교하는데 참조가 동일하므로 "변경 없음"으로 판단해 리렌더를 건너뜁니다. 내용은 이미 바뀌었는데 화면만 그대로인, 디버깅이 까다로운 버그입니다. `setItems(items.toSorted())` 또는 `setItems([...items].sort())`로 해결합니다.

</details>

---

**Q4.** 다음 코드의 실행 결과는?

```javascript
const grouped = Object.groupBy([1, 2, 1, 2, 2], num =>
  num % 2 === 0 ? 'even' : 'odd'
);
```

a) `[[1, 1], [2, 2, 2]]`\
b) `{ odd: [1, 1], even: [2, 2, 2] }`\
c) `{ even: 3, odd: 2 }`\
d) `[{ odd: 1 }, { even: 2 }, { odd: 1 }, { even: 2 }, { even: 2 }]`

<details>
<summary>정답 보기</summary>

<b>b) `{ odd: [1, 1], even: [2, 2, 2] }`</b> — 전달한 함수가 <b>반환하는 라벨의 개수만큼</b> 결과 객체의 키가 만들어지고, 각 라벨로 평가된 값들이 해당 배열에 담깁니다. 라벨을 3개 반환하면 키도 3개가 생성됩니다.

</details>

---

**Q5.** 다음 중 <b>"원본 변경 여부"라는 논쟁 자체가 성립하지 않는</b> 메서드는?

a) `flat`\
b) `toSorted`\
c) `findLastIndex`\
d) `splice`

<details>
<summary>정답 보기</summary>

<b>c) `findLastIndex`</b> — 배열을 훑어 <b>인덱스 하나를 돌려주는 조회 전용 메서드</b>이므로 바꿀 대상이 애초에 없습니다. `flat`·`toSorted`·`splice`는 모두 배열을 변형해 결과를 만들기 때문에 "원본을 건드릴지 말지"의 선택지가 존재하지만, `findLastIndex`는 그 축에 놓이지 않습니다. 이 메서드가 챕터에 등장하는 이유는 <b>ES2023 신규 기능</b>이라는 점과 <b>콜백을 인자로 받는 고차 함수</b> 예시라는 두 가지입니다.

</details>
