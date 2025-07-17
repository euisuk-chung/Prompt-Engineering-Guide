# 멀티모달 체인 오브 쏘트 프롬프트(Multimodal CoT Prompting)

[Zhang 외, 2023](https://arxiv.org/abs/2302.00923)은 최근 멀티모달 체인 오브 쏘트(Multimodal Chain-of-Thought, CoT) 프롬프트 기법을 제안했습니다. 기존 CoT는 언어 모달리티에 집중했으나, 멀티모달 CoT는 텍스트와 비전을 결합한 2단계 프레임워크를 도입합니다. 첫 단계에서는 멀티모달 정보를 바탕으로 근거(rationale)를 생성하고, 두 번째 단계에서는 생성된 근거를 활용해 답을 추론합니다.

멀티모달 CoT 모델(1B)은 ScienceQA 벤치마크에서 GPT-3.5보다 더 우수한 성능을 보였습니다.

이미지 출처: [Zhang 외, 2023](https://arxiv.org/abs/2302.00923)

추가 참고 자료:
- [Language Is Not All You Need: Aligning Perception with Language Models](https://arxiv.org/abs/2302.14045) (2023년 2월)