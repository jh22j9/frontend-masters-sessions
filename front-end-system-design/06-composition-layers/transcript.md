## 원문 (English)

>> Evgenii Ray: Now let's understand how the browsers utilize the GPU. So the old browsers used only the CPU to render the page, but the new browsers use CPU and GPU together in parallel to optimize the rendering. And let's now understand how the GPU is used via the browser. So would render four boxes here. Each box is absolutely positioned and slightly moved on top of each other. And here's our html tree. So apparently, for the browser to optimize the html for the GPU rendering, it needs to have some kind of rendering tree representation which is GPU aware so the browser constructs their render object tree. So where for each DOM element, we create a new render object. And as you can see, it's just one-to-one copy, but they have completely different properties that the DOM element contains all the information about html, or all its properties. While the Render Objects knows how to draw this element on the bitmap using the graphical context, which is passed from the top. And as you can see, so the GPU is good at rendering many things, but if we imagine that every render object is responsible for drawing itself on the page, it doesn't look like a very optimized way to use the GPU. So that's why there is additional concept called Render Layer. So the Render Layer is constructed when we actually use the position relative or absolute. So when we move elements to the stacking context, so basically create a new layer. Also, it's constructed for the root element, so we always need to have a root layer where we render the things. And when we utilize some accelerated context like Canvas or CSS filters. And let's try to construct the Render Layer. So if we take the first html Render Object, we'll see that because it's the root element, the first root Render Layer will be constructed. But then we have the body and the main tag. Because the body and the main doesn't have any special properties, this Rendering Objects will be assigned to the root Render Layer. And now let's move on to these four divs element. So, because each of these div element is positioned absolutely, this means that we need to promote these objects to separate Render Layers. So that's why we created a new four Rendering Layers. And also this rendering layers will be applied to the root Render Layer. So each Render Layer kind of has a children property, and it stores the Render Layer as a linked list, in a stack order. So the last layer will be the the first rendered. We may have hundreds of Render Layers, it's still not efficient to use the GPU for hundreds of objects. And remember, the GPU is good at rendering many things. So how does the browser go about that? Apparently, there is an additional layer called Graphic Layer, which is constructed when we use some 3D acceleration. So it can be perspective change, or we use some video element where we need decoding from the GPU. And if we try to construct a Graphic Layer for that, so first, we'll start with a root Render Layer. Because it's created by html element, we initiate the first Graphic Layer, we need to have at least one when we load the page. So for the rest of the rendering layers, because they do not utilize any 3D acceleration, we don't actually need additional rendering layer. That's why all this rendering layer will be applied to the one Graphic Layer. So now we have a Graphic Layer that has the graphic context and knows how to draw the rendering layer. And now we have the full cycle of how browser renders things. But let's run a quick demo to see how this actually works. So if we open the following CodePen, and open the debug mode. Now we need to transition to the layer step.

>> Evgenii Ray: So we'll see that a single Graphic Layer right now is created, and it utilizes 8.7 megabytes of the GPU memory. So the key thing about Graphic Layers, we can utilize the Graphic Layer to optimize certain parts of the apps. So for instance, you could have the widget that relies on very advanced animations. Then you can say, hey, browser, I want this on a separate Graphic Layer so you can optimize this widget and I can see my snappy animation 120 FPS. But the thing about the Graphic Layer is it utilizes the CPU and the GPU RAM. So every time we create a new Graphic Layer, we actually take a hit on the browser resources. And it's very easy to prove by applying the styles to the box. And let's promote each box through separate Graphic Layer. So let's say,

>> Evgenii Ray: We can apply 3D transformation. And if we go back to the layer step, we'll see now that we have five graphic layers in total. And the first one still utilizes 8.7 megabytes, but each newly created one utilizes more than half megabyte of the virtual memory. And the problem of that is, imagining you have at least with hundreds of elements inside, and you decided to promote these elements to the Graphic Layer. This means that you immediately consume around maybe 100 megabytes of the GPU memory. And GPU memory is not just used for the browser and rendering the page, it's used for the system. And your application may become unresponsive because the system will have to utilize the additional swap cache. And when we use the swap, then we get the unresponsiveness of the app. So the summary for the Graphic Layers it's a very powerful thing to utilize when you build your complex apps, but you need to make sure that you utilize it right because it uses the VRAM and also the CPU. So instead of optimizing the app you may end up de-optimizing your application.

>> Speaker 2: Is there any good rule of thumb for avoiding CPU bound operations in CSS?

