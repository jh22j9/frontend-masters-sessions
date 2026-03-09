# Front-End System Design - 브라우저 서식 맥락(Browser Formatting Context) 스크립트 번역

우리는 "브라우저 서식 맥락(Browser Formatting Context)"이라는 용어를 여러 번 언급했습니다. 이제 그것이 무엇인지 이해해 봅시다.

요소가 있고 일종의 영역(Realm)이 있다고 했습니다. 요소가 이 영역에 들어가면 규칙이 적용되는데, 이 규칙들은 꽤 임의적일 수 있습니다. 예를 들어 "요소가 이 영역에 들어가면 부모 너비의 100%를 차지하고, 위에서 아래로 렌더링된다"라고 말할 수 있습니다. 이렇게 하면 기본적으로 블록 서식 맥락을 재현한 것입니다.

그런데 이 예시를 좀 더 복잡하게 만들어서, 서식 맥락 안에 서식 맥락을 만들 수 있을까요? 당연히 가능합니다. 새로운 서식 맥락을 만들고 요소 2를 그 안에 배치하면, 규칙이 동일하다고 가정할 때 해당 요소는 부모 컨테이너 너비의 100%를 차지합니다. 하지만 핵심적인 차이점은 내부 서식 맥락이 외부 서식 맥락과 완전히 격리된다는 것입니다. 요소가 일단 영역에 들어가면, 외부 맥락의 다른 어떤 규칙에도 영향을 받을 수 없습니다.

이 예시를 더 복잡하게 만들어서 인라인 서식 맥락이라고 부를 수 있는 새로운 유형의 서식 맥락을 만들 수도 있습니다. 지금부터 "서식 맥락"이라는 단어를 여러 번 반복할 것입니다. 요소 3이 이 맥락에 들어갈 수 있고, "요소가 인라인 서식 맥락에 들어가면 왼쪽에서 오른쪽으로 렌더링되며, 포함하는 콘텐츠의 너비만큼만 차지해야 한다"라고 말할 수 있습니다. 요소 3이 전체 너비를 차지하지 않기 때문에, 요소 4가 맥락에 나타나면 문자열처럼 한 줄에 나란히 렌더링됩니다.

---

Evgenii: 그러면 서식 맥락의 핵심 개념은 무엇일까요? 첫 번째는 격리성(Isolation)입니다. 맥락 내의 요소들은 외부 맥락의 규칙으로부터 차폐됩니다. 또한 확장성(Scalability)입니다. CSS 스펙 개발자들이 요소에 적용할 새로운 규칙 집합을 도입하고 싶을 때, 새로운 맥락을 생성합니다. 가장 잘 알려진 것이 flex나 grid 같은 것입니다. 그리고 예측 가능성(Predictability)도 있습니다. 엄격한 규칙 집합이 있으면 요소의 위치와 배치를 항상 알 수 있습니다.

---

이제 브라우저가 되어 다음 HTML 페이지를 한 줄씩 읽으면서, 브라우저가 하는 것처럼 서식 맥락을 생성해 봅시다.

HTML부터 시작합니다. HTML 태그는 페이지를 시작할 때 항상 새로운 서식 맥락을 생성합니다. 그런데 body 태그를 렌더링할 때, body에는 특별한 속성이 지정되어 있지 않으므로 body는 서식 맥락에 진입하기만 하고 새로운 것을 생성하지는 않습니다. heading도 마찬가지이고, 정렬 목록(ordered list)도 마찬가지입니다.

하지만 목록 항목(list element)에 이르면, display: inline-block이 적용된 것을 볼 수 있습니다. 요소에 display에 대한 명시적 스타일 속성이 설정되어 있으면, 이는 브라우저에게 새로운 서식 맥락을 생성해야 한다고 지시하는 것입니다. 이 경우 inline-block은 블록 서식 맥락의 특수한 경우입니다. 인라인 블록 서식 맥락이라는 것은 없습니다. 따라서 새로운 서식 맥락을 시작하고, 그 안에 배치되는 모든 요소는 인라인 블록 항목으로 렌더링됩니다. 다음 목록 항목, 문단 3, 문단 4에도 동일하게 적용됩니다.

