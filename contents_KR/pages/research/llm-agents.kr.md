# LLM 에이전트(LLM Agents)

LLM 기반 에이전트(이하 LLM 에이전트)는 LLM(대형 언어 모델, Large Language Model)과 계획(Planning), 메모리(Memory) 등 핵심 모듈을 결합한 아키텍처를 통해 복잡한 작업을 수행할 수 있는 LLM 응용 시스템을 의미합니다. LLM 에이전트 구축 시, LLM은 작업 또는 사용자 요청을 완수하는 데 필요한 일련의 작업 흐름을 제어하는 메인 컨트롤러(혹은 "두뇌") 역할을 합니다. LLM 에이전트는 계획, 메모리, 도구 사용 등 주요 모듈이 필요할 수 있습니다.

LLM 에이전트의 유용성을 설명하기 위해 다음과 같은 질문에 답하는 시스템을 구축한다고 가정해봅시다:

> 2023년 미국의 1일 평균 칼로리 섭취량은 얼마인가요?

위 질문은 LLM이 이미 관련 지식을 내장하고 있다면 직접 답변할 수 있습니다. 만약 LLM이 관련 지식을 갖고 있지 않다면, 건강 관련 정보나 보고서에 접근할 수 있는 간단한 RAG 시스템을 활용할 수 있습니다. 이제 시스템에 더 복잡한 질문을 던져봅시다:

> 지난 10년간 미국 성인의 1일 평균 칼로리 섭취량 추이는 어떻게 변화했으며, 이 변화가 비만율에 어떤 영향을 미쳤나요? 또한, 이 기간 동안 비만율 추이의 그래프도 제공해 주세요.

이처럼 복잡한 질문에는 LLM만으로는 충분하지 않습니다. LLM에 외부 지식 기반을 결합한 RAG 시스템을 구축해도 위와 같은 복합 쿼리를 완전히 해결하기는 어렵습니다. 이는 해당 질문이 여러 하위 작업으로 분해되어야 하며, 각 하위 작업을 도구와 일련의 작업 흐름을 통해 해결해야 하기 때문입니다. 가능한 해결책은 LLM 에이전트가 검색 API, 건강 관련 논문, 공공/사설 건강 데이터베이스 등에 접근해 칼로리 섭취 및 비만 관련 정보를 제공하도록 하는 것입니다.

또한, LLM은 관련 데이터를 활용해 트렌드 그래프를 생성할 수 있는 "코드 인터프리터" 도구에도 접근해야 합니다. 이러한 고수준 컴포넌트 외에도, 전체 작업을 계획하고, 작업 흐름과 관찰, 진행 상황을 추적할 수 있는 메모리 모듈이 필요합니다.

## LLM 에이전트 프레임워크

![](../../img/agents/agent-framework.png)

일반적으로 LLM 에이전트 프레임워크는 다음과 같은 핵심 컴포넌트로 구성됩니다:
- 사용자 요청(User Request): 사용자 질문 또는 요청
- 에이전트/두뇌(Agent/Brain): 에이전트의 핵심, 조정자 역할
- 계획(Planning): 향후 행동 계획 수립 지원
- 메모리(Memory): 과거 행동 관리

### 에이전트(Agent)

범용 LLM이 시스템의 두뇌, 에이전트 모듈, 조정자 역할을 수행합니다. 이 컴포넌트는 에이전트의 동작 방식과 접근 가능한 도구(및 도구 세부 정보)를 명시한 프롬프트 템플릿을 통해 활성화됩니다.

