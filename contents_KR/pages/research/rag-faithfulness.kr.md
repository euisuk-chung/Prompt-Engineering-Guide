# RAG 모델은 얼마나 신실한가?

import {Bleed} from 'nextra-theme-docs'

<iframe width="100%"
  height="415px"
  src="https://www.youtube.com/embed/eEU1dWVE8QQ?si=b-qgCU8nibBCSX8H" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowFullScreen
  />

[Wu et al. (2024)](https://arxiv.org/abs/2404.10198)의 이 새로운 논문은 RAG와 LLM의 내부 사전 지식 사이의 줄다리기를 정량화하는 것을 목표로 합니다.

분석을 위해 질문 답변에서 GPT-4와 다른 LLM에 초점을 맞춥니다.

올바른 검색된 정보를 제공하면 모델의 대부분의 실수가 수정된다는 것을 발견했습니다(94% 정확도).

!["RAG Faithfulness"](../../img/research/rag-faith.png)
*출처: [Wu et al. (2024)](https://arxiv.org/abs/2404.10198)*

문서에 더 많은 잘못된 값이 포함되고 LLM의 내부 사전 지식이 약할 때, LLM은 잘못된 정보를 암송할 가능성이 더 높습니다. 그러나 LLM은 더 강한 사전 지식을 가질 때 더 저항력이 있는 것으로 발견됩니다.

이 논문은 또한 "수정된 정보가 모델의 사전 지식에서 더 많이 벗어날수록 모델이 그것을 선호할 가능성이 낮아진다"고 보고합니다.

많은 개발자와 회사들이 프로덕션에서 RAG 시스템을 사용하고 있습니다. 이 연구는 지원, 모순 또는 완전히 잘못된 정보를 포함할 수 있는 다양한 종류의 맥락 정보가 주어졌을 때 LLM을 사용할 때 위험을 평가하는 것의 중요성을 강조합니다. 