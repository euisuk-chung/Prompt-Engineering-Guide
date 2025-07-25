# Grok-1

Grok-1은 314B 파라미터를 가진 mixture-of-experts(MoE) 대규모 언어 모델(LLM)로, 기본 모델 가중치와 네트워크 아키텍처의 오픈 릴리스를 포함합니다.

Grok-1은 xAI에 의해 훈련되었으며 추론 시 주어진 토큰에 대해 가중치의 25%를 활성화하는 MoE 모델로 구성됩니다. Grok-1의 사전 훈련 종료 날짜는 2023년 10월입니다.

[공식 발표](https://x.ai/blog/grok-os)에서 언급된 바와 같이, Grok-1은 사전 훈련 단계의 원시 기본 모델 체크포인트로, 대화 에이전트와 같은 특정 애플리케이션을 위해 미세조정되지 않았음을 의미합니다.

이 모델은 Apache 2.0 라이선스 하에 [출시](https://github.com/xai-org/grok-1)되었습니다.

## 결과 및 기능(Results and Capabilities)

초기 [발표](https://x.ai/blog/grok)에 따르면, Grok-1은 추론 및 코딩 작업에서 강력한 기능을 보여줍니다. 마지막으로 공개된 결과에 따르면 Grok-1은 HumanEval 코딩 작업에서 63.2%, MMLU에서 73%를 달성합니다. 일반적으로 ChatGPT-3.5와 Inflection-1을 능가하지만 여전히 GPT-4와 같은 개선된 모델에는 뒤처집니다.

!["Grok-1 Benchmark Results"](../../img/grok/grok-reasoning.png)

Grok-1은 또한 헝가리 전국 고등학교 수학 기말고사에서 GPT-4의 B(68%)에 비해 C(59%)를 기록한 것으로 보고되었습니다.

!["Grok-1 Benchmark Results"](../../img/grok/grok-math.png)

모델을 확인해보세요: https://github.com/xai-org/grok-1

Grok-1의 크기(314B 파라미터)로 인해 xAI는 모델을 테스트하기 위해 다중 GPU 머신을 권장합니다.

## 참고문헌(References)

- [Open Release of Grok-1](https://x.ai/blog/grok-os)
- [Announcing Grok](https://x.ai/blog/grok) 