>> Evgenii Ray: There is a website called what triggers CSS or something, so you can actually see what is the pipeline will be triggered by changing the certain CSS properties, you just need to make sure. So its always the rule of thumb, try and minimize any reflow. Always try to change the properties that do not trigger the full pipeline. So this is the rule of thumb. There is no good reason to just always use the full pipeline, because it always affects the application performance.

>> Speaker 2: Yeah, I found it, it's csstriggers.com.

>> Evgenii Ray: Yeah.

>> Speaker 2: And then Lucas says 8.7 megabits sounds like a lot to me, am I wrong? And what could be a good healthy number?

>> Evgenii Ray: No, actually it's a relatively small number. Usually websites utilized around hundreds megabytes of the virtual memory. And it doesn't grow linear, because we need to initialize the initial context, we always pay the price of this initial utilization. So it will be kind of the constant. But if you try to promote too many layers, then you may have an issue later.

>> Speaker 2: Yeah, I think there's a tweet on about Frontend Masters that they were watching a course, and taking notes, and doing all these things on our website. And it was taking like one tenth the resources as another just marketing page that was just trying to show a couple animations or whatever. And how absurd it is, how people can abuse the CSS engine or just JavaScript in general, having so much on the page and things shifting around, like how much that it boats up the browser.

>> Evgenii Ray: So it may impact the performance significantly. So for instance, if you have lists of items and each item will be promoted to the Graphic Layer, then I think it will take around 200 megabytes of memory easily. And we need to remember that the webinar is consumed mostly by mobile devices, and when we have the mobile device, we don't have such benefit. We don't have RTX 4090 everywhere. [LAUGH] And that's why we need to be aware of the layers that we're utilizing in our app. Just make sure that you use them wisely. So for instance, if you have high activity widget that have using some nice animation, you can promote this to the separate layer. But if you have a static page and you just promote every element to the Graphic Layer, then it's where we can get an issue.

>> Speaker 2: I have heard that CSS transforms are good optimizations over reflows. So, how do we correlate this now given transforms take a hit on the GPU memory?

>> Evgenii Ray: So it's always better to utilize GPU than CPU, because when you utilize CPU you block the rendering thread and you will see that your responsiveness drops. So if you if you need to choose what to utilize always utilize GPU. You need to put a lot of effort to make sure that your app is unresponsive because of GPU. But it's very easy to make a few mistakes and then get a frame drop when you utilize the CPU.

---

## 번역 (Korean)

>> Evgenii Ray: 이제 브라우저가 GPU를 어떻게 활용하는지 살펴보겠습니다. 예전 브라우저는 페이지 렌더링에 CPU만 사용했지만, 최신 브라우저는 렌더링 최적화를 위해 CPU와 GPU를 병렬로 함께 사용합니다. 브라우저가 GPU를 어떻게 활용하는지 구체적으로 이해해 봅시다. 여기서는 네 개의 박스를 렌더링합니다. 각 박스는 절대 위치(absolutely positioned)로 지정되어 서로 살짝 겹쳐져 있으며, HTML 트리 구조는 다음과 같습니다.

브라우저가 GPU 렌더링을 위해 HTML을 최적화하려면, GPU를 인식하는 렌더링 트리 표현이 필요합니다. 이를 위해 브라우저는 렌더 오브젝트 트리(Render Object Tree)를 구성합니다. 각 DOM 엘리먼트마다 하나의 렌더 오브젝트(Render Object)가 생성되는 일대일 대응 구조입니다. 다만 두 가지는 완전히 다른 속성을 가집니다. DOM 엘리먼트는 HTML에 관한 모든 정보와 속성을 담고 있는 반면, 렌더 오브젝트는 위에서 전달된 그래픽 컨텍스트(graphical context)를 사용해 해당 엘리먼트를 비트맵(bitmap)에 어떻게 그릴지를 알고 있습니다.

GPU는 많은 것을 동시에 렌더링하는 데 뛰어나지만, 모든 렌더 오브젝트가 각자 페이지에 자신을 직접 그려야 한다고 가정하면 GPU를 최적으로 활용하는 방식이 아닙니다. 그래서 등장한 추가 개념이 바로 렌더 레이어(Render Layer)입니다.

렌더 레이어는 `position: relative` 또는 `position: absolute`를 사용할 때 생성됩니다. 즉, 엘리먼트를 스태킹 컨텍스트(stacking context)로 이동시켜 새로운 레이어를 만들 때 생성됩니다. 또한 루트 엘리먼트에도 렌더 레이어가 생성되는데, 모든 것을 렌더링할 루트 레이어는 항상 필요합니다. Canvas나 CSS 필터와 같은 가속 컨텍스트(accelerated context)를 사용할 때도 생성됩니다.

