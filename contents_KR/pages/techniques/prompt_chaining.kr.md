# 프롬프트 체이닝(Prompt Chaining)

## 프롬프트 체이닝 소개

LLM의 신뢰성과 성능을 높이기 위한 중요한 프롬프트 엔지니어링(Prompt Engineering) 기법 중 하나는 과제를 하위 과제로 분해하는 것입니다. 하위 과제를 식별한 뒤, LLM에 각 하위 과제를 프롬프트로 제시하고, 그 응답을 다음 프롬프트의 입력으로 활용합니다. 이를 프롬프트 체이닝(Prompt Chaining)이라 하며, 하나의 과제를 여러 하위 작업으로 나누어 프롬프트 연쇄를 만드는 방식입니다.

프롬프트 체이닝은 LLM이 한 번에 복잡한 프롬프트로는 해결하기 어려운 과제를 단계별로 처리할 수 있게 해줍니다. 체인 내 각 프롬프트는 생성된 응답에 추가 변환이나 처리를 거쳐 최종 목표 상태에 도달하도록 합니다.

프롬프트 체이닝은 성능 향상뿐 아니라, LLM 애플리케이션의 투명성, 제어 가능성, 신뢰성을 높여줍니다. 즉, 모델 응답의 문제를 더 쉽게 디버깅하고, 각 단계별로 성능을 분석·개선할 수 있습니다.

특히 LLM 기반 대화형 어시스턴트 구축, 애플리케이션의 개인화 및 사용자 경험 개선에 매우 유용합니다.

## 프롬프트 체이닝 활용 사례

### 문서 QA를 위한 프롬프트 체이닝

프롬프트 체이닝은 여러 연산이나 변환이 필요한 다양한 시나리오에 활용할 수 있습니다. 예를 들어, LLM을 활용해 대용량 문서에서 질문에 답하는 경우, 첫 번째 프롬프트는 질문에 관련된 인용구(quote)를 추출하고, 두 번째 프롬프트는 추출된 인용구와 원문을 바탕으로 답변을 생성하도록 설계할 수 있습니다. 즉, 문서 내 질문에 답하기 위해 두 개의 프롬프트를 연쇄적으로 사용합니다.

아래 첫 번째 프롬프트는 문서와 질문이 주어졌을 때, 관련 인용구를 추출합니다. 예시에서는 문서 부분에 `{{document}}` 플레이스홀더를 사용했습니다. 테스트 시 [prompt engineering](https://en.wikipedia.org/wiki/Prompt_engineering) 위키피디아 문서 등에서 복사해 붙여넣어도 좋습니다. 이 작업은 긴 컨텍스트가 필요하므로 OpenAI의 `gpt-4-1106-preview` 모델을 사용하였으나, Claude 등 다른 롱컨텍스트 LLM도 활용 가능합니다.

프롬프트 1:
```
You are a helpful assistant. Your task is to help answer a question given in a document. The first step is to extract quotes relevant to the question from the document, delimited by ####. Please output the list of quotes using <quotes></quotes>. Respond with "No relevant quotes found!" if no relevant quotes were found.

####
{{document}}
####
```

프롬프트 1의 출력 예시:
```
<quotes>
- Chain-of-thought (CoT) prompting[27]
- Generated knowledge prompting[37]
- Least-to-most prompting[38]
- Self-consistency decoding[39]
- Complexity-based prompting[41]
- Self-refine[42]
- Tree-of-thought prompting[43]
- Maieutic prompting[45]
- Directional-stimulus prompting[46]
- Textual inversion and embeddings[59]
- Using gradient descent to search for prompts[61][62][63][64]
- Prompt injection[65][66][67]
</quotes>
```

첫 번째 프롬프트에서 반환된 인용구는 두 번째 프롬프트의 입력으로 활용할 수 있습니다. 인용구 내 인용번호(citation)는 추가 프롬프트로 제거하거나, 무시해도 무방합니다. 두 번째 프롬프트는 추출된 인용구와 원문, 그리고 질문을 바탕으로 친절하고 정확한 답변을 생성합니다.

프롬프트 2:
```
Given a set of relevant quotes (delimited by <quotes></quotes>) extracted from a document and the original document (delimited by ####), please compose an answer to the question. Ensure that the answer is accurate, has a friendly tone, and sounds helpful.

####
{{document}}
####

<quotes>
- Chain-of-thought (CoT) prompting[27]
- Generated knowledge prompting[37]
- Least-to-most prompting[38]
- Self-consistency decoding[39]
- Complexity-based prompting[41]
- Self-refine[42]
- Tree-of-thought prompting[43]
- Maieutic prompting[45]
- Directional-stimulus prompting[46]
- Textual inversion and embeddings[59]
- Using gradient descent to search for prompts[61][62][63][64]
- Prompt injection[65][66][67]
</quotes>
```

프롬프트 2의 출력 예시:
```
The prompting techniques mentioned in the document include:

1. Chain-of-thought (CoT) prompting[27]
2. Generated knowledge prompting[37]
3. Least-to-most prompting[38]
4. Self-consistency decoding[39]
5. Complexity-based prompting[41]
6. Self-refine[42]
7. Tree-of-thought prompting[43]
8. Maieutic prompting[45]
9. Directional-stimulus prompting[46]
10. Textual inversion and embeddings[59]
11. Using gradient descent to search for prompts[61][62][63][64]
12. Prompt injection[65][66][67]

Each of these techniques employs unique strategies to enhance or specify the interactions with large language models to produce the desired outcomes.
```

이처럼 프롬프트 체이닝은 여러 단계의 연산이나 변환이 필요한 응답을 생성할 때 유용한 기법입니다. 연습 삼아, 응답에서 인용번호([27] 등)를 제거하는 프롬프트를 추가로 설계해볼 수도 있습니다.

Claude LLM을 활용한 프롬프트 체이닝 예시는 [이 문서](https://docs.anthropic.com/claude/docs/prompt-chaining)에서 더 확인할 수 있습니다. 본 예시는 해당 문서의 사례를 참고·응용하였습니다.
