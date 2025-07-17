# RAG용 합성 데이터셋 생성(Generating Synthetic Dataset for RAG)

## RAG용 합성 데이터 구축
머신러닝 엔지니어(Machine Learning Engineer)의 현실에서는 라벨이 지정된 데이터가 부족하거나 거의 없는 경우가 많습니다. 일반적으로 이를 인지하면, 프로젝트는 데이터 수집과 라벨링이라는 긴 과정을 시작하게 되고, 몇 달이 지나서야 비로소 솔루션 개발에 착수할 수 있습니다.

하지만 대형 언어 모델(LLM, Large Language Model)의 등장으로 일부 제품에서는 패러다임이 전환되었습니다. 이제 LLM의 일반화 능력에 의존해 아이디어를 즉시 실험하거나 AI 기반 기능을 빠르게 개발할 수 있습니다. 만약 의도한 대로 동작한다면, 그때 전통적인 개발 프로세스를 시작하면 됩니다.

![AI 기반 제품의 패러다임 전환](../../img/synthetic_rag/synthetic_rag_1.png)

이미지 출처: [The Rise of the AI Engineer, by S. Wang](https://www.latent.space/p/ai-engineer)

새롭게 주목받는 접근법 중 하나가 [RAG(Retrieval Augmented Generation, 검색 증강 생성)](https://www.promptingguide.ai/techniques/rag)입니다. RAG는 모델의 지식만으로는 충분하지 않은 지식 집약적 작업에 사용됩니다. RAG는 정보 검색 컴포넌트와 텍스트 생성 모델을 결합합니다. 이 접근법에 대해 더 알고 싶다면 [가이드의 관련 섹션](https://www.promptingguide.ai/techniques/rag)을 참고하세요.

RAG의 핵심 요소는 관련 문서를 식별해 LLM에 전달하는 검색(Retrieval) 모델입니다. 검색 모델의 성능이 좋을수록 제품이나 기능의 결과도 좋아집니다. 이상적으로는 검색이 바로 잘 동작해야 하지만, 실제로는 언어나 도메인에 따라 성능이 크게 저하되는 경우가 많습니다.

예를 들어, 체코 법률(체코어) 기반 챗봇을 만들어야 하거나, 인도 시장에 특화된 세무 어시스턴트(오픈AI의 GPT-4 발표 사례)를 설계해야 한다고 가정해봅시다. 이럴 때 검색 모델이 가장 관련성 높은 문서를 자주 놓치거나 전반적으로 성능이 떨어져 시스템 품질이 제한될 수 있습니다.

하지만 해결책이 있습니다. 최근에는 기존 LLM을 활용해 새로운 LLM/검색기/기타 모델 학습용 데이터를 합성(synthesize)하는 트렌드가 부상하고 있습니다. 이 과정은 프롬프트 기반 쿼리 생성(prompt-based query generation)을 통해 LLM을 표준 크기의 인코더로 증류(distill)하는 것으로 볼 수 있습니다. 증류는 계산적으로 비용이 크지만, 추론 비용을 크게 줄이고 특히 저자원 언어나 특수 도메인에서 성능을 크게 향상시킬 수 있습니다.

이 가이드에서는 ChatGPT, GPT-4 등 최신 텍스트 생성 모델을 활용해 지침에 따라 대량의 합성 데이터를 생성합니다. [Dai et al. (2022)](https://arxiv.org/abs/2209.11755)는 단 8개의 수작업 라벨 예시와 대규모 비라벨 데이터(예: 모든 법률 문서)만으로도 거의 SOTA(State-of-the-Art) 성능에 도달할 수 있음을 보였습니다. 이 연구는 합성 데이터가 감독 데이터가 부족한 상황에서 작업 특화 검색기 학습에 매우 유용함을 입증합니다.

## 도메인 특화 데이터셋 생성
LLM을 활용하려면 간단한 설명과 몇 개의 수작업 예시가 필요합니다. 검색 작업마다 검색 의도(search intent)가 다르므로 "관련성(relevance)"의 정의도 달라집니다. 즉, 동일한 (쿼리, 문서) 쌍이라도 검색 의도에 따라 관련성이 완전히 달라질 수 있습니다. 예를 들어, 논증 검색 작업(argument retrieval)은 지지 논거를 찾지만, 다른 작업은 반대 논거를 요구할 수 있습니다([ArguAna 데이터셋](https://aclanthology.org/P18-1023/) 참고).

아래 예시는 이해를 돕기 위해 영어로 작성되었지만, 실제 데이터는 어떤 언어든 상관없습니다. ChatGPT/GPT-4는 저자원 언어도 효율적으로 처리합니다.

*프롬프트 예시:*
```
Task: Identify a counter-argument for the given argument.

Argument #1: {insert passage X1 here}

A concise counter-argument query related to the argument #1: {insert manually prepared query Y1 here}

Argument #2: {insert passage X2 here}
A concise counter-argument query related to the argument #2: {insert manually prepared query Y2 here}

<- paste your examples here ->

Argument N: Even if a fine is made proportional to income, you will not get the equality of impact you desire. This is because the impact is not proportional simply to income, but must take into account a number of other factors. For example, someone supporting a family will face a greater impact than someone who is not, because they have a smaller disposable income. Further, a fine based on income ignores overall wealth (i.e. how much money someone actually has: someone might have a lot of assets but not have a high income). The proposition does not cater for these inequalities, which may well have a much greater skewing effect, and therefore the argument is being applied inconsistently.

A concise counter-argument query related to the argument #N:
```

*출력 예시:*
```
punishment house would make fines relative income
```

일반적으로 이러한 프롬프트는 다음과 같이 표현할 수 있습니다:

$(e_{prompt}, e_{doc}(d_{1}), e_{query}(q_1), . . . , e_{doc}(d_k), e_{query}(q_k), e_{doc}(d))$

여기서 $e_{doc}$와 $e_{query}$는 작업 특화 문서, 쿼리 설명이고, $e_{prompt}$는 ChatGPT/GPT-4용 작업 특화 프롬프트/지침, $d$는 LLM이 쿼리를 생성할 새 문서입니다.

이 프롬프트에서 마지막 문서 $d$와 생성된 쿼리만이 로컬 모델 추가 학습에 사용됩니다. 이 방식은 목표 검색 코퍼스 $D$가 있지만, 새로운 작업에 대한 주석 쿼리-문서 쌍이 부족할 때 적용할 수 있습니다.

전체 파이프라인 개요:

![PROMPTGATOR 데이터셋 생성 및 학습 개요](../../img/synthetic_rag/synthetic_rag_2.png)

이미지 출처: [Dai et al. (2022)](https://arxiv.org/abs/2209.11755)

예시 주석을 신중하게 준비하는 것이 중요합니다. 예를 들어 20개 정도를 준비해두고, 프롬프트에는 무작위로 2~8개만 넣는 것이 좋습니다. 이렇게 하면 주석 비용을 크게 늘리지 않고도 합성 데이터의 다양성을 높일 수 있습니다. 단, 예시는 대표성 있고, 형식이 정확하며, 쿼리 길이나 톤 등 세부 사항까지 명확해야 합니다. 예시와 지침이 정교할수록 합성 데이터의 품질이 높아집니다. 품질이 낮은 few-shot 예시는 학습 모델의 성능을 저하시킬 수 있습니다.

대부분의 경우, ChatGPT와 같은 저렴한 모델로도 충분합니다. 이 모델은 비영어권이나 특수 도메인에서도 잘 동작합니다. 예를 들어, 지침과 4~5개 예시가 포함된 프롬프트는 보통 700토큰(검색기 제약상 각 문단은 128토큰 이하)이고, 생성은 25토큰입니다. 따라서 5만 개 문서 코퍼스에 대해 합성 데이터셋을 생성해 로컬 모델을 파인튜닝할 경우 비용은 `50,000 * (700 * 0.001 * $0.0015 + 25 * 0.001 * $0.002) = 55` 달러(여기서 `$0.0015`, `$0.002`는 GPT-3.5 Turbo API의 1,000토큰당 비용)입니다. 동일 문서에 대해 2~4개의 쿼리 예시를 생성하는 것도 가능합니다. 특히 일반 도메인(예: 영어 뉴스 검색)이 아니라 특정 도메인(예: 체코 법률)이라면 추가 학습의 효과가 큽니다.

5만이라는 수치는 임의가 아닙니다. [Dai et al. (2022)](https://arxiv.org/abs/2209.11755) 연구에 따르면, 합성 데이터로 학습한 모델이 수작업 데이터로 학습한 모델과 동등한 품질을 내려면 약 5만 개의 라벨 데이터가 필요합니다. 만약 제품 출시 전 1만 개 이상의 예시를 수집해야 한다면, 한 달 이상 걸리고 인건비도 1,000달러를 훨씬 초과할 것입니다. 반면, 합성 데이터와 로컬 검색기 학습을 활용하면 며칠 만에 두 자릿수 성능 향상을 달성할 수 있습니다!

![합성 데이터셋 vs 수작업 라벨 데이터셋](../../img/synthetic_rag/synthetic_rag_3.png)

이미지 출처: [Dai et al. (2022)](https://arxiv.org/abs/2209.11755)

아래는 동일 논문에서 BeIR 벤치마크 일부 데이터셋에 사용된 프롬프트 템플릿 예시입니다.

![PROMPTGATOR 논문의 프롬프트 템플릿](../../img/synthetic_rag/synthetic_rag_4.png)

이미지 출처: [Dai et al. (2022)](https://arxiv.org/abs/2209.11755) 