렌더 레이어 구성 과정을 살펴봅시다. 첫 번째 HTML 렌더 오브젝트를 살펴보면, 루트 엘리먼트이기 때문에 첫 번째 루트 렌더 레이어가 생성됩니다. 그 다음의 `body`와 `main` 태그는 특별한 속성이 없으므로, 해당 렌더 오브젝트들은 루트 렌더 레이어에 할당됩니다. 이제 네 개의 `div` 엘리먼트로 넘어가면, 각 `div`가 절대 위치(absolute)로 지정되어 있기 때문에 별도의 렌더 레이어로 승격(promote)해야 합니다. 그 결과 네 개의 새로운 렌더 레이어가 생성되고, 이 렌더 레이어들은 루트 렌더 레이어에 귀속됩니다.

각 렌더 레이어는 자식(children) 속성을 가지며, 렌더 레이어들을 스택 순서로 연결 리스트(linked list) 형태로 저장합니다. 마지막 레이어가 가장 먼저 렌더링됩니다. 렌더 레이어가 수백 개가 된다 해도, 수백 개의 오브젝트에 GPU를 사용하는 것은 여전히 비효율적입니다. GPU는 많은 것을 동시에 렌더링하는 데 강점이 있다는 점을 기억하세요. 그렇다면 브라우저는 어떻게 이 문제를 해결할까요?

바로 그래픽 레이어(Graphic Layer)라는 추가 레이어가 있습니다. 이는 3D 가속(3D acceleration)을 사용할 때 생성됩니다. 원근감(perspective) 변경이나 GPU 디코딩이 필요한 `video` 엘리먼트 사용 시가 그 예입니다.

그래픽 레이어 구성을 살펴보면, 먼저 루트 렌더 레이어에서 시작합니다. HTML 엘리먼트로 생성되므로 첫 번째 그래픽 레이어를 초기화합니다. 페이지 로드 시 적어도 하나는 필요합니다. 나머지 렌더 레이어들은 3D 가속을 사용하지 않기 때문에 추가 그래픽 레이어가 필요하지 않습니다. 따라서 모든 렌더 레이어가 하나의 그래픽 레이어에 적용됩니다. 이렇게 그래픽 컨텍스트를 가지고 렌더 레이어를 그리는 방법을 아는 그래픽 레이어가 완성됩니다. 이로써 브라우저가 렌더링하는 전체 사이클이 완성됩니다.

실제로 어떻게 동작하는지 간단한 데모로 확인해 봅시다. CodePen을 열고 디버그 모드로 전환한 뒤, 레이어(layer) 단계로 이동합니다.

>> Evgenii Ray: 현재 하나의 그래픽 레이어가 생성되어 있으며, GPU 메모리를 8.7 메가바이트 사용하고 있음을 확인할 수 있습니다.

그래픽 레이어의 핵심은 앱의 특정 부분을 최적화하는 데 활용할 수 있다는 점입니다. 예를 들어, 매우 고급 애니메이션을 사용하는 위젯이 있다면 브라우저에게 해당 위젯을 별도의 그래픽 레이어에 배치하도록 지시할 수 있습니다. 그러면 브라우저가 해당 위젯을 최적화하여 120FPS의 부드러운 애니메이션을 구현할 수 있습니다. 그러나 그래픽 레이어는 CPU와 GPU RAM을 모두 사용합니다. 새로운 그래픽 레이어를 생성할 때마다 브라우저 리소스에 부담을 줍니다. 박스에 스타일을 적용하여 이를 쉽게 확인할 수 있습니다. 각 박스를 별도의 그래픽 레이어로 승격시켜 보겠습니다.

>> Evgenii Ray: 3D 변환(3D transformation)을 적용할 수 있습니다. 레이어 단계로 돌아가면, 총 다섯 개의 그래픽 레이어가 생성된 것을 확인할 수 있습니다. 첫 번째 레이어는 여전히 8.7 메가바이트를 사용하지만, 새로 생성된 각 레이어는 0.5 메가바이트 이상의 가상 메모리를 사용합니다.

