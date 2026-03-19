## 원문 (English)

So what is the problem with these? Because these two metrics used to be the only way we measured performance. It was the only ways we could measure performance, is we would track how long till DOM content loaded, and how long to load? And we would track those over time and that was the only way to really know, how fast our site was loading, the load word didn't have a problem yet. Because load originally meant that the document was ready.

In fact, that was so common that early JavaScript frameworks like jQuery, which is still awesome, jQuery, lots of our code that we would write back in the day would say something like this, document ready, and then we would do stuff to it. And it was just so common like, this is what meant is we would download a page, the page would be done, it would be ready. And then, we could start asynchronous tasks, we could start things that needed to happen after the fact. In this case, we would do some ajax, which was the old way we would do fetch. We would fetch the shopping cart, and put some data about how many things were in the user shopping cart. And this was great, and everything was good.

But then, 2010 happened, and in 2010, this new fancy idea called client-side rendering happened, which is all you crazy kids know today. Back in the day, we used a bunch of frameworks that you don't even know about anymore, like backbone, and knockout, and jQuery UI, which were the early ways that we did this. And suddenly, applications changed their structure, and a website could now look like this.

This might look familiar from more modern JavaScript frameworks, cuz a lot of them still do this. We have an HTML, that HTML is largely empty, it's got a single div that has an ID like app. And then, I'm still using old jQuery syntax here just for connection to the other stuff. When the document is ready, which happens almost instantly because the document isn't doing anything, we start up our application, and we do things to it. This is how client-side rendered React or Vue, or any of those things would do things, where the load event happens almost right away, and then we do all of the work.

So, how does that apply if the old GeoCities sites switched to client-side rendering? Well, before, here's where our document content loaded and load event were, and it was great, and they were meaningful events that told us about the performance of the site. But if we rendered geocities.com using a client-side rendering kind of pattern, suddenly those things move up here. DOM content loaded and load happened instantly, and there's no meaningful connection to the user experience. The user experience could be crap slow, the end of this page could be one second, 10 seconds, 30 seconds, a minute, but the metrics didn't mean anything.

And so, we needed new ways to measure performance. Specifically, Google needed new ways to measure performance, because Google makes money off search. Google wants to deliver the best search results possible so that more people use Google to search and Google can show you more ads, fundamentally how Google works. So, Google has realized that users think a search result is better if the website is fast, they will stay on that page longer if the website is fast. If they can make more websites faster, they can reward the fast sites, people will see the search results are better.

But how do they do that? How could they know that? They need a way of comparing a site objectively. If we look at foo.com and bar.com, how do they know which one is faster? If they're equally good for content and backlinks and all the other things, how do they know which one to rank better than the other one? If they were to look at DOM content loaded and load, it would be largely due to how they were built, a client-side rendered site would look super fast, even if it feels super slow, and vice versa.

So, Google needed new ways to measure performance objectively, based on the user perception of the performance of the site, and it works regardless of the frameworks or technologies used to build the site. And that's where the Core Web Vitals come in, which is a new set of measurements that was introduced to try and address these.

---

## 번역 (Korean)

그렇다면 이 두 지표(DOMContentLoaded, Load)의 문제는 무엇일까요? 이 두 지표는 한때 성능을 측정하는 유일한 방법이었습니다. DOMContentLoaded까지 걸리는 시간과 Load까지 걸리는 시간을 추적하는 것이 성능 측정의 전부였고, 시간의 흐름에 따라 이 수치들을 기록하는 것만이 사이트 로딩 속도를 파악하는 유일한 방법이었습니다. 당시에는 `load`라는 개념 자체에 문제가 없었습니다. `load`는 원래 문서가 준비(ready) 상태가 되었다는 것을 의미했기 때문입니다.

실제로 이 개념은 매우 보편적이어서, jQuery와 같은 초기 자바스크립트 프레임워크(지금도 훌륭합니다)에서 작성하던 코드 대부분은 `document ready` 이후에 작업을 수행하는 형태였습니다. 이는 매우 자연스러운 패턴이었습니다. 페이지를 다운로드하고, 페이지가 완성되어 준비되면 비동기 작업을 시작하는 흐름이었습니다. 예를 들어, 당시에는 `fetch`의 전신인 Ajax를 사용하여 장바구니 데이터를 가져와 사용자의 장바구니에 담긴 상품 수를 표시했습니다. 이 방식은 훌륭하게 작동했고, 모든 것이 잘 돌아갔습니다.

