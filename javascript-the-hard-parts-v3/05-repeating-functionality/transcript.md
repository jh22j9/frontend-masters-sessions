# JavaScript: The Hard Parts v3 - Repeating Functionality

## 원문 (English)

Okay, folks. So we are back building out this function here that's gonna take in an array. Copy right Mr. Bob it was take in an array by the looks of it, 1, 2, 3 here, and create a new array, is then gonna iterate through loop through the input array. Take each element mode from other than by two, fill up the output array with all those doubled values and return out that annuity filled out for the array. Okay, it's something we do, some sort of task like this. We're gonna build it out, diagram it out, but we're then gonna see another function that's going to look, I'm gonna sound out my best position. We're gonna see another function that's, that's why I don't like podiums. We're gonna see another function that's gonna look particularly frustratingly similar. And yet, we're gonna build it out from scratch, again. All right, let's start with our friend Adam. Here we go, Adam. Take it away. Let's start walking through our code here. What are we doing, Adam, in line one of our code here? What are we storing into Global Memory, Adam?

>> Speaker 2: Yeah, function definition with the label copier A and multiply-

>> Will Sentance: Very nice, yeah, the function definition with the label copy array, gonna try and keep this neat, copy array and multiply by two, excellent. The whole function definition is saved, there we go. So, there it is, copy array and multiply by two, it's saved. Let's hit up Matt now, with what is our next line that gets done by JavaScript, executed by JavaScript, Matt?

>> Speaker 2: We're gonna define the constant of my array.

>> Will Sentance: Beautiful. Thank you, Matt. It's got what value?

>> Speaker 2: An array of 123.

>> Will Sentance: Beautiful. Thank you, Matt. Now we hit the next Matt, keep going. Now we're gonna define the constant of result. Excellent, I'm really enjoying Matt's loud and confident voice here. So we're gonna keep staying with Matt. Matt, it's uninitialized for now, well we've got to go and do what?

>> Speaker 2: Execution context.

>> Will Sentance: Execution context to execute what?

>> Speaker 2: Copy array and multiply by two.

>> Will Sentance: Copy array and multiply by two, there it is, multiply by two. And we're gonna pass in what as our argument?

>> Speaker 2: My array.

>> Will Sentance: Yeah, which is one, two, three, the array one, two, three. Let's create that brand new-

>> Speaker 3: Execution context.

>> Will Sentance: Well done, people. While I just get my edge here. Okay, there is the execution context, here it is. Let's just put on the call stack, we're good. We're recording this version, right, aren't we? So we're gonna be extra complete in all our, all our stuff. So that for posterity. We have, yeah, copy, array. Multiply by 2, we're running it, it's on our call stack. We always have global, come to second down, I promise. We always have global, did I get anything wrong?

>> Speaker 2: No, you just got it, I was gonna ask about the global.

>> Will Sentance: Excellent, global's there on the, I did just get it, excellent, [LAUGH] correct answer, well done well. And now, here's our execution context. With the thread of execution going through the code line by line on the left hand side here, I'm not gonna always write out every line of code, but roughly we're going through the thread of execution. Running the code, and then here's our local memory. Matt, back to you. What is the first thing in our local memory? What are we storing in there immediately?

>> Speaker 2: Our array parameter.

>> Will Sentance: Yes, with assigned what argument?

>> Speaker 2: One two three.

>> Will Sentance: Yep, one two three, there it is. My array shot in, fills an array, one two three. Then Dan, we hit what line next? We hit the body of the function now. Which says to do what?

>> Speaker 2: We want to create a new constant output.

>> Will Sentance: He's spot on, well done, exactly. And assign a what?

>> Speaker 2: A empty array.

>> Will Sentance: Excellent from Dan. Now we hit our for loop. Here it is, for loop. We're not gonna go into the intricacies of how you know it's declared but we know that the body of it the bit between the curly braces that line is gonna be done X number of times. How many number of times? As long as I is less than the length of the array, I saw zero, the length of the array's three, we're gonna increment increase I each time. And then we're going to run that line as long as I continues to be less or zero continues to be, I can do to be less than the length of the array, so that's gonna be three times. We're gonna see it play out into the body of the folder we go. I'm going to put these on the left-hand side here, not copying them, but just so we can work with them more easily. We have our array one two three and our output is an empty array, Kayla?

Let's hit the body of the for loop. Array position zero is what, Kayla?

>> Speaker 4: One.

>> Will Sentance: One, we take the one and we do what with it, Kayla?

>> Speaker 4: We multiply it by 2-

>> Will Sentance: x when we get 2 we do what with that, Kayla?

>> Speaker 4: We push it into the output array.

>> Will Sentance: We push into the output array, there it is. Two, beautiful, good job. Kayla, excellent. Next one Kayla, this time I's value is what?

>> Speaker 4: 2.

>> Will Sentance: There you go don't worry, i's value next time.

>> Speaker 4: 1.