문제는 수백 개의 엘리먼트를 포함하는 경우를 상상해 보면 명확해집니다. 이 엘리먼트들을 그래픽 레이어로 승격시키면 즉시 약 100 메가바이트의 GPU 메모리를 소비하게 됩니다. GPU 메모리는 브라우저 렌더링뿐만 아니라 시스템 전체에서 사용됩니다. 시스템이 추가 스왑 캐시(swap cache)를 사용해야 하는 상황이 오면 애플리케이션이 응답 불능 상태가 될 수 있습니다. 스왑을 사용하게 되면 앱의 응답성이 떨어집니다.

정리하자면, 그래픽 레이어는 복잡한 앱을 구축할 때 매우 강력한 도구이지만, VRAM과 CPU를 모두 소비하기 때문에 올바르게 활용해야 합니다. 잘못 사용하면 앱을 최적화하는 것이 아니라 오히려 성능을 저하시킬 수 있습니다.

>> Speaker 2: CSS에서 CPU 바운드 연산을 피하기 위한 좋은 경험 법칙이 있을까요?

>> Evgenii Ray: "What Triggers CSS" 같은 이름의 웹사이트가 있는데, 특정 CSS 속성을 변경했을 때 어떤 파이프라인이 트리거되는지 실제로 확인할 수 있습니다. 기본 원칙은 항상 리플로우(reflow)를 최소화하는 것입니다. 전체 파이프라인을 트리거하지 않는 속성만 변경하도록 노력하세요. 이것이 핵심 원칙입니다. 전체 파이프라인을 항상 사용해야 할 이유는 없으며, 그렇게 하면 항상 애플리케이션 성능에 영향을 미칩니다.

>> Speaker 2: 찾았습니다. csstriggers.com이네요.

>> Evgenii Ray: 맞습니다.

>> Speaker 2: Lucas가 8.7 메가비트는 꽤 많은 것 같다고 하는데, 제가 잘못 이해한 건가요? 적정한 수치는 어느 정도일까요?

>> Evgenii Ray: 아니요, 사실 상대적으로 작은 수치입니다. 보통 웹사이트는 수백 메가바이트의 가상 메모리를 사용합니다. 선형적으로 증가하지 않는데, 초기 컨텍스트를 초기화해야 하기 때문에 항상 초기 사용량의 비용을 지불하게 됩니다. 따라서 이 초기값은 거의 상수에 가깝습니다. 그러나 레이어를 너무 많이 승격시키면 나중에 문제가 생길 수 있습니다.

>> Speaker 2: Frontend Masters에 관한 트윗이 있었는데, 강의를 듣고 노트를 작성하는 등 우리 사이트에서 여러 작업을 하면서도 단순히 몇 가지 애니메이션을 보여주려는 마케팅 페이지보다 리소스를 10분의 1밖에 사용하지 않는다는 내용이었습니다. CSS 엔진을 남용하거나 자바스크립트를 과도하게 사용하여 페이지에 수많은 요소를 배치하고 이것들이 움직이면 브라우저에 얼마나 큰 부담을 주는지 정말 놀랍습니다.

>> Evgenii Ray: 그것은 성능에 상당한 영향을 미칠 수 있습니다. 예를 들어, 항목 목록이 있고 각 항목이 그래픽 레이어로 승격된다면 쉽게 200 메가바이트의 메모리를 차지할 것입니다. 또한 웹을 주로 모바일 기기에서 사용한다는 점을 기억해야 합니다. 모바일 기기에서는 RTX 4090 같은 고성능 GPU의 혜택을 누릴 수 없습니다. [웃음] 그래서 앱에서 사용하는 레이어에 대해 인식하고 있어야 합니다. 현명하게 사용하세요. 애니메이션이 활발하게 사용되는 위젯이라면 별도 레이어로 승격시킬 수 있습니다. 그러나 정적 페이지에서 모든 엘리먼트를 그래픽 레이어로 승격시키면 문제가 발생할 수 있습니다.

>> Speaker 2: CSS 트랜스폼(transform)이 리플로우(reflow)보다 좋은 최적화 방법이라고 들었습니다. 트랜스폼이 GPU 메모리에 부담을 준다는 점을 고려하면 이것을 어떻게 연결해서 이해해야 할까요?

>> Evgenii Ray: CPU보다 GPU를 활용하는 것이 항상 더 낫습니다. CPU를 사용하면 렌더링 스레드(rendering thread)가 차단되어 응답성이 떨어집니다. 무엇을 사용할지 선택해야 한다면 항상 GPU를 선택하세요. GPU 때문에 앱이 응답 불능 상태가 되게 만들려면 상당한 노력이 필요합니다. 반면 CPU를 사용하면 몇 가지 실수만으로도 쉽게 프레임 드롭이 발생합니다.
