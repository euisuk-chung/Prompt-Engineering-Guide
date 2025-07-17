# LM-가이드 연쇄적 사고(LM-Guided Chain-of-Thought)

[Lee 등(2024)](https://arxiv.org/abs/2404.03414)은 소형 언어 모델(small language model, LM)을 활용해 대형 언어 모델(LLM, Large Language Model)의 추론 능력을 향상시키는 새로운 방법을 제안했습니다.

이 방법은 먼저 대형 언어 모델이 생성한 근거(rationale)를 소형 LM에 지식 증류(knowledge distillation)하여, 소형 LM의 추론 능력 격차를 줄이고자 합니다.

핵심적으로, 근거(rationale)는 경량 LM이 생성하고, 답변 예측(answer prediction)은 고정된(frozen) 대형 LM이 담당합니다. 이 리소스 효율적 접근법은 대형 모델의 파인튜닝(fine-tuning) 없이, 근거 생성만을 소형 언어 모델에 위임합니다.

지식 증류된 LM은 추가적으로 여러 근거 지향(rational-oriented) 및 과제 지향(task-oriented) 보상 신호를 활용한 강화학습(reinforcement learning)으로 최적화됩니다.

![](../../img/research/guided-cot.png)
*출처: https://arxiv.org/pdf/2404.03414.pdf*

이 프레임워크는 멀티홉 추출형 질의응답(multi-hop extractive question answering)에서 테스트되었으며, 답변 예측 정확도 측면에서 모든 기존 기준(baseline)보다 뛰어난 성능을 보였습니다. RL(강화학습)은 생성된 근거의 품질을 높여, 질의응답 성능을 추가로 향상시킵니다.

이 논문에서 제안한 LM-가이드 CoT 프롬프트 기법은 표준 프롬프트(standard prompting)와 CoT 프롬프트 모두를 능가합니다. 자기 일관성 디코딩(self-consistency decoding) 역시 성능을 높입니다.

이 접근법은 근거 생성에 소형 언어 모델을 영리하게 활용한 사례입니다. 일반적으로 이러한 능력은 대형 언어 모델이 더 뛰어나다고 여겨지지만, 본 연구는 소형 모델로도 탁월한 결과를 얻을 수 있음을 보여줍니다. 과제를 이처럼 분해하는 방식은 개발자에게 시사하는 바가 큽니다. 모든 작업을 반드시 대형 모델로 처리할 필요는 없습니다. 파인튜닝 시, 어떤 측면을 최적화할지 고민하고, 소형 언어 모델이 이를 대신할 수 있는지 실험해보는 것이 유용합니다. 