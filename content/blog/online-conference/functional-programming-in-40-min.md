---
title: GOTO 2018 • Functional Programming in 40 Minutes • Russ Olsen
date: "2020-03-10T00:00:00Z"
---

| 제목   | GOTO 2018 • Functional Programming in 40 Minutes • Russ Olsen |
| ------ | ------------------------------------------------------------- |
| 요약   | FP에 관한 간단한 intro                                        |
| 시청일 | 2020.03.10                                                    |
| 링크   | [Youtube](https://www.youtube.com/watch?v=0if71HOyVjY)        |

## 느낀점 :

전체 강의는 굉장히 짜임새 있는 형식으로 진행되며, 기초적인 수준에서 FP의 방식에 대한 이해를 돕는 강의인것 같다.

이 강의는 다음과 같은 세 부분으로 나누어 진행된다

- What is it?
- What's it like?
- Does it works?

### What is it

보통 FP에 대해 언급할 때, 프로그래밍에 대해 알고 있는 모든것을 잊으라 라는 얘기를 듣는다(Forget everything you know about programming)

이는 사실과 다르다. FP는 프로그래밍에서 중요한 부분들을 그대로 유지한 채 쌓는 아이디어이다. 우리가 FP를 한다고 해서 Array, Function과 같은 Data type, ifs, iteration, indentation등을 잊을 필요는 없다.

FP는 우리가 아는 프로그래밍에 기초하여 Refactor 하는것에 가깝다.

FP에서는 기존에 프로그래밍에서 갖고 있는 요소들을 기반으로 한채로 두가지 아이디어를 더 보탠다.

그 중 하나는 수학적인 함수에 대한 부분이다.
프로그래밍에서도 함수는 존재한다. (method, procedures, subroutins, functions)

그러나 이들은 수학적에서 말하는 함수와는 차이가 존재한다. 그 가장 명확한 차이는 side effect에서 온다.

우리는 프로그래밍에서 함수를 사용하며 자연스래 side effect가 일어나길 기대한다. (파일 입출력 등) 그러나 수학적인 함수에서는 함수는 동일한 값에 대해서는 항상 동일한 값을 제공해야 하고, side effect를 발생하지 않는다는 점이 차이가 있다. 즉, 순수 함수여야 한다.([pure function, wiki](https://en.wikipedia.org/wiki/Pure_function))

또 다른 중요한 요소는 Immutable이다.
만약 우리가 여러 함수들의 흐름에서 하나의 값이 바뀌면, 그 값의 변화를 따라가기 위해 들여야 하는 노력들을 생각해보면 크다는 것을 알 수 있다. 위에서 함수형으로 작성하여 코드를 큰 무리 없이 흐르듯이 이해해 갈 수 있는데, 그중에 의도치 않는 값 하나가 변하는 것을 상상해 보자. 이 문제를 해결하기 위해 사용되는 방식이 Immutable이다.

보통 Immutable을 하기 위해서 copy를 하곤 한다.
근데, 우리가 생각하는 것 처럼 copy를 하는 것은 아니다. 만약 수백만개의 아이템이 있는데, 한두개의 아이템을 바꿔야 하는 경우에도 전체 배열을 copy해야 할까?

이를 위해 Persistent Data structure를 사용한다.
이 방식의 요점은, 배열과 같은 구조를 트리의 형식으로 변화하고, 변화된 부분만 갱신하는 방법니다. 변화된 부분 이외에는 복사가 필요없으므로 자원의 재사용이 용이하며 copy로 부터 오게 되는 단점을 극복할 수 있다.

그러나, 문제는 우리는 현실 세계에서 프로그래밍이 side effect를 이르키길 원한다는 점이다. 프로그래밍을 하며 side effect를 최대한 피하기 위해 노력했지만, 이 것이 필요하다는게 아이러니 하다. (여기서 내 생각에는, 중요한 부분은 side effect가 나쁘다가 아니라, 의도치 않은 side effect가 나쁘다 인것 같다)

프로그래밍 언어에서는 side effect를 제공하기 위해 언어별로 다른 접근법을 제공한다.

강의에서는 두가지 방법에 대한 side effect를 어떻게 이르킬 것인지 설명한다.

1. 값을 변경하기 위해 (immutable에 대한 반대)
2. 외부 세상과 상호 작용 하기 위해 (write file 등)

값을 변경하기 위해서 예시로 강의에서 클로저 라는 언어를 예시로 들고, Atoms라는 개념에 함수를 전달하여(수학적인 함수) atoms가 그 함수에 따라 side effect를 이르키도록 한다. 이 방식을 통해, 몇개의 함수가 동시적으로 오던지 너무 쉽게 해결할 수 있고, 이는 DB 에서의 트랜잭션과 유사한 모습을 보이기도 한다고 언급한다.

또 외부와의 상호작용을 위한 방식으로는 주로 다양한 언어에서 queue와 유사한 구조를 사용하여 처리한다고 한다. (NodeJS에서도 유사하게 처리됨) 수행해야 한 작업을 queue에 넣고, 언어가 자체적으로 queue를 처리함으로서 side effect를 이르킨다.

### What's it like?

> No Magic!

FP는 뭔가 특별히 마법같은 기능을 제공한는게 아니다.

FP를 적용하여도 여전히 [Off-by-one error](https://en.wikipedia.org/wiki/Off-by-one_error), Redundant 코드, 나쁜 코드 등이 모두 존재할 수 있다.

FP를 적용하므로서 무언가 magic같이 얻는 장점이 있다면, 그것을 thread와 관련된 것일 것이다.
FP를 사용시 thread간의 변화를 너무나도 쉽게 방지할 수 있다. 모든 것은 input-> output 이므로 상태는 존재하거나 존재하지 않거나 둘 중 하나이다.

### Does it works?

가능하다. 끗~
