# Will Sentance - Declarative & Readable Code

## 원문 (English)

How was this all possible, and we already saw hints of this from Michael's question, from multiple other questions. How was this possible? How am I able to pass in function code, function as an input to another function?

As we said, in many languages, this violates our idea of what declaring or saving a function even is doing. Not in JavaScript, because in JavaScript functions are first-class objects. That means they have all, they are just objects. Now they have extra ability, the ability to be called, to be invoked, to be run, but they are just objects. Therefore, they can be treated and coexist with any other object, they can be assigned to variables, we'll see in a moment the ever more popular style of declaring functions that actually does involve assigning to a label.

We can assign them as properties on other objects, where they're known as what, Chris, what's a function on an object known as? A function on an object is known as a method. Dot, don't, do not fear. We can pass functions in as inputs. We know we can pass an object in. Nobody's worried about passing an object, if we declare an object into, we can declare an object, we know we can pass an object in to another function. We can also pass functions in as inputs to other functions.

And then finally, the fact that functions are just full on objects that we can pass around, we can move around our code, also means we can return them as output values. Here we returned out an array, we can also return out a function as an output value from another function. When we do that, we get the most obscure feature of JavaScript known as closure. And that's what we'll see after this, but first, let's identify which of these was our higher order function, our most profound of constructs.

Surely we had to declare it with a special keyword to say there's a special higher order function. And which of these was our callback function? Austin, what do you think was our higher order function a grander function? I think it's copy array and manipulate. And what do you think was our callback function, our little baby function that can be passed in? That'd be multiply by 2, that'd be multiply by 2.

Our higher order function, the outer function that could take in another function, or, as we'll see, return out. Either take in or as we'll see, return out, another function, and our callback function is the function we insert in.

Now, as I say, I don't know if I love the term callback function here. We're going to see later on that there's a whole realm of JavaScript, where we are passing in another function, and it's called the callback function, because it's going to run, be called back, it's going to call you back, call us back, come back much later. Here, our multiply by 2, our callback function, was run directly inside copy array and manipulate. They are still called higher-order functions and callback functions, but you could also call our callback function here a handler, it's handling what to do to each element, you could call it some sort of transformation function, it's doing the transformation, an argument function. We'll see that often in languages, especially where we pass them in without a label they're called lambda functions, but they're still technically called callback functions. I just think it's a strange name because that suggests that they come back later. No, no, no, they run right there.

In asynchronous JavaScript to come, before the end of this marathon that we're doing together we will see functions that very, very much get passed into another function, and they disappear off to be called back later on. But that ain't here. OK.

So higher-order functions, takes in a function, passes out a function, it's just a term. We didn't need to declare, copy, array and manipulate with a special type of function, expecting another function. Just does it. Nothing intrinsically different about them.

But callback and higher-order functions, they simplify our code and let us keep it, do not repeat yourself. We get more declarative and readable code, even here, and you wait till we see this tidied up a little bit, even here, I got to describe what I wanted to have happen. Compare this innards of copy array and manipulate, where I'm picking each element, making changes to it, new app, this is very imperative style, step-by-step how to do it. Instead, I got this very readable style. What do you want to have the output of this line be? I want you to take this data, copy array, and manipulate it via this transformation. I want you to take this data and multiply each element by 2. It's far more readable.

Look at the innards of that function. By the way, declarative code is never in under the hood of any declarative code, there has to be some imperative code of how actually to do it. Declarative code is just when you're using it, it's far more readable. So in this case, you are copy and manipulating this data by this change to each element, that is far more readable potentially, and especially when you use other ones like filter or reduce, than handwriting what to do with the data.

These are very popular in interviews. And they are the backbone of asynchronous JavaScript, which is what makes JavaScript so powerful and popular, why it's so important, because it is the language of the web. Of doing lots of work at once, especially talking to many other computers, where callbacks are a vital part of that pattern. They sit under promises, under async await, and all of these portions.

---

## 번역 (Korean)

이것이 어떻게 가능했을까요? Michael의 질문과 여러 다른 질문들에서 이미 힌트를 보았습니다. 어떻게 함수 코드를, 즉 함수를 다른 함수의 입력값으로 전달할 수 있었을까요?

많은 언어에서는 이것이 함수를 선언하거나 저장하는 것에 대한 우리의 기본 개념을 위반합니다. 하지만 JavaScript에서는 다릅니다. JavaScript에서 함수는 일급 객체(first-class object)이기 때문입니다. 즉, 함수는 그 자체로 그냥 객체(object)입니다. 물론 호출(call)되고, 실행(invoke)될 수 있는 추가적인 능력을 가지고 있지만, 근본적으로는 그냥 객체입니다. 따라서 다른 모든 객체와 동일하게 취급될 수 있습니다. 변수에 할당할 수 있고, 잠시 후에 보게 될 점점 더 많이 사용되는 함수 선언 방식도 실제로 레이블(label)에 할당하는 방식을 사용합니다.

함수를 다른 객체의 프로퍼티(property)로 할당할 수도 있는데, 그럴 때 뭐라고 부르죠, Chris? 객체에 속한 함수를 무엇이라 부르나요? — 메서드(method)입니다. — 맞습니다. 두려워하지 마세요. 함수를 입력값으로 전달할 수도 있습니다. 객체를 전달할 수 있다는 것은 이미 알고 있습니다. 객체를 다른 함수에 전달하는 것은 누구도 이상하게 생각하지 않습니다. 마찬가지로 함수도 다른 함수의 입력값으로 전달할 수 있습니다.

