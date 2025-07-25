# Gemini 1.5 Pro

Google은 긴 형태의 콘텐츠에 대한 기억과 추론과 같은 기능에 중점을 둔 컴퓨팅 효율적인 다중모달 Mixture-of-Experts 모델인 Gemini 1.5 Pro를 소개합니다. 이 AI 모델은 수백만 토큰을 포함할 수 있는 긴 문서, 수 시간의 비디오와 오디오를 포함하여 추론할 수 있습니다. Gemini 1.5 Pro는 긴 문서 QA, 긴 비디오 QA, 긴 컨텍스트 ASR에서 최첨단 성능을 향상시킵니다. Gemini 1.5 Pro는 표준 벤치마크에서 Gemini 1.0 Ultra와 동등하거나 능가하며, 최소 1천만 토큰까지 거의 완벽한 검색(>99%)을 달성하여 다른 긴 컨텍스트 LLM과 비교하여 상당한 발전을 이루었습니다.

이번 출시의 일환으로 Google은 Google AI Studio에서 시도해볼 수 있는 새로운 실험적 100만 토큰 컨텍스트 윈도우 모델도 선보입니다. 맥락을 설명하자면, 200K는 현재 사용 가능한 모든 LLM 중 가장 큰 컨텍스트 윈도우입니다. 100만 컨텍스트 윈도우로 Gemini 1.5 Pro는 Google AI Studio에서 대용량 PDF, 코드 저장소, 심지어 긴 비디오를 프롬프트로 포함하는 모든 종류의 사용 사례를 해제하는 것을 목표로 합니다. 동일한 입력 시퀀스에서 오디오, 시각, 텍스트, 코드 입력의 혼합을 지원합니다.

