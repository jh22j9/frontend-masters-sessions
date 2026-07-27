현재 IDE에서 열려 있는 md 파일의 디렉토리를 기준으로, 해당 챕터의 강의에서 쓸 만한 개발/업무 영어 표현을 추출하여 `english.md` 파일을 생성하세요.

## 입력 파일 탐색 규칙

1. 현재 IDE에서 열려 있는 파일의 디렉토리를 확인합니다.
2. 해당 디렉토리에서 `transcript.md`를 읽습니다. (`transcipt.md` 등 오타 파일명도 함께 확인합니다.)
3. 같은 디렉토리의 `note.md`(또는 `{챕터번호}-{주제}.md`)도 함께 읽어 맥락과 용어 표기를 파악합니다.
4. transcript 계열 파일이 없으면 note만으로 추출하되, 원문 인용은 note에 남아 있는 영어 표현으로 한정합니다.

## 출력 파일 규칙

- 경로: 현재 챕터 디렉토리의 `english.md`
- 이미 존재하면 <b>덮어쓰지 않고</b> 중복되지 않는 표현만 각 섹션 표의 아래에 이어서 추가합니다.

## 추출 기준

<b>핵심은 관용구 모음이 아니라, 개발자가 동작·설계를 설명할 때 그대로 꺼내 쓸 수 있는 문장 단위 표현입니다.</b>

우선순위대로:

1. <b>동작 서술</b> — 코드가 무슨 일을 하는지 설명하는 표현
   예: `mutate the input array`, `return out a brand new array`, `the array it's being run on`
2. <b>설계 원칙 서술</b> — 왜 그렇게 해야 하는지 설명하는 표현
   예: `don't change the underlying data`, `have consequences outside of their explicit output`
3. <b>업무 표현</b> — 회의·리뷰·문서에서 쓰는 일반 표현
   예: `dig into the design`, `get hung up on`, `that's not the end of the world`

<b>제외 대상:</b>

- 강의 고유명사, 진행 멘트("let's do that fairly quickly", "hooray" 등)
- 별도 설명 없이 뜻이 자명한 단어 하나짜리 항목 (`array`, `function` 등)
- 해당 강의에만 통하는 임시 변수명·예시 값 (`array 2 is 123` 등)

## 파일 형식

- 제목: `# {강좌명} - {주제} 영어 표현` (H1)
- 섹션은 다음 3개로 고정하며, 해당 챕터에 항목이 없는 섹션은 생략합니다.
  - `## 1. 동작 서술`
  - `## 2. 설계 원칙 서술`
  - `## 3. 업무 표현`
- 각 섹션은 <b>4열 표</b>로 작성합니다: `| 영어 표현 | 한국어 | 예문 | 강의 원문 |`
- `영어 표현` 열은 원형에 가깝게 다듬습니다. (예: "we are going to mutate our input array" → `mutate the input array`)
- `예문` 열은 그 표현을 넣어 <b>실무에서 그대로 말하거나 쓸 수 있는 완결된 영어 문장</b>을 작성합니다.
  - 강의 원문의 구어체·말더듬을 정리한 자연스러운 문장이어야 합니다.
  - 15단어 안팎의 한 문장으로 유지하고, 강의의 임시 변수명 대신 실제 코드에서 쓸 법한 맥락을 씁니다.
  - 메서드명은 백틱으로 감쌉니다. (예: `` `sort` mutates the input array. ``)
- `강의 원문` 열은 transcript에서 <b>실제 문장을 그대로</b> 인용하고 큰따옴표로 감쌉니다. 길면 앞뒤를 `…`로 줄이되 표현이 포함된 구간은 남깁니다.
- 섹션당 5~12개, 챕터 전체로 15~30개를 목표로 합니다.
- 섹션 사이에 `---` 구분선을 넣습니다.
- 표 아래에 필요하면 `> ` 인용문으로 짧은 사용 팁을 1~2줄 덧붙일 수 있습니다.
- 한국어 본문에서 강조는 `**볼드**` 대신 `<b>` 태그를 사용합니다.

## 형식 예시

```markdown
# JavaScript: The Hard Parts v3 - Non-Mutating Array Methods 영어 표현

## 1. 동작 서술

| 영어 표현 | 한국어 | 예문 | 강의 원문 |
|---|---|---|---|
| mutate the input array | 입력 배열을 변경하다 | `sort` mutates the input array, so copy it before you call it. | "we are going to mutate, change our input array" |
| return out a brand new array | 완전히 새로운 배열을 반환하다 | `map` returns out a brand new array and leaves the original untouched. | "Also returns out brand new array, does not change the 123." |

---

## 2. 설계 원칙 서술

| 영어 표현 | 한국어 | 예문 | 강의 원문 |
|---|---|---|---|
| don't change the underlying data | 기반 데이터는 건드리지 않다 | Change how the user sees it, but don't change the underlying data. | "please don't change the underlying" |
```

## 주의사항

- 강의에 실제로 등장한 표현만 다룹니다. 사전에서 가져온 표현을 임의로 추가하지 않습니다.
- 기술 용어 표기는 같은 챕터 노트와 동일하게 맞춥니다.
- 한국어 뜻은 사전식 나열이 아니라, 그 문맥에서 실제로 쓰이는 한 가지 뜻으로 씁니다.
- transcript의 구어체 반복·말더듬은 인용에서 제거하지 말고, 잘라내야 할 때만 `…`를 씁니다.

$ARGUMENTS
