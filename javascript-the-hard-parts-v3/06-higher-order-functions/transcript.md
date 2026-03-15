# JavaScript: The Hard Parts v3 - Higher Order Functions

## 원문 (English)

>> Will Sentance: Todd, take it away, Todd. Line one, what are we doing, my friend, here?
>> Speaker 2: We're defining the function, copy, array and divide by two.
>> Will Sentance: Perfect, by the way, I did the worst thing there. I did this right as you're talking, Todd. I know how upset Mark will be by that. Define the function, copy array. People, we're going to through it all over again, because again, I want you to feel in your guts how frustrating it is to do this all over again. This is code you're writing from scratch, again. And you're feeling my pain of it. Ain't no copy and pasting here. In fact, it's a clean whiteboard process. That's how hard it is to rewrite code. Surely, there must be a better way, right? Surely, right now, no. Right now, no, these are doing different things. The functions codes are different. And so we need to do it all over again. I means only one character different, but it's different. All right, Todd, next line.
>> Speaker 2: It'll define the constant myarray. Sign the value. So in the array of 1, 2 and 3.
>> Will Sentance: Beautiful. Thank you, Todd. Next line, Todd.
>> Speaker 2: Define the constant result and call the function copyArrayAndDivideBy2, passing in the argument of 1, 2, and 3.
>> Will Sentance: Very nice, exactly, call the function copyArray. Divide by 2 to be really bad if he doesn't exactly same question again, go right divided by 2 parts in my array one two three. Very nice by the way, do we notice how nice that complete technical communication was? I knew exactly what was happening. Thank you, Todd. Okay, we go into the execution context. Exactly, right. Let's put it on there you go put it on the call stack. People, this is for this is for our long term audience. They love it when we complete. CompleteArrayAndDivideBy2, it's being executed Global's always there Don't worry, Dan. Also is there, there it is. Excellent. Let's go into the execution context. And in local memory is what first Kayla?
>> Speaker 3: We define the parameter array with the value passed in of 123.
>> Will Sentance: Yeah, very nice. Which is known as our?
>> Speaker 3: A parameter.
>> Will Sentance: So a parameter is assigned our, everyone together?
>> Speaker 2: Argument.
>> Will Sentance: Argument very nice, excellent. And Kayla thanks very much on to the next line, Kayla, what's the fist thing we do in that local memory?
>> Speaker 3: We define a constant output to empty array.
>> Will Sentance: Beautiful. Excellent let's get moving on our iteration our looping through our doing a line of code the body of the for loop multiple times. Okay? There is all right, we hit the body of follow. Eyes value is what initially Taylor?
>> Speaker 3: Zero.
>> Will Sentance: Zero we take array position or asub or array index zero and get the value?
>> Speaker 3: One.
>> Will Sentance: One. We divide it by two, we get, Kayla? [LAUGH]
>> Speaker 3: 0.5.
>> Will Sentance: 0.5, or as we say in my country, everybody, maybe you've watched this all before, we say nor point five, which is a real thing. Even though it sounds like something from dynamic. Then we hit array sub, or array index, or array position one, which is the number two we divide by two we get one. Okay, we go it there it is 1.5 as well. Good, there it is. Final line of the function, Jeff, says to do what?
>> Speaker 2: Returned output.
>> Will Sentance: Yeah, I'd like it to be more precise in that Jeff. Is it output which to me output represents this whole combination of label or identifier and value but it's not the whole is it? It's just the-
>> Speaker 2: Return the value assigned.
>> Will Sentance: Yeah, exactly. Very nice. Which. Which is the array 0.51, 1.5. Excellent from Jeff. Thank you. And that's stored in global under what constant Jeff.
>> Speaker 2: Result?
>> Will Sentance: Result. Excellent. There we go. We love this function.
>> Speaker 2: Pop off the coaster.
>> Will Sentance: Again Peter, you've got to pop it off the call stack, he's spot on. Do we like this function, nah, his function is fine. Do we like that we wrote this function again from scratch? Everybody, who knows [INAUDIBLE] look at that, okay. Do we like this function? Yes, his usual function we use this with the task a lot, but Did I really have to rebuild it from scratch? I guess I did though. Because any change- I can't edit functions, right? Not once I've saved them. Unless- well, before we see, let's for now see another function. Suppose you wanted to take an array this time, create a brand new array, fill it with each element with three added to it. Yeah, exactly. It's funny people literally do groan slightly, which is very rude, Todd. [LAUGH] So, Todd, which principle are we breaking here do you think?
>> Speaker 2: Not dry.
>> Will Sentance: It's not dry, we've got. The principle is don't repeat yourself, we're rewriting. Every time we do this, we're taking an array, creating a new output array iterating through the looping through the input array, doing something to each element, returning out of the full, fully populated output array. We saw that 10 squared, nine squared, eight squared. We in the end made it well. We did some things to make sure we didn't have to rewrite each time we were able to adjust for new data each time. Can anybody tell me what could we do here? Not in terms of like a fancy title word from the talk, but there's no more intuitive sense. What could we do to save ourselves Adam, perhaps? To save ourselves from writing out these functions one after another and what can we do in a sort of intuitive sense here?
>> Speaker 2: We teeing up for a higher order function.
>> Will Sentance: Okay, I said don't mention the title of the talk [LAUGH] Dan,
>> Speaker 2: Pass functionality as a parameter.
>> Will Sentance: Beautifully put by Dan. Yes, spot on. We could leave some of our functionality. We could write these function ones, leave it in a blank where the changes is. We don't call this function copy123 and divide it by 2, copy123 and multiply it by 2. We call it copyArray. And then when we run the function, we fill in that parameter, With the specific data to make that function reusable. Folk, same thing with functionality. We can leave a little blank for the specific functionality and only fill it in once we run the function. But can I do it like this, people? Can I just leave, I don't know, can I just insert Plus three, almost as a string like that. Am I allowed to insert that and have that passed in? Am I allowed to pass in little strings of code and execute them in JavaScript? Nah. You can imagine a language that would. It's not saying you can't. It's not inherently, but not in JavaScript. So instead, how do we wrap up functionality? Bundle up functionality, such that it can execute Such as it can be passed through into and be then executed run inside another function, how can we wrap up functionality code to be passed around in JavaScript? Braden, how can we wrap it up, wrap it up with?
>> Speaker 2: In function.
>> Will Sentance: In a function he's spot on and that's exactly what we're gonna do, we could generalize our function, let's call it copyright and do something. Copy array and manipulate and look at this. I've left a little blank instructions and just as array. Wherever you saw array inside of copyright and divide by two, it literally became 123. We're gonna see that instructions is literally gonna be filled in with Multiply by two. Have a look at that. We've got multiply by two function, now I've kept it on one line but it's no different to a normal function. If run takes the number three, will it turn out three by two? We pass in multiply by two, we're gonna walk through this line by line, don't worry people. But it's literally gonna replace the word instructions there inside of copy array manipulate Save declare define the code, it's going to replace it when we run that code is going to fill in instructions with, multiply by two. And then look, multiply by two parenths. And then a reposition zeros can be the number one, multiply by two persons one. That's to say run, multiply by two With the input of 1 and return out 1 by 2 return out 2 and our instructions array I which will become multiply by 2 array position 0. That is to say the number 1 multiple by 2 parentheses 1 will return out 2 and that 2 will be pushed. Into output. We're gonna achieve exactly the same thing we did here where array position zero with a number one, multiply by two, but now we do a reposition zero is a number one, and then we stick it into multiply by two and do one by two inside their return out. The two inputs to output.