## 아키텍처(Architecture)
Gemini 1.5 Pro는 Gemini 1.0의 다중모달 기능을 기반으로 한 희소 Mixture-of-Experts(MoE) Transformer 모델입니다. MoE의 장점은 활성화되는 파라미터 수를 일정하게 유지하면서 모델의 총 파라미터가 증가할 수 있다는 것입니다. [기술 보고서](https://storage.googleapis.com/deepmind-media/gemini/gemini_v1_5_report.pdf)에는 너무 많은 세부사항이 없지만, Gemini 1.5 Pro가 훈련 컴퓨팅을 훨씬 적게 사용하고, 서빙이 더 효율적이며, 긴 컨텍스트 이해(최대 1천만 토큰)를 가능하게 하는 아키텍처 변경을 포함한다고 보고됩니다. 이 모델은 다양한 모달리티를 포함한 데이터로 사전 훈련되고 다중모달 데이터로 지시 튜닝되며, 인간 선호도 데이터를 기반으로 추가 튜닝됩니다.

## 결과(Results)
Gemini 1.5 Pro는 모든 모달리티, 즉 텍스트, 비디오, 오디오에서 최대 100만 토큰까지 거의 완벽한 "바늘" 검색을 달성합니다. Gemini 1.5 Pro의 컨텍스트 윈도우 지원을 관점에서 보면, Gemini 1.5 Pro는 다음으로 확장할 때 처리하고 검색 성능을 유지할 수 있습니다:

- ~22시간의 녹음
- 10 x 1440페이지 책
- 전체 코드베이스
- 1fps에서 3시간 비디오

!["Gemini 1.5 Pro Retrieval Results"](../../img/gemini/gemini-retrieval.png)

Gemini 1.5 Pro는 수학, 과학, 추론, 다국어, 비디오 이해, 코드에서 상당한 성능으로 대부분의 벤치마크에서 Gemini 1.0 Pro를 능가합니다. 아래는 다양한 Gemini 모델의 결과를 요약한 표입니다. Gemini 1.5 Pro는 훨씬 적은 훈련 컴퓨팅을 사용함에도 불구하고 벤치마크의 절반에서 Gemini 1.0 Ultra를 능가합니다.

!["Gemini 1.5 Pro Results"](../../img/gemini/gemini-pro-results.png)

## 기능(Capabilities)

나머지 하위 섹션들은 Gemini 1.5 Pro로 가능한 다양한 기능을 강조하며, 대량의 데이터 분석부터 긴 컨텍스트 다중모달 추론까지 범위를 다룹니다. 일부 기능은 논문, 커뮤니티, 우리의 실험에서 보고되었습니다.

### 긴 문서 분석(Long Document Analysis)

Gemini 1.5 Pro의 문서 처리 및 분석 능력을 보여주기 위해 매우 기본적인 질문 답변 작업부터 시작합니다. Google AI Studio의 Gemini 1.5 Pro 모델은 최대 100만 토큰을 지원하므로 전체 PDF를 업로드할 수 있습니다. 아래 예시는 단일 PDF가 `What is the paper about?`라는 간단한 프롬프트와 함께 업로드되었음을 보여줍니다:

!["Gemini 1.5 Pro Results"](../../img/gemini/galactica.png)

모델의 응답은 [Galactica 논문](https://arxiv.org/abs/2211.09085)에 대한 적절한 요약을 제공하여 정확하고 간결합니다. 위 예시는 Google AI Studio 내에서 자유형 프롬프트를 사용하지만 업로드된 PDF와 상호작용하기 위해 채팅 형식을 사용할 수도 있습니다. 제공된 문서에서 답변을 받고 싶은 많은 질문이 있다면 유용한 기능입니다.

!["Gemini 1.5 Pro Chat"](../../img/gemini/galactica-chat.png)

긴 컨텍스트 윈도우를 활용하기 위해 이제 두 개의 PDF를 업로드하고 두 PDF에 걸친 질문을 해보겠습니다.

!["Gemini 1.5 Pro Results"](../../img/gemini/galactica-2.png)

응답은 합리적이며 흥미로운 부분은 [LLM에 대한 조사 논문](https://arxiv.org/abs/2303.18223)인 첫 번째 논문에서 추출된 정보가 표에서 나온다는 것입니다. "아키텍처" 정보도 올바르게 보입니다. 그러나 "성능" 섹션은 첫 번째 논문에서 찾을 수 없기 때문에 거기에 속하지 않습니다. 이 작업에서는 프롬프트 `Please list the facts mentioned in the first paper about the large language model introduced in the second paper.`를 맨 위에 두고 `Paper 1`과 `Paper 2`와 같은 태그로 논문에 라벨을 붙이는 것이 중요했습니다. 이 실험과 관련된 또 다른 후속 작업은 논문 세트와 요약 방법에 대한 지시를 업로드하여 관련 작업 섹션을 작성하는 것입니다. 또 다른 재미있는 작업은 모델에게 조사에 최신 LLM 논문을 통합하도록 요청하는 것입니다.

### 비디오 이해(Video Understanding)

Gemini 1.5 Pro는 처음부터 다중모달 기능으로 훈련되었으며 비디오 이해 기능도 보여줍니다. [Andrej Karpathy의 최근 LLM 강의](https://www.youtube.com/watch?v=zjkBMFhNj_g&ab_channel=AndrejKarpathy) 중 하나로 몇 가지 프롬프트를 테스트했습니다.

이 짧은 데모를 위해 `Chat prompt`를 만들고 Karpathy의 강의가 포함된 YouTube 비디오를 업로드했습니다. 첫 번째 질문은 `What is the lecture about?`입니다. 여기서는 특별한 것은 없지만 응답은 강의를 정확하게 요약하여 적절합니다.

!["Gemini 1.5 Pro Chat"](../../img/gemini/chat-1.png)

모델이 수행하도록 프롬프트한 두 번째 작업은 강의의 간결한 개요(한 페이지 길이)를 제공하는 것입니다. 응답은 다음과 같습니다(간결함을 위해 편집됨):

```markdown
## Large Language Models: A New Operating System?
Introduction:

- LLMs are trained on massive amounts of text data and can generate human-quality text.
- They are becoming increasingly powerful and are being used for a wide range of tasks, such as writing, translation, and coding.

How LLMs Work:

- LLMs are neural networks that predict the next word in a sequence.
- They learn by iteratively adjusting their parameters to make better predictions.
- While we can measure their performance, we don't fully understand how the billions of parameters collaborate to achieve it.

Training LLMs:

- Requires massive amounts of data and computing power.
- Can be thought of as "compressing the internet" into a single file.
- For example, training LLaMA 2 70B required 6,000 GPUs for 12 days and cost ~$2 million.

LLM Security:

- LLM security is a new and rapidly evolving field.
- Some key security risks include:
    - Jailbreaking: bypassing safety mechanisms to generate harmful content.
    - Prompt injection: injecting malicious code into prompts to control the LLM's output.
    - Data poisoning / Backdoor attacks: inserting crafted text into the training data to influence the LLM's behavior.
...
```

요약은 매우 간결하며 강의의 좋은 개요와 핵심 요점을 나타냅니다. 전체 출력의 정확성을 평가하지는 않았지만 `"training LLaMA 2 70B required 6,000 GPUs for 12 days and cost ~$2 million."`과 같은 정보성 있는 비트를 모델이 출력하는 것을 보는 것이 흥미롭습니다.

구체적인 세부사항이 중요할 때는 모델이 때때로 다양한 이유로 "환각"하거나 잘못된 정보를 검색할 수 있다는 점을 염두에 두세요. 예를 들어, 모델에게 `What are the FLOPs reported for Llama 2 in the lecture?`라고 프롬프트했을 때 `The lecture reports that training Llama 2 70B required approximately 1 trillion FLOPs.`라고 응답했는데 이는 정확하지 않습니다. 올바른 응답은 `~1e24 FLOPs`여야 합니다. 기술 보고서에는 비디오에 대한 구체적인 질문을 받았을 때 이러한 긴 컨텍스트 모델이 실패하는 많은 사례가 포함되어 있습니다.

다음 작업은 비디오에서 표 정보를 추출하는 것을 포함합니다. 예시는 프롬프트와 응답을 보여줍니다. 모델이 표를 생성했고 몇 가지 세부사항은 맞고 일부는 틀렸다는 것을 관찰할 수 있습니다. 예를 들어, 표 열은 올바르지만 행 라벨 중 하나가 잘못되었습니다(즉, `Concept Resolution`은 `Coref Resolution`이어야 함). 다른 표와 텍스트 상자와 같은 다른 요소들로 이러한 추출 작업을 몇 가지 테스트했고 유사한 불일치를 발견했습니다.

기술 보고서에 문서화된 흥미로운 예시는 모델이 특정 장면이나 타임스탬프를 기반으로 비디오에서 세부사항을 검색하는 능력입니다. 첫 번째 예시에서 모델에게 특정 섹션이 시작되는 위치로 답변하도록 프롬프트했습니다.

프롬프트:
```
At what timestamp does the LLM OS section start?
```

출력:
```
The LLM OS section starts at 42:17.
```

응답이 올바릅니다.

다음 예시에서 모델에게 슬라이드에서 발견된 차트를 설명하도록 요청합니다.

프롬프트:
```
Can you explain the chart (on the right-hand side) on the self-improvement slide?
```

출력:
```
The chart shows the performance of AlphaGo Zero over time, as measured by its Elo rating. The Elo rating is a measure of a player's skill in a game, with higher ratings indicating stronger players. The chart shows that AlphaGo Zero's performance improved rapidly over the course of 40 days, eventually surpassing the performance of all other versions of AlphaGo, including the version that defeated the world champion Go player Lee Sedol. This improvement was achieved entirely through self-play, with no human intervention or historical data.
```

모델은 차트의 결과를 설명하기 위해 제공된 정보를 잘 활용하는 것 같습니다. 아래는 해당 슬라이드의 스냅샷입니다:

!["AlphaGo Zero"](../../img/gemini/chart.png)

### 코드 추론(Code Reasoning)
긴 컨텍스트 추론으로 Gemini 1.5 Pro는 코드베이스에 대한 질문에 답할 수 있습니다. Google AI Studio를 사용하여 Gemini 1.5 Pro는 최대 100만 토큰을 허용하므로 전체 코드베이스를 업로드하고 다양한 질문이나 코드 관련 작업으로 프롬프트할 수 있습니다. 기술 보고서는 모델에게 전체 JAX 코드베이스(~746K 토큰)를 컨텍스트로 제공하고 핵심 자동 미분 방법의 위치를 식별하도록 요청하는 예시를 제공합니다.

!["Gemini 1.5 Pro Jax"](../../img/gemini/jax.png)

### 영어에서 Kalamang 번역(English to Kalamang Translation)
Gemini 1.5 Pro는 전 세계적으로 200명 미만의 화자가 사용하는 언어인 Kalamang에 대한 문법 매뉴얼(500페이지의 언어학 문서, 사전, ~400개의 병렬 문장)을 제공받을 수 있으며, 동일한 콘텐츠에서 학습하는 사람의 수준에서 영어를 Kalamang으로 번역합니다. 이는 긴 컨텍스트를 통해 활성화된 Gemini 1.5 Pro의 in-context 학습 능력을 보여줍니다.

!["Gemini 1.5 Pro Multilinguality"](../../img/gemini/kalamang.png)

그림 출처: [Gemini 1.5: Unlocking multimodal understanding across millions of tokens of context](https://storage.googleapis.com/deepmind-media/gemini/gemini_v1_5_report.pdf)

## 참고문헌(References)

- [Gemini 1.5: Unlocking multimodal understanding across millions of tokens of context](https://storage.googleapis.com/deepmind-media/gemini/gemini_v1_5_report.pdf)
- [Gemini 1.5: Our next-generation model, now available for Private Preview in Google AI Studio](https://developers.googleblog.com/2024/02/gemini-15-available-for-private-preview-in-google-ai-studio.html)