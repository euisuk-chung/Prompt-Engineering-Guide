# 자동 프롬프트 엔지니어(Automatic Prompt Engineer, APE)

이미지 출처: [Zhou 등(2022)](https://arxiv.org/abs/2211.01910)

[Zhou 등(2022)](https://arxiv.org/abs/2211.01910)은 자동 프롬프트 엔지니어(Automatic Prompt Engineer, APE)라는 프레임워크를 제안했습니다. 이는 자동 지시문 생성 및 선택을 위한 프레임워크로, 지시문 생성 문제를 자연어 합성(natural language synthesis)으로 보고, LLM(대형 언어 모델, Large Language Model)을 활용한 블랙박스 최적화(black-box optimization) 문제로 접근합니다.

첫 단계에서는 대형 언어 모델(추론 모델, inference model)에 출력 예시(output demonstration)를 제공하여, 특정 과제에 대한 지시문 후보(instruction candidate)를 생성합니다. 이 후보들은 탐색(search) 과정을 안내합니다. 각 지시문은 타깃 모델(target model)에서 실행되고, 평가 점수(evaluation score)에 따라 가장 적합한 지시문이 선택됩니다.

APE는 사람이 설계한 "Let's think step by step" 프롬프트([Kojima 등, 2022](https://arxiv.org/abs/2205.11916))보다 더 우수한 제로샷 연쇄적 사고(zero-shot Chain-of-Thought, CoT) 프롬프트를 발견합니다.

"Let's work this out in a step by step way to be sure we have the right answer."라는 프롬프트는 연쇄적 사고(chain-of-thought) 추론을 유도하며, MultiArith 및 GSM8K 벤치마크에서 성능을 향상시킵니다:

이미지 출처: [Zhou 등(2022)](https://arxiv.org/abs/2211.01910)

이 논문은 프롬프트 엔지니어링(Prompt Engineering)과 관련된 중요한 주제, 즉 프롬프트의 자동 최적화(automatic prompt optimization) 개념을 다룹니다. 본 가이드에서는 이 주제를 깊이 다루지 않지만, 관심 있는 분들을 위해 주요 논문을 소개합니다:

- [Prompt-OIRL](https://arxiv.org/abs/2309.06553) - 오프라인 역강화학습(offline inverse reinforcement learning)을 활용해 쿼리 의존적 프롬프트를 생성하는 방법 제안
- [OPRO](https://arxiv.org/abs/2309.03409) - LLM이 프롬프트를 최적화하도록 하는 아이디어: "Take a deep breath" 프롬프트가 수학 문제 성능을 향상시킴
- [AutoPrompt](https://arxiv.org/abs/2010.15980) - 그래디언트 기반 탐색(gradient-guided search)으로 다양한 과제에 자동 프롬프트 생성
- [Prefix Tuning](https://arxiv.org/abs/2101.00190) - NLG 과제에 대해 학습 가능한 연속 프리픽스(prefix)를 추가하는 경량 대안
- [Prompt Tuning](https://arxiv.org/abs/2104.08691) - 역전파(backpropagation)로 소프트 프롬프트를 학습하는 메커니즘 제안
