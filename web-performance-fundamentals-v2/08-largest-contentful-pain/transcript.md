## 원문 (English)

>> Todd Gardner: Google's Core Web Vitals. We're gonna be on this one for a little while. The Core Web Vitals measure three things, three specific kinds of performance. One, how fast your site visibly loads to the user, not the load event, but how fast the site feels like it has loaded in terms the user viewing it. The second thing is how smooth it loads. How smooth is the sequence of events that happens during that load. And third, how quickly can users interact with your site? And those are measured with three different metrics called the Largest Contentful Paint, or LCP, the Cumulative Layout Shift, or CLS, and Interaction to Next Paint, or INP.

Now, we're gonna go into each one of these in a lot of detail. But first, let's just talk about, so this is important again, because search ranking incorporates the core web vitals and other page experience metrics. These things are ranking factors, these things specifically. So there's other performance metrics, and they're important for various reasons, but these three we talk about a lot in web performance world and in SEO world because they are direct ranking factors.

So let's get into them. LCP, the Largest Contentful Paint, the LCP is how fast your site visibly loads the most important element, that's what it's trying to load. How fast is the most important element on your site load? Now, what's important? What's important element? Who gets to decide what the important thing is? Do you get to tell Google like, hey, this product image is the most important thing? No, Google doesn't trust you because if you got to tell Google what the most important thing is, you'd put some little empty div at the top of the page that loads right away as the most important and you'd have super fast LCP scores, that doesn't work. It needs to be in a way that developers can't influence this in negative ways. It needs to be about what the user sees as the important thing.

And so, instead of important, we actually measure the largest. The largest thing by pixel area on the page. But wouldn't that just be the body elements? No, there's only specific things we care about. We care about these things, we look at images, videos, CSS background images, and any DOM element that contains text. We look at all of those elements and we try and decide what the largest is.

But there's some rules, because not every one of these elements, even though they exist on the page, would be visible or interesting to the user. So there's some rules that apply to this. One is, it can't have a 0 opacity. If you've made the element transparent, it doesn't count for LCP, you can't trick it, right? Otherwise, we can take an element, make it really big, make its opacity 0, and it would always be our LCP element and we could hack the metric. Its size needs to be smaller than the whole page. You can't just throw an element and cover the entire viewport and call that the LCP element because Google will say, that doesn't count, no, that's a background. And the third thing is that, it doesn't count for low entropy images less than 0.05.

And a lot of you should be saying right now, Todd, WTF is entropy, that is too big of a word for this class. No, it's not, because it's important for LCP, so let's talk about entropy. Here's an image, this is the hero image for developer stickers. Unencoded, this is 3.9 megabytes, which means it's 31 million bytes. Entropy is calculated by the number of bits per visible pixel shown. So if we render this image at 2,800 by 1,200, which is the biggest it can be shown at, that means we're gonna render those 31 million bytes across 3.3 million pixels. Entropy is just dividing those, it's saying, how much data are we showing? How much data density are we showing in this image? So, the score here is 9.39, this would qualify as an LCP image.

Now, why is this important? Well, a common pattern that a lot of people will use is to use images like this. Here's a heavily blurred and down sampled version of the same image. And unencoded, that image is 17 bytes, 17 bytes, and its dimension is really small as well. So it's only 200 by 88. And so, when we render this image, let's say we rendered this image as a placeholder, we stuck this in and then used JavaScript to replace it later, so that the user kind of felt like it wasn't as slow. The entropy of this is 17/17,000 or 0.001, so it's very, very low entropy. There's very little visual data in this image. It's basically a blank color with a little bit of variation to it.

So why is this important? Well, that image right there, if I tried to use this as a placeholder, doesn't count for LCP. It doesn't matter when that loads, it doesn't matter how fast we made it. Google doesn't thinks that counts because the user doesn't think that counts as gonna wait for that full image to come in and would trigger the LCP event. So, this pattern doesn't act, even though it would help user experience, does not actually help you improve Largest Contentful Paint.

How do you get this information? Here's a little sample, you can actually calculate this in JavaScript. This sample and all the other ones like it are actually in the repo. If you look in, public, assets, JavaScript, code samples, here's all the different assets that I have screenshot or all the different bits of code that I've screenshotted. So if you wanna actually use one and play with it, they're all in here, including this one that I just referenced.

