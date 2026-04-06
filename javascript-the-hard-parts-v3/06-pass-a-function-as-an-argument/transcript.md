# Will Sentance - Pass a Function as an Argument

## 원문 (English)

So we did love these three very repeatable functions, very reusable, not very reusable functions. Let's introduce now a copy array and we'll call it manipulate. We don't yet know what we're going to do to it, and this function, people, we're going to build out line by line.

So we're going to define copy array and manipulate. We're then going to define a new function which we haven't seen, or we saw it earlier, but we haven't seen in this code, multiply by 2. All it's going to do is take in an input and return out that input doubled. But it's got to be wrapped in a function, it can't just be 2 as a string.

We're then going to run copy array and manipulate, pass in our 123, our array, and pass in a whole function, and it's going to be the whole function definition, all of its code into the running of copy array and manipulate, where we're then going to create a new output array, empty for now. We're then going to loop through each element of the input array 123. Take the first element, 1, throw it into instructions. Which will be multiplied by 2, run that new execution context, 1 by 2, return it out, and that whole instructions parentheses array, or let's call it multiply by 2 parentheses 1, is going to evaluate, turn into its output, which will be 1 by 2, 2, and that'll be pushed to our output array.

So here we go, people, let's do it, and we will, from this, see all there is to see in our relatively otherwise obscure magic behind some of the functions we use every day: map, reduce, filter, and some we use every day that historically did not behave like this, reverse being one of them.

And we will also in a moment learn about some new, only in the last two years, I think it's ES 2023, functions that finally turn some of our built-in array methods that did not follow this pattern into functions that do follow this pattern. Things like toReversed, but we'll come to that very soon. For now, let's jump in.

Here we go. We have a global execution context, a global memory, and we get defining, and my lucky person is going to be Joe. Joe, what are we doing in line one? You did so well, you're back up. What are we declaring in line one? Walk me through. Declaring a function called copy array and manipulate. Manipulate, yep. And that'll be a little nice F box all the code. Oh, I love the way you put that. Yep, beautiful.

Then we will go down to do the same thing, we'll declare another function, multiply by 2. Which is an even smaller little one, yeah, it's also a little exactly, exactly. Let's also, as we've turned on JavaScript, have our call stack initialized with what on the very bottom. What are we always running, yeah, global, well done, exactly, global execution context. That's where we're running, executing our code from the moment we start JavaScript.

OK, excellent. Now, what line do we hit, Joe? Now we are pointed to the constant result. Yes, we declare result, and we are going to assign it, what, do we know what to store in it yet? No, no uninitial. It's going to be uninitialized, exactly. Used to be undefined, but now we're dealing with constants, we definitely can't change a data type from undefined to the return value. There are other things which might be interesting about how we can mutate constants, but we certainly can't change its data type from undefined to an array, or whatever we get out of calling this function. It remains uninitialized, it hasn't been assigned anything yet, and we have to go and do the work, which is what, Joe? So we're getting a new execution context. Yes, excellent, to run copy array and manipulate.

I think if you say it so fast, you should have to be the one who has to write it up. OK, to which we're passing what arguments? The first argument is an array holding the values of integers 1, 2, and 3 in positions 0, 1, and 2. And the second argument is the multiply by 2 function, so yeah, it's the whole function definition.

To be honest, I should really, I don't know, what color can I use, I should really like, you know, it's not multiply by 2, is it? Sorry, it's not the label multiply by 2, we use the label multiply by 2 to find the function code, right, the F box as you put it, and that's what's going to be passed in. What's its label before we even go any further, Joe? What's the label by which we can refer to multiply by 2 inside the execution context of copy array and manipulate? It's not going to be multiply by 2, what are we going to refer to it as? Instructions. In fact, we're going to just literally replace its label, we're going to take this function code and inside here give it a new label, so I'm just going to kind of make that clear that the pink bit is what we're actually passing in I'm going to put little quotes around this, I kept multiply by 2 there just for our visual, but that's OK.

