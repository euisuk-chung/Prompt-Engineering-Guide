# 방향성 자극 프롬프트(Directional Stimulus Prompting)

[Li 등(2023)](https://arxiv.org/abs/2302.11520)은 LLM(대형 언어 모델, Large Language Model)이 원하는 요약을 더 잘 생성할 수 있도록 유도하는 새로운 프롬프트 기법을 제안했습니다.

조정 가능한 정책 언어 모델(policy LM)을 훈련시켜 자극/힌트(stimulus/hint)를 생성합니다. 최근 LLM 최적화를 위해 RL(강화학습, Reinforcement Learning) 활용이 늘고 있습니다.

아래 그림은 방향성 자극 프롬프트(Directional Stimulus Prompting)와 표준 프롬프트의 비교를 보여줍니다. 정책 LM은 소형으로도 구현 가능하며, 블랙박스 형태의 고정 LLM을 안내하는 힌트를 생성하도록 최적화할 수 있습니다.

이미지 출처: [Li 등(2023)](https://arxiv.org/abs/2302.11520)

전체 예시는 곧 추가될 예정입니다!