Now, this one here, all right, this one actually allows us to calculate the entropy of every image on the page. We can do this with JavaScript. We can grab every single image that has a source. We can figure out how big it is, how many bytes are in it. We can figure out how many pixels are being rendered, and divide them across, that's kind of interesting. We can go in and figure out which images may or may not qualify for entropy. If you don't use this blurring effect or if you don't use backgroundy patterns and stuff like that, this isn't super useful. This is specifically if your website uses this sort of pattern to try and improve LCP.

So what is the largest? Well, depending on when things load, a couple of different things could be the largest. So, we look at everything on the page and we calculate its size. So sometimes, if that hero image is particularly slow to load, sometimes that takes 30 seconds to load and it's outside of the LCP window. This thing is the largest thing, which is hero content P, which is 662 pixels by 112 pixels, or 74,000 total pixels. More often, this is the LCP image on our site, which is the desk hero desktop, which is 1740 by 640 or 1.1 million pixels, which is by far the largest thing on the page.

So, the LCP in our case is measuring, how long does it take to bring down that giant hero image? And if we replace it with the blur one, it's not actually gonna fix it at all because that'd be too low of entropy.

So here's what that looks like if we put it in sequence. So here's that film strip of all the different things that this page looks like during its load sequence, this is in mobile. So when we first load the page, we got a couple of empty pages. We start seeing things draw in. Eventually, this hero image starts to draw. We're waiting for it, we're waiting for it. When that element is complete, the LCP event triggers, which is before the load event, in this case.

Now, it doesn't have to be, depending on the website and how you've done things, your load event could be way before LCP, or it could be way after LCP. In the Core Web Vital world, it doesn't matter anymore, really, the LCP triggers when that largest pixel thing were to fire.

Now, if we were to add something else to this page, like, let's say we took this big obnoxious banner, and we made it 80% of the viewport. That would become the largest thing, and the LCP would now be that element. So you can decide what you want your LCP to be through design. What do you think the largest thing on your page should be, and how do you make that as fast as possible? Because the design of making something the largest is going to give the indication to the user that this is the important thing, this is why the user's here.

So some other considerations, LCP measurement stops after the first user interaction. So that if the user starts doing things, not scrolling, but clicking, mousing over things, typing, whatever, the LCP calculation stops. The user has gotten the indication that the page is done and they can start interacting, and so, if things render after that, it doesn't matter.

How fast does this need to be? So according to Google, if for Google to consider LCP good, this needs to be in 2.5 seconds or less. Now, that 2.5 second number is just, that's what they pulled out of their butts, but it's based on data. Think back to why is web performance important, and the research is that anytime after two seconds, a user is going to experience an interruption in their flow. So we're trying to get that less than that. We're trying to get, so that the page feels like it's done in 2.5 seconds or less.

Now, what happens if you're not there? What happens if you're more than 2.5 seconds? Well, now we get into penalty zone, and the penalties get worse depending on how bad it gets. What are the penalties? We don't know. These are proprietary secrets of Google, we don't know exactly what the page algorithm is. But we do know that as soon as you fall beyond that 2.5 seconds, there's a direct correlation between your scores and your page rank for those top ten rankings.

>> Todd Gardner: That's LCP.

>> Speaker 2: You mentioned LCP monitoring will stop after the user starts interaction. What if the user is clicking their mouse or hovering an empty part of the page before LCP is done?

>> Todd Gardner: Yeah, great question. So, hovering, I think I misspoke, hovering does not trigger as an interaction, right? The user's always hovering on a desktop, they're hovering over something, so it's not about hovering. But if they click, because some users do nervously click, they tap empty bits of background, that does stop LCP. So, there are some limitations to these metrics. So if you have a bunch of users who are all very anxious people and are all constantly clicking on something, your LCP metrics are gonna be off. They're gonna trigger right away whatever has already loaded when the user starts clicking. Now, in a large population sample, generally, that won't be super meaningful, but if you're looking at one particular user's LCP, that can definitely throw those numbers off. Generally, with good design and without super anxious users, the user won't start interacting until they feel the page is done enough for them to start doing things, which is good enough for Google's purpose. If the page, or at least the part of the page that the user sees, looks done enough that the user can start interacting, then LCP doesn't matter anymore. And Google has measured the thing that they were trying to do.

