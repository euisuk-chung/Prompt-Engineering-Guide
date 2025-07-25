# RAG를 통한 구조화된 출력에서의 환각 현상 감소

import {Bleed} from 'nextra-theme-docs'

<iframe width="100%"
  height="415px"
  src="https://www.youtube.com/embed/TUL5guqZejw?si=Doc7lzyAY-SKr21L" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowFullScreen
  />

ServiceNow의 연구자들이 구조화된 출력 작업을 위한 효율적인 RAG 시스템을 배포하는 방법에 대해 논의하는 [새로운 논문](https://arxiv.org/abs/2404.08189)을 공유했습니다.

!["RAG Hallucination"](../../img/research/structured_outputs.png)

RAG 시스템은 작은 언어 모델과 매우 작은 검색기를 결합합니다. RAG가 제한된 리소스 환경에서 강력한 LLM 기반 시스템을 배포할 수 있게 하면서 환각 현상과 같은 문제를 완화하고 출력의 신뢰성을 높일 수 있음을 보여줍니다.

이 논문은 자연어 요구사항을 워크플로우(JSON 형식)로 번역하는 매우 유용한 엔터프라이즈 응용 프로그램을 다룹니다. 이 작업에서 많은 생산성을 얻을 수 있지만 더 많은 최적화가 달성될 수 있습니다(예: 추론적 디코딩 사용 또는 JSON 대신 YAML 사용).

이 논문은 실제 세계를 위한 RAG 시스템을 효과적으로 개발하는 방법에 대한 훌륭한 통찰력과 실용적인 팁을 제공합니다. 