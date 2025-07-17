# 사고의 나무(Tree of Thoughts, ToT)

복잡한 탐색 또는 전략적 선견지명이 요구되는 과제에서는 기존의 단순 프롬프트(prompt) 기법만으로는 한계가 있습니다. 최근 [Yao 등(2023)](https://arxiv.org/abs/2305.10601) 및 [Long(2023)](https://arxiv.org/abs/2305.08291)은 사고의 나무(Tree of Thoughts, ToT)라는 프레임워크를 제안했습니다. 이는 연쇄적 사고(chain-of-thought) 프롬프트를 일반화하고, 문제 해결 과정에서 중간 단계 역할을 하는 다양한 사고(thought)를 탐색하도록 유도합니다.

ToT는 사고(thought)들의 트리(tree)를 유지합니다. 여기서 각 사고는 문제 해결을 위한 중간 단계의 일관된 언어 시퀀스를 의미합니다. 이 접근법은 언어 모델이 문제 해결을 위한 중간 사고를 생성하고, 이를 스스로 평가(self-evaluate)하며, 신중한 추론 과정을 통해 진행 상황을 점검할 수 있게 합니다. 사고의 생성 및 평가 능력은 탐색 알고리즘(예: 너비 우선 탐색(breadth-first search, BFS), 깊이 우선 탐색(depth-first search, DFS))과 결합되어, 선견지명(lookahead)과 백트래킹(backtracking)이 가능한 체계적 사고 탐색을 가능하게 합니다.

ToT 프레임워크의 개요는 아래와 같습니다:

이미지 출처: [Yao 등(2023)](https://arxiv.org/abs/2305.10601)

ToT를 사용할 때는 과제별로 후보(candidate) 수와 사고/단계(thought/step) 수를 정의해야 합니다. 예를 들어, 논문에서 제시된 Game of 24(24 게임) 수리 추론 과제에서는 사고를 3단계로 분해하고, 각 단계마다 중간 방정식을 만듭니다. 각 단계에서 상위 b=5개의 후보만을 유지합니다.

Game of 24에서 ToT의 BFS를 수행할 때, 언어 모델은 각 사고 후보가 24에 도달할 수 있는지 "확실(sure)/아마도(maybe)/불가능(impossible)"으로 평가하도록 프롬프트됩니다. 저자에 따르면, "목표는 소수의 lookahead 시도 내에 판정 가능한 올바른 부분 해(partial solution)를 촉진하고, 상식적으로 '너무 크거나/작은' 불가능한 부분 해는 제거하며, 나머지는 '아마도'로 유지하는 것"입니다. 각 사고별로 3회 샘플링합니다. 이 과정은 아래와 같이 시각화됩니다:

이미지 출처: [Yao 등(2023)](https://arxiv.org/abs/2305.10601)

아래 그림의 결과에서 볼 수 있듯, ToT는 다른 프롬프트 기법 대비 월등한 성능을 보입니다:

이미지 출처: [Yao 등(2023)](https://arxiv.org/abs/2305.10601)

코드는 [여기](https://github.com/princeton-nlp/tree-of-thought-llm)와 [여기](https://github.com/jieyilong/tree-of-thought-puzzle-solver)에서 확인할 수 있습니다.

전체적으로 [Yao 등(2023)](https://arxiv.org/abs/2305.10601)과 [Long(2023)](https://arxiv.org/abs/2305.08291)의 주요 아이디어는 유사합니다. 모두 다중 라운드 대화 기반 트리 탐색(tree search)을 통해 LLM의 복잡한 문제 해결 능력을 강화합니다. 주요 차이점 중 하나는 [Yao 등(2023)](https://arxiv.org/abs/2305.10601)은 DFS/BFS/빔 서치(beam search) 등 일반적 탐색 전략을 사용하고, [Long(2023)](https://arxiv.org/abs/2305.08291)은 강화학습(reinforcement learning, RL)로 훈련된 "ToT 컨트롤러(ToT Controller)"가 트리 탐색 전략(언제 백트래킹할지, 몇 단계까지 되돌릴지 등)을 결정한다는 점입니다. DFS/BFS/빔 서치는 특정 문제에 특화되지 않은 일반적 탐색 전략입니다. 반면, RL 기반 ToT 컨트롤러는 새로운 데이터셋이나 셀프플레이(self-play, 예: AlphaGo vs 브루트포스 탐색)로부터 학습할 수 있어, 고정된 LLM 환경에서도 지속적으로 진화하고 새로운 지식을 습득할 수 있습니다.

[Hulbert(2023)](https://github.com/dave1010/tree-of-thought-prompting)은 ToT 프레임워크의 주요 개념을 단일 프롬프트로 구현한 Tree-of-Thought Prompting을 제안했습니다. LLM이 중간 사고를 한 번의 프롬프트에서 평가하도록 하는 방식입니다. 예시 프롬프트는 다음과 같습니다:

```
Imagine three different experts are answering this question.
All experts will write down 1 step of their thinking,
then share it with the group.
Then all experts will go on to the next step, etc.
If any expert realises they're wrong at any point then they leave.
The question is...
```

[Sun(2023)](https://github.com/holarissun/PanelGPT)은 Tree-of-Thought Prompting을 대규모 실험으로 벤치마크하고, LLM 간 패널 토론(panel discussion) 프롬프트 기법인 PanelGPT를 제안했습니다.