마지막으로, 함수가 코드 어디서든 전달하고 이동시킬 수 있는 완전한 객체라는 사실은, 다른 함수의 출력값(output value)으로 반환할 수도 있다는 것을 의미합니다. 이전 예제에서는 배열을 반환했지만, 함수를 다른 함수의 출력값으로 반환하는 것도 가능합니다. 바로 그렇게 할 때, JavaScript에서 가장 난해한 기능인 클로저(closure)가 등장합니다. 그 내용은 이후에 살펴보겠습니다. 먼저, 우리 코드에서 어느 것이 고차 함수(higher-order function)이고 어느 것이 콜백 함수(callback function)인지 짚어보겠습니다.

고차 함수를 선언할 때 특별한 키워드가 필요했을까요? Austin, 우리 코드에서 어느 것이 고차 함수, 즉 더 큰 역할을 하는 함수라고 생각하나요? — `copyArrayAndManipulate`라고 생각합니다. — 그렇다면 입력으로 전달될 수 있는 작은 함수, 즉 콜백 함수는 무엇이라고 생각하나요? — `multiplyBy2`입니다.

고차 함수는 다른 함수를 입력으로 받거나, 곧 보게 될 것처럼 다른 함수를 출력으로 반환하는 외부 함수입니다. 그리고 콜백 함수는 그 안에 삽입되는 함수입니다.

사실 저는 여기서 '콜백 함수'라는 용어를 그대로 쓰는 것이 썩 마음에 들지 않습니다. 이후에 JavaScript의 또 다른 영역에서, 다른 함수에 전달된 함수가 나중에 호출되어 돌아오는 경우를 보게 될 것입니다. 그 경우에는 '콜백(callback)'이라는 이름이 정확히 맞습니다. 그러나 지금 우리의 `multiplyBy2`는 `copyArrayAndManipulate` 내부에서 즉시 실행됩니다. 이런 함수는 여전히 고차 함수와 콜백 함수라고 불리지만, 핸들러(handler, 각 요소에 무엇을 할지 처리하는 것), 변환 함수(transformation function), 또는 인자 함수(argument function)라고 부를 수도 있습니다. 특히 레이블 없이 전달되는 경우, 다른 언어에서는 람다 함수(lambda function)라고도 부릅니다. 하지만 기술적으로는 여전히 콜백 함수입니다. 다만 '나중에 돌아온다'는 뉘앙스를 주기 때문에 어색하다는 것입니다. 여기서는 그 자리에서 즉시 실행됩니다.

비동기(asynchronous) JavaScript에서는, 이 마라톤이 끝나기 전에 보게 되겠지만, 다른 함수에 전달된 후 한참 뒤에 호출되어 돌아오는 함수를 실제로 만나게 됩니다. 하지만 그것은 지금의 이야기가 아닙니다.

고차 함수는 함수를 입력으로 받거나 출력으로 반환하는 함수입니다. 그냥 용어일 뿐입니다. `copyArrayAndManipulate`를 선언할 때 다른 함수를 받는다는 것을 명시하는 특별한 키워드가 필요하지 않았습니다. 그냥 그렇게 동작할 뿐입니다. 본질적으로 다를 것은 없습니다.

하지만 콜백 함수와 고차 함수는 코드를 단순화하고 DRY(Don't Repeat Yourself, 중복 배제) 원칙을 지킬 수 있게 해줍니다. 선언적(declarative)이고 가독성 높은 코드를 작성할 수 있게 됩니다. 이 코드가 조금 더 정돈되면 더욱 명확해집니다. `copyArrayAndManipulate`의 내부를 보면, 각 요소를 하나씩 꺼내서 변경하는 명령형(imperative) 스타일, 즉 단계별로 어떻게 할지 기술하는 방식입니다. 반면 사용하는 쪽의 코드는 이렇게 읽힙니다: "이 데이터를 가져다가 이 변환을 적용해서 조작하라. 각 요소에 2를 곱하라." 훨씬 가독성이 높습니다.

함수 내부를 보면, 어떤 선언적 코드도 그 내부(under the hood)에는 반드시 명령형 코드가 존재합니다. 실제로 어떻게 수행할지 기술해야 하기 때문입니다. 선언적 코드란 사용하는 쪽의 코드가 훨씬 읽기 쉽다는 것을 의미합니다. 이 경우, 데이터를 복사하고 각 요소에 이 변환을 적용해서 조작한다는 것이 훨씬 명확하게 읽힙니다. 특히 `filter`나 `reduce` 같은 메서드를 사용할 때는, 데이터를 직접 손으로 처리하는 코드보다 훨씬 가독성이 높습니다.

이 개념들은 기술 면접에서도 매우 자주 등장합니다. 그리고 비동기 JavaScript의 근간이기도 합니다. 비동기 JavaScript야말로 JavaScript를 강력하고 인기 있게 만드는 핵심이며, 웹의 언어로서 그렇게 중요한 이유입니다. 동시에 많은 작업을 처리하고, 특히 여러 다른 서버와 통신하는 데 있어 콜백은 그 패턴의 핵심적인 부분입니다. 프로미스(Promise), 비동기/대기(async/await), 그리고 이 모든 개념들의 기반이 됩니다.