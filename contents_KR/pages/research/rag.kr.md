# 검색 증강 생성(Retrieval Augmented Generation, RAG) for LLMs

LLM(대형 언어 모델, Large Language Model) 활용 시 도메인 지식의 공백, 사실성 문제, 환각(hallucination) 등 다양한 과제에 직면하게 됩니다. 검색 증강 생성(Retrieval Augmented Generation, RAG)은 LLM에 데이터베이스 등 외부 지식을 결합함으로써 이러한 문제를 완화하는 솔루션을 제공합니다. RAG는 지식 집약적 시나리오나 지속적으로 지식이 갱신되는 도메인 특화 응용에 특히 유용합니다. RAG의 핵심 장점은 LLM을 과제별로 재학습하지 않아도 된다는 점이며, 최근 대화형 에이전트 등에서 그 활용이 급증하고 있습니다.

이 문서에서는 최근 설문 논문 [Retrieval-Augmented Generation for Large Language Models: A Survey](https://arxiv.org/abs/2312.10997) (Gao 등, 2023)의 주요 결과와 실용적 인사이트를 요약합니다. 특히 RAG 시스템을 구성하는 다양한 컴포넌트(검색, 생성, 증강)의 최신 접근법, SOTA RAG, 평가, 응용, 기술을 중점적으로 다룹니다.

## RAG 소개

RAG는 입력을 받아, 소스(예: 위키피디아)에서 관련/지원 문서를 검색합니다. 검색된 문서들은 원래 입력 프롬프트와 함께 컨텍스트로 연결되어 텍스트 생성기에 입력되고, 최종 출력이 생성됩니다. 이 방식은 사실이 시간이 지나며 변할 수 있는 상황에 적응적입니다. LLM의 파라메트릭 지식(parametric knowledge)은 정적이기 때문에, RAG는 재학습 없이 최신 정보를 검색 기반 생성(retrieval-based generation)으로 활용할 수 있게 해줍니다.

즉, RAG에서 검색된 증거는 LLM 응답의 정확성, 제어 가능성, 관련성을 높이는 역할을 합니다. 이로 인해 RAG는 변화가 많은 환경에서 환각이나 성능 저하 문제를 줄이는 데 효과적입니다.

최근에는 사전학습 최적화보다는 RAG와 ChatGPT, Mixtral 등 강력한 파인튜닝 모델의 장점을 결합하는 방향으로 발전하고 있습니다.

## RAG 패러다임

RAG 시스템은 Naive RAG → Advanced RAG → Modular RAG로 진화해왔습니다. 이는 성능, 비용, 효율성의 한계를 극복하기 위한 변화입니다.

- **Naive RAG:** 전통적 색인-검색-생성 프로세스를 따릅니다. 입력 → 관련 문서 검색 → 프롬프트와 결합 → 모델 응답 생성. 대화형 응용에서는 대화 이력도 프롬프트에 통합할 수 있습니다. 한계로는 낮은 정밀도/재현율, 오래된 정보, 중복/반복, 스타일/톤 불일치, 검색 정보에 과도하게 의존하는 생성 등이 있습니다.
- **Advanced RAG:** 데이터 색인, 임베딩, 검색, 사후처리 등 각 단계를 최적화하여 검색 품질을 높입니다. 예) 데이터 세분화, 색인 구조 최적화, 메타데이터 추가, 임베딩 파인튜닝, 동적 임베딩, 재순위화, 프롬프트 압축 등.
- **Modular RAG:** 검색, 메모리, 융합, 라우팅, 예측, 태스크 어댑터 등 다양한 모듈을 조합해 문제 맥락에 맞게 유연하게 구성할 수 있습니다.

추가적으로, 하이브리드 검색(키워드+의미론), 재귀적 검색, StepBack 프롬프트, 하위 질의, 가상 문서 임베딩(HyDE) 등 다양한 최적화 기법이 제안되고 있습니다.

## RAG 프레임워크

- **검색(Retrieval):** 의미 표현 개선(청킹, 임베딩 파인튜닝), 질문-문서 정렬(질문 재작성, 임베딩 변환), 검색기-LLM 정렬(피드백 기반 파인튜닝, 어댑터 활용) 등 다양한 방법으로 검색 품질을 높입니다.
- **생성(Generation):** 검색 결과를 바탕으로 자연스러운 텍스트를 생성합니다. 검색 후처리(정보 압축, 재순위화) 또는 LLM 파인튜닝을 통해 생성 품질을 높일 수 있습니다.
- **증강(Augmentation):** 검색된 문맥을 생성 작업에 효과적으로 통합합니다. 증강 단계(사전학습, 파인튜닝, 추론), 증강 소스(비정형/정형/LLM 생성 데이터), 증강 프로세스(반복/재귀/적응형 검색 등) 등 다양한 방식이 있습니다.

