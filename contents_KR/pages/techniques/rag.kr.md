# 검색 증강 생성(Retrieval Augmented Generation, RAG)

범용 언어 모델(general-purpose language model)은 감정 분석(sentiment analysis), 개체명 인식(named entity recognition) 등 여러 일반적인 과제에 파인튜닝(fine-tuning)하여 활용할 수 있습니다. 이러한 과제는 추가적인 배경 지식이 필요하지 않은 경우가 많습니다.

더 복잡하고 지식 집약적인 과제에서는, 외부 지식 소스에 접근하여 작업을 수행하는 언어 모델 기반 시스템을 구축할 수 있습니다. 이를 통해 생성 결과의 사실적 일관성(factual consistency)을 높이고, 응답의 신뢰성을 개선하며, "환각(hallucination)" 문제를 완화할 수 있습니다.

Meta AI 연구진은 이러한 지식 집약적 과제에 대응하기 위해 [검색 증강 생성(Retrieval Augmented Generation, RAG)](https://ai.facebook.com/blog/retrieval-augmented-generation-streamlining-the-creation-of-intelligent-natural-language-processing-models/) 기법을 제안했습니다. RAG는 정보 검색(information retrieval) 컴포넌트와 텍스트 생성(text generation) 모델을 결합합니다. RAG는 파인튜닝이 가능하며, 전체 모델을 재학습하지 않고도 내부 지식을 효율적으로 수정할 수 있습니다.

RAG는 입력을 받아, 소스(예: 위키피디아(Wikipedia))에서 관련/지원 문서를 검색합니다. 검색된 문서들은 원래 입력 프롬프트와 함께 컨텍스트(context)로 연결되어 텍스트 생성기에 입력되고, 최종 출력이 생성됩니다. 이 방식은 사실이 시간이 지나며 변할 수 있는 상황에 적응적입니다. LLM(대형 언어 모델, Large Language Model)의 파라메트릭 지식(parametric knowledge)은 정적이기 때문에, RAG는 재학습 없이 최신 정보를 검색 기반 생성(retrieval-based generation)으로 활용할 수 있게 해줍니다.

Lewis 등(2021)은 RAG를 위한 범용 파인튜닝 레시피를 제안했습니다. 사전학습된 시퀀스-투-시퀀스(seq2seq) 모델을 파라메트릭 메모리(parametric memory)로, 위키피디아의 밀집 벡터 인덱스(dense vector index)를 비파라메트릭 메모리(non-parametric memory, 신경망 기반 검색기(neural retriever)로 접근)로 사용합니다. 아래는 해당 접근법의 개요입니다:

이미지 출처: [Lewis et al. (2021)](https://arxiv.org/pdf/2005.11401.pdf)

RAG는 [Natural Questions](https://ai.google.com/research/NaturalQuestions), [WebQuestions](https://paperswithcode.com/dataset/webquestions), CuratedTrec 등 여러 벤치마크에서 강력한 성능을 보입니다. MS-MARCO, Jeopardy 질문 테스트에서도 RAG는 더 사실적이고 구체적이며 다양한 응답을 생성합니다. FEVER 사실 검증(fact verification)에서도 결과가 개선됩니다.

이는 RAG가 지식 집약적 과제에서 언어 모델의 출력을 향상시키는 실질적 옵션임을 보여줍니다.

최근에는 이러한 검색기 기반 접근법(retriever-based approach)이 더욱 대중화되어, ChatGPT 등 주요 LLM과 결합해 성능과 사실 일관성을 높이고 있습니다.

## RAG 활용 사례: 친근한 머신러닝(ML) 논문 제목 생성

아래는 오픈소스 LLM을 활용해 RAG 시스템을 구축, 간결한 머신러닝 논문 제목을 생성하는 노트북 튜토리얼입니다:

- [RAG 시작하기: 예제 노트북](https://github.com/dair-ai/Prompt-Engineering-Guide/blob/main/notebooks/pe-rag.ipynb)

## 참고 문헌

- [Retrieval-Augmented Generation for Large Language Models: A Survey](https://arxiv.org/abs/2312.10997) (2023년 12월)
- [Retrieval Augmented Generation: Streamlining the creation of intelligent natural language processing models](https://ai.meta.com/blog/retrieval-augmented-generation-streamlining-the-creation-of-intelligent-natural-language-processing-models/) (2020년 9월)