>> Speaker 2: The DOMContentLoaded and LoadEvents, do they apply to single page applications?

>> Todd Gardner: Yes, all websites have these events. These events exist regardless of the technology you use. However, when they are fired will greatly depend on what is shipped in the initial HTML that comes out of your server. So, in the example of an entirely client-side rendered application, where that HTML document doesn't really have very much in it at all, DOMContentLoaded and load are probably going to fire very, very fast. They're gonna fire as soon as that initial HTML content is out because there's nothing in it yet, and then your client side rendering application picks up, usually triggered by DOMContentLoaded or load, and starts doing things. And it's this pattern that really made those two metrics kind of worthless in terms of performance measurement, because they didn't mean anything.

Now, I'm keeping them in all of these slides for completeness, our example application is partially client-side rendered. There's some stuff that happens on the client, there's some stuff that happens on the server, and so, understanding when these happens can still be useful, in that case, for my own metrics, for my own understanding of a website. But as far as Google's concerned in terms of objectively comparing sites, they don't mean anything at all to Google.

---

## 번역 (Korean)

>> Todd Gardner: Google의 핵심 웹 지표(Core Web Vitals)에 대해 이야기해 보겠습니다. 이 주제는 꽤 오래 다룰 예정입니다. 핵심 웹 지표는 세 가지, 즉 세 가지 구체적인 성능 유형을 측정합니다. 첫째, 사이트가 사용자에게 얼마나 빠르게 시각적으로 로딩되는가입니다. load 이벤트가 아니라, 사용자가 실제로 보기에 사이트가 얼마나 빠르게 로딩된 것처럼 느껴지는가를 말합니다. 둘째, 얼마나 부드럽게 로딩되는가입니다. 로딩 중에 발생하는 일련의 이벤트들이 얼마나 매끄럽게 이어지는가입니다. 셋째, 사용자가 사이트와 얼마나 빠르게 상호작용할 수 있는가입니다. 이 세 가지는 각각 최대 콘텐츠풀 페인트(Largest Contentful Paint, LCP), 누적 레이아웃 이동(Cumulative Layout Shift, CLS), 다음 페인트까지의 상호작용(Interaction to Next Paint, INP)이라는 세 가지 지표로 측정됩니다.

각 지표에 대해 자세히 살펴볼 예정입니다. 하지만 먼저 짚고 넘어가야 할 중요한 점이 있습니다. 검색 순위(search ranking)는 핵심 웹 지표와 기타 페이지 경험 지표를 반영합니다. 이것들이 바로 순위 결정 요소(ranking factors)입니다. 물론 다른 성능 지표들도 있고 그것들도 중요하지만, 웹 성능 분야와 SEO 분야에서 이 세 가지를 특히 많이 다루는 이유는 직접적인 순위 결정 요소이기 때문입니다.

본론으로 들어가겠습니다. LCP, 즉 최대 콘텐츠풀 페인트(Largest Contentful Paint)는 사이트에서 가장 중요한 요소가 얼마나 빠르게 시각적으로 로딩되는지를 측정합니다. 그렇다면 무엇이 중요한 요소일까요? 누가 중요한 것을 결정하나요? 개발자가 Google에 "이 상품 이미지가 가장 중요한 요소입니다"라고 알려줄 수 있을까요? 그렇지 않습니다. Google이 개발자를 신뢰하지 않는 이유는, 만약 개발자가 중요한 요소를 직접 지정할 수 있다면, 페이지 상단에 즉시 로딩되는 작은 빈 div를 배치하고 그것을 가장 중요한 요소로 지정해서 LCP 점수를 인위적으로 높일 수 있기 때문입니다. 이 방식은 작동하지 않아야 합니다. 개발자가 부정적인 방식으로 영향을 줄 수 없어야 하며, 사용자가 중요하다고 인식하는 요소를 기준으로 해야 합니다.