그런데 2010년이 되면서 상황이 바뀌었습니다. 클라이언트 사이드 렌더링(Client-Side Rendering)이라는 새로운 개념이 등장한 것입니다. 오늘날의 개발자들에게는 너무나 익숙한 방식이지만, 당시에는 Backbone, Knockout, jQuery UI 같이 지금은 거의 사용하지 않는 프레임워크들이 이 방식을 초기에 구현했습니다. 이와 함께 애플리케이션의 구조가 급격히 변화하기 시작했고, 웹사이트는 새로운 형태를 갖추게 되었습니다.

이 구조는 현대 자바스크립트 프레임워크에서도 낯설지 않을 것입니다. HTML 파일은 내용이 거의 없고, `id="app"`과 같은 단일 `div` 하나만 포함하는 형태입니다. 그리고 `document ready` 시점에 애플리케이션을 초기화하고 모든 작업을 수행합니다. 문서 자체에 내용이 없으므로 `document ready`는 거의 즉시 발생합니다. 이것이 바로 클라이언트 사이드 렌더링 방식의 React나 Vue가 동작하는 방식입니다. `load` 이벤트가 거의 즉시 발생하고, 그 이후에 실제 작업이 모두 이루어집니다.

그렇다면 만약 과거의 GeoCities 사이트들이 클라이언트 사이드 렌더링 방식으로 전환한다면 어떻게 될까요? 기존 방식에서는 DOMContentLoaded와 Load 이벤트가 사이트의 실제 성능을 의미 있게 반영하는 지표였습니다. 그러나 geocities.com을 클라이언트 사이드 렌더링 패턴으로 구현하면, 이 두 이벤트는 페이지 로딩 초반부에 즉시 발생해 버립니다. DOMContentLoaded와 Load가 거의 즉시 완료되더라도, 실제 사용자 경험과는 아무런 연관이 없게 됩니다. 사용자 경험은 1초, 10초, 30초, 심지어 1분이 걸릴 수도 있지만, 이 지표들은 아무런 의미를 갖지 못합니다.

따라서 새로운 성능 측정 방법이 필요하게 되었습니다. 특히 Google 입장에서는 더욱 절실했습니다. Google은 검색으로 수익을 창출하는 회사이기 때문입니다. Google은 더 많은 사람들이 자신의 검색 서비스를 이용하고 광고를 보도록 최상의 검색 결과를 제공하고자 합니다. 이것이 Google의 근본적인 비즈니스 모델입니다. Google은 웹사이트가 빠를수록 사용자들이 검색 결과를 더 좋게 평가하고, 해당 페이지에 더 오래 머문다는 사실을 인식했습니다. 빠른 사이트들을 더 많이 만들 수 있다면, 빠른 사이트에 높은 순위를 부여하고, 사람들은 검색 결과의 품질이 더 향상되었다고 느끼게 됩니다.

그런데 Google은 어떻게 이를 판단할 수 있을까요? 사이트를 객관적으로 비교하는 방법이 필요합니다. foo.com과 bar.com을 비교할 때, 어느 쪽이 더 빠른지 어떻게 판단할 수 있을까요? 콘텐츠 품질, 백링크 등 다른 요소들이 동등하다면, 어느 사이트를 더 높이 랭킹할지 어떻게 결정할 수 있을까요? DOMContentLoaded와 Load를 기준으로 삼는다면, 그 결과는 사이트가 어떻게 구현되었느냐에 크게 좌우될 것입니다. 클라이언트 사이드 렌더링 사이트는 실제로는 느리더라도 매우 빠르게 보일 수 있고, 반대의 경우도 발생할 수 있습니다.

결국 Google은 사이트 구현에 사용된 프레임워크나 기술에 관계없이, 사용자가 체감하는 성능을 기반으로 사이트를 객관적으로 측정하는 새로운 방법이 필요했습니다. 바로 이런 문제를 해결하기 위해 등장한 것이 **코어 웹 바이탈(Core Web Vitals)**입니다. 이는 기존 지표의 한계를 극복하기 위해 새롭게 도입된 측정 기준 모음입니다.
