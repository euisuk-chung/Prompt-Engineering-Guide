# 프롬프팅 기초(Basics of Prompting)

## LLM 프롬프팅

간단한 프롬프트로 많은 것을 달성할 수 있지만, 결과의 품질은 제공하는 정보의 양과 프롬프트가 얼마나 잘 구성되었는지에 따라 달라집니다. 프롬프트는 모델에 전달하는 *지침* 또는 *질문*과 같은 정보를 포함할 수 있으며, *컨텍스트*, *입력*, 또는 *예시*와 같은 다른 세부사항도 포함할 수 있습니다. 이러한 요소들을 사용하여 모델을 더 효과적으로 지시하여 결과의 품질을 향상시킬 수 있습니다.

간단한 프롬프트의 기본 예시를 통해 시작해보겠습니다:

*프롬프트*

```md
The sky is
```

*출력:*
```md
blue.
```

OpenAI Playground나 다른 LLM 플레이그라운드를 사용하고 있다면, 다음 스크린샷과 같이 모델에 프롬프팅할 수 있습니다:

![소개 이미지](../../img/introduction/sky.png)

OpenAI Playground 시작 방법에 대한 튜토리얼은 다음과 같습니다:

<iframe width="100%"
  height="415px"
  src="https://www.youtube.com/embed/iwYtzPJELkk?si=irua5h_wHrkNCY0V" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowFullScreen
  />

`gpt-3.5-turbo` 또는 `gpt-4`와 같은 OpenAI 채팅 모델을 사용할 때, `system`, `user`, `assistant`의 세 가지 다른 역할을 사용하여 프롬프트를 구조화할 수 있다는 점을 주목해야 합니다. 시스템 메시지는 필수는 아니지만 어시스턴트의 전반적인 행동을 설정하는 데 도움이 됩니다. 위의 예시는 모델에 직접 프롬프팅하는 데 사용할 수 있는 사용자 메시지만 포함합니다. 간단함을 위해, 명시적으로 언급되지 않는 한 모든 예시는 `gpt-3.5-turbo` 모델에 프롬프팅하기 위해 `user` 메시지만 사용할 것입니다. 위 예시의 `assistant` 메시지는 모델 응답에 해당합니다. 원하는 행동의 예시를 전달하기 위해 어시스턴트 메시지를 정의할 수도 있습니다. 채팅 모델 작업에 대한 자세한 내용은 [여기](https://www.promptingguide.ai/models/chatgpt)에서 배울 수 있습니다.

위의 프롬프트 예시에서 언어 모델이 `"The sky is"` 컨텍스트에 맞는 의미 있는 토큰 시퀀스로 응답하는 것을 관찰할 수 있습니다. 출력은 예상과 다르거나 달성하고자 하는 작업과 거리가 멀 수 있습니다. 실제로, 이 기본 예시는 시스템으로 달성하고자 하는 구체적인 것에 대해 더 많은 컨텍스트나 지침을 제공할 필요성을 강조합니다. 이것이 바로 프롬프트 엔지니어링의 핵심입니다.

조금 개선해보겠습니다:

*프롬프트:*
```
Complete the sentence: 

The sky is
```

*출력:*

```
blue during the day and dark at night.
```

더 나아졌나요? 위의 프롬프트로 모델에게 문장을 완성하도록 지시하므로, 정확히 말한 대로 수행하므로("문장을 완성하라") 결과가 훨씬 좋아 보입니다. 모델이 원하는 작업을 수행하도록 효과적인 프롬프트를 설계하는 이 접근 방식이 이 가이드에서 **프롬프트 엔지니어링**이라고 하는 것입니다.

위의 예시는 오늘날 LLM으로 가능한 것의 기본적인 설명입니다. 오늘날의 LLM은 텍스트 요약부터 수학적 추론, 코드 생성까지 모든 종류의 고급 작업을 수행할 수 있습니다.

## 프롬프트 형식

위에서 매우 간단한 프롬프트를 시도했습니다. 표준 프롬프트는 다음과 같은 형식을 가집니다:

```
<Question>?
```

또는

```
<Instruction>
```

이를 다음과 같이 질문-답변(QA) 형식으로 포맷할 수 있으며, 이는 많은 QA 데이터셋에서 표준입니다:

```
Q: <Question>?
A: 
```

위와 같이 프롬프팅할 때, 이를 *제로샷 프롬프팅(zero-shot prompting)*이라고도 하며, 즉, 달성하고자 하는 작업에 대한 예시나 데모 없이 모델에 직접 응답을 요청하는 것입니다. 일부 대형 언어 모델은 제로샷 프롬프팅을 수행할 수 있는 능력을 가지고 있지만, 이는 해당 작업의 복잡성과 지식, 그리고 모델이 잘 수행하도록 훈련된 작업에 따라 달라집니다.

구체적인 프롬프트 예시는 다음과 같습니다:

*프롬프트*
```
Q: What is prompt engineering?
```

더 최근의 모델 중 일부에서는 시퀀스가 구성되는 방식에 따라 모델이 질문-답변 작업으로 이해하므로 "Q:" 부분을 건너뛸 수 있습니다. 다시 말해, 프롬프트는 다음과 같이 단순화될 수 있습니다:

*프롬프트*
```
What is prompt engineering?
```

위의 표준 형식을 고려할 때, 프롬프팅의 인기 있고 효과적인 기법 중 하나는 *퓨샷 프롬프팅(few-shot prompting)*으로, 예시(즉, 데모)를 제공합니다. 퓨샷 프롬프트를 다음과 같이 포맷할 수 있습니다:

```
<Question>?
<Answer>

<Question>?
<Answer>

<Question>?
<Answer>

<Question>?

```

QA 형식 버전은 다음과 같습니다:

```
Q: <Question>?
A: <Answer>

Q: <Question>?
A: <Answer>

Q: <Question>?
A: <Answer>

Q: <Question>?
A:
```

QA 형식을 사용할 필요가 없다는 점을 기억하세요. 프롬프트 형식은 해당 작업에 따라 달라집니다. 예를 들어, 간단한 분류 작업을 수행하고 작업을 보여주는 예시를 제공할 수 있습니다:

*프롬프트:*
```
This is awesome! // Positive
This is bad! // Negative
Wow that movie was rad! // Positive
What a horrible show! //
```

*출력:*
```
Negative
```

퓨샷 프롬프트는 인컨텍스트 학습(in-context learning)을 가능하게 하며, 이는 언어 모델이 몇 가지 데모를 통해 작업을 학습하는 능력입니다. 제로샷 프롬프팅과 퓨샷 프롬프팅에 대해 다음 섹션에서 더 광범위하게 논의할 것입니다.

<Callout type= "info" emoji="🎓">
새로운 AI 과정에서 고급 프롬프팅 방법에 대해 더 자세히 알아보세요. [지금 참여하세요!](https://dair-ai.thinkific.com/)
추가 20% 할인을 위해 코드 PROMPTING20을 사용하세요.
</Callout>
