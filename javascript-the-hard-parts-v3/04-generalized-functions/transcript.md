# JavaScript: The Hard Parts v3 - Generalized Functions & Higher Order Functions

## 원문 (English)

Now we move on to a cool part of functional programming, which maybe some of you thought the functional programming talk I gave here. But let's start off with why do we even have functions? Why do we even have them? Let's see why. Let's create a function 10 squared, takes no input, returns 10 by 10, and for once I want the exact wordings of these functions. So, Ryan, what's the exact wording of this function? Function 10 squared take no input returns, what'll be the exact wording Ryan, here? We're gonna use the traditional style. By the way, I should say, we're gonna see an ever more popular style of declaring and saving functions in a moment in a little bit. But for now, let's use the traditional function declaration style. How do I declare? How do I save a function 10 square? What keyword do I tell the computer, I'm gonna save a function?

Function. What should I call it? 10 squared? Yeah, does it take any inputs? No, so empty parenthesis. And then in the body of the function Ryan, we've got? Returns 10 times 10. Yeah, brilliant, Ryan. Ryan is disappointed with this function I can tell already by his tone of voice. We call it, we get what Ryan? It's gonna evaluate to what everybody? 100. Great, but do we write this function? No.

I love this function, this is a terrible function. What about 9 squared? Ethan, what would I do to change this or no? Can't edit functions once I've saved them. Wanna build 9 squared. Off I go. What would I write, Ethan? Declare a function, 9 squared. Do I have to write, so you suffer with me, and we realize why we have functions. I'm gonna write it out again, yet okay. Returns 9 times 9. Actually, the fact it takes so long to wipe the board down is something of a metaphor for our premise. That's equivalent to editing and writing code, it's very appropriate. Yeah, do we write this function, Ethan? No. No, we don't wanna rewrite stuff like this.

But what about, there you go, 9 squared? Add an 8 squared function, 125 squared function people. What principle, everybody, are we breaking? Does everyone know? Yeah, DRY. Don't repeat yourself, right? This is a fundamental principle in programming, write as little as possible. And not just from a premise of words per hour per dollar. But, in a deeper sense, we don't wanna be rewriting code when we don't have to. Because it gets much, much harder to track and maintain what we're doing if we do so. We're breaking our principal DRY, Don't Repeat Yourself.

So, what could we do here, instead? Turning to, Peter, what could we do here, instead, besides writing out a function 9 squared? A function 8 squared? What is the thing that's the same, each time? So, what could we do, instead, here, Peter? The squaring is the same. So, we could generalize the input. Beautiful, fantastic, we can generalize the function to make it more reusable.

We could build a function like squared, no, we save the code of it, but we leave a little bit of that code TBD. Specifically, we leave exactly what data, what thing we're gonna multiply by itself. Cuz they're all the same. 9 by 9. 10 by 10. The odd number multiplied by itself. If instead I can leave the thing I'm gonna to multiply blank until I run the code, I write the code once of multiplying them by itself. And then run it and dynamically at the moment of running the function, turn it into 10 by 10. Turn it into 9 by 9. Turn it into 8 by 8. Literally, the 10 is gonna fly and replace num, I'm gonna get 10 by 10. The 8 is gonna fly, I'm gonna get 8 by 8. This is our premise or co premise of functions. Making code that we can write once and then reuse again and again with different values, different data. That is what a parameter is for.

But wait folk, what if, here we go. Parameters, placeholders, meaning we don't need to decide what data to run our functionality on until we run the function. Folk, what if it weren't just the data that we could leave TBD in our function? What if it weren't just leaving a blank to fill in with numbers or strings or rate? What if we could also leave a little bit of our code, TBD? Leave a little bit of our code only to be determined once we actually run the function? Leave a little bit of our functionality to be left blank? And when we run the function, fill that functionality in? That would make our functions super reusable, super general. And that's exactly what higher order functions are gonna do. We may not want to decide exactly what some of our functionality is, until we run our function.

---

## 번역 (Korean)

