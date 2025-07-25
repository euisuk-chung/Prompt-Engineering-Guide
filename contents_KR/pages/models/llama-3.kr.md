# Llama 3

Meta는 최근 Llama 3라는 새로운 대규모 언어 모델(LLM) 패밀리를 [소개](https://llama.meta.com/llama3/)했습니다. 이 출시에는 8B 및 70B 파라미터 사전 훈련 및 지시 튜닝 모델이 포함됩니다.

## Llama 3 아키텍처 세부사항(Llama 3 Architecture Details)

다음은 Llama 3의 언급된 기술 세부사항 요약입니다:

- 표준 디코더 전용 트랜스포머를 사용합니다.
- 어휘는 128K 토큰입니다.
- 8K 토큰 시퀀스로 훈련됩니다.
- grouped query attention(GQA)을 적용합니다.
- 15T 이상의 토큰으로 사전 훈련됩니다.
- SFT, rejection sampling, PPO, DPO의 조합을 포함하는 사후 훈련을 포함합니다.

## 성능(Performance)

특히, Llama 3 8B(지시 튜닝)는 [Gemma 7B](https://www.promptingguide.ai/models/gemma)와 [Mistral 7B Instruct](https://www.promptingguide.ai/models/mistral-7b)를 능가합니다. Llama 3 70은 [Gemini Pro 1.5](https://www.promptingguide.ai/models/gemini-pro)와 [Claude 3 Sonnet](https://www.promptingguide.ai/models/claude-3)을 광범위하게 능가하며 Gemini Pro 1.5와 비교할 때 MATH 벤치마크에서 약간 뒤처집니다.

!["Llama 3 Performance"](../../img/llama3/llama-instruct-performance.png)
*출처: [Meta AI](https://ai.meta.com/blog/meta-llama-3/)*

사전 훈련된 모델들은 또한 AGIEval(영어), MMLU, Big-Bench Hard와 같은 여러 벤치마크에서 다른 모델들을 능가합니다.

!["Llama 3 Performance"](../../img/llama3/llama3-pretrained-results.png)
*출처: [Meta AI](https://ai.meta.com/blog/meta-llama-3/)*

## Llama 3 400B

Meta는 또한 여전히 훈련 중이고 곧 출시될 400B 파라미터 모델을 출시할 것이라고 보고했습니다! 파이프라인에는 다중모달 지원, 다국어 기능, 더 긴 컨텍스트 윈도우에 대한 노력도 있습니다. Llama 3 400B의 현재 체크포인트(2024년 4월 15일 기준)는 MMLU 및 Big-Bench Hard와 같은 일반적인 벤치마크에서 다음과 같은 결과를 생성합니다:

!["Llama 3 400B"](../../img/llama3/llama-400b.png)
*출처: [Meta AI](https://ai.meta.com/blog/meta-llama-3/)*

Llama 3 모델의 라이선싱 정보는 [모델 카드](https://github.com/meta-llama/llama3/blob/main/MODEL_CARD.md)에서 찾을 수 있습니다.

## Llama 3 확장 리뷰(Extended Review of Llama 3)

다음은 Llama 3에 대한 더 긴 리뷰입니다:

<iframe width="100%"
  height="415px"
  src="https://www.youtube.com/embed/h2aEmciRd6U?si=m7-xXu5IWpB-6mE0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowFullScreen
  /> 