>> Will Sentance: Don't worry people, in my new hot pot I literally only iterate to section make people consistently get that wrong every single time. Because in the end, this is a bat. It's a dated interface for data. But if I've got a collection of data, I don't really care about the index position most of the time, I just want to get the next element. And there's obviously a dated interface dated way of accessing data. I take my data like a anachronistic document any better is it. [LAUGH] Okay, an old fashioned I'd say, like a not perfectly designed, or, because we don't really care about the index most of the time we're using it, we just wanna get the element. And there are new ways of doing it, we're sticking with the traditional way here. That's what I was gonna say, we're not gonna go through the for loop's sort of journey, but it is an interesting interface for data. And Kayla sort of showed us there why, ultimately, seasoned engineers every time like Kayla will still go. Anyway, you get the point. So there it is, position 1, get to 2, take it, multiplied by 2 to get 4, push it to output. There we have it. So for loops, we do it again. There we go, 2, 4, 6.

And now the final line Kayla, of copy array multiplied by 2 says to do what?

>> Speaker 4: We return the output array.

>> Will Sentance: Yeah, we return the value of output into what global constant, Kayla?

>> Speaker 4: Result.

>> Will Sentance: Into result, well on. And there it is, folk. We took in one, two, three, we ran copyArrayMultipleBy2 on that argument, we've created a brand new array and returned out that array filled in with our doubled elements. Good task, there we go, great. One second, Jason. Well done, everybody.

You love this function. [LAUGH] You're gonna love my next function. I don't even wanna get started on how exciting my next function is. Look at this one. [LAUGH] It moved, it's less compelling when it moves. Okay, everyone can see how much it's changing here, people. One character. Literally one character, exactly. What do you think has changed? Everything's the same, but we're gonna rebuild it from scratch because we want to feel in our,

>> Will Sentance: Gut, so to speak. That this is problematic, and there must be a better way.

---

## 번역 (Korean)

자, 여러분. 이제 다시 돌아와서 이 함수를 만들어보겠다. 배열을 입력받아, 보다시피 1, 2, 3이 들어오고, 새 배열을 생성한다. 그런 다음 입력 배열을 순회하면서 각 요소를 2로 곱하고, 그 두 배 값들로 출력 배열을 채운 뒤 반환한다. 이런 종류의 작업을 하는 것이다. 이 함수를 만들어보고 다이어그램으로 그려볼 것이다. 그런 다음 또 다른 함수를 보게 될 텐데, 매우 짜증스러울 정도로 비슷하게 생긴 함수다. 그럼에도 불구하고 처음부터 다시 만들어볼 것이다. 자, Adam부터 시작해보자. Adam, 시작해봐. 코드를 한 줄씩 살펴보자. Adam, 우리 코드 첫 번째 줄에서는 무엇을 하고 있는가? 전역 메모리(Global Memory)에 무엇을 저장하는가?

>> Adam: 네, `copyArrayAndMultiplyBy2`라는 레이블의 함수 정의를 저장합니다.

>> Will Sentance: 아주 좋다. `copyArray`라는 레이블의 함수 정의, 정확히는 `copyArrayAndMultiplyBy2`다. 함수 정의 전체가 저장된다. 이제 Matt에게, JavaScript가 다음으로 실행하는 줄은 무엇인가?

>> Matt: 상수 `myArray`를 정의할 것입니다.

>> Will Sentance: 훌륭하다. 그 값은 무엇인가?

>> Matt: 1, 2, 3으로 이루어진 배열입니다.

>> Will Sentance: 훌륭하다. 계속 Matt. 이제 상수 `result`를 정의한다. Matt의 크고 자신감 있는 목소리가 정말 마음에 든다. 계속 Matt에게. Matt, 지금은 초기화되지 않은 상태인데, 이제 무엇을 해야 하는가?

>> Matt: 실행 컨텍스트(Execution Context)를 만들어야 합니다.

>> Will Sentance: 무엇을 실행하기 위한 실행 컨텍스트인가?

>> Matt: `copyArrayAndMultiplyBy2`를 실행하기 위해서입니다.

>> Will Sentance: `copyArrayAndMultiplyBy2`, 2를 곱하는 함수다. 인수(argument)로 무엇을 전달하는가?

>> Matt: `myArray`입니다.

>> Will Sentance: 맞다, 값은 1, 2, 3 배열이다. 새로운 실행 컨텍스트를 생성하자.

>> Speaker 3: 실행 컨텍스트입니다.

>> Will Sentance: 잘했다. 콜 스택(Call Stack)에 올려놓겠다. 이 버전을 녹화하고 있으니 빠짐없이 완성하겠다. 콜 스택에 `copyArrayAndMultiplyBy2`가 올라가 있다. 항상 global이 아래에 있다. 맞는가?

>> Matt: 네, 방금 추가하셨어요.