따라서 '중요한' 요소 대신, 실제로는 '가장 큰' 요소를 측정합니다. 페이지에서 픽셀 면적 기준으로 가장 큰 요소입니다. 그러면 body 요소가 되는 것 아닐까요? 아닙니다. 특정 요소들만 대상으로 합니다. 이미지, 동영상, CSS 배경 이미지, 그리고 텍스트를 포함하는 모든 DOM 요소를 대상으로 하며, 이들 중 가장 큰 것을 찾습니다.

하지만 몇 가지 규칙이 있습니다. 페이지에 존재하는 모든 요소가 사용자에게 보이거나 의미 있는 것은 아니기 때문입니다. 첫째, 불투명도(opacity)가 0인 요소는 제외됩니다. 요소를 투명하게 만들면 LCP 대상에서 제외됩니다. 그렇지 않으면 요소를 크게 만들고 불투명도를 0으로 설정해서 항상 LCP 요소로 만드는 방식으로 지표를 조작할 수 있기 때문입니다. 둘째, 크기가 전체 페이지보다 작아야 합니다. 뷰포트 전체를 덮는 요소를 LCP 요소로 지정할 수 없습니다. Google은 그것을 배경으로 간주하고 인정하지 않습니다. 셋째, 엔트로피(entropy)가 0.05 미만인 저엔트로피 이미지는 대상에서 제외됩니다.

"Todd, 엔트로피가 뭔가요? 이 수업에서 다루기엔 너무 어려운 단어 아닌가요?"라고 생각하시는 분들이 있을 텐데요. 아닙니다. LCP에서 중요한 개념이니 엔트로피에 대해 이야기해 봅시다. 예시 이미지가 있습니다. 이것은 개발자 스티커(developer stickers)의 히어로 이미지입니다. 인코딩되지 않은 상태에서 이 이미지는 3.9MB, 즉 3,100만 바이트입니다. 엔트로피는 표시된 가시 픽셀당 비트 수로 계산됩니다. 이 이미지를 최대 크기인 2,800×1,200으로 렌더링하면, 3,100만 바이트를 330만 픽셀에 나누어 표현하게 됩니다. 엔트로피는 단순히 이 두 값을 나눈 것입니다. 즉, 얼마나 많은 데이터를, 얼마나 높은 데이터 밀도로 표현하고 있는가를 나타냅니다. 이 경우 점수는 9.39로, LCP 이미지로 인정됩니다.

이것이 왜 중요할까요? 많은 개발자들이 자주 사용하는 패턴이 있습니다. 같은 이미지의 심하게 흐리게 처리되고 다운샘플링된 버전을 사용하는 것입니다. 인코딩되지 않은 상태에서 그 이미지는 17바이트에 불과하고 크기도 200×88으로 매우 작습니다. 이 이미지를 플레이스홀더(placeholder)로 사용하고 나중에 JavaScript로 교체해서 사용자가 로딩이 느리지 않다고 느끼게 하는 방식입니다. 이 이미지의 엔트로피는 17/17,000, 즉 0.001로 매우 낮습니다. 시각적 데이터가 거의 없는 이미지로, 사실상 약간의 변화가 있는 단색에 불과합니다.

이것이 왜 중요할까요? 이 이미지를 플레이스홀더로 사용하면 LCP 대상이 되지 않습니다. 언제 로딩되든, 얼마나 빠르게 로딩되든 상관없습니다. Google은 이것을 유효한 LCP로 인정하지 않습니다. 사용자도 이것을 기다리지 않고 실제 전체 이미지가 로딩될 때까지 기다리기 때문에, 그것이 LCP 이벤트를 발생시킵니다. 따라서 이 패턴은 사용자 경험 개선에는 도움이 될 수 있어도, 최대 콘텐츠풀 페인트(LCP) 점수 향상에는 실질적인 도움이 되지 않습니다.

이 정보를 어떻게 확인할 수 있을까요? JavaScript로 엔트로피를 계산하는 코드 샘플이 있습니다. 이 샘플과 유사한 코드들은 모두 저장소(repository)에 있습니다. public > assets > JavaScript > code samples 경로에서 스크린샷으로 캡처한 다양한 코드 예시들을 찾을 수 있습니다. 직접 사용해 보고 싶다면 방금 언급한 샘플을 포함해 모두 그곳에 있습니다.