필수는 아니지만, 에이전트에 프로필이나 페르소나를 부여해 역할을 정의할 수 있습니다. 이러한 프로필 정보는 프롬프트에 작성되며, 역할, 성격, 사회적 정보, 기타 인구통계 정보 등이 포함될 수 있습니다. [Wang 등(2023)](https://arxiv.org/abs/2308.11432)에 따르면, 에이전트 프로필 정의 전략에는 수작업, LLM 생성, 데이터 기반 방식이 있습니다.

### 계획(Planning)

#### 피드백 없는 계획(Planning Without Feedback)

계획 모듈은 에이전트가 사용자 요청을 해결하기 위해 필요한 단계 또는 하위 작업을 분해하는 데 도움을 줍니다. 이 단계는 에이전트가 문제를 더 잘 추론하고 신뢰성 있게 해결책을 찾도록 합니다. 계획 모듈은 LLM을 활용해 세부 계획(하위 작업 포함)을 생성합니다. 대표적인 작업 분해 기법으로는 [연쇄적 사고(Chain of Thought)](https://www.promptingguide.ai/techniques/cot), [사고의 나무(Tree of Thoughts)](https://www.promptingguide.ai/techniques/tot)가 있으며, 각각 단일 경로(single-path)와 다중 경로(multi-path) 추론으로 분류됩니다. 아래 그림은 [Wang 등(2023)](https://arxiv.org/abs/2308.11432)에서 공식화한 다양한 전략을 비교합니다:

![](../../img/agents/task-decomposition.png)

#### 피드백 기반 계획(Planning With Feedback)

위 계획 모듈은 피드백을 포함하지 않아, 복잡한 작업의 장기 계획(long-horizon planning)에 한계가 있습니다. 이를 해결하기 위해, 과거 행동과 관찰에 기반해 실행 계획을 반복적으로 반영(reflect) 및 개선(refine)할 수 있는 메커니즘을 도입할 수 있습니다. 이는 과거 실수를 교정하고 결과 품질을 높이는 데 중요합니다. 실제 환경 및 복잡한 과제에서는 시행착오가 핵심이므로 더욱 필요합니다. 대표적인 반영/비평(critic) 메커니즘으로는 [ReAct](https://www.promptingguide.ai/techniques/react), [Reflexion](https://arxiv.org/abs/2303.11366)이 있습니다.

예를 들어, ReAct는 추론(Thought)과 행동(Action)을 결합해, LLM이 복잡한 작업을 해결할 수 있도록 `Thought`, `Action`, `Observation` 단계를 반복적으로 수행합니다. ReAct는 환경으로부터 관찰(observation) 형태의 피드백을 받습니다. 기타 피드백에는 인간 또는 모델 피드백이 포함될 수 있습니다. 아래 그림은 ReAct의 예시와 질의응답 수행 단계별 흐름을 보여줍니다:

![](../../img/react.png)

### 메모리(Memory)

메모리 모듈은 에이전트의 내부 로그(과거 사고, 행동, 환경 관찰, 사용자와의 상호작용 등)를 저장합니다. LLM 에이전트 관련 문헌에서는 다음 두 가지 주요 메모리 유형이 보고됩니다:
- **단기 메모리(Short-term memory)**: 에이전트의 현재 상황에 대한 컨텍스트 정보를 포함하며, 일반적으로 인컨텍스트 학습(in-context learning)으로 구현됩니다. 컨텍스트 윈도우 제약으로 인해 짧고 유한합니다.
- **장기 메모리(Long-term memory)**: 에이전트의 과거 행동과 사고를 장기간 보존 및 회수할 수 있도록 하며, 외부 벡터 스토어(vector store)를 활용해 필요 시 관련 정보를 빠르고 확장성 있게 제공합니다.

하이브리드 메모리는 단기 및 장기 메모리를 결합해 장기 추론 및 경험 축적 능력을 강화합니다.

에이전트 구축 시 고려할 수 있는 다양한 메모리 포맷도 존재합니다. 대표적으로 자연어, 임베딩, 데이터베이스, 구조화 리스트 등이 있으며, 예를 들어 [GITM(Ghost in the Minecraft)](https://arxiv.org/abs/2305.17144)은 키는 자연어, 값은 임베딩 벡터로 구성된 키-값 구조를 활용합니다.

계획 및 메모리 모듈은 에이전트가 동적 환경에서 효과적으로 과거 행동을 회상하고 미래 행동을 계획할 수 있도록 지원합니다.

### 도구(Tools)

도구는 LLM 에이전트가 외부 환경(예: 위키피디아 검색 API, 코드 인터프리터, 수학 엔진 등)과 상호작용할 수 있도록 하는 기능입니다. 데이터베이스, 지식베이스, 외부 모델 등도 도구에 포함될 수 있습니다. 에이전트가 외부 도구와 상호작용할 때는 워크플로우를 통해 하위 작업을 완수하고 사용자 요청을 만족시키는 데 필요한 관찰 또는 정보를 획득합니다. 앞서 예시로 든 건강 관련 쿼리에서, 코드 인터프리터는 코드를 실행해 요청된 그래프 정보를 생성하는 도구입니다.

LLM은 다양한 방식으로 도구를 활용합니다:
- [MRKL](https://arxiv.org/abs/2205.00445): LLM과 전문가 모듈(LLM 또는 계산기, 날씨 API 등 심볼릭 모듈)을 결합한 프레임워크
- [Toolformer](https://arxiv.org/abs/2302.04761): LLM이 외부 도구 API를 사용할 수 있도록 파인튜닝
- [Function Calling](https://www.promptingguide.ai/applications/function_calling): 도구 API 집합을 정의해 모델 요청에 포함, LLM의 도구 사용 능력 강화
- [HuggingGPT](https://arxiv.org/abs/2303.17580): LLM을 태스크 플래너로 활용해 다양한 AI 모델을 연결, 복합 AI 과제 해결

![](../../img/agents/hugginggpt.png)

## LLM 에이전트 응용 사례

![](../../img/agents/chemcrow.png)
*ChemCrow 에이전트: 유기 합성, 신약 개발, 소재 설계 등 다양한 과제 수행. 그림 출처: Bran 등, 2023*

이 섹션에서는 LLM 에이전트가 복잡한 추론 및 상식적 이해 능력 덕분에 효과적으로 적용된 도메인 및 사례를 소개합니다.

### 주요 LLM 기반 에이전트 사례

- [Ma 등(2023)](https://arxiv.org/abs/2307.15810): 정신 건강 지원을 위한 대화형 에이전트의 효과 분석. 불안 완화에 도움을 주지만, 때때로 유해한 콘텐츠를 생성할 수 있음.
- [Horton(2023)](https://arxiv.org/abs/2301.07543): LLM 기반 에이전트에 부여된 소유권, 선호도, 성격을 통해 인간의 경제 행동을 시뮬레이션.
- [Generative Agents](https://arxiv.org/abs/2304.03442), [AgentSims](https://arxiv.org/abs/2308.04026): 가상 마을에서 다수의 에이전트를 구성해 인간의 일상생활 시뮬레이션.
- [Blind Judgement](https://arxiv.org/abs/2301.05327): 여러 언어 모델을 활용해 실제 대법원 판결 예측.
- [Ziems 등(2023)](https://arxiv.org/abs/2305.03514): 연구자 지원(초록 생성, 스크립트 작성, 키워드 추출 등) 에이전트.
- [ChemCrow](https://arxiv.org/abs/2304.05376): 화학 데이터베이스를 활용해 합성, 신약, 신소재 설계 등 자율적 과제 수행.
- [Boiko 등(2023)]: 여러 LLM을 결합해 과학 실험의 설계, 계획, 실행 자동화.
- [Math Agents]: 수학 문제 탐구, 발견, 증명 지원. [EduChat](https://arxiv.org/abs/2308.02773), [CodeHelp](https://arxiv.org/abs/2308.06921): 교육용 LLM 에이전트 사례.
- [Mehta 등(2023)](https://arxiv.org/abs/2304.10750): 3D 시뮬레이션 환경에서 인간 건축가와 AI 에이전트의 상호작용 지원 프레임워크.
- [ChatDev](https://arxiv.org/abs/2307.07924), [ToolLLM](https://arxiv.org/abs/2307.16789), [MetaGPT](https://arxiv.org/abs/2308.00352): 코딩, 디버깅, 테스트 등 소프트웨어 엔지니어링 자동화 지원 에이전트.
- [D-Bot](https://arxiv.org/abs/2308.05481): 데이터베이스 유지보수 경험을 축적하고 진단/최적화 조언을 제공하는 LLM 기반 데이터베이스 관리자.
- [IELLM](https://arxiv.org/abs/2304.14354): 석유 및 가스 산업 문제 해결을 위한 LLM 적용.
- [Dasgupta 등(2023)](https://arxiv.org/abs/2302.00763): 구현형 추론 및 작업 계획을 위한 통합 에이전트 시스템.
- [OS-Copilot](https://arxiv.org/abs/2402.07456): 웹, 코드 터미널, 파일, 멀티미디어, 다양한 서드파티 앱 등 운영체제(OS) 내 다양한 요소와 인터페이스할 수 있는 범용 에이전트 프레임워크.

### LLM 에이전트 도구 및 프레임워크

![](../../img/agents/autogen.png)
*AutoGen 기능; 그림 출처: https://microsoft.github.io/autogen*

아래는 LLM 에이전트 구축에 활용되는 주요 도구 및 프레임워크입니다:
- [LangChain](https://python.langchain.com/docs/get_started/introduction): LLM 기반 응용 및 에이전트 개발 프레임워크
- [AutoGPT](https://github.com/Significant-Gravitas/AutoGPT): AI 에이전트 구축 도구 제공
- [Langroid](https://github.com/langroid/langroid): 멀티 에이전트 프로그래밍 지원, 메시지 기반 협업
- [AutoGen](https://microsoft.github.io/autogen/): 다중 에이전트가 상호 대화하며 과제 해결
- [OpenAgents](https://github.com/xlang-ai/OpenAgents): 오픈 플랫폼, 다양한 언어 에이전트 사용 및 호스팅
- [LlamaIndex](https://www.llamaindex.ai/): 커스텀 데이터 소스를 LLM에 연결하는 프레임워크
- [GPT Engineer](https://github.com/gpt-engineer-org/gpt-engineer): 코드 자동 생성 및 개발 작업 지원
- [DemoGPT](https://github.com/melih-unsal/DemoGPT): Streamlit 앱 자동 생성 에이전트
- [GPT Researcher](https://github.com/assafelovic/gpt-researcher): 다양한 과제에 대한 종합적 온라인 리서치 지원 에이전트
- [AgentVerse](https://github.com/OpenBMB/AgentVerse): 다양한 응용에서 다중 LLM 에이전트 배포 지원
- [Agents](https://github.com/aiwaves-cn/agents): 자율 언어 에이전트 구축용 오픈소스 라이브러리/프레임워크(장단기 메모리, 도구 사용, 웹 탐색, 다중 에이전트 통신, 인간-에이전트 상호작용, 심볼릭 제어 등 지원)
- [BMTools](https://github.com/OpenBMB/BMTools): LLM의 도구 활용 확장 및 커뮤니티 도구 공유 플랫폼
- [crewAI](https://www.crewai.io/): 엔지니어를 위한 강력하고 간편한 AI 에이전트 프레임워크
- [Phidata](https://github.com/phidatahq/phidata): 함수 호출 기반 AI 어시스턴트 구축 툴킷

## LLM 에이전트 평가

![](../../img/agents/agentbench.png)
*AgentBench: 실제 과제 및 8개 환경에서 LLM 에이전트 평가용 벤치마크. 그림 출처: Liu 등, 2023*

LLM 자체 평가와 마찬가지로, LLM 에이전트 평가 역시 매우 도전적입니다. [Wang 등(2023)]에 따르면, 일반적인 평가 방법은 다음과 같습니다:
- **인간 평가(Human Annotation)**: 인간 평가자가 정직성, 유용성, 몰입도, 편향성 등 다양한 측면에서 LLM 결과를 직접 평가
- **튜링 테스트(Turing Test)**: 인간 평가자가 실제 인간과 에이전트의 결과를 비교, 구분 불가 시 인간 수준 성능으로 간주
- **지표(Metrics)**: 에이전트 품질을 반영하는 정교한 지표(과제 성공률, 인간 유사성, 효율성 등)
- **프로토콜(Protocols)**: 지표 활용 방식을 결정하는 평가 프로토콜(실제 환경 시뮬레이션, 사회적 평가, 다중 과제 평가, 소프트웨어 테스트 등)
- **벤치마크(Benchmarks)**: LLM 에이전트 평가용 다양한 벤치마크([ALFWorld](https://alfworld.github.io/), [IGLU](https://arxiv.org/abs/2304.10750), [Tachikuma](https://arxiv.org/abs/2307.12573), [AgentBench](https://github.com/THUDM/AgentBench), [SocKET](https://arxiv.org/abs/2305.14938), [AgentSims](https://arxiv.org/abs/2308.04026), [ToolBench](https://arxiv.org/abs/2305.16504), [WebShop](https://arxiv.org/abs/2207.01206), [Mobile-Env](https://github.com/stefanbschneider/mobile-env), [WebArena](https://github.com/web-arena-x/webarena), [GentBench](https://arxiv.org/abs/2308.04030), [RocoBench](https://project-roco.github.io/), [EmotionBench](https://project-roco.github.io/), [PEB](https://arxiv.org/abs/2308.06782), [ClemBench](https://arxiv.org/abs/2305.13455), [E2E](https://arxiv.org/abs/2308.04624) 등)

## 과제 및 한계(Challenges)

LLM 에이전트는 아직 초기 단계이므로, 구축 시 다양한 과제와 한계가 존재합니다:
- **역할 수행 능력(Role-playing capability)**: LLM 에이전트는 도메인별 과제 완수를 위해 역할 적응이 필요합니다. LLM이 특정 역할을 잘 표현하지 못할 경우, 해당 역할/심리 캐릭터 데이터를 활용해 파인튜닝이 필요할 수 있습니다.
- **장기 계획 및 컨텍스트 한계(Long-term planning and finite context length)**: 장기 이력 기반 계획은 오류 발생 시 복구가 어렵고, LLM의 컨텍스트 길이 한계로 인해 단기 메모리 활용에 제약이 있습니다.
- **범용 인간 정렬(Generalized human alignment)**: 다양한 인간 가치와의 정렬이 어려우며, 고급 프롬프트 전략 설계로 재정렬이 필요할 수 있습니다.
- **프롬프트 견고성 및 신뢰성(Prompt robustness and reliability)**: LLM 에이전트는 메모리, 계획 등 다양한 모듈별 프롬프트를 사용하므로, 프롬프트 변화에 따른 신뢰성 저하 문제가 빈번합니다. 잠재적 해결책으로는 반복적 실험, 자동 프롬프트 최적화, GPT 기반 자동 프롬프트 생성 등이 있습니다. LLM의 환각(hallucination) 문제 역시 LLM 에이전트에서 빈번히 발생합니다. 자연어 기반 외부 컴포넌트 인터페이스 과정에서 상충 정보가 유입되어 환각 및 사실성 문제가 발생할 수 있습니다.
- **지식 경계(Knowledge boundary)**: LLM의 내부 지식이 시뮬레이션 효과에 영향을 미칠 수 있으며, 사용자에게 알려지지 않은 지식 활용, 편향 등으로 인해 에이전트 행동에 영향을 줄 수 있습니다.
- **효율성(Efficiency)**: LLM 에이전트는 LLM이 처리하는 요청이 많아 효율성 저하 및 비용 증가 문제가 발생할 수 있습니다.

## 참고문헌(References)

- [LLM Powered Autonomous Agents](https://lilianweng.github.io/posts/2023-06-23-agent/)
- [MRKL Systems: A modular, neuro-symbolic architecture that combines large language models, external knowledge sources and discrete reasoning](https://arxiv.org/abs/2205.00445)
- [A Survey on Large Language Model based Autonomous Agents](https://arxiv.org/abs/2308.11432)
- [The Rise and Potential of Large Language Model Based Agents: A Survey](https://arxiv.org/abs/2309.07864)
- [Large Language Model based Multi-Agents: A Survey of Progress and Challenges](https://arxiv.org/abs/2402.01680)
- [Cognitive Architectures for Language Agents](https://arxiv.org/abs/2309.02427)
- [Introduction to LLM Agents](https://developer.nvidia.com/blog/introduction-to-llm-agents/)
- [LangChain Agents](https://python.langchain.com/docs/use_cases/tool_use/agents)
- [Building Your First LLM Agent Application](https://developer.nvidia.com/blog/building-your-first-llm-agent-application/)
- [Building LLM applications for production](https://huyenchip.com/2023/04/11/llm-engineering.html#control_flow_with_llm_agents)
- [Awesome LLM agents](https://github.com/kaushikb11/awesome-llm-agents)
- [Awesome LLM-Powered Agent](https://github.com/hyp1231/awesome-llm-powered-agent#awesome-llm-powered-agent)
- [Functions, Tools and Agents with LangChain](https://www.deeplearning.ai/short-courses/functions-tools-agents-langchain/)