---

## 번역 (Korean)

>> Will Sentance: Todd, 이어서 진행해 주세요. 첫 번째 줄에서 무엇을 하고 있나요?
>> Speaker 2: `copyArrayAndDivideBy2` 함수를 정의하고 있습니다.
>> Will Sentance: 좋습니다. 그런데 제가 최악의 짓을 했네요. Todd가 말하는 도중에 이걸 했으니까요. Mark가 얼마나 화낼지 알 것 같아요. 함수 `copyArray`를 정의합니다. 여러분, 우리는 이 과정을 처음부터 다시 반복할 겁니다. 이걸 또 처음부터 다시 작성하는 것이 얼마나 답답한 일인지 몸소 느껴봤으면 해서요. 이 코드를 처음부터 다시 작성하는 겁니다. 제 고통을 함께 느껴보세요. 복붙(copy and paste) 같은 건 없습니다. 빈 화이트보드에서 처음부터 다시 작성하는 거예요. 코드를 다시 작성한다는 게 얼마나 힘든 일인지 알 수 있죠. 더 나은 방법이 분명 있을 것 같지 않나요? 지금 당장은 없습니다. 지금은 없어요. 이 두 함수는 서로 다른 동작을 합니다. 함수 코드가 다르기 때문에 처음부터 다시 작성해야 합니다. 문자 하나만 다를 뿐이지만, 엄연히 다릅니다. Todd, 다음 줄로 넘어가세요.
>> Speaker 2: 상수 `myArray`를 정의하고 `[1, 2, 3]` 배열 값을 할당합니다.
>> Will Sentance: 좋아요, 감사합니다. 다음 줄, Todd.
>> Speaker 2: 상수 `result`를 정의하고, `copyArrayAndDivideBy2` 함수를 `[1, 2, 3]`을 인수로 전달하여 호출합니다.
>> Will Sentance: 아주 좋습니다. 정확해요. `copyArrayAndDivideBy2` 함수를 호출하며 `myArray` [1, 2, 3]을 전달하는 거죠. 기술적 소통이 얼마나 명확했는지 눈치채셨나요? 무슨 일이 일어나고 있는지 정확히 알 수 있었습니다. 감사합니다, Todd. 자, 이제 실행 컨텍스트(execution context)로 들어갑니다. 콜 스택(call stack)에 올려봅시다. 장기 시청자분들을 위해 빠짐없이 진행하겠습니다. `copyArrayAndDivideBy2`가 실행 중입니다. 전역(Global)은 항상 거기 있습니다. 걱정 마세요, Dan. 실행 컨텍스트로 들어가 보겠습니다. 로컬 메모리(local memory)에는 무엇이 먼저 들어오나요, Kayla?
>> Speaker 3: 파라미터(parameter) `array`를 정의하고 전달된 값인 `[1, 2, 3]`을 할당합니다.
>> Will Sentance: 네, 아주 좋아요. 이것은 무엇으로 알려져 있나요?
>> Speaker 3: 파라미터(parameter)요.
>> Will Sentance: 파라미터에는 무엇이 할당되죠? 다 같이!
>> Speaker 2: 인수(argument)요.
>> Will Sentance: 인수(argument), 아주 좋습니다. Kayla, 다음 줄에서 로컬 메모리에 가장 먼저 하는 것은 무엇인가요?
>> Speaker 3: 상수 `output`을 빈 배열로 정의합니다.
>> Will Sentance: 좋습니다. 이제 반복(iteration), 즉 for 루프의 본문을 여러 번 실행하는 과정으로 넘어가겠습니다. `i`의 초기값은 무엇인가요, Taylor?
>> Speaker 3: 0이요.
>> Will Sentance: 0입니다. 배열의 인덱스(index) 0번 위치의 값을 가져오면?
>> Speaker 3: 1이요.
>> Will Sentance: 1이죠. 2로 나누면, Kayla? [웃음]
>> Speaker 3: 0.5요.
>> Will Sentance: 0.5입니다. 제 나라에서는 "nought point five"라고 부르는데, 실제로 쓰는 표현이에요. 역동적인 언어에서 나온 것처럼 들리지만요. 그 다음 배열 인덱스(index) 1번 위치의 값, 즉 숫자 2를 2로 나누면 1이 됩니다. 1.5도 마찬가지로 나왔네요. 좋아요. 함수의 마지막 줄에서 Jeff, 무엇을 하나요?
>> Speaker 2: `output`을 반환(return)합니다.
>> Will Sentance: 좀 더 정확하게 표현해 줬으면 해요, Jeff. `output` 자체는 레이블(label, 식별자)과 값의 조합 전체를 의미하는데, 반환하는 건 그 전체가 아니잖아요. 정확히는-
>> Speaker 2: 할당된 값을 반환합니다.
>> Will Sentance: 맞아요, 정확합니다. 즉 배열 `[0.5, 1, 1.5]`를 반환하는 거죠. Jeff, 훌륭합니다. 그리고 그 값은 전역(global)에서 어떤 상수에 저장되나요, Jeff?
>> Speaker 2: `result`요.
>> Will Sentance: `result`입니다. 훌륭해요.
>> Speaker 2: 콜 스택에서 팝(pop)해야 하죠.
>> Will Sentance: Peter, 맞아요. 콜 스택에서 팝(pop off)해야 합니다. 정확합니다. 이 함수 자체는 괜찮습니다. 하지만 이 함수를 처음부터 다시 작성했다는 사실이 마음에 드나요? 함수를 저장한 이후에는 편집할 수 없잖아요. 그렇죠? 일단 저장하고 나면 수정할 수 없습니다. 자, 지금은 다른 함수 하나를 더 살펴보죠. 이번에는 배열을 받아서, 각 요소에 3을 더한 새로운 배열을 만들고 싶다고 해봅시다. 맞아요. 사람들이 실제로 약간 신음 소리를 내는데, 그건 정말 무례한 거예요, Todd. [웃음] 그렇다면 Todd, 우리가 어떤 원칙을 위반하고 있다고 생각하나요?
>> Speaker 2: DRY 원칙을 위반하고 있어요.
>> Will Sentance: 맞습니다, DRY 원칙이죠. 원칙은 "반복하지 말라(Don't Repeat Yourself)"입니다. 우리는 매번 코드를 다시 작성하고 있어요. 배열을 받아서, 새 출력 배열을 만들고, 입력 배열을 순회하며, 각 요소에 어떤 작업을 수행하고, 완전히 채워진 출력 배열을 반환하는 패턴을 매번 반복하고 있습니다. 10의 제곱, 9의 제곱, 8의 제곱에서도 봤죠. 결국 우리는 데이터만 바꾸면 되도록 해결책을 만들었습니다. 여기서 어떻게 할 수 있을까요? 멋진 용어를 쓰지 말고, 직관적인 의미에서 우리가 무엇을 할 수 있을까요? Adam, 한 함수씩 작성하는 수고를 덜려면 어떻게 하면 될까요?
>> Speaker 2: 고차 함수(higher order function)를 준비하고 있는 거죠.
>> Will Sentance: 이미 이 강의의 제목을 언급하지 말라고 했잖아요 [웃음] Dan,
>> Speaker 2: 기능(functionality)을 파라미터(parameter)로 전달하면 됩니다.
>> Will Sentance: Dan이 정말 멋지게 표현했네요. 맞아요. 우리는 기능의 일부를 비워둘 수 있습니다. 함수를 한 번만 작성하되, 변경되는 부분은 빈칸으로 남겨두는 거죠. `copyArrayAndDivideBy2`, `copyArrayAndMultiplyBy2` 같은 이름 대신, 그냥 `copyArray`라고 부릅니다. 그리고 함수를 실행할 때, 파라미터에 구체적인 데이터를 채워 넣어서 함수를 재사용 가능(reusable)하게 만드는 거죠. 기능(functionality)도 마찬가지입니다. 구체적인 기능을 위한 빈칸을 만들어두고, 함수를 실행할 때만 채워 넣으면 됩니다. 하지만 이렇게 할 수 있을까요? 예를 들어 `+3`을 거의 문자열(string)처럼 그냥 삽입할 수 있을까요? JavaScript에서 코드 조각(strings of code)을 전달해서 실행할 수 있을까요? 안 됩니다. 그런 언어를 상상할 수도 있겠죠. 불가능한 것은 아니지만, JavaScript에서는 그렇게 되지 않습니다. 그렇다면 기능을 어떻게 묶을 수 있을까요? 다른 함수 안에 전달되어 실행될 수 있도록 기능을 묶으려면 JavaScript에서 어떻게 해야 할까요? Braden, 어떻게 묶을 수 있을까요?
>> Speaker 2: 함수(function) 안에 넣으면 됩니다.
>> Will Sentance: 함수 안에 넣는 거예요. 정확합니다. 그게 바로 우리가 할 일입니다. 함수를 일반화(generalize)할 수 있어요. 이름을 `copyArrayAndManipulate`라고 지어봅시다. 보세요, `instructions`라는 빈칸(파라미터)을 남겨두었습니다. `copyArrayAndDivideBy2` 안에서 `array`가 `[1, 2, 3]`으로 채워졌던 것처럼, `instructions`는 `multiplyBy2`로 채워질 겁니다. 보세요, `multiplyBy2` 함수가 있습니다. 한 줄로 작성했지만, 일반 함수와 다를 게 없습니다. 숫자 3을 받으면 3 곱하기 2를 반환하겠죠. `multiplyBy2`를 전달하면, 코드를 한 줄씩 살펴볼 때 `instructions`라는 단어가 `multiplyBy2`로 대체됩니다. 그러면 `multiplyBy2(array[i])`가 되는데, `array[0]`은 숫자 1이므로, `multiplyBy2(1)`은 2를 반환하고 그 2가 `output`에 푸시(push)됩니다. 우리가 이전에 배열 인덱스 0번의 값 1에 `multiplyBy2`를 적용했던 것과 정확히 같은 결과를 얻을 수 있습니다. 이제 인덱스 0번 값인 1을 `multiplyBy2`에 넣어서 `1 * 2 = 2`를 계산하고, 그 결과가 `output`에 들어가는 방식입니다.