이 코드는 페이지의 모든 이미지에 대한 엔트로피를 계산합니다. JavaScript로 구현 가능합니다. source 속성이 있는 모든 이미지를 가져와서 바이트 수와 렌더링되는 픽셀 수를 파악하고 이를 나눌 수 있습니다. 이를 통해 각 이미지가 엔트로피 기준을 충족하는지 확인할 수 있습니다. 흐림 효과나 배경 패턴을 사용하지 않는다면 이 코드가 특별히 유용하지 않을 수 있습니다. LCP 개선을 위해 이런 패턴을 사용하는 웹사이트에 특히 유용합니다.

그렇다면 실제로 가장 큰 요소는 무엇일까요? 로딩 타이밍에 따라 달라질 수 있습니다. 페이지의 모든 요소를 살펴보고 크기를 계산합니다. 히어로 이미지가 특히 느리게 로딩되어 30초가 걸린다면 LCP 측정 범위를 벗어날 수 있습니다. 그 경우 가장 큰 요소는 662×112픽셀, 약 74,000 총 픽셀인 히어로 콘텐츠 문단(hero content P)이 됩니다. 더 일반적으로는, 사이트의 LCP 이미지는 1740×640픽셀, 즉 약 110만 픽셀인 데스크 히어로 데스크톱(desk hero desktop) 이미지로, 페이지에서 단연 가장 큰 요소입니다.

따라서 이 경우 LCP가 측정하는 것은 그 거대한 히어로 이미지를 불러오는 데 얼마나 걸리는가입니다. 흐린 이미지로 교체해도 엔트로피가 너무 낮기 때문에 실제로는 개선되지 않습니다.

이것을 순서대로 배치해 보면 다음과 같습니다. 모바일에서 이 페이지의 로딩 순서를 필름 스트립으로 나타낸 것입니다. 처음에는 빈 페이지가 보이다가, 요소들이 그려지기 시작합니다. 점차 히어로 이미지가 그려지고, 기다리다 보면 그 요소가 완성되는 순간 LCP 이벤트가 발생합니다. 이 경우에는 load 이벤트보다 먼저 발생합니다.

반드시 그런 것은 아닙니다. 웹사이트 구성 방식에 따라 load 이벤트가 LCP보다 훨씬 먼저 발생할 수도 있고, 훨씬 나중에 발생할 수도 있습니다. 핵심 웹 지표(Core Web Vital) 관점에서는 load 이벤트 타이밍이 더 이상 중요하지 않으며, LCP는 해당 최대 픽셀 요소가 완성될 때 발생합니다.

만약 페이지에 뷰포트의 80%를 차지하는 큰 배너를 추가한다면, 그것이 가장 큰 요소가 되어 LCP 기준 요소가 됩니다. 따라서 디자인을 통해 LCP 요소를 결정할 수 있습니다. 페이지에서 가장 큰 요소가 무엇이어야 하는지 생각하고, 그것을 어떻게 최대한 빠르게 만들 것인지를 고민해야 합니다. 가장 크게 만든다는 디자인 선택 자체가 사용자에게 "이것이 중요한 요소이고, 이것이 사용자가 여기 있는 이유"라는 신호를 줍니다.

추가 고려 사항이 있습니다. LCP 측정은 첫 번째 사용자 상호작용 이후에 중단됩니다. 사용자가 스크롤이 아니라 클릭, 마우스 오버, 타이핑 등의 행동을 시작하면 LCP 계산이 멈춥니다. 사용자가 페이지가 완료되었다는 신호를 받고 상호작용을 시작했다고 판단하기 때문에, 그 이후에 렌더링되는 요소는 LCP에 영향을 주지 않습니다.

LCP가 얼마나 빨라야 할까요? Google 기준에 따르면, LCP가 양호(good)로 인정받으려면 2.5초 이내여야 합니다. 이 2.5초라는 수치는 데이터에 기반한 것이긴 하지만, 어느 정도 임의적인 기준입니다. 웹 성능이 왜 중요한지를 생각해 보면, 연구에 따르면 2초 이상이 되면 사용자의 집중 흐름(flow)이 끊기는 경험을 하게 됩니다. 따라서 2.5초 이내에 페이지가 완성된 것처럼 느껴지도록 하는 것이 목표입니다.

