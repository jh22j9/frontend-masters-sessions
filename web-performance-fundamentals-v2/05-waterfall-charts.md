# Web Performance Fundamentals - Measuring Web Performance

## 1. 강의 개요
이 세션에서는 **웹 성능을 어떻게 수치로 측정하는가**를 다룹니다. "내 사이트가 충분히 빠른가?"라는 질문에 답하기 위해 어떤 도구와 지표를 사용하는지 배웁니다.

| 주제 | 설명 |
|------|------|
| 레거시 지표 | 오래전부터 사용해온 성능 지표와 한계점 |
| Core Web Vitals | Google SEO 성능 판단 기준 |
| 기타 지표 | 그 외 관련 지표들 |
| 데이터 수집 방법 | 성능 지표를 어떻게 캡처하는가 |
| 브라우저 지원 현황 | 각 지표의 브라우저 호환성 |
| 워터폴 차트 / 플레임 차트 | 성능 데이터를 시각적으로 읽는 방법 |

## 2. 레거시 지표 vs 코어 웹 바이탈
성능 측정 방식은 크게 레거시 지표와 코어 웹 바이탈로 구분됩니다.

* **레거시 지표**: 인터넷 초창기부터 사용해온 지표로, 페이지가 "빠른지" 판단하는 전통적인 기준입니다. 현재는 한계가 있어 단독 사용이 적합하지 않을 수 있습니다.
* **Core Web Vitals (코어 웹 바이탈)**: Google이 정의한 성능 지표로, SEO 랭킹에 직접적인 영향을 미칩니다. 실제 사용자 경험을 기반으로 한 측정 방식으로, 단순히 "빠른가"가 아니라 사용자가 실제로 느끼는 경험을 측정한다는 점에서 레거시 지표와 차별화됩니다.

## 3. 워터폴 차트 구조 이해
> *"워터폴 차트를 오래전부터 봐왔지만, 정말 제대로 이해한 건 최근 4~5년이다."* — Todd Gardner

**워터폴 차트(Waterfall Chart)** 는 웹 페이지 로딩 과정에서 발생하는 모든 네트워크 요청을 시간 순서대로 시각화한 차트입니다.

* **X축**: 시간 (단위: ms 또는 μs)
* **Y축**: 각 네트워크 요청 (리소스)
* 한눈에 리소스 간 **의존성과 순서**를 파악할 수 있습니다.
* 대략 훑어봐도 60% 정보를 얻을 수 있지만, 세부 구조를 알면 훨씬 더 많은 인사이트를 얻을 수 있습니다.

아래는 단일 리소스(HTML 문서)를 로딩할 때의 워터폴 차트 구조입니다.

![워터폴 차트 기본 구조 — 단일 Document 요청의 각 구간 (Queue/Connecting → Request Waiting → Content Downloading → Waiting on Main)](./images/05-waterfall-chart-basic-structure.png)

| 구간 | 설명 |
|------|------|
| **Start (시작)** | 요청이 필요하다는 것을 인지한 시점입니다. |
| **Queue / Connecting** | 요청을 보내야 하지만 아직 보내지 못하는 대기 시간입니다. 브라우저가 바쁘거나, 큐가 꽉 찼거나, 기타 제약이 있을 때 발생합니다. |
| **Request Waiting** | 서버에 요청을 보내고 응답이 오기까지 기다리는 시간입니다. (TTFB와 관련) |
| **Content Downloading** | 실제 데이터가 스트리밍되어 들어오는 시간입니다. 수신하면서 파싱을 병행합니다. |
| **Waiting on Main** | 데이터를 받았지만 브라우저 메인 스레드가 바빠서 처리를 못 하고 있는 시간입니다. |
| **End (종료)** | 해당 리소스 처리 완료 |

* `Request Waiting` 시간이 길다면 서버 응답 속도 문제, `Content Downloading`이 길다면 파일 크기 문제일 가능성이 높습니다.

워터폴 차트에서 색상은 리소스의 종류를 나타냅니다.

| 색상 | 리소스 타입 | 예시 |
|------|------------|------|
| 🔵 **파란색** | HTML 문서 | `index.html` |
| 🟣 **보라색** | 스타일시트 | `.css` 파일 |
| 🟡 **노란색** | JavaScript | `.js` 파일 |
| 🟢 **초록색** | 이미지 | `.png`, `.jpg`, `.webp` |
| 🩵 **청록색** | 폰트 | `.woff2`, `.ttf` |
| ⬜ **회색** | 기타 | fetch, iframe, 미디어 파일 등 |

## 4. 실제 워터폴 차트 분석
아래는 단순한 웹페이지를 로딩할 때의 워터폴 차트 흐름입니다.

![워터폴 차트 실제 예시 — HTML(파란색) → CSS 2개(보라색) → JS(노란색) + 이미지 2개(초록색) 순서로 로딩되는 의존성 구조](./images/05-waterfall-chart-example.png)


* **HTML 먼저 다운로드** — 브라우저가 HTML을 파싱하면서 필요한 모든 리소스를 발견합니다.
* **스타일시트 병렬 다운로드** — CSS 2개를 동시에 내려받습니다. (렌더 블로킹!)
* **JS + 이미지 다운로드 시작** — CSS 완료 후 시작됩니다. 이 예시에서는 동시 다운로드 용량을 2로 단순화했으므로, JS(1) + 이미지-1(1)로 용량이 꽉 찬 상태입니다.
* **두 번째 이미지 지연** — 용량 초과로 대기하다가 이미지-1이 완료된 후 시작됩니다. (Todd: *"the browser had the capacity of two, which isn't what the browser's capacity actually is, it just works functionally for this example"*)

워터폴 분석 시 핵심 관찰 포인트는 다음과 같습니다.

* 브라우저는 **동시에 다운로드할 수 있는 수가 제한**되어 있습니다. (HTTP/1.1 기준 실제 브라우저는 도메인당 6개이며, 위 예시는 설명을 위해 2개로 단순화한 것입니다.)
* 워터폴이 **길고 직선형**일수록 순차적으로 처리되고 있다는 의미로, 최적화 여지가 있습니다.
* **폭포수처럼 계단식**으로 내려가는 구조가 이상적입니다.
* Chrome DevTools → Network 탭에서 직접 워터폴 차트를 확인할 수 있습니다. "Disable cache"를 켜고 측정하면 실제 최초 방문자 경험을 시뮬레이션할 수 있습니다.

**WebPageTest**를 사용하면 실제 사이트의 워터폴 차트를 온라인에서 분석할 수 있습니다. URL을 입력하면 워터폴 차트, 필름스트립, Core Web Vitals 등 종합적인 성능 리포트를 제공합니다.

* [WebPageTest 분석 결과 — global musinsa](https://www.catchpoint.com/webpagetest/results?publicurl=https%3A%2F%2Fpublic.catchpoint.com%2FUI%2FEntry%2FWPTITP%2FARNE-D-E-B2ABf_MjedMn8GgAA-N)

---

## Q&A

<details>
<summary>워터폴 차트에서 파란색 Document는 무엇을 포함하나요?</summary>

워터폴 차트의 파란색 막대는 **HTML 문서(.html 파일)** 자체를 의미합니다. 서버가 응답으로 내려주는 HTML 파일이며, 다음과 같은 내용을 포함합니다.

* **DOM 구조** — `<html>`, `<head>`, `<body>` 등 페이지의 뼈대
* **리소스 참조** — `<link rel="stylesheet">`, `<script src="">`, `<img src="">` 등 외부 리소스를 불러오는 태그들
* **인라인 콘텐츠** — `<style>`, `<script>` 태그 안에 직접 작성된 CSS/JS

브라우저는 이 Document를 스트리밍으로 받으면서 파싱해 DOM 트리를 구축하고, 리소스 참조를 발견하는 즉시 추가 다운로드를 시작합니다. 워터폴에서 Document 막대 아래로 CSS, JS, 이미지 막대들이 펼쳐지는 이유가 바로 이 때문입니다.

</details>

<details>
<summary>React SPA와 Next.js SSR의 워터폴 차트는 어떻게 다른가요?</summary>

**React SPA** 는 HTML이 빈 껍데기이므로 JS 번들이 실행된 후에야 콘텐츠가 표시됩니다.

```
[React SPA]
1. [🔵 HTML — 껍데기, 매우 짧음         ]
2.    [🟣 CSS]
3.    [🟡 main.bundle.js ━━━━━━━━━━━━━━━]
4.    [🟡 vendor.bundle.js ━━━━━━━━━━━━━]
5.                                       [⬜ /api/products]
6.                                       [🟢 image-1.png]
7.                                       [🟢 image-2.png]

                    ↑ 콘텐츠가 여기서야 보임 (JS 실행 후)
```

**Next.js SSR** 은 서버에서 HTML을 완성해서 내려주므로 Preload Scanner가 이미지를 일찍 발견하고, API 호출 없이 콘텐츠가 즉시 표시됩니다.

```
[Next.js SSR]
1. [🔵 HTML — 실제 콘텐츠 포함, 길음 ━━━━]
2.    [🟣 CSS]
3.    [🟡 main.bundle.js ━━━━━━━━━━━━━━━]   ← hydration용
4.    [🟡 vendor.bundle.js ━━━━━━━━━━━━━]
5.    [🟢 image-1.png]   ← HTML에 있어서 Preload Scanner가 즉시 발견
6.    [🟢 image-2.png]

        ↑ 콘텐츠가 여기서 보임 (HTML 수신 즉시)
```

| | React SPA | Next.js SSR |
|---|---|---|
| HTML 크기 | 작음 (껍데기) | 큼 (콘텐츠 포함) |
| 이미지 시작 시점 | JS 실행 후 (늦음) | HTML 수신 직후 (빠름) |
| API 호출 | 클라이언트에서 별도 fetch | 서버에서 처리 → HTML에 포함 |
| 첫 콘텐츠 노출 | JS 번들 실행 후 | HTML 도착 즉시 |
| JS 역할 | 렌더링 | Hydration (인터랙션 연결) |

</details>