## RAG vs 파인튜닝

RAG는 새로운 지식 통합에, 파인튜닝은 내부 지식/출력 형식/복잡한 지침 학습에 강점을 가집니다. 두 방법은 상호 보완적으로 활용될 수 있으며, 프롬프트 엔지니어링도 결과 최적화에 중요한 역할을 합니다.

## RAG 평가

RAG 시스템의 성능 평가는 검색 품질(NDCG, Hit Rate 등)과 생성 품질(정확성, 관련성, 유해성 등) 모두를 측정합니다. 주요 평가 지표로는 컨텍스트 관련성, 답변 충실성, 답변 관련성, 잡음 견고성, 부정 거부, 정보 통합, 반사실 견고성 등이 있습니다. RGB, RECALL 등 벤치마크와 RAGAS, ARES, TruLens 등 자동화 도구가 활용됩니다.

## RAG의 도전과제와 미래

- 컨텍스트 길이 확장에 따른 적응
- 반사실/적대적 정보에 대한 견고성
- 하이브리드 접근법 최적화
- LLM 역할 확장
- 스케일링 법칙 연구
- 상용화 수준의 성능/보안/프라이버시
- 멀티모달 RAG(텍스트 외 이미지, 오디오, 코드 등)
- 평가 메트릭 및 해석 가능성 연구

## RAG 도구

