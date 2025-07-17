# 자동 추론 및 도구 활용(Automatic Reasoning and Tool-use, ART)

연쇄적 사고(Chain-of-Thought, CoT) 프롬프트와 도구(tool)를 교차(interleaved) 활용하는 방식은 LLM(대형 언어 모델, Large Language Model)로 다양한 과제를 해결하는 데 강력하고 견고한 접근법임이 입증되었습니다. 이러한 방식은 일반적으로 과제별 시연(demonstration)을 수작업으로 설계하고, 모델 생성과 도구 사용을 정교하게 교차 배치해야 합니다. [Paranjape 등(2023)](https://arxiv.org/abs/2303.09014)은 고정된 LLM을 활용해 중간 추론 단계를 프로그램 형태로 자동 생성하는 새로운 프레임워크 ART를 제안했습니다.

ART의 작동 방식은 다음과 같습니다:
- 새로운 과제가 주어지면, 과제 라이브러리(task library)에서 다단계 추론 및 도구 사용 시연을 선택합니다.
- 테스트 시점(test time)에는 외부 도구가 호출될 때마다 생성을 일시 중지하고, 도구의 출력을 통합한 뒤 생성을 재개합니다.

ART는 모델이 시연(demonstration)으로부터 일반화하여 새로운 과제를 분해하고, 적절한 위치에서 도구를 활용하도록 유도합니다(제로샷, zero-shot 방식). 또한 ART는 확장성이 뛰어나, 사람이 추론 단계의 오류를 수정하거나 새로운 도구를 추가할 때 단순히 과제 및 도구 라이브러리를 업데이트하면 됩니다. 전체 과정은 아래와 같이 시각화됩니다:

이미지 출처: [Paranjape 등(2023)](https://arxiv.org/abs/2303.09014)

ART는 BigBench 및 MMLU 벤치마크의 미지의 과제(unseen task)에서 few-shot 프롬프트 및 자동 CoT보다 월등한 성능을 보이며, 인간 피드백이 반영될 경우 수작업 CoT 프롬프트의 성능도 능가합니다.

아래 표는 ART가 BigBench 및 MMLU 과제에서 보인 성능을 요약한 것입니다:

이미지 출처: [Paranjape 등(2023)](https://arxiv.org/abs/2303.09014)