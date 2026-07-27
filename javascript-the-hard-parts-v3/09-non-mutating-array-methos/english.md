# JavaScript: The Hard Parts v3 - Non-Mutating Array Methods 영어 표현

## 1. 동작 서술

| 영어 표현 | 한국어 | 강의 원문 |
|---|---|---|
| mutate / change the input array | 입력 배열을 변경하다 | "we are going to mutate, change our input array" |
| return out a brand new array | 완전히 새로운 배열을 반환하다 | "Also returns out brand new array, does not change the 123." |
| do the work on the array directly | 배열에 직접 작업을 수행하다 | "Some of the built-in major methods in JavaScript actually do the work on the array directly." |
| the array it's being run on | 메서드가 실행되는 대상 배열 | "The one that they're being run on." |
| evaluate to a brand new array | 완전히 새로운 배열로 평가되다 | "call on array 2 the toReversed method, which is going to evaluate to a brand new array 321" |
| store it into a new label | 새 라벨에 저장하다 | "we don't want to lose it, we're going to store it into a new label, reversed, 321" |
| take out the subarray and assign it as individual elements | 하위 배열을 꺼내 개별 요소로 넣다 | "that will then take out the subarray and assign it as individual elements in the main array" |
| go one layer deep / go as deep as we like | 한 단계 깊이 들어가다 / 원하는 만큼 깊이 들어가다 | "In this case we only had to go 1 layer deep… this flat function will actually allow us to go as many deep as we like" |
| it defaults to one | 기본값은 1이다 | "it defaults to one in this case, but we can pass in and go as deep as we want" |
| order those automatically by our numeric hierarchy | 숫자 순서 체계에 따라 자동으로 정렬하다 | "our sort is going to then order those automatically by our numeric hierarchy" |
| already numerically sorted in ascending order | 이미 숫자 오름차순으로 정렬되어 있다 | "it's already numerically sorted in ascending order" |
| stay fixed | 그대로 고정되어 있다 | "it's a brand new mutated array, meaning our array 2 stays fixed" |
| be long gone | 사라진 지 오래다 | "our array 1 has been fully mutated at this point to 136, our 123 is long gone for array one" |
| the first one it comes across | 처음 마주치는 것 | "the first 2 that it comes across is at position 4 in our 5 element array" |

> 리뷰나 PR 설명에서 <b>"이 함수는 원본을 안 건드린다"</b>를 말할 때는 `it does not mutate the input array`, <b>"새 배열을 만들어 반환한다"</b>는 `it returns out a brand new array`가 그대로 쓰입니다.

---

## 2. 설계 원칙 서술

| 영어 표현 | 한국어 | 강의 원문 |
|---|---|---|
| have consequences outside of its explicit output | 명시적 출력 바깥에서 결과를 일으키다 | "I do not want my functions to have consequences outside of their explicit output." |
| not defined explicitly within the code's definition | 코드 정의 안에 명시적으로 드러나 있지 않다 | "The output is not defined explicitly within the code's definition." |
| don't change the underlying data | 기반 데이터는 건드리지 않다 | "you're constantly changing how the user sees it, but please don't change the underlying" |
| take the data and manipulate it for display | 데이터를 가져와 표시를 위해 조작하다 | "a lot of the time it's not trying to actually manipulate the data itself, but to take the data and manipulate it for display" |
| be passed to some other evaluation | 다른 평가로 전달되다 | "we're going to use that data when we are reversing it to be displayed or to be passed to some other evaluation" |
| not to actually change the intrinsic data | 데이터의 본질 자체를 바꾸려는 것이 아니다 | "not to actually change the intrinsic data" |
| in UI-oriented environments | UI 지향적인 환경에서는 | "So even more in UI oriented environments, you don't want to change underlying data" |
| under the hood | 내부적으로는 | "it's quite a sophisticated function under the hood, this group by" |
| pass in another function to it | 다른 함수를 인자로 전달하다 | "higher-order functions, ones that we can pass in another function to" |
| take in each element and run a function on it | 각 요소를 받아 그 위에 함수를 실행하다 | "takes in each element of the array going backwards starting with the last one and runs a function on it" |
| take in some instructions applied to each element | 각 요소에 적용될 지시사항을 인자로 받다 | "it took in some instructions that were going to then be applied to each element of that array" |
| make for readable code | 읽기 좋은 코드를 만들어주다 | "they make up a major part of JavaScript's array methods, and they make for readable code" |

> `have consequences outside of its explicit output`은 <b>부수 효과(side effect)</b>를 전문 용어 없이 풀어 설명할 때 유용합니다.

---

## 3. 업무 표현

| 영어 표현 | 한국어 | 강의 원문 |
|---|---|---|
| map something out in detail | ~을 상세히 정리해 보여주다 | "We're not going to map them out in quite the same amount of detail" |
| dig into the design | 설계를 깊이 파고들다 | "I'm not here to sort of dig into the splice method design" |
| get hung up on | ~에 얽매이다 | "the idea with all of these is not to get hung up and say, oh my goodness" |
| around since day one | 처음부터 존재해온 | "the key array methods in JavaScript around since day one did actually change the input array" |
| as old as time | 아주 오래된 | "These are as old as time, they're built into JavaScript" |
| a whole wave of changes | 대대적인 변화의 물결 | "imagine if you do a whole wave of changes in 2015 ES6" |
| that may not seem like the end of the world | 큰일 날 정도로 보이지 않을 수도 있다 | "and that may not seem like the end of the world" |
| ongoing innovation | 지속적인 혁신 | "my point again is to show we've got ongoing innovation in our array methods" |
| a laundry list of features | 장황한 기능 나열 목록 | "hard parts is never about laundry lists of features" |
| the language is becoming harder to follow | 언어가 점점 따라가기 어려워지다 | "the language is becoming harder to follow" |
| read about it in the docs as I need it | 필요할 때 문서에서 찾아 읽어보다 | "I have some more built-in functions that I can go and read about in the docs as I need them" |