>> Will Sentance: 훌륭하다, global도 있다. 이제 실행 컨텍스트가 생성되었다. 스레드 오브 익스큐션(Thread of Execution)이 왼쪽에서 코드를 한 줄씩 실행하고 있다. 모든 코드를 다 쓰지는 않겠지만 대략적으로 실행 흐름을 따라가고 있다. 그리고 로컬 메모리(Local Memory)가 있다. Matt, 로컬 메모리에 가장 먼저 저장되는 것은 무엇인가?

>> Matt: 배열 매개변수(array parameter)입니다.

>> Will Sentance: 맞다, 어떤 인수가 할당되는가?

>> Matt: 1, 2, 3입니다.

>> Will Sentance: 맞다, 1, 2, 3이다. `myArray`가 들어와서 배열을 채운다. 그럼 Dan, 다음 줄은 무엇인가? 이제 함수 본문에 진입하는데, 무엇을 하라고 하는가?

>> Dan: 새로운 상수 `output`을 만들겠습니다.

>> Will Sentance: 정확하다. 무엇을 할당하는가?

>> Dan: 빈 배열입니다.

>> Will Sentance: Dan, 훌륭하다. 이제 `for` 루프에 진입한다. `for` 루프가 어떻게 선언되는지 세부 사항은 다루지 않겠지만, 중괄호 사이의 본문이 특정 횟수만큼 실행된다는 것은 알고 있다. 몇 번 실행되는가? `i`가 배열의 길이보다 작은 동안, `i`는 0에서 시작하고 배열의 길이는 3이므로, 매번 `i`를 증가시키면서 세 번 실행된다. 이제 `for` 루프 본문을 살펴보자. 왼쪽에 배열 1, 2, 3과 빈 배열인 `output`을 놓겠다. Kayla, `for` 루프 본문을 시작해보자. 배열의 위치(index) 0은 무엇인가?

>> Kayla: 1입니다.

>> Will Sentance: 1이다. 그 1을 가져와서 Kayla, 무엇을 하는가?

>> Kayla: 2를 곱합니다.

>> Will Sentance: 2를 곱해서 2를 얻으면, Kayla, 그것으로 무엇을 하는가?

>> Kayla: `output` 배열에 넣습니다(push).

>> Will Sentance: `output` 배열에 push한다. 2가 들어갔다. 훌륭하다. Kayla, 다음 번 `i`의 값은 무엇인가?

>> Kayla: 2입니다.

>> Will Sentance: 맞다. 다음 차례 `i`의 값은?

>> Kayla: 1입니다.

>> Will Sentance: 걱정하지 마라. 이 `for` 루프의 인터페이스 때문에 숙련된 엔지니어들도 항상 헷갈려한다. 결국 `for` 루프는 데이터를 다루기에는 다소 구식(dated)인 인터페이스다. 데이터 컬렉션이 있을 때 인덱스 위치는 대부분의 경우 신경 쓰지 않고, 그냥 다음 요소를 얻고 싶을 뿐이다. 인덱스를 통해 데이터에 접근하는 방식은 시대에 뒤떨어진 방식이다. 인덱스를 대부분 신경 쓰지 않으면서도 매번 인덱스로 요소를 꺼내야 한다. 더 새로운 방법들이 있지만 여기서는 전통적인 방식을 유지한다. `for` 루프의 여정을 깊이 파고들지는 않겠지만, 데이터를 위한 흥미로운 인터페이스임은 분명하다. Kayla도 보여주었듯이 숙련된 엔지니어들도 여전히 헷갈린다. 어쨌든 요점은 이렇다. 인덱스 1에서 값 2를 가져와 2를 곱하면 4, `output`에 push한다. `for` 루프를 다시 반복하면 결과는 2, 4, 6이 된다.

이제 `copyArrayMultiplyBy2`의 마지막 줄, Kayla, 무엇을 하라고 하는가?

>> Kayla: `output` 배열을 반환합니다(return).

>> Will Sentance: 맞다. `output`의 값을 전역 상수(global constant) 어디에 반환하는가?

>> Kayla: `result`에 반환합니다.

>> Will Sentance: `result`에 반환한다. 여러분, 1, 2, 3을 입력받아 `copyArrayMultiplyBy2`를 실행하고, 새 배열을 만들어 두 배가 된 요소들로 채워 반환했다. 훌륭한 작업이다.

잠깐만, Jason. 모두 수고했다. 이 함수가 마음에 들었을 것이다. 다음 함수는 더 마음에 들 것이다. 다음 함수가 얼마나 흥미로운지 말하기 시작하고 싶지도 않다. 이것을 보라. 움직이니까 덜 인상적이지만 어쨌든. 얼마나 바뀌는지 보이는가? 딱 한 글자다. 글자 그대로 한 글자만 바뀌었다. 무엇이 바뀌었다고 생각하는가? 모든 것이 동일하지만, 처음부터 다시 만들어볼 것이다. 왜냐하면 직관적으로 느껴야 하기 때문이다.

이것이 문제가 있고, 더 나은 방법이 있어야 한다는 것을.