- [LangChain](https://www.langchain.com/), [LlamaIndex](https://www.llamaindex.ai/), [DSPy](https://github.com/stanfordnlp/dspy) 등 종합 프레임워크
- [Flowise AI](https://flowiseai.com/), [HayStack](https://haystack.deepset.ai/), [Meltano](https://meltano.com/), [Cohere Coral](https://cohere.com/coral) 등 특화 도구
- Weaviate의 Verba, Amazon Kendra 등 클라우드 서비스

## 결론

RAG 시스템은 고도화·맞춤화가 빠르게 진행되고 있으며, 다양한 도메인에서 활용도가 높아지고 있습니다. 하이브리드, 자체 검색 등 다양한 연구가 활발하며, 평가 도구와 메트릭에 대한 수요도 증가하고 있습니다.

---

## RAG 연구 인사이트

아래 표는 RAG 관련 최신 연구 논문과 주요 인사이트를 정리한 것입니다.

| 인사이트 | 논문 | 날짜 |
| --- | --- | --- |
| LLM 보조 훈련을 위한 검색 증강 시뮬레이터 | [KAUCUS: Knowledge Augmented User Simulators for Training Language Model Assistants](https://aclanthology.org/2024.scichat-1.5) | 2024년 3월 |
| Corrective Retrieval Augmented Generation(CRAG) 제안, 검색기 자기 수정 및 검색 문서 활용도 개선 | [Corrective Retrieval Augmented Generation](https://arxiv.org/abs/2401.15884) | 2024년 1월 |
| RAPTOR: 재귀적 임베딩·클러스터링·요약 트리 기반 검색 | [RAPTOR: Recursive Abstractive Processing for Tree-Organized Retrieval](https://arxiv.org/abs/2401.18059) | 2024년 1월 |
| 다단계 LM-검색기 상호작용 기반 다중 레이블 분류 | [In-Context Learning for Extreme Multi-Label Classification](https://arxiv.org/abs/2401.12178) | 2024년 1월 |
| 다국어 제로샷 성능 향상을 위한 의미 유사 프롬프트 추출 | [From Classification to Generation: Insights into Crosslingual Retrieval Augmented ICL](https://arxiv.org/abs/2311.06595) | 2023년 11월 |
| Chain-of-Note: 검색 문서 순차적 독서 메모 기반 견고성 강화 | [Chain-of-Note: Enhancing Robustness in Retrieval-Augmented Language Models](https://arxiv.org/abs/2311.09210) | 2023년 11월 |
| 불필요 토큰 제거로 효율적 답변 생성 | [Optimizing Retrieval-augmented Reader Models via Token Elimination](https://arxiv.org/abs/2310.13682) | 2023년 10월 |
| 소형 LM 검증기 기반 지식 증강 LM 검증 | [Knowledge-Augmented Language Model Verification](https://arxiv.org/abs/2310.12836) | 2023년 10월 |
| RAG 4대 능력(잡음 견고성 등) 벤치마크 | [Benchmarking Large Language Models in Retrieval-Augmented Generation](https://arxiv.org/abs/2309.01431) | 2023년 10월 |
| Self-RAG: 검색·자기성찰 기반 품질·사실성 향상 | [Self-RAG: Learning to Retrieve, Generate, and Critique through Self-Reflection](https://arxiv.org/abs/2310.11511) | 2023년 10월 |
| GAR-meets-RAG: 생성 증강 검색 기반 제로샷 IR | [GAR-meets-RAG Paradigm for Zero-Shot Information Retrieval](https://arxiv.org/abs/2310.20158) | 2023년 10월 |
| InstructRetro: 대규모 사전학습+지침튜닝 검색 모델 | [InstructRetro: Instruction Tuning post Retrieval-Augmented Pretraining](https://arxiv.org/abs/2310.07713) | 2023년 10월 |
| RA-DIT: 검색·생성 양방향 미세조정 | [RA-DIT: Retrieval-Augmented Dual Instruction Tuning](https://arxiv.org/abs/2310.01352) | 2023년 10월 |
| 무관 문맥 견고성 강화 | [Making Retrieval-Augmented Language Models Robust to Irrelevant Context](https://arxiv.org/abs/2310.01558) | 2023년 10월 |
| 단순 검색 증강 vs. 긴 컨텍스트 LLM 비교 | [Retrieval meets Long Context Large Language Models](https://arxiv.org/abs/2310.03025) | 2023년 10월 |
| RECOMP: 검색 문서 요약 압축 | [RECOMP: Improving Retrieval-Augmented LMs with Compression and Selective Augmentation](https://arxiv.org/abs/2310.04408) | 2023년 10월 |
| 검색-생성 상호작용 기반 추론 능력 강화 | [Retrieval-Generation Synergy Augmented Large Language Models](https://arxiv.org/abs/2310.05149) | 2023년 10월 |
| Tree of Clarifications: 모호성 해소 트리 기반 장문 생성 | [Tree of Clarifications: Answering Ambiguous Questions with Retrieval-Augmented Large Language Models](https://arxiv.org/abs/2310.14696) | 2023년 10월 |
| Self-Knowledge Guided Retrieval Augmentation | [Self-Knowledge Guided Retrieval Augmentation for Large Language Models](https://arxiv.org/abs/2310.05002) | 2023년 10월 |
| RAGAS: 자동화 평가 메트릭 | [RAGAS: Automated Evaluation of Retrieval Augmented Generation](https://arxiv.org/abs/2309.15217) | 2023년 9월 |
| GenRead: 생성→검색 기반 답변 | [Generate rather than Retrieve: Large Language Models are Strong Context Generators](https://arxiv.org/abs/2209.10063) | 2023년 9월 |
| Haystack DiversityRanker 등 컨텍스트 최적화 | [Enhancing RAG Pipelines in Haystack: Introducing DiversityRanker and LostInTheMiddleRanker](https://towardsdatascience.com/enhancing-rag-pipelines-in-haystack-45f14e2bc9f5) | 2023년 8월 |
| KnowledGPT: KB 검색·저장 연동 | [KnowledGPT: Enhancing Large Language Models with Retrieval and Storage Access on Knowledge Bases](https://arxiv.org/abs/2308.11761) | 2023년 8월 |
| RAVEN: 검색 증강 인코더-디코더 기반 in-context 학습 | [RAVEN: In-Context Learning with Retrieval Augmented Encoder-Decoder Language Models](https://arxiv.org/abs/2308.07922) | 2023년 8월 |
| RaLLe: 오픈소스 RAG 개발·평가 프레임워크 | [RaLLe: A Framework for Developing and Evaluating Retrieval-Augmented Large Language Models](https://arxiv.org/abs/2308.10633) | 2023년 8월 |
| Lost in the Middle: 컨텍스트 위치 변화에 따른 LLM 성능 저하 | [Lost in the Middle: How Language Models Use Long Contexts](https://arxiv.org/abs/2307.03172) | 2023년 7월 |
| 반복적 검색-생성 시너지 | [Enhancing Retrieval-Augmented Large Language Models with Iterative Retrieval-Generation Synergy](https://arxiv.org/abs/2305.15294) | 2023년 5월 |
| FLARE: 미래 예측 기반 능동 검색 | [Active Retrieval Augmented Generation](https://arxiv.org/abs/2305.06983) | 2023년 5월 |
| AAR: 범용 검색 플러그인 | [Augmentation-Adapted Retriever Improves Generalization of Language Models as Generic Plug-In](https://arxiv.org/abs/2305.17331) | 2023년 5월 |
| 구조화 데이터 밀집 검색 개선 | [Structure-Aware Language Model Pretraining Improves Dense Retrieval on Structured Data](https://arxiv.org/abs/2305.19912) | 2023년 5월 |
| Chain-of-Knowledge: 이질적 소스 동적 통합 | [Chain-of-Knowledge: Grounding Large Language Models via Dynamic Knowledge Adapting over Heterogeneous Sources](https://arxiv.org/abs/2305.13269) | 2023년 5월 |
| Knowledge Graph-Augmented LM | [Knowledge Graph-Augmented Language Models for Knowledge-Grounded Dialogue Generation](https://arxiv.org/abs/2305.18846) | 2023년 5월 |
| Rewrite-Retrieve-Read: RL 기반 쿼리 최적화 | [Query Rewriting for Retrieval-Augmented Large Language Models](https://arxiv.org/abs/2305.14283) | 2023년 5월 |
| Self Memory 기반 반복 생성 | [Lift Yourself Up: Retrieval-augmented Text Generation with Self Memory](https://arxiv.org/abs/2305.02437) | 2023년 5월 |
| PKG: 파라메트릭 지식 안내 모듈 | [Augmented Large Language Models with Parametric Knowledge Guiding](https://arxiv.org/abs/2305.04757) | 2023년 5월 |
| RET-LLM: 일반 읽기-쓰기 메모리 | [RET-LLM: Towards a General Read-Write Memory for Large Language Models](https://arxiv.org/abs/2305.14322) | 2023년 5월 |
| Prompt-Guided Retrieval Augmentation | [Prompt-Guided Retrieval Augmentation for Non-Knowledge-Intensive Tasks](https://arxiv.org/abs/2305.17653) | 2023년 5월 |
| UPRISE: 제로샷 프롬프트 검색 | [UPRISE: Universal Prompt Retrieval for Improving Zero-Shot Evaluation](https://arxiv.org/abs/2303.08518) | 2023년 3월 |
| SLM+LLM 적응형 필터-재순위 | [Large Language Model Is Not a Good Few-shot Information Extractor, but a Good Reranker for Hard Samples!](https://arxiv.org/abs/2303.08559) | 2023년 3월 |
| HyDE: 가설적 문서 기반 검색 | [Precise Zero-Shot Dense Retrieval without Relevance Labels](https://arxiv.org/abs/2212.10496) | 2022년 12월 |
| DSP: Demonstrate-Search-Predict 프레임워크 | [Demonstrate-Search-Predict: Composing retrieval and language models for knowledge-intensive NLP](https://arxiv.org/abs/2212.14024) | 2022년 12월 |
| Interleaving Retrieval with CoT | [Interleaving Retrieval with Chain-of-Thought Reasoning for Knowledge-Intensive Multi-Step Questions](https://arxiv.org/abs/2212.10509) | 2022년 12월 |
| 롱테일 지식 학습 한계 | [Large Language Models Struggle to Learn Long-Tail Knowledge](https://arxiv.org/abs/2211.08411) | 2022년 11월 |
| Recitation-Augmented LM | [Recitation-Augmented Language Models](https://arxiv.org/abs/2210.01296) | 2022년 10월 |
| Promptagator: 8개 예시 기반 밀집 검색 | [Promptagator: Few-shot Dense Retrieval From 8 Examples](https://arxiv.org/abs/2209.11755) | 2022년 9월 |
| Atlas: 소수 예시 기반 검색 증강 학습 | [Atlas: Few-shot Learning with Retrieval Augmented Language Models](https://arxiv.org/abs/2208.03299) | 2022년 8월 |
| 학습 데이터 검색 기반 NLG/NLU 성능 향상 | [Training Data is More Valuable than You Think: A Simple and Effective Method by Retrieving from Training Data](https://arxiv.org/abs/2203.08773) | 2022년 3월 |
| Automaton-augmented Retrieval | [Neuro-Symbolic Language Modeling with Automaton-augmented Retrieval](https://arxiv.org/abs/2201.12431) | 2022년 1월 |
| RETRO: 2조 토큰 검색 기반 LLM | [Improving language models by retrieving from trillions of tokens](https://arxiv.org/abs/2112.04426) | 2021년 12월 |
| 제로샷 슬롯 필링용 RAG | [Robust Retrieval Augmented Generation for Zero-shot Slot Filling](https://arxiv.org/abs/2108.13934) | 2021년 8월 |
| RAG: seq2seq+밀집 벡터 인덱스 | [Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2005.11401) | 2020년 5월 |
| Dense Passage Retrieval | [Dense Passage Retrieval for Open-Domain Question Answering](https://arxiv.org/abs/2004.04906) | 2020년 4월 |