이제 함수형 프로그래밍(Functional Programming)의 흥미로운 부분으로 넘어간다. 먼저 함수가 왜 존재하는지부터 시작해보자. `10 squared`라는 함수를 만들어보자. 입력값은 없고, 10 곱하기 10을 반환한다. 전통적인 함수 선언(Function Declaration) 방식을 사용할 것이다. 잠시 후에 함수를 선언하고 저장하는 더 대중적인 방식도 살펴볼 것이지만, 지금은 전통적인 방식을 사용한다. 함수를 저장하겠다고 컴퓨터에 알리는 키워드는 무엇인가?

`function` 키워드를 사용한다. 함수명은 `tenSquared`, 입력값 없이 빈 괄호, 함수 본문에는 `return 10 * 10`을 작성한다. 이 함수를 호출하면 100이 반환된다. 그런데 이 함수를 실제로 작성할 것인가? 아니다.

이 함수가 마음에 들지만, 사실 끔찍한 함수다. `9 squared`는 어떻게 하는가? 함수는 저장한 뒤에는 편집할 수 없으므로, `nineSquared`라는 새 함수를 별도로 선언해야 한다. 본문에는 `return 9 * 9`. 칠판을 지우는 데 시간이 오래 걸린다는 사실 자체가 우리의 전제에 대한 은유다. 코드를 편집하고 작성하는 것과 동등한 행위다. 이 함수를 작성하겠는가? 아니다. 이런 코드를 반복해서 작성하고 싶지 않다.

`8 squared`, `125 squared` 함수까지 추가하면 어떤 원칙을 위반하는 것인가? **DRY(Don't Repeat Yourself)** 원칙이다. 이는 프로그래밍의 근본 원칙 중 하나로, 가능한 한 적게 작성하라는 것이다. 단순히 시간이나 비용의 문제가 아니라, 더 근본적인 의미에서 불필요하게 코드를 반복 작성하면 작업 내용을 추적하고 유지보수하기가 훨씬 어려워지기 때문이다.

그렇다면 대신 무엇을 할 수 있는가? `9 squared`, `8 squared` 함수를 일일이 작성하는 대신, 매번 동일한 것이 무엇인지 생각해보자. **제곱(squaring)** 연산이다. 그렇다면 입력값을 일반화(generalize)할 수 있다. 함수를 더 재사용 가능하게 만들기 위해 일반화하는 것이다.

`squared`라는 함수를 만들되, 어떤 값을 제곱할지는 미정(TBD, To Be Determined)으로 남겨둔다. 9×9, 10×10 모두 같은 패턴이다. 코드를 실행하기 전까지 곱할 대상을 비워두고, 제곱 연산 코드는 한 번만 작성한다. 그리고 실행 시점에 동적으로, 10이 들어오면 10×10, 9가 들어오면 9×9, 8이 들어오면 8×8이 된다. 말 그대로 10이 날아와서 `num`을 대체하면 10×10이 된다. 이것이 함수의 핵심 전제다. 코드를 한 번 작성하고, 서로 다른 값(데이터)으로 반복 재사용하는 것. 이것이 바로 **매개변수(parameter)**의 역할이다.

그런데 여기서 한 단계 더 나아가보자. 매개변수(parameter)는 자리 표시자(placeholder)로서, 함수를 실행하기 전까지 어떤 데이터를 사용할지 결정하지 않아도 된다는 의미다. 그렇다면 데이터뿐만 아니라 **코드의 일부 자체**도 미정으로 남겨둘 수 있다면 어떨까? 숫자나 문자열을 채워 넣는 자리뿐만 아니라, 코드의 일부 기능(functionality)을 비워두고 함수를 실제로 실행할 때 그 기능을 채워 넣을 수 있다면? 그렇게 되면 함수는 훨씬 더 재사용 가능하고 범용적이 된다. 이것이 바로 **고차 함수(Higher Order Functions)**가 하는 일이다. 함수를 실행하기 전까지 일부 기능을 확정하지 않아도 되는 것이다.