2.5초를 초과하면 어떻게 될까요? 패널티 구간에 진입하게 되며, 상황이 나빠질수록 패널티도 커집니다. 구체적인 패널티 내용은 공개되지 않은 Google의 독점 정보입니다. 하지만 2.5초를 넘는 순간부터 점수와 상위 10개 검색 결과 순위 사이에 직접적인 상관관계가 있다는 것은 알려져 있습니다.

>> Todd Gardner: 이상이 LCP입니다.

>> 청중: 사용자가 상호작용을 시작하면 LCP 측정이 중단된다고 하셨는데요. LCP가 완료되기 전에 사용자가 빈 페이지 영역에 클릭하거나 마우스를 올리면 어떻게 되나요?

>> Todd Gardner: 좋은 질문입니다. 제가 잘못 말했는데, 마우스 올리기(hovering)는 상호작용으로 인식되지 않습니다. 데스크톱에서는 항상 마우스가 어딘가에 올려져 있으므로, 마우스 올리기는 기준이 되지 않습니다. 하지만 클릭의 경우, 일부 사용자들은 불안해서 배경의 빈 곳을 습관적으로 클릭하는데, 이는 LCP를 중단시킵니다. 이처럼 이 지표들에는 몇 가지 한계가 있습니다. 클릭을 자주 하는 사용자들이 많다면 LCP 지표가 왜곡될 수 있습니다. 사용자가 클릭을 시작하는 순간 이미 로딩된 요소가 LCP를 즉시 발생시키기 때문입니다. 대규모 표본에서는 이 영향이 크지 않을 수 있지만, 특정 사용자 한 명의 LCP를 분석할 때는 이 점이 수치를 왜곡할 수 있습니다. 일반적으로 좋은 디자인과 평균적인 사용자 행동을 전제로 하면, 사용자는 페이지가 충분히 완성된 것처럼 보일 때까지 상호작용을 시작하지 않습니다. 이는 Google이 측정하고자 하는 목적에 부합합니다. 사용자에게 보이는 페이지 부분이 충분히 완성된 것처럼 보여 상호작용을 시작할 수 있다면, LCP는 더 이상 중요하지 않습니다. Google은 측정하고자 했던 것을 이미 측정한 것입니다.

>> 청중: DOMContentLoaded와 Load 이벤트도 싱글 페이지 애플리케이션(Single Page Application)에 적용되나요?

>> Todd Gardner: 네, 모든 웹사이트에 이 이벤트들이 존재합니다. 사용하는 기술에 관계없이 이벤트들은 존재합니다. 하지만 언제 발생하는지는 서버에서 전송되는 초기 HTML에 무엇이 담겨 있는지에 크게 달려 있습니다. 완전히 클라이언트 사이드 렌더링(client-side rendering)되는 애플리케이션의 경우, HTML 문서에 실제 내용이 거의 없기 때문에 DOMContentLoaded와 load 이벤트는 매우 빠르게 발생합니다. 초기 HTML 콘텐츠가 준비되는 즉시 발생하고, 이후 클라이언트 사이드 렌더링 애플리케이션이 일반적으로 DOMContentLoaded나 load 이벤트를 트리거로 하여 동작을 시작합니다. 이 패턴이 바로 이 두 지표를 성능 측정 관점에서 사실상 무의미하게 만든 원인입니다.

이 슬라이드들에 계속 포함시키는 이유는 완전성을 위해서입니다. 예시 애플리케이션은 부분적으로 클라이언트 사이드 렌더링을 사용합니다. 클라이언트에서 처리되는 부분과 서버에서 처리되는 부분이 모두 있어서, 언제 이벤트들이 발생하는지 이해하는 것이 개인적인 지표 분석이나 웹사이트 이해에는 여전히 유용할 수 있습니다. 하지만 Google이 사이트들을 객관적으로 비교하는 관점에서는, 이 이벤트들은 Google에게 전혀 의미가 없습니다.
