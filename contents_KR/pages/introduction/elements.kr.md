# 프롬프트의 구성 요소(Elements of a Prompt)

프롬프트 엔지니어링의 예시와 응용을 더 많이 다룰수록, 특정 요소들이 프롬프트를 구성한다는 것을 알게 될 것입니다.

프롬프트는 다음 요소들 중 하나를 포함합니다:

**지침(Instruction)** - 모델이 수행하기를 원하는 특정 작업이나 지침

**컨텍스트(Context)** - 모델이 더 나은 응답을 하도록 안내할 수 있는 외부 정보나 추가 컨텍스트

**입력 데이터(Input Data)** - 응답을 찾고자 하는 입력이나 질문

**출력 표시자(Output Indicator)** - 출력의 유형이나 형식

<iframe width="100%"
  height="415px"
  src="https://www.youtube.com/embed/kgBZhJnh-vk?si=-a-KvhmXFJMtAuCB" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowFullScreen
  />

프롬프트 요소를 더 잘 보여주기 위해, 텍스트 분류 작업을 수행하는 것을 목표로 하는 간단한 프롬프트 예시가 있습니다:

*프롬프트*
```
Classify the text into neutral, negative, or positive

Text: I think the food was okay.

Sentiment:
```

위의 프롬프트 예시에서, 지침은 분류 작업인 "Classify the text into neutral, negative, or positive"에 해당합니다. 입력 데이터는 "I think the food was okay." 부분에 해당하고, 사용된 출력 표시자는 "Sentiment:"입니다. 이 기본 예시는 컨텍스트를 사용하지 않지만, 이것도 프롬프트의 일부로 제공될 수 있습니다. 예를 들어, 이 텍스트 분류 프롬프트의 컨텍스트는 모델이 작업을 더 잘 이해하고 기대하는 출력 유형을 안내하는 데 도움이 되는 프롬프트의 일부로 제공되는 추가 예시일 수 있습니다.

프롬프트에 네 가지 요소가 모두 필요하지는 않으며, 형식은 해당 작업에 따라 달라집니다. 다음 가이드에서 더 구체적인 예시들을 다룰 것입니다.

<Callout type= "info" emoji="🎓">
새로운 AI 과정에서 고급 프롬프팅 방법에 대해 더 자세히 알아보세요. [지금 참여하세요!](https://dair-ai.thinkific.com/)
추가 20% 할인을 위해 코드 PROMPTING20을 사용하세요.
</Callout>