And as you said, a brand new, everyone together for the energy, a brand new what? Execution context. A brand new what? Execution. So everyone was great. I was catching someone's eye who was, no, to be fair, if you're sitting by yourself right next to me, is it extra weird to shout execution context by your, OK.

Alright, there it is, new execution context, gets added to the call stack, right? Copy. Sorry, it's just I love that I choose these long names in order to make it easier to understand what these are doing. But of course, could you think of a more silly idea than to choose long names for functions when you have to write them out in what a ridiculous idea.

OK, into copy array and manipulate we go, it's top of the call stack, the thread of execution's woven its way in, and we hit first our parameter argument combinations in our local memory. So this is, yeah, go ahead, Joe, please. So in the local we have array with the array 123, and we have instructions as the F block that was initially known as multiply by 2. Oh, thank you, Joe, you're so good. Exactly, instructions is the code, the function code, which we can just run by using the, well, let's actually go ahead.

Yeah, and then we have the const output. So this was previously known as multiply by 2, do you mind if I just put like this above, just so that we remember, but we know that we can't refer to it as that. It's lost that name, it doesn't care about that name because now we'll refer to it inside of this copy array and manipulate as what? Instructions, and that's so that we don't just get to pass in multiply by 2, but we could also pass in add 3. We could also pass in divide by 2. We can pass in whatever specific code we want to have.

Well, hopefully we've written this function in such a way that that specific code will hit first this element, then this element, then this element, and respectively pass them out to what new constant that we're declaring, Joe? Output, output, which is for now, an empty array, an empty, a big old empty array.

So, let's head up here, and I'm just going to make it easier for us to visualize. I'm not actually copying these, I'm just making it easier to visualize. And we have, OK, so we hit the body of the for loop. And we are going to, oh, go ahead, Joe. Yeah, so now we're inside the for loop. Our initial I value is 0. Beautiful, which means we grab the 0th element of what? Of the array. Yes, which is position, yeah, which is zero value. I'll never, I tell you, it's a weird, it's a weird interface, exactly which is the number what? 1, 1. OK.

Now, so array position 0 has turned into the number 1 has evaluated, has turned into a value. Nothing in JavaScript when it's running stays a label, it has to be evaluated, turned into whatever it actually is in memory. Here, array position 0 is the number 1. What else is a label here that needs to be turned into what it actually is? Instructions, which is going to turn into multiply by 2. Exactly.

So we have instructions position, taking away position 0, which is 1, we know that that really is running multiply by 2 with input of 1, at least the multiply by 2 function we defined out here. So we create a brand new what, Joe? Execution context, execution context, exactly. Into it we go, and in its local memory, we have an input, beautiful, of 1. Input, we look down to our function definition, and we see input is now going to be filled in with 1, we then do what? We return 1 times 2, which is 2. And that returns out into what? And it's pushed into what? It's pushed into the output arrays first position.

What did I miss out? What did I forget to do? Michael, what did I forget to do with my instructions being invoked, we saw Pere, oh yeah, by the way, how do we know to invoke, Joe, instructions, how do we know to run it? Because we called it within as an argument in the push function. What does the instructions have on the end of it that says go run me? Parentheses, exactly, that's really crucial. If we just referred to it, we'd just be kind of copying the function code in and we have to say go run it without the parens with the input of 1.

And we then added it to what, Michael? Call stack. Exactly, I'm going to put instructions because that's what we're calling it inside. And so now that's top of our call stack, but we then exited it, our thread wove in, it's weaving back out. And so, what do we, yes, we pop it off, exactly, well done to Michael, we pop it off.

And now we hit, we put our 2 into output, but it's a for loop, so we hit the body of the for loop again, this time, Michael, i's value is what? 1, 1, therefore, array position 1 is what? So 2, 2, beautiful, and so we have instructions, parens, 2. Instructions. So this execution context is closed instructions parens 2.

What is instructions being fully replaced by as we evaluate that line? Multiply by 2. Yeah, spot on, Matt, exactly, so array, as we hit that line there, Michael, array position 0, array is 123, so array position 1 is the number 2, instructions got fully replaced, just like array did with. So 123 filled in the array parameter and instructions got filled in with the code of what function we defined outside of copy array and manipulate, Michael? Multiply by 2. He's spot on, exactly, and so we're really running multiply by 2, and we know we could also be running something else. Add 3, whatever we want, because our copy array and manipulate can take in any functionality.