이제 이 section을 렌더링합니다. section을 렌더링할 때, display: flex가 적용된 것을 볼 수 있습니다. 이는 플렉스 서식 맥락이 사용된다는 의미이므로, 모든 항목이 플렉스 아이템이 됩니다. 그런데 이제 span 요소가 있고, span은 인라인 서식 맥락을 생성한다고 논의했던 것을 기억하세요. span 내부에 배치되는 모든 것은 인라인 콘텐츠가 되므로, 새로운 인라인 서식 맥락이 생성됩니다.

이것이 브라우저가 다양한 유형의 맥락을 구성하고 요소를 렌더링하는 방식입니다.

---

수강생 2: 맥락은 HTML 태그 자체가 설정하는 것인가요, 아니면 CSS와 더 관련이 있는 것인가요? CSS로 일부 태그의 기본 동작을 덮어쓸 수 있잖아요, 맞죠?

Evgenii: HTML 페이지를 시작하면 기본적으로 항상 블록 서식 맥락이 설정됩니다. 이것이 기본 동작입니다. 하지만 HTML 요소에 스타일을 설정하면 이를 재정의할 수 있다고 생각합니다. flex로 설정하면 플렉스 서식 맥락이 되어야 할 것입니다.

수강생 2: 아, 그러면 CSS에 의해 결정되는 것이군요?

Evgenii: 네.

수강생 2: 알겠습니다. 태그는 단지 기본값을 제공하는 것이군요, 이해했습니다.

---

## 원문 스크립트 (English)

So we mentioned the term browser formatting context multiple times. So let's understand what is that. So we mentioned that you have an elements and some kind of a realm. So, when the element falls into this realm, the rules are applied, and the rules can be pretty random. So we can say, okay, if the element falls into the realm, then, for instance, the element should take 100% of the parent width, and it also should be rendered from top to bottom. So we basically replicated the block formatting context.

But can we slightly complicate this example and say create a formatting context within the formatting context? And apparently, we can do that. So, we can create a new formatting context, then place the element 2, and we assume that the rules are the same, and the element will take the 100% of the parent container width. But the key difference here that the inner formatting context is completely isolated from the outer formatting context. Once element enters the realm, so it cannot be impacted by any other rules of outer context.

We can also complicate this example by creating a new type formatting context, which we can call inline formatting context. So, and I'm gonna repeat the word formatting context multiple times now. So, the element 3 may enter the context and we can say, okay, when the element enters the inline formatting context, it should be rendered from left to right, and it should take the width of the content it contains. And since the element 3 doesn't span full width, so when the element 4 appears in the context, so they render in a row as a string.

Evgenii: So, what are the key ideas of the formatting context? The first one is isolation, so elements within the context are shielded from the rules of the external context, it's also the scalability. The CSS spec developers, when they want to introduce a new rule set that apply to elements, they create a new context, and the most known one is like flex or the grid. And it's also the predictability. So when you have a strict set of rules, you always know the position of the element and the placement.

So it's time to become a browser and try to render the following HTML page going line by line and creating the formatting context like the browser does. So, we start with the HTML. So HTML tag, when you initiate the page, always creates a new formatting context. But when we render the body tag, since body doesn't have any special properties assigned to that. So, the body just enters the formatting context, but it doesn't create a new one. So, same applies to the heading, and same applies to an ordered list.

But when it comes to the list element, you see that we applied display inline block. So, when the element has explicit property set for the style for display. This means that this instructs the browser that we need to create a new formatting context. In this case, inline block is the special case of the block formatting context, so there is no inline block formatting context. So we initiated this new formatting context and every element placed now within that will be rendered as an inline block item. And the same applies to the next list item, to the paragraph three and paragraph four.

So, now we render this section. So when we render this section, we see that there is a display flex applied. This means that the flex formatting context is used, so every item becomes the flex item. But one thing now we have the span element, and remember we discussed that span creates the inline formatting context. So whatever thing that is placed inside the span becomes inline content, so we created in a new inline formatting context.

So, this is how browser constructs the different type of contexts, and how the elements are rendered.

Speaker 2: So, can you clarify the context set by the HTML tags themselves or does it have more to do with this CSS? Cuz you could overwrite the default behavior of some of these tags with CSS, right?

Evgenii: So, when you initiate the HTML page, so it always sets the block formatting context by default, this is the default behavior. But I think you can override this if you set the style to HTML element. So if you reset the flex, then it will have to become the flex formatting context.

Speaker 2: Okay, so it's more about the CSS?

Evgenii: Yeah.

Speaker 2: Okay, the tags just give you the defaults, got it.
