# LLM을 위한 중간 수정 및 검색을 통한 추론

import {Bleed} from 'nextra-theme-docs'

<iframe width="100%"
  height="415px"
  src="https://www.youtube.com/embed/13fr5m6ezOM?si=DH3XYfzbMsg9aeIx" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowFullScreen
  />

[Chi et al. (2024)](https://arxiv.org/abs/2404.05966)의 이 연구는 구성 요소로 분해할 수 있는 작업에 대한 일반적인 추론 및 검색을 위한 접근 방식을 제시합니다.

제안된 그래프 기반 프레임워크인 THOUGHTSCULPT는 반복적인 자기 수정 기능을 통합하고 LLM이 생각의 얽힌 네트워크를 구축할 수 있도록 합니다.

트리를 사용하여 추론 과정을 형성하는 Tree-of-thoughts와 같은 다른 접근 방식과 달리, 이 새로운 접근 방식은 Monte Carlo Tree Search (MCTS)를 통합하여 검색 공간을 효율적으로 탐색합니다.

이 새로운 방법은 LLM 기반 생각 평가기를 사용하여 후보 부분 출력에 대한 피드백을 제공합니다. 그런 다음 생각 생성기 구성 요소가 잠재적 솔루션을 생성합니다. 생각 평가기와 생각 생성기는 확장 단계로 간주되며 현재 솔루션을 개선하는 데 도움이 됩니다.

!["ThoughtSculpt"](../../img/research/thoughtsculpt.png)

마지막으로, 의사결정 시뮬레이터(MCTS 프로세스의 일부로 작동)는 연속적인 사고 라인을 시뮬레이션하여 경로의 잠재적 가치를 평가합니다.

지속적인 생각 반복 능력으로 인해 THOUGHTSCULPT는 개방형 생성, 다단계 추론, 창의적 아이디어 생성과 같은 작업에 특히 적합합니다.

우리는 유사한 개념과 검색 알고리즘을 사용하여 LLM의 추론 능력과 복잡한 추론과 계획이 필요한 문제를 해결하는 능력을 향상시키는 더 고급 접근 방식을 더 많이 보게 될 수 있습니다. 이 연구 트렌드를 추적하기 위한 훌륭한 논문입니다. 