Into multiply by 2 we go, or instructions, we go, and we do brand new execution context, add it to the call stack, and it returns out what, Joe? 2 by 2, 4, 4, which we then put where, or we push where, sorry, puts are wrong, we then push where, Joe? Position 1 of the output. Beautiful, and we have 2, 4. Are we going to do it again? No. But yes, exactly, we are going to do it again, and we get 6. So our output array is 2, 4, and 6, and now, Austin, we've got our output array.

What do we do with it in the very last line of copy array and manipulate, because we don't want it stuck inside copy array and manipulate, we want it somewhere else, so what do we do? We're going to return it. We return, let's be really precise, we return what? Return the output array? Absolutely, or return the value of output, which is our array of what values 2, 4, 2, 4, 6 we return the value of the output array, and our whole copy array and manipulate invocation or call or execution gets thrown away and replaced with 2, 4, 6, which gets assigned to what global constant, Austin? Result, result. There it is.

Well done, everybody. We managed to run or create a function, copy array and manipulate that did not predefine what we did to each element. Instead, we left it blank with a TBD, a label, a parameter. That when we ran copy array and manipulate, that's when we inserted the actual specifics of what we wanted to do.

What, Michael, did we call our parameter, our placeholder, our TBD to be filled in later? What do we call it in copy array and manipulate? Instructions. Instructions, spot on. And what do we call our one for the array, Michael? Array, array, well done. Yeah, exactly, array and instructions.

People that allowed us, but notice, it's a bit of a flip, we had array position 0, do you remember earlier we had array position 0, which was, let's say 1, and then we multiply by 2, operators are like funny little functions where they almost take in the stuff on either side, right? We're now saying, take this bit and run, wrap this bit up and throw the 1 into a function that does that work, and then output the result. Because if we want to pass functionality around in JavaScript, can we just pass 2 around?

How do we wrap up functionality work that we, instructions that we want to have done in JavaScript, there are, there are always certain things that I end up going on and on about because they're on my mind at a certain point. What is, so, what is, how do we wrap up functionality that we want to pass around in JavaScript, in this case, we want to pass in as an input when we run copy array and manipulate so we can leave TBD a portion of the code. How do we wrap up functionality in JavaScript? Everyone together. Function. Yay, very good, hold on.

---

## 번역 (Korean)

이렇게 우리는 세 가지 반복 가능한 함수들을 살펴보았습니다. 재사용하기 어려운 함수들이었죠. 이번에는 배열을 복사하는 함수를 만들고 `copyArrayAndManipulate`라고 부르겠습니다. 이 함수로 정확히 무엇을 할지는 아직 모릅니다. 이 함수를 한 줄씩 직접 만들어 보겠습니다.

먼저 `copyArrayAndManipulate`를 정의합니다. 그런 다음 이전에 잠깐 등장했지만 이 코드에서는 아직 보지 못한 새 함수 `multiplyBy2`를 정의합니다. 이 함수는 입력값을 받아서 두 배로 만든 값을 반환할 것입니다. 단, 반드시 함수로 감싸야 합니다. 그냥 숫자 2를 문자열로 전달할 수는 없습니다.

그런 다음 `copyArrayAndManipulate`를 실행할 것입니다. 배열 `[1, 2, 3]`을 전달하고, 함수 전체를 두 번째 인자로 전달합니다. 함수 정의 코드 전체가 `copyArrayAndManipulate`의 실행 안으로 전달되는 것입니다. 그 안에서 새로운 출력 배열(output array)을 생성하는데, 처음에는 비어 있습니다. 그리고 입력 배열 `[1, 2, 3]`의 각 요소를 순회합니다. 첫 번째 요소인 1을 `instructions`에 던져 넣으면, `multiplyBy2`가 실행되어 새로운 실행 컨텍스트(execution context)가 만들어지고, 1×2의 결과가 반환되어 출력 배열에 추가됩니다.

자, 시작합니다. 이 과정에서 우리가 매일 사용하는 함수들의 동작 원리를 이해하게 될 것입니다. `map`, `reduce`, `filter`가 그것들입니다. 그리고 역사적으로 이 패턴을 따르지 않았지만 매일 사용하는 `reverse` 같은 함수들도 있습니다.

곧 ES2023에서 새로 추가된 함수들도 살펴볼 것입니다. 기존에 이 패턴을 따르지 않던 내장 배열 메서드(array method)들을 이 패턴을 따르도록 만든 함수들입니다. `toReversed` 같은 것들이 그 예인데, 잠시 후에 다루겠습니다. 지금은 바로 시작하겠습니다.

시작합니다. 전역 실행 컨텍스트(global execution context)와 전역 메모리(global memory)가 있습니다. Joe, 1번째 줄에서 무엇을 하고 있나요? 잘 하셨으니까 다시 나와 보세요. 1번째 줄에서 무엇을 선언하고 있나요? 설명해 보세요. — `copyArrayAndManipulate`라는 함수를 선언하고 있습니다. — 맞습니다. 함수 코드 전체가 하나의 박스(F box)에 담기는 거죠. — 정말 좋은 표현이에요. 맞습니다.

그 다음으로 내려가서 같은 작업을 합니다. `multiplyBy2`라는 또 다른 함수를 선언합니다. 훨씬 작은 함수입니다. 그리고 JavaScript를 실행하면, 콜 스택(call stack)의 맨 아래에는 항상 무엇이 있어야 하죠? 항상 실행되고 있는 것은요? — 전역 실행 컨텍스트요. — 맞습니다. JavaScript를 시작하는 순간부터 코드를 실행하는 곳이 바로 거기입니다.

좋습니다. 이제 어떤 줄을 실행하나요, Joe? — 상수 `result`가 있는 줄입니다. — 맞습니다. `result`를 선언하는데, 지금 당장 무엇을 저장할지 알고 있나요? — 아니요, 아직 초기화가 안 된 상태입니다. — 맞습니다, 미초기화(uninitialized) 상태입니다. 이전에는 `undefined`였지만, 지금은 상수(const)를 다루고 있기 때문에 `undefined`에서 반환값으로 데이터 타입을 변경할 수 없습니다. 상수를 변경하는 방식에 대해 흥미로운 점들이 있지만, `undefined`에서 배열이나 이 함수의 반환값으로 데이터 타입을 바꿀 수는 없습니다. 아직 아무것도 할당되지 않은 미초기화 상태로 남아 있고, 우리는 실제 작업을 수행해야 합니다. 무엇을 하는 거죠, Joe? — 새로운 실행 컨텍스트를 만듭니다. — 맞습니다. `copyArrayAndManipulate`를 실행하기 위해서입니다.

그렇게 빠르게 말하시면 직접 쓰셔야 할 것 같네요. 어떤 인자(argument)를 전달하나요? — 첫 번째 인자는 0, 1, 2번 위치에 정수 1, 2, 3을 담고 있는 배열입니다. 두 번째 인자는 `multiplyBy2` 함수, 즉 함수 정의 전체입니다.

사실 한 가지 짚고 넘어가야 할 것이 있습니다. 전달되는 것은 `multiplyBy2`라는 레이블(label)이 아닙니다. `multiplyBy2`라는 레이블을 통해 함수 코드를 찾고, 그 코드 자체가 전달되는 것입니다. `copyArrayAndManipulate`의 실행 컨텍스트 안에서 `multiplyBy2`를 어떤 이름으로 참조할 수 있을까요, Joe? `multiplyBy2`가 아니라 무엇으로 부르게 되나요? — `instructions`입니다. — 맞습니다. 레이블이 완전히 바뀌는 것입니다. 함수 코드를 가져와서 이 안에서는 새로운 레이블을 부여합니다. 시각적으로 확인하기 위해 `multiplyBy2`는 남겨두겠지만, 실제로 전달되는 것은 함수 코드 자체입니다.

자, 모두 함께 에너지를 모아서 — 새로운 무엇이 만들어지나요? — 실행 컨텍스트요! 또 새로운 무엇? — 실행 컨텍스트! — 모두 잘하셨습니다.

새 실행 컨텍스트가 콜 스택(call stack)에 추가됩니다. `copyArrayAndManipulate`를 실행합니다. 이렇게 긴 이름을 붙이면 무엇을 하는지 이해하기 쉬울 것 같아서였는데, 막상 직접 써야 할 때는 정말 황당한 생각이었네요.

`copyArrayAndManipulate` 안으로 들어갑니다. 현재 콜 스택의 최상단에 있고, 실행 흐름(thread of execution)이 여기로 진입했습니다. 먼저 로컬 메모리(local memory)의 매개변수-인자(parameter-argument) 조합을 확인합니다. Joe, 설명해 주세요. — 로컬 메모리에는 배열 `[1, 2, 3]`을 담은 `array`와, 원래 `multiplyBy2`라고 알려진 함수 코드를 담은 `instructions`가 있습니다. — 정말 잘하셨어요. 맞습니다. `instructions`는 함수 코드 자체로, 괄호를 사용해서 실행할 수 있습니다.

그리고 `const output`이 있습니다. 원래 `multiplyBy2`였던 것인데, 이제는 `instructions`라는 이름으로 참조해야 합니다. 이렇게 하면 `multiplyBy2`만 전달하는 것이 아니라, `add3`이나 `divideBy2` 등 원하는 어떤 코드든 전달할 수 있습니다.

이 함수는 전달받은 코드가 배열의 첫 번째 요소, 두 번째 요소, 세 번째 요소를 차례로 처리하도록 작성되어 있습니다. 결과들은 어떤 상수에 담기나요, Joe? — `output`입니다. 지금은 비어 있는 배열이고요. — 맞습니다.

이제 `for` 루프 본체로 진입합니다. Joe, 설명해 주세요. — 이제 `for` 루프 안에 있습니다. `i`의 초기값은 0입니다. — 맞습니다. 그러면 배열의 0번 인덱스(index) 요소를 가져오는데, 그 값은 무엇이죠? — 1입니다. — 맞습니다.

JavaScript에서 실행 중에는 레이블이 그대로 남지 않습니다. 모든 것은 메모리에 실제로 저장된 값으로 평가(evaluate)되어야 합니다. 여기서 `array[0]`은 숫자 1입니다. 또 다른 레이블은 무엇이 실제 값으로 대체되어야 하나요? — `instructions`입니다. `multiplyBy2` 함수 코드로 대체됩니다. — 맞습니다.

즉, `instructions(array[0])`은 실질적으로 입력값 1로 `multiplyBy2`를 실행하는 것입니다. 새로운 무엇이 만들어지나요, Joe? — 실행 컨텍스트입니다. — 맞습니다. 로컬 메모리에 `input`이 1로 설정됩니다. 함수 정의를 보면, `input`에 1이 채워지고, 그다음에 무엇을 하나요? — 1×2를 반환해서 2가 됩니다. — 그리고 그 결과는 어디로 가나요? — 출력 배열(output array)의 첫 번째 위치에 추가됩니다.

제가 빠뜨린 것이 있습니다. Michael, `instructions`가 호출될 때 제가 빠뜨린 것이 무엇인가요? Joe, `instructions`를 어떻게 실행하라고 지시하나요? — `push` 함수의 인자로 호출했기 때문입니다. — `instructions` 뒤에 무엇이 붙어야 실행하라는 의미가 될까요? — 괄호입니다. — 맞습니다. 정말 중요한 포인트입니다. 괄호 없이 참조만 하면 함수 코드를 복사하는 것에 불과합니다. 입력값 1과 함께 실행하라고 하려면 괄호가 반드시 있어야 합니다.

그리고 무엇에 추가되나요, Michael? — 콜 스택입니다. — 맞습니다. 내부에서 부르는 이름인 `instructions`를 콜 스택에 추가합니다. 실행이 끝나면 팝(pop) 되어 제거됩니다. Michael, 잘하셨습니다.

이제 2가 `output`에 추가됩니다. 하지만 `for` 루프이므로 루프 본체를 다시 실행합니다. Michael, 이번에 `i`의 값은 무엇인가요? — 1입니다. — 그러면 `array[1]`은 무엇인가요? — 2입니다. — 맞습니다. 이제 `instructions(2)`를 실행합니다.

이 줄을 평가할 때 `instructions`는 실제로 무엇으로 대체되나요? — `multiplyBy2`입니다. — 맞습니다. `array[1]`은 2이고, `instructions`는 `copyArrayAndManipulate` 밖에서 정의한 어떤 함수의 코드로 대체될까요, Michael? — `multiplyBy2`입니다. — 정확합니다. 우리는 실제로 `multiplyBy2`를 실행하는 것이며, `add3` 같은 다른 함수를 전달할 수도 있습니다. `copyArrayAndManipulate`는 어떤 기능이든 받아들일 수 있기 때문입니다.

`multiplyBy2`, 즉 `instructions`로 진입합니다. 새로운 실행 컨텍스트를 만들어 콜 스택에 추가하고, 반환값은 무엇인가요, Joe? — 2×2=4입니다. — 그 값을 어디에 추가하나요, Joe? — `output`의 1번 인덱스 위치입니다. — 잘하셨습니다. 이제 `[2, 4]`가 되었습니다. 또 반복할까요? — 아니요. — 하지만 반복합니다. 6이 추가되어 출력 배열은 `[2, 4, 6]`이 됩니다. Austin, 이 출력 배열로 무엇을 하나요?

`copyArrayAndManipulate`의 마지막 줄에서 이 배열을 함수 내부에 가두어 두지 않고 외부로 내보내려면 어떻게 하나요? — 반환합니다. — 정확히 무엇을 반환하나요? — 출력 배열을 반환합니다. — 맞습니다. `output`의 값, 즉 `[2, 4, 6]`을 반환합니다. 그러면 `copyArrayAndManipulate`의 호출 전체가 `[2, 4, 6]`으로 대체되고, 어떤 전역 상수에 할당되나요, Austin? — `result`입니다. — 바로 그것입니다.

모두 수고하셨습니다. 우리는 각 요소에 무엇을 할지 미리 정의하지 않은 함수 `copyArrayAndManipulate`를 성공적으로 만들었습니다. 대신 그 부분을 미지수(TBD)로, 레이블로, 즉 매개변수(parameter)로 남겨두었습니다. 실제로 `copyArrayAndManipulate`를 실행할 때 구체적인 내용을 채워 넣는 방식입니다.

Michael, 이 매개변수, 즉 나중에 채워질 자리 표시자(placeholder)를 `copyArrayAndManipulate` 안에서 뭐라고 불렀나요? — `instructions`입니다. — 맞습니다. 그리고 배열을 담는 매개변수는 무엇이었나요, Michael? — `array`입니다. — 잘하셨습니다. `array`와 `instructions`, 바로 이것입니다.

이것이 우리에게 가능하게 해준 것은 흥미로운 발상의 전환입니다. 이전에는 `array[0]`이 1이고, 거기에 2를 곱했습니다. 연산자(operator)는 양쪽의 값을 입력으로 받는 작은 함수 같은 것입니다. 이제 우리는 이렇게 말하는 것입니다: 이 값을 가져와서 함수로 감싸고, 1을 그 작업을 수행하는 함수에 던져 넣어 결과를 출력하라고요. JavaScript에서 기능(functionality)을 전달하려면 단순히 숫자 2를 전달할 수 있을까요?

JavaScript에서 수행하길 원하는 기능, 즉 원하는 동작을 어떻게 감쌀 수 있을까요? `copyArrayAndManipulate`를 실행할 때 코드의 일부분을 미지수로 남겨두기 위해 입력으로 전달하려고 합니다. JavaScript에서 기능을 감싸는 방법은 무엇인가요? 모두 함께 — **함수(Function)!** — 